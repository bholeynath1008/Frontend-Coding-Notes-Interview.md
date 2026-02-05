# JavaScript Promises - Complete Guide

| Method | Description | Settles when | Returns |
|--------|-------------|--------------|---------|
| `Promise.all()` | All succeed | First rejection OR all fulfill | Array of values |
| `Promise.allSettled()` | All complete (success/fail) | All settle | Array of status objects |
| `Promise.race()` | First to settle (any) | First settles | Value/reason of first |
| `Promise.any()` | First to succeed | First fulfillment OR all reject | Value OR AggregateError |
| `Promise.resolve()` | Creates resolved promise | Immediately | Resolved promise |
| `Promise.reject()` | Creates rejected promise | Immediately | Rejected promise |

# JavaScript Promises - Complete Guide with Quick Reference

## 📌 **Creating Promises**

### **Traditional Constructor** - Standard promise creation with executor function
```javascript
const promise = new Promise((resolve, reject) => {
    // Async operation
    if (success) resolve(value);
    else reject(error);
});
```
**Returns:** Promise object  
**Use Case:** Standard promise creation when you control async logic internally

---

### **Promise.withResolvers()** - Creates promise with external resolve/reject control
```javascript
const { promise, resolve, reject } = Promise.withResolvers();
// Control externally: resolve(value) or reject(error)
```
**Returns:** Object with {promise, resolve, reject}  
**Use Case:** When you need to resolve/reject promise outside executor function

---

## 📌 **Consuming Promises**

### **.then() / .catch() / .finally()** - Chainable promise handlers
```javascript
promise
    .then(value => console.log(value))      // Success handler
    .catch(error => console.error(error))   // Error handler
    .finally(() => console.log("Done"));    // Cleanup handler
```
**Returns:** New promise for chaining  
**Use Case:** Handling promise results with separate success/error callbacks

---

### **async/await** - Synchronous-style promise handling
```javascript
async function getData() {
    try {
        const value = await promise;  // Pause until promise settles
        return value;
    } catch (error) {
        console.error(error);
    }
}
```
**Returns:** Promise (implicitly)  
**Use Case:** Writing asynchronous code that looks synchronous, better error handling

---

## 🚀 **Promise Combinator Methods**

### **Promise.all()** - Wait for ALL promises to SUCCEED
```javascript
Promise.all([promise1, promise2, promise3])
    .then(allValues => console.log(allValues))  // [val1, val2, val3]
    .catch(error => console.error(error));      // If ANY fails
```
**Returns:** Promise that resolves to array of values  
**Use Case:** Multiple independent async operations where ALL must succeed (e.g., fetching multiple API endpoints)

---

### **Promise.allSettled()** - Wait for ALL promises to COMPLETE (success/fail)
```javascript
Promise.allSettled([promise1, promise2])
    .then(results => {
        results.forEach(result => console.log(result.status)); // "fulfilled" or "rejected"
    });
```
**Returns:** Promise that resolves to array of result objects with status/value/reason  
**Use Case:** When you need results from all promises regardless of failures (e.g., batch processing with partial success)

---

### **Promise.race()** - Get FIRST promise to SETTLE (win or lose)
```javascript
Promise.race([fastPromise, slowPromise])
    .then(firstResult => console.log(firstResult))  // From whichever settles first
    .catch(error => console.error(error));          // If first to settle is rejection
```
**Returns:** Promise with value/reason of first settled promise  
**Use Case:** Timeouts, caching with expiration, fastest response wins

---

### **Promise.any()** - Get FIRST promise to SUCCEED
```javascript
Promise.any([promise1, promise2])
    .then(firstSuccess => console.log(firstSuccess))  // First successful value
    .catch(aggregateError => {                        // If ALL fail
        console.error(aggregateError.errors);
    });
```
**Returns:** Promise with value of first fulfilled promise OR AggregateError if all reject  
**Use Case:** Multiple fallback sources, redundancy (e.g., multiple CDN sources)

---

## ⚡ **Promise Utility Methods**

### **Promise.resolve()** - Create immediately RESOLVED promise
```javascript
const resolved = Promise.resolve("Immediate value");
// Equivalent to: new Promise(resolve => resolve("Immediate value"))
```
**Returns:** Promise already resolved with given value  
**Use Case:** Converting values to promises, starting promise chains, testing

---

### **Promise.reject()** - Create immediately REJECTED promise
```javascript
const rejected = Promise.reject(new Error("Failed"));
```
**Returns:** Promise already rejected with given reason  
**Use Case:** Error testing, starting rejection chains, API mocking

---

## 🔗 **Promise Chaining Patterns**

### **Sequential Chaining** - Run promises one after another
```javascript
operation1()
    .then(result1 => operation2(result1))  // Waits for operation1
    .then(result2 => operation3(result2)); // Waits for operation2
```
**Returns:** Final promise in chain  
**Use Case:** Dependent async operations (output of one is input to next)

---

### **Parallel Chaining** - Run promises in parallel, process together
```javascript
const [result1, result2] = await Promise.all([
    asyncOp1(),
    asyncOp2()
]);
```
**Returns:** Array of results  
**Use Case:** Independent async operations that can run simultaneously

---

## 🛡️ **Error Handling Patterns**

### **Catch at End** - Handle any error in chain
```javascript
promiseChain
    .then(step1)
    .then(step2)
    .catch(error => console.error("Chain failed:", error));
```
**Use Case:** Centralized error handling for entire promise chain

---

### **Individual Error Recovery** - Handle errors at specific steps
```javascript
operation1()
    .catch(error => "default value")      // Recover from operation1 failure
    .then(value => operation2(value))     // Continue with recovered value
```
**Use Case:** Graceful degradation, fallback values

---

## 📊 **Quick Comparison Table**

| Method | Description | Returns When Success | Returns When Error | Best For |
|--------|-------------|---------------------|-------------------|----------|
| **Promise.all()** | All must succeed | Array of values | First error | Dependent operations |
| **Promise.allSettled()** | All complete | Status objects | Never errors | Batch processing |
| **Promise.race()** | First to settle | First value | First error | Timeouts |
| **Promise.any()** | First to succeed | First value | AggregateError | Fallback sources |

---

## 🎯 **Real-World Examples**

### **API Requests with Timeout**
```javascript
async function fetchWithTimeout(url, timeout = 5000) {
    const fetchPromise = fetch(url);
    const timeoutPromise = new Promise((_, reject) => 
        setTimeout(() => reject(new Error("Timeout")), timeout)
    );
    
    return Promise.race([fetchPromise, timeoutPromise]);
    // Use Case: Prevent hanging requests
}
```

### **Loading Multiple Resources**
```javascript
async function loadPageResources() {
    const [userData, posts, settings] = await Promise.all([
        fetch("/api/user"),
        fetch("/api/posts"),
        fetch("/api/settings")
    ]);
    // Use Case: Parallel API calls for page initialization
}
```

### **Fallback Strategy**
```javascript
async function getDataWithFallback() {
    return Promise.any([
        fetchPrimarySource(),
        fetchBackupSource1(),
        fetchBackupSource2()
    ]);
    // Use Case: High availability systems with multiple data sources
}
```

### **Progress Tracking**
```javascript
async function processBatch(items) {
    const results = await Promise.allSettled(
        items.map(item => processItem(item))
    );
    
    const successful = results.filter(r => r.status === 'fulfilled');
    const failed = results.filter(r => r.status === 'rejected');
    
    // Use Case: Batch operations with progress reporting
}
```

## 💡 **Key Principles**

1. **Promises are eager** - Start executing immediately when created
2. **Immutable** - Once settled (fulfilled/rejected), cannot change
3. **Chaining** - `.then()` always returns a new promise
4. **Error Propagation** - Errors bubble up until caught
5. **Microtask Queue** - Promise callbacks run after current synchronous code

## 🚨 **Common Pitfalls**

```javascript
// ❌ WRONG: Forgetting return in .then()
promise.then(value => {
    process(value);  // No return = next .then() gets undefined
});

// ✅ CORRECT: Always return
promise
    .then(value => {
        return process(value);  // Explicit return
    })
    .then(processed => console.log(processed));

// ❌ WRONG: Not handling promise rejections
unhandledPromise();  // Creates "Unhandled promise rejection"

// ✅ CORRECT: Always add error handling
unhandledPromise().catch(console.error);
```
