# Idempotency: Preventing Duplicate Transactions in E-Commerce


**Q: How do you prevent duplicate transactions in e-commerce order placement?**

> "We implement **idempotency** using a client-generated **idempotency key** sent in the request header. When the order service receives a request, it first checks if that key already exists in our idempotency store (Redis + PostgreSQL). If found, we return the cached response — meaning the duplicate request doesn't create a new order.
>
> Each step in our saga also has its own idempotency:
> - **Inventory:** Uses `ON CONFLICT` to prevent double reservation
> - **Payment:** Passes idempotency key to gateway so authorization isn't duplicated
> - **Compensations:** Check state before refunding (don't refund twice)
>
> We also use **optimistic locking** with version numbers for status updates. Keys expire after 24 hours to manage storage. This ensures that network retries, double-clicks, or saga retries never cause duplicate transactions."


## Idempotency Key Lifecycle

```
┌─────────────────────────────────────────────────────────────┐
│                    IDEMPOTENCY KEY FLOW                      │
└─────────────────────────────────────────────────────────────┘

1. CLIENT GENERATES KEY
   └── UUID v4: "f47ac10b-58cc-4372-a567-0e02b2c3d479"

2. SEND WITH REQUEST
   └── Header: Idempotency-Key: f47ac10b-58cc-4372-a567-0e02b2c3d479

3. SERVER RECEIVES
   ├── Check if key exists in database
   ├── If exists → return cached response
   └── If new → create lock, process request, store result

4. STORE RESULT
   └── Key, resource_id, response, status

5. EXPIRE AFTER 24-48 HOURS
   └── Cleanup job removes old keys
```

---
## 1. What is Idempotency?

**Definition:** An operation is idempotent if performing it multiple times has the same effect as performing it once.

> **Simple Analogy:** 
> - ❌ **Not idempotent:** "Add $10 to cart" (doing it twice adds $20)
> - ✅ **Idempotent:** "Set cart total to $50" (doing it twice still $50)

---

## 2. Why Idempotency Matters in E-Commerce

| Problem | Consequence |
|---------|-------------|
| User double-clicks "Place Order" | Two orders created, charged twice |
| Network timeout → client retries | Payment captured multiple times |
| Saga retry after step failure | Inventory deducted twice |
| Webhook redelivery | Duplicate shipment created |

**Without idempotency:** Customer gets charged twice, inventory oversold, unhappy users, refund costs.

---

## 3. The Idempotency Key Pattern

### 3.1 How It Works

```
Client                                    Server
   │                                         │
   │  POST /orders                           │
   │  Headers: Idempotency-Key: UUID-123     │
   │────────────────────────────────────────▶│
   │                                         │
   │                                    ┌────▼────┐
   │                                    │ Check if │
   │                                    │ key exists│
   │                                    └────┬────┘
   │                                         │
   │                              ┌──────────┼──────────┐
   │                              │          │          │
   │                         Key exists   Key new    Key expired
   │                              │          │          │
   │                    Return cached   Process    Reject/
   │                    response once   & store    Error
   │                              │          │
   │◀────────────────────────────────┘          │
   │                                         │
   │◀────────────────────────────────────────┘
```

### 3.2 Implementation

```sql
-- Idempotency table
CREATE TABLE idempotency_keys (
    key VARCHAR(255) PRIMARY KEY,
    resource_type VARCHAR(50),      -- order, payment, shipment
    resource_id VARCHAR(100),       -- created order ID
    response_payload JSONB,         -- cached response
    status VARCHAR(20),             -- PROCESSING, COMPLETED, FAILED
    created_at TIMESTAMP,
    expires_at TIMESTAMP
);
```

---

## 4. Idempotency in Each Transaction Step

### Step 1: Validate Order
| Aspect | Details |
|--------|---------|
| **Idempotency Key Source** | Client-generated UUID in header |
| **Store Response** | Cache validation result |
| **Duplicate Handling** | Return same validation result |

### Step 2: Reserve Inventory
| Aspect | Details |
|--------|---------|
| **Idempotency Key** | `order_id + sku` |
| **Operation** | `UPDATE inventory SET reserved = reserved + 1 WHERE sku = ? AND order_id NOT IN (processed_orders)` |
| **Duplicate Handling** | Second call detects reservation already exists, returns success |

```sql
-- Idempotent inventory reserve
INSERT INTO inventory_reservations (order_id, sku, quantity, created_at)
VALUES ('ORD-123', 'SKU-456', 2, NOW())
ON CONFLICT (order_id, sku) DO NOTHING;
-- If already exists, no change
```

### Step 3: Authorize Payment
| Aspect | Details |
|--------|---------|
| **Idempotency Key** | `order_id` or `idempotency_key` sent to payment gateway |
| **Operation** | Payment gateway uses key to prevent double authorization |
| **Duplicate Handling** | Gateway returns existing authorization ID |

```javascript
// Payment service call
const authResponse = await paymentGateway.authorize({
    idempotencyKey: `ORDER-${orderId}`,  // Gateway dedupes
    amount: order.total,
    paymentMethod: order.paymentMethod
});
// If key already used, returns same authorization
```

### Step 4: Create Shipment
| Aspect | Details |
|--------|---------|
| **Idempotency Key** | `order_id` |
| **Operation** | Check if shipment exists before creating |
| **Duplicate Handling** | Return existing shipment ID |

```javascript
async function createShipment(orderId) {
    const existing = await db.findShipmentByOrderId(orderId);
    if (existing) return existing;  // Idempotent return
    
    return await shippingService.create({ orderId });
}
```

### Step 5: Capture Payment
| Aspect | Details |
|--------|---------|
| **Idempotency Key** | `authorization_id + capture_attempt` |
| **Operation** | Capture with idempotency key to gateway |
| **Duplicate Handling** | Gateway prevents double capture |

### Step 6: Update Order Status
| Aspect | Details |
|--------|---------|
| **Idempotency Key** | `order_id + target_status` |
| **Operation** | Version-based or condition update |
| **Duplicate Handling** | Second update sees status already changed, no-op |

```sql
-- Optimistic locking / version-based
UPDATE orders 
SET status = 'CONFIRMED', version = version + 1
WHERE order_id = 'ORD-123' AND version = 5;
-- If version mismatch, update does nothing
```

---

## 5. Idempotency Key Lifecycle

```
┌─────────────────────────────────────────────────────────────┐
│                    IDEMPOTENCY KEY FLOW                      │
└─────────────────────────────────────────────────────────────┘

1. CLIENT GENERATES KEY
   └── UUID v4: "f47ac10b-58cc-4372-a567-0e02b2c3d479"

2. SEND WITH REQUEST
   └── Header: Idempotency-Key: f47ac10b-58cc-4372-a567-0e02b2c3d479

3. SERVER RECEIVES
   ├── Check if key exists in database
   ├── If exists → return cached response
   └── If new → create lock, process request, store result

4. STORE RESULT
   └── Key, resource_id, response, status

5. EXPIRE AFTER 24-48 HOURS
   └── Cleanup job removes old keys
```

---

## 6. Idempotency Across Saga Steps

### Problem: Saga retry can cause duplicate compensations

```javascript
// Bad: Not idempotent compensation
async function compensatePayment(context) {
    await paymentService.refund(context.paymentId);  // If called twice, refunds twice!
}

// Good: Idempotent compensation
async function compensatePayment(context) {
    const refundStatus = await paymentService.getRefundStatus(context.paymentId);
    if (refundStatus === 'NOT_REFUNDED') {
        await paymentService.refund(context.paymentId);
    }
    // If already refunded, no-op
}
```

### Idempotent Compensation Pattern

```javascript
class IdempotentSagaStep {
    async execute(context) {
        const key = `${context.orderId}:execute`;
        if (await this.alreadyExecuted(key)) {
            return this.getCachedResult(key);
        }
        const result = await this.doExecute(context);
        await this.storeResult(key, result);
        return result;
    }
    
    async compensate(context) {
        const key = `${context.orderId}:compensate`;
        if (await this.alreadyCompensated(key)) return;
        
        await this.doCompensate(context);
        await this.markCompensated(key);
    }
}
```

---

## 7. Idempotency Strategies by Operation Type

| Operation Type | Idempotency Strategy | Example |
|----------------|---------------------|---------|
| **Create** | Idempotency key + unique constraint | Order creation with order_id unique |
| **Update** | Version/ETag + conditional update | `UPDATE ... WHERE version = old_version` |
| **Delete** | Always idempotent | Deleting same resource twice → same result |
| **Compensation** | Check state before action | Refund only if not already refunded |
| **Notification** | Deduplication key | Email sent tracking with notification_id |

---

## 8. Common Idempotency Implementation Patterns

### Pattern 1: Database Unique Constraint
```sql
CREATE TABLE orders (
    id UUID PRIMARY KEY,
    order_number VARCHAR(50) UNIQUE,  -- Business key
    idempotency_key VARCHAR(255) UNIQUE  -- Or separate table
);
```

### Pattern 2: Redis Cache + Database
```javascript
async function processWithIdempotency(key, processor) {
    // 1. Check Redis cache (fast)
    const cached = await redis.get(key);
    if (cached) return JSON.parse(cached);
    
    // 2. Check database (fallback)
    const existing = await db.findIdempotencyRecord(key);
    if (existing) {
        await redis.setex(key, 3600, existing.response);
        return existing.response;
    }
    
    // 3. Process and store
    const result = await processor();
    await db.saveIdempotencyRecord(key, result);
    await redis.setex(key, 3600, JSON.stringify(result));
    return result;
}
```

### Pattern 3: Distributed Lock
```javascript
async function idempotentOperation(key, callback) {
    const lock = await redis.lock(`idempotent:${key}`, 5000);
    try {
        const existing = await getResult(key);
        if (existing) return existing;
        
        const result = await callback();
        await saveResult(key, result);
        return result;
    } finally {
        await lock.release();
    }
}
```

---

## 9. Idempotency in Distributed Sagas

### Saga State with Idempotency Tracking

```sql
CREATE TABLE saga_step_executions (
    id UUID PRIMARY KEY,
    saga_id VARCHAR(100),
    step_name VARCHAR(100),
    execution_type VARCHAR(20),  -- 'FORWARD' or 'COMPENSATE'
    execution_key VARCHAR(255) UNIQUE,  -- Idempotency key for this step
    result JSONB,
    executed_at TIMESTAMP
);
```

### Retry with Idempotency
```javascript
async function executeStepWithIdempotency(step, context) {
    const key = `${context.sagaId}:${step.name}:forward`;
    
    // Check if already executed this step in this saga
    const existing = await db.findStepExecution(key);
    if (existing) {
        console.log(`Step ${step.name} already executed, skipping`);
        return existing.result;
    }
    
    // Execute and record
    const result = await step.execute(context);
    await db.recordStepExecution(key, step.name, 'FORWARD', result);
    return result;
}
```

---

## 10. Quick Reference: Idempotency Rules

| Rule | Explanation |
|------|-------------|
| **1. Always use idempotency keys** | Client-generated, server-stored, expires after 24h |
| **2. Store response, not just status** | Return same response on duplicate calls |
| **3. Make compensations idempotent** | Check state before refunding/releasing |
| **4. Use database constraints** | Unique keys prevent duplicate inserts |
| **5. Version all updates** | Optimistic locking prevents double updates |
| **6. Clean up old keys** | Prevent unbounded storage growth |
| **7. Log duplicate attempts** | Monitor for unexpected retry patterns |

---

## 11. Interview Answer Template

**Q: How do you prevent duplicate transactions in e-commerce order placement?**

> "We implement **idempotency** using a client-generated **idempotency key** sent in the request header. When the order service receives a request, it first checks if that key already exists in our idempotency store (Redis + PostgreSQL). If found, we return the cached response — meaning the duplicate request doesn't create a new order.
>
> Each step in our saga also has its own idempotency:
> - **Inventory:** Uses `ON CONFLICT` to prevent double reservation
> - **Payment:** Passes idempotency key to gateway so authorization isn't duplicated
> - **Compensations:** Check state before refunding (don't refund twice)
>
> We also use **optimistic locking** with version numbers for status updates. Keys expire after 24 hours to manage storage. This ensures that network retries, double-clicks, or saga retries never cause duplicate transactions."
