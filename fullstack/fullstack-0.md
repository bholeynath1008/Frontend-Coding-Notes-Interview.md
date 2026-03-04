# 🏦  Senior Full Stack Engineer
## Complete Interview Q&A Guide · Project Aurora

> **How to use this guide:** Every question includes a structured answer approach, code examples where relevant, and a ⚡ Pro Tip showing what separates a senior answer from a mid-level one. All answers are tied to your actual resume experience.

---

## 📋 Table of Contents

1. [Mock Live Coding Test Overview](#mock-live-coding-test)
2. [Section 1: Core JavaScript / TypeScript / Node.js](#section-1-core-javascript--typescript--nodejs)
3. [Section 2: React / Next.js / TailwindCSS](#section-2-react--nextjs--tailwindcss)
4. [Section 3: Testing](#section-3-testing)
5. [Section 4: Web Services & APIs](#section-4-web-services--apis)
6. [Section 5: Cross-Browser & Mobile Design](#section-5-cross-browser--mobile-design)
7. [Section 6: DevOps & Deployment](#section-6-devops--deployment)
8. [Section 7: Architecture & System Design](#section-7-architecture--system-design)
9. [Section 8: SQL & Data](#section-8-sql--data)
10. [Section 9: Behavioral & Leadership](#section-9-behavioral--leadership)
11. [Final Tips: Do's and Don'ts](#final-tips-dos-and-donts)

---

## 🖥️ Mock Live Coding Test

### ✅ General Preparation Tips

#### 1. Environment Setup
- Install **Node.js LTS** — verify with `node --version`
- Install **pnpm** — `npm install -g pnpm`
- Install **TypeScript** globally — `npm install -g typescript`
- Scaffold a test Express app before the interview so your environment is proven working
- Use **VS Code** with these extensions: ESLint, Prettier, TypeScript, REST Client

```bash
# Verify your environment is ready
node --version    # should be LTS (20.x or 22.x)
pnpm --version
tsc --version
```

#### 2. Brush Up On Key Concepts

| Topic | What to Know |
|---|---|
| **Node.js** | Event loop phases, async/await, Promises, modules (ESM vs CJS) |
| **TypeScript** | Interfaces, type aliases, generics, union types, type guards |
| **Express.js** | Middleware order, async error handling, request/response lifecycle |
| **FinTech** | Accounts, transactions, balances, idempotency, fraud detection, KYC/AML basics |

---

### 🛠️ Exercise #1: Bug Fixing

**Goal:** Assess debugging skills and code comprehension under time pressure.

#### Preparation Strategy
- Practice reading **unfamiliar code** quickly — scan for structure before diving into logic
- Use TypeScript's type system as your first debugging tool — type errors often reveal the bug
- Know these **common Express.js bugs cold:**
  - Incorrect middleware order (auth before routes, error handler last)
  - Missing `await` on async operations
  - Async errors not passed to `next(err)` — Express won't catch them automatically
  - Mutating `req`/`res` objects incorrectly

#### What to Look For in the Bug Fixing Exercise

```typescript
// ❌ BUGGY CODE — find all the issues
app.post('/transfer', async (req, res) => {
  const { fromAccount, toAccount, amount } = req.body;

  // BUG 1: No input validation — amount could be negative, null, or a string
  // BUG 2: Missing await — returns a Promise, not the account object
  const account = db.findById(fromAccount);

  // BUG 3: This always evaluates wrong because account is a Promise object
  if (account.balance < amount) {
    res.status(400).json({ error: 'Insufficient funds' });
    // BUG 4: Missing return — execution continues after sending response!
  }

  // BUG 5: No try/catch — unhandled rejection crashes the process
  const result = await db.transfer(fromAccount, toAccount, amount);
  res.json(result);
  // BUG 6: No audit logging — critical for FinTech compliance
});
```

```typescript
// ✅ FIXED VERSION
app.post('/transfer', async (req, res, next) => {
  try {
    const { fromAccount, toAccount, amount } = req.body;

    // Fix 1: Validate all inputs
    if (!fromAccount || !toAccount || typeof amount !== 'number' || amount <= 0) {
      return res.status(400).json({ error: 'INVALID_INPUT' });
    }

    // Fix 2: await the DB call
    const account = await db.findById(fromAccount);

    // Fix 3 + 4: Correct check with early return
    if (account.balance < amount) {
      return res.status(422).json({ error: 'INSUFFICIENT_FUNDS' });
    }

    // Fix 5: Wrapped in try/catch, errors passed to next()
    const result = await db.transfer(fromAccount, toAccount, amount);

    // Fix 6: Audit log
    await auditLog.write({ fromAccount, toAccount, amount, userId: req.user.id });

    res.status(201).json(result);
  } catch (err) {
    next(err); // passes to Express error middleware
  }
});
```

> **⚡ During the interview:** Narrate your thought process out loud. Say: *"I see this function is async but I don't see await before the DB call — that means account holds a Promise object, not the actual data. The balance check will always be wrong."* Thinking out loud IS the test.

#### Common Bug Categories to Memorize

| Bug Type | Example | Fix |
|---|---|---|
| Missing `await` | `const user = db.find(id)` | `const user = await db.find(id)` |
| Missing `return` after `res.send()` | Code runs after response sent | Add `return` before `res.status(...).json(...)` |
| Stale closure in `useEffect` | Captures old variable value | Add variable to deps array |
| Mutating state directly | `state.items.push(x)` | `setState([...state.items, x])` |
| No error handling | Unhandled promise rejection | Wrap in `try/catch`, pass to `next(err)` |
| Infinite re-render loop | `useEffect` with no deps calling `setState` | Add proper dependency array |

---

### 🔁 Exercise #2: Transfer API

**Goal:** Build a secure, reliable fund transfer endpoint from scratch.

#### Preparation Strategy
- Know RESTful API design principles — correct HTTP verbs, status codes, resource naming
- Implement input validation with **Zod** (preferred) or Joi
- Handle all edge cases: insufficient funds, invalid accounts, duplicate requests
- **Idempotency is mandatory** — this is the #1 FinTech API requirement

#### Complete Transfer API Implementation

```typescript
import express, { Request, Response, NextFunction } from 'express';
import { z } from 'zod';
import { v4 as uuidv4 } from 'uuid';

const router = express.Router();

// Step 1: Define and validate input schema with Zod
const TransferSchema = z.object({
  fromAccountId: z.string().uuid(),
  toAccountId:   z.string().uuid(),
  amount:        z.number().int().positive(), // ← always use cents (integers), never floats!
  currency:      z.literal('USD'),
  memo:          z.string().max(255).optional()
});

// POST /api/v1/transfers
router.post('/transfers', authMiddleware, async (req: Request, res: Response, next: NextFunction) => {
  try {
    // Step 2: Validate input
    const parsed = TransferSchema.safeParse(req.body);
    if (!parsed.success) {
      return res.status(400).json({ error: 'INVALID_INPUT', details: parsed.error.flatten() });
    }
    const { fromAccountId, toAccountId, amount, currency, memo } = parsed.data;

    // Step 3: Check idempotency key (prevents duplicate transfers on retry)
    const idempotencyKey = req.headers['idempotency-key'] as string;
    if (idempotencyKey) {
      const existing = await redis.get(`idem:${idempotencyKey}`);
      if (existing) {
        return res.status(200).json(JSON.parse(existing)); // return cached result
      }
    }

    // Step 4: Authorize — verify the user OWNS the fromAccount (IDOR prevention)
    const userId = req.user.id;
    const fromAccount = await AccountRepository.findById(fromAccountId);
    if (!fromAccount || fromAccount.userId !== userId) {
      return res.status(403).json({ error: 'FORBIDDEN' });
    }

    // Step 5: Check balance
    if (fromAccount.balance < amount) {
      return res.status(422).json({ error: 'INSUFFICIENT_FUNDS' });
    }

    // Step 6: Execute atomically — both debit and credit in a single DB transaction
    const transfer = await db.$transaction(async (tx) => {
      await tx.account.update({
        where: { id: fromAccountId },
        data: { balance: { decrement: amount } }
      });
      await tx.account.update({
        where: { id: toAccountId },
        data: { balance: { increment: amount } }
      });
      return tx.transfer.create({
        data: {
          id: uuidv4(),
          fromAccountId,
          toAccountId,
          amount,
          currency,
          memo,
          status: 'COMPLETED',
          userId,
          createdAt: new Date()
        }
      });
    });

    // Step 7: Cache result for idempotency
    if (idempotencyKey) {
      await redis.setex(`idem:${idempotencyKey}`, 86400, JSON.stringify(transfer));
    }

    // Step 8: Audit log every transfer (compliance requirement)
    await AuditLog.write({
      action: 'TRANSFER',
      userId,
      fromAccountId,
      toAccountId,
      amount,
      transferId: transfer.id,
      ip: req.ip,
      timestamp: new Date()
    });

    return res.status(201).json(transfer);

  } catch (err) {
    next(err);
  }
});
```

#### Key Design Decisions to Mention Proactively

| Decision | Why It Matters |
|---|---|
| **Integers/cents for amount** | `0.1 + 0.2 = 0.30000000000000004` in JS — floats are unsafe for money |
| **Idempotency key** | Client can safely retry on network failure without double-charging |
| **Atomic DB transaction** | Both debit + credit succeed or both roll back — no partial state |
| **IDOR check** | Verify user OWNS the account, not just that they're authenticated |
| **Audit logging** | Legal/compliance requirement — all money movement must be traceable |
| **HttpOnly cookies for auth** | Never store JWT in localStorage — vulnerable to XSS attacks |

---

### 🧠 FinTech-Specific Considerations

| Area | What to Know |
|---|---|
| **Security** | Validate all inputs (Zod/Joi), sanitize data, never expose account numbers in logs, use HTTPS/TLS everywhere |
| **Concurrency** | Use DB transactions or optimistic locking to prevent race conditions in balance checks |
| **Auditability** | Every transfer, login attempt, and error must be logged with userId, IP, and timestamp |
| **Compliance** | KYC (Know Your Customer) and AML (Anti-Money Laundering) — be aware even if not directly implementing |
| **Idempotency** | All financial operations must be safely retryable — same input always produces same result |
| **Error messages** | Never expose stack traces or internal DB errors to clients — both UX and security |

---

## Section 1: Core JavaScript / TypeScript / Node.js

---

### Q1: What are the key differences between JavaScript and TypeScript? When would you choose one over the other?

#### How to Answer

**TypeScript is a statically typed superset of JavaScript** — it compiles to JS and adds type safety at build time, not runtime.

Key differences:

| Feature | JavaScript | TypeScript |
|---|---|---|
| Type checking | Runtime only | Compile time |
| IDE support | Basic autocomplete | Full type inference, refactoring |
| Interfaces/Generics | No native support | First-class feature |
| Error detection | In production | In your editor |
| Learning curve | Lower | Slightly higher upfront |

**When to choose TypeScript:** Any production application with a team, anything handling money or sensitive data, large codebases where refactoring confidence matters.

**When plain JS is okay:** Quick scripts, prototypes, small utilities where type overhead isn't worth it.

**Your resume connection:** *"At Bank of America we used TypeScript throughout — it caught interface mismatches between our BFF layer and internal services before they ever hit production. In a banking context, a type error on an account balance field could mean the wrong amount displayed to a customer."*

> **⚡ Pro Tip:** Always tie TypeScript back to safety in FinTech. Interviewers love hearing that you use it to prevent data shape mismatches at the API boundary — especially at a bank.

---

### Q2: How does the event loop work in Node.js? How are async operations handled?

#### How to Answer

Node.js is **single-threaded but non-blocking** — the event loop enables handling thousands of concurrent operations without spawning threads.

**Event loop phases (in order):**

1. **Timers** — executes `setTimeout` / `setInterval` callbacks whose delay has elapsed
2. **Pending callbacks** — I/O callbacks deferred from the previous iteration
3. **Poll** — retrieves new I/O events and executes their callbacks; the loop waits here if the queue is empty
4. **Check** — `setImmediate()` callbacks execute here
5. **Close callbacks** — socket/stream close events

**How async works:** DB queries, HTTP calls, and file I/O are offloaded to libuv's thread pool or the OS kernel. When the operation completes, its callback is placed in the appropriate queue. The main thread never blocks — it just processes the queue.

**Important nuance:** `process.nextTick()` fires **before** the next event loop iteration (before even the Timers phase), which is different from `setImmediate()`.

```javascript
console.log('1 - sync');

process.nextTick(() => console.log('2 - nextTick'));

setTimeout(() => console.log('3 - setTimeout'), 0);

setImmediate(() => console.log('4 - setImmediate'));

// Output: 1 → 2 → 3 → 4
// nextTick always fires before the next loop iteration
```

**Your resume connection:** *"In my PG&E BFF layer I used async/await for all GraphQL resolvers. Understanding the event loop helped me identify that a CPU-heavy data transformation was blocking the poll phase — I moved it to a worker thread."*

> **⚡ Pro Tip:** Senior engineers name specific phases. Saying "it handles async without blocking" is mid-level. Mentioning the poll phase, libuv, and `process.nextTick` vs `setImmediate` is senior-level.

---

### Q3: Describe how you would structure a large-scale Node.js application for maintainability and scalability.

#### How to Answer

Use a **layered architecture** — each layer has one responsibility:

```
src/
  routes/          # HTTP endpoint definitions, input parsing only
  controllers/     # Orchestrate request handling, call services
  services/        # All business logic (transferFunds, validateBalance)
  repositories/    # All DB access encapsulated here
  middleware/      # auth.ts, rateLimit.ts, errorHandler.ts, logger.ts
  config/          # database.ts, env.ts, constants.ts
  types/           # Shared TypeScript interfaces and types
  utils/           # Pure utility functions (formatCurrency, maskAccountNumber)
```

**Key principles:**

- **Dependency injection** — don't instantiate services inside controllers; inject them for testability
- **Centralized error handling** — one Express error middleware at the bottom catches all thrown errors
- **Environment config** — `dotenv` + a typed config module; never hardcode secrets
- **Repository pattern** — data access abstracted so you can swap MongoDB for PostgreSQL without touching business logic
- **Feature flags** — for gradual rollout of new banking features

```typescript
// Correct layering example
// routes/transfer.routes.ts
router.post('/transfers', authMiddleware, transferController.create);

// controllers/transfer.controller.ts
async create(req: Request, res: Response, next: NextFunction) {
  try {
    const result = await transferService.execute(req.body, req.user.id);
    res.status(201).json(result);
  } catch (err) {
    next(err);
  }
}

// services/transfer.service.ts — business logic lives here
async execute(dto: TransferDTO, userId: string): Promise<Transfer> {
  await this.validateBalance(dto.fromAccountId, dto.amount);
  return this.transferRepository.create({ ...dto, userId });
}
```

**Your resume connection:** *"At Bank of America I built the Node.js BFF with this layered pattern. When we migrated from MongoDB to PostgreSQL for one service, only the repository file changed — controllers and services were untouched."*

> **⚡ Pro Tip:** Name the pattern: *"I follow the Repository Pattern with a Service layer."* Senior engineers name patterns, they don't just describe them.

---

## Section 2: React / Next.js / TailwindCSS

---

### Q4: How does React's virtual DOM work, and why is it beneficial?

#### How to Answer

React maintains a **lightweight in-memory representation** of the real DOM (the virtual DOM tree).

**The reconciliation process:**

1. On state/prop change, React creates a **new virtual DOM tree**
2. React **diffs** the new tree against the previous one (the reconciliation algorithm)
3. React computes the **minimal set of real DOM mutations** needed
4. Real DOM updates are **batched and applied** — React 18 batches all updates automatically

**Why it's beneficial:**
- Avoids expensive direct DOM manipulation on every state change
- Predictable rendering — UI is always a function of state
- Enables React's concurrent rendering features (React 18)

**Performance tools built on this foundation:**

| Tool | What it does |
|---|---|
| `React.memo` | Skips re-render if props haven't changed (shallow comparison) |
| `useMemo` | Memoizes expensive computed values between renders |
| `useCallback` | Memoizes function references — prevents unnecessary re-renders of child components |
| Virtualization | Only renders visible rows in DOM (react-window, react-virtual) |

**Your resume connection:** *"In the PG&E dashboard with 100K+ rows, reconciliation was doing thousands of unnecessary DOM comparisons per interaction. I combined React.memo on row components with react-virtual — render time dropped from 4 seconds to under 200ms."*

> **⚡ Pro Tip:** Always connect virtual DOM theory to a concrete optimization you shipped. Generic answers don't impress at senior level.

---

### Q5: What are the advantages of using Next.js over Create React App?

#### How to Answer

Create React App (CRA) is a client-side-only SPA setup. Next.js is a full-stack framework with multiple rendering strategies.

| Feature | Create React App | Next.js |
|---|---|---|
| Rendering | Client-side only | SSR, SSG, ISR, RSC, Client |
| SEO | Poor (blank HTML shell) | Excellent (pre-rendered HTML) |
| Performance | Blank screen until JS loads | HTML arrives instantly |
| API routes | Separate backend required | Built-in API routes / Server Actions |
| Routing | React Router (manual) | File-system routing (automatic) |
| Bundle splitting | Manual configuration | Automatic per-page |
| Active development | Deprecated / archived | Actively maintained by Vercel |

**Next.js rendering modes for banking apps:**

```typescript
// App Router (Next.js 13+) — recommended for new projects

// React Server Component — fetches data on server, zero client JS
// Perfect for: account balances (always fresh), secure data fetching
async function AccountBalance({ accountId }: { accountId: string }) {
  const balance = await getBalance(accountId); // runs on server
  return <div>${balance / 100}</div>; // no API call exposed to client
}

// Client Component — needed for interactivity
'use client';
function TransferForm() {
  const [amount, setAmount] = useState('');
  // ...
}
```

**Your resume connection:** *"At Bank of America I used Next.js API Routes as the BFF layer — no separate Express server needed. At PG&E I built a document management system using SSR so document metadata appeared instantly on load without a loading spinner."*

> **⚡ Pro Tip:** Mention React Server Components explicitly — this is the future of Next.js and signals you're current with the 2024/2025 ecosystem.

---

### Q6: How do you implement responsive design using TailwindCSS? Can you give an example?

#### How to Answer

TailwindCSS uses a **utility-first, mobile-first** approach. Responsive breakpoints are applied as prefixes — styles without a prefix apply to all screen sizes; prefixed styles override at that breakpoint and above.

**Breakpoints:**

| Prefix | Min Width | Use case |
|---|---|---|
| (none) | 0px | Mobile base styles |
| `sm:` | 640px | Large mobile / small tablet |
| `md:` | 768px | Tablet |
| `lg:` | 1024px | Desktop |
| `xl:` | 1280px | Large desktop |

**Example — Banking Dashboard Layout:**

```tsx
// Responsive account summary cards
function AccountSummary() {
  return (
    // Mobile: 1 column, Desktop: 3 columns
    <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 p-4">

      <div className="
        bg-white rounded-lg shadow-md p-4
        border border-gray-200
        hover:shadow-lg transition-shadow
      ">
        {/* Mobile: stacked, Desktop: side by side */}
        <div className="flex flex-col sm:flex-row sm:items-center sm:justify-between">
          <span className="text-sm text-gray-500 font-medium">Checking Account</span>
          {/* Mobile: smaller text, Desktop: larger */}
          <span className="text-2xl lg:text-3xl font-bold text-gray-900 mt-1 sm:mt-0">
            $4,231.00
          </span>
        </div>
      </div>

    </div>
  );
}
```

**Best practices for banking apps:**
- Always design mobile-first — many users check balances on their phone
- Use Tailwind's `dark:` prefix for dark mode support
- Leverage `@apply` in CSS for frequently repeated component patterns
- Use `container` class with `mx-auto` for consistent max-width on large screens

**Your resume connection:** *"At NAPA I used TailwindCSS to build the product catalog and NAPA Rewards enrollment forms — the responsive grid layout worked across all devices without custom media queries."*

> **⚡ Pro Tip:** Mention mobile-first as a philosophy, not just a technique — for a bank with millions of mobile users, it's a business priority.

---

## Section 3: Testing

---

### Q7: How would you structure unit and integration tests for a React application?

#### How to Answer

Follow the **Testing Trophy** (Kent C. Dodds) — prioritize integration tests over pure unit tests for React apps because they test how components actually work together.

**Test structure:**

```
src/
  components/
    TransferForm/
      TransferForm.tsx
      TransferForm.test.tsx      ← component integration test
      TransferForm.stories.tsx   ← Storybook
  services/
    transfer.service.test.ts     ← unit test (pure logic)
  __tests__/
    transfer-flow.e2e.cy.ts      ← Cypress E2E
```

**Component integration test (Jest + React Testing Library):**

```typescript
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import { http, HttpResponse } from 'msw';
import { server } from '../mocks/server'; // MSW server
import TransferForm from './TransferForm';

describe('TransferForm', () => {
  it('shows error when transfer fails with insufficient funds', async () => {
    // Arrange: mock API to return error
    server.use(
      http.post('/api/transfers', () => {
        return HttpResponse.json(
          { error: 'INSUFFICIENT_FUNDS' },
          { status: 422 }
        );
      })
    );

    // Act
    render(<TransferForm />);
    fireEvent.change(screen.getByLabelText('Amount'), { target: { value: '9999999' } });
    fireEvent.click(screen.getByRole('button', { name: /transfer/i }));

    // Assert
    await waitFor(() => {
      expect(screen.getByText(/insufficient funds/i)).toBeInTheDocument();
    });
  });
});
```

**Unit test (service logic):**

```typescript
describe('transferService.validateBalance', () => {
  it('throws INSUFFICIENT_FUNDS when balance < amount', async () => {
    const mockRepo = { findById: jest.fn().mockResolvedValue({ balance: 100 }) };
    const service = new TransferService(mockRepo);

    await expect(service.validateBalance('acc-1', 200))
      .rejects.toThrow('INSUFFICIENT_FUNDS');
  });
});
```

**Your resume connection:** *"At PG&E I achieved 80%+ coverage using Vitest + RTL for component tests and Cypress for E2E. At NAPA and BofA I maintained 85%+ using Jest + Cypress with coverage as a CI quality gate."*

> **⚡ Pro Tip:** Reference RTL's philosophy: "Test behavior, not implementation." Query by accessible roles (`getByRole`) not test IDs where possible — it also validates accessibility at the same time.

---

### Q8: What are the differences between Jest, Playwright, and Cypress? When would you use each?

#### How to Answer

| Tool | Type | Runs In | Best For |
|---|---|---|---|
| **Jest** | Unit + Integration | Node.js (no browser) | Functions, services, React components with RTL |
| **Cypress** | E2E | Real browser (Chromium) | Full user flows, great DX, time-travel debugging |
| **Playwright** | E2E | Real browsers (Chrome, Firefox, Safari, Edge) | Cross-browser testing, parallel runs, mobile viewports |

**When to use each in a banking context:**

- **Jest:** Test transfer validation logic, React form components, API route handlers (with Supertest)
- **Cypress:** Test the complete transfer flow (login → fill form → confirm → success screen) against a staging environment
- **Playwright:** Cross-browser compliance testing — banking apps must work in IE11/Edge for enterprise customers; test mobile viewports for iOS Safari and Android Chrome

**Testing pyramid for this role:**

```
         /\
        /  \         Playwright/Cypress E2E
       /    \        (few, slow, expensive, high confidence)
      /------\
     /        \      Jest Integration (API routes, multi-component)
    /          \
   /------------\
  /              \   Jest Unit (many, fast, cheap)
 /________________\
```

**Your resume connection:** *"At PG&E I used Vitest + RTL for unit/component tests, Cypress for E2E. At NAPA I maintained 85%+ coverage with Jest + Cypress. The JD lists Playwright as an option — I'm familiar with it for cross-browser testing scenarios."*

> **⚡ Pro Tip:** The JD lists "Playwright or Cypress" — mention you know both. Highlight Playwright's multi-browser advantage for a bank with strict browser compatibility requirements.

---

### Q9: How do you mock API calls in your tests?

#### How to Answer

**Three approaches, each with a use case:**

**1. jest.mock() — for unit tests mocking modules directly**

```typescript
// Mock an entire module
jest.mock('../services/transferService', () => ({
  execute: jest.fn().mockResolvedValue({ transferId: 'abc-123', status: 'COMPLETED' })
}));
```

**2. MSW (Mock Service Worker) — preferred for component tests**

MSW intercepts at the network level — tests behave closer to production. Same mocks work in both Jest (Node) and browser.

```typescript
// mocks/handlers.ts
import { http, HttpResponse } from 'msw';

export const handlers = [
  http.post('/api/transfers', () => {
    return HttpResponse.json({ transferId: 'uuid-123', status: 'COMPLETED' }, { status: 201 });
  }),
  // Override for error cases in specific tests:
  http.post('/api/transfers', () => {
    return HttpResponse.json({ error: 'INSUFFICIENT_FUNDS' }, { status: 422 });
  })
];
```

**3. cy.intercept() — for Cypress E2E tests**

```typescript
// Cypress: stub network, test UI response
cy.intercept('POST', '/api/transfers', {
  statusCode: 201,
  body: { transferId: 'uuid-123', status: 'COMPLETED' }
}).as('transferRequest');

cy.get('[data-testid=submit]').click();
cy.wait('@transferRequest');
cy.contains('Transfer successful').should('be.visible');
```

> **⚡ Pro Tip:** Mention MSW proactively — most candidates only know `jest.mock()`. MSW tests at the network layer which means your tests are more realistic and the same mock definitions work across Jest and browser-based tests.

---

## Section 4: Web Services & APIs

---

### Q10: What are the key differences between REST and SOAP APIs?

#### How to Answer

| Feature | REST | SOAP |
|---|---|---|
| Protocol | HTTP only | HTTP, SMTP, TCP |
| Data format | JSON (typically) | XML only |
| Contract | OpenAPI/Swagger (optional) | WSDL (mandatory, strict) |
| Overhead | Lightweight | Heavy (XML envelope, headers) |
| Error handling | HTTP status codes | SOAP Fault element |
| State | Stateless | Can be stateful |
| Tooling | Widely supported | Enterprise/legacy systems |

**Why this matters for First Horizon:** The JD explicitly lists "SOAP & REST" in the tech stack. Legacy banking systems (core banking, payment processors) often expose SOAP APIs. Your BFF layer in Node.js will consume SOAP and present clean REST to the React frontend.

```typescript
// Consuming a SOAP API from Node.js BFF
import soap from 'strong-soap';

async function getAccountBalance(accountId: string): Promise<number> {
  const client = await soap.createClient(CORE_BANKING_WSDL_URL);
  const result = await client.GetAccountBalanceAsync({ accountId });
  // Transform XML response to clean JSON for the frontend
  return result.Balance.Amount;
}

// Expose as clean REST endpoint
router.get('/accounts/:id/balance', async (req, res) => {
  const balance = await getAccountBalance(req.params.id);
  res.json({ accountId: req.params.id, balance, currency: 'USD' });
});
```

> **⚡ Pro Tip:** Frame it as: *"The BFF pattern lets us abstract SOAP complexity — the React frontend only sees clean REST JSON, while Node.js handles the SOAP envelope transformation internally."* This shows architectural thinking.

---

### Q11: How do you handle API versioning in a production environment?

#### How to Answer

**Three main strategies:**

**1. URL path versioning (recommended — most common in banking)**
```
GET /api/v1/transfers
GET /api/v2/transfers   ← new version with breaking changes
```

**2. Header versioning**
```
GET /api/transfers
Accept: application/vnd.firsthorizon.v2+json
```

**3. Query parameter versioning (least preferred)**
```
GET /api/transfers?version=2
```

**Best practices for production:**

```typescript
// Express — clean version routing
const v1Router = express.Router();
const v2Router = express.Router();

v1Router.post('/transfers', v1TransferController.create);
v2Router.post('/transfers', v2TransferController.create); // new fields, new behavior

app.use('/api/v1', v1Router);
app.use('/api/v2', v2Router);

// Deprecation strategy:
// 1. Release v2 alongside v1
// 2. Add deprecation headers to v1 responses
// 3. Communicate sunset date to clients (6–12 months for banking)
// 4. Remove v1 after sunset
```

**Key principles:**
- Never make breaking changes to an existing version
- Use deprecation headers (`Deprecation`, `Sunset`) to communicate timeline
- Maintain at least 2 versions simultaneously during migration periods
- Document all versions in OpenAPI/Swagger

> **⚡ Pro Tip:** Mention the **Sunset HTTP header** — it lets API clients programmatically know when a version will be removed. Shows you think about API consumers, not just the API itself.

---

### Q12: How would you secure an API endpoint?

#### How to Answer

**Defense in depth — multiple layers:**

```typescript
// Complete security middleware stack for a banking API
import helmet from 'helmet';
import rateLimit from 'express-rate-limit';
import { z } from 'zod';

// 1. Security headers (XSS protection, CSP, HSTS, etc.)
app.use(helmet());

// 2. Rate limiting — stricter on sensitive endpoints
const transferLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 10, // 10 transfer attempts per window
  standardHeaders: true,
  legacyHeaders: false,
});

// 3. Authentication middleware — verify JWT
async function authMiddleware(req, res, next) {
  const token = req.cookies.accessToken; // HttpOnly cookie — not localStorage!
  if (!token) return res.status(401).json({ error: 'UNAUTHORIZED' });

  try {
    const payload = jwt.verify(token, process.env.JWT_SECRET);
    req.user = payload;
    next();
  } catch {
    res.status(401).json({ error: 'TOKEN_EXPIRED' });
  }
}

// 4. Input validation — reject before any DB access
app.post('/transfers',
  transferLimiter,
  authMiddleware,
  async (req, res) => {
    const result = TransferSchema.safeParse(req.body);
    if (!result.success) return res.status(400).json({ error: 'INVALID_INPUT' });
    // ...
  }
);
```

**Security checklist for banking APIs:**

| Layer | Implementation |
|---|---|
| Transport | TLS 1.2+ everywhere, HSTS headers |
| Auth | JWT (15min expiry) + refresh tokens in HttpOnly cookies |
| Authorization | Check user OWNS the resource (IDOR prevention) |
| Input | Zod/Joi validation on ALL request bodies |
| Rate limiting | Per-IP + per-user limits on sensitive endpoints |
| Headers | Helmet.js for security headers |
| Logging | Log all requests — mask sensitive fields |
| Secrets | Environment variables, never in code or logs |

> **⚡ Pro Tip:** Lead with **IDOR (Insecure Direct Object Reference)** — verifying the user owns the resource, not just that they're authenticated. It's the #1 banking API vulnerability and shows you think like a security engineer.

---

## Section 5: Cross-Browser & Mobile Design

---

### Q13: What are some common challenges in cross-browser development, and how do you address them?

#### How to Answer

**Common challenges and solutions:**

| Challenge | Root Cause | Solution |
|---|---|---|
| CSS rendering differences | Browsers have different default styles | CSS reset/normalize (`@tailwind base`) |
| Flexbox/Grid gaps (older Safari) | Older WebKit bugs | Use `gap` polyfills or margin fallbacks |
| JavaScript API support | Browser vendors ship features at different rates | Check caniuse.com; use polyfills or feature detection |
| Font rendering | OS-level rendering differences | Specify font stacks, test on real devices |
| Input type differences | `<input type="date">` varies widely | Use consistent UI libraries or custom components |
| WebSocket support | Older browsers or proxy issues | Feature detect, fallback to long-polling |

**Practical approach:**

```typescript
// Feature detection instead of browser detection
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/sw.js');
}

// CSS: progressive enhancement with fallbacks
.balance-grid {
  display: flex;         /* fallback */
  display: grid;         /* modern browsers override */
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
}
```

**Banking-specific concerns:**
- Internet Explorer/Edge legacy for enterprise business customers
- Cross-browser form validation behavior (required, pattern attributes)
- Consistent number formatting for currency across locales
- PDF viewer compatibility for statements

**Your resume connection:** *"At NAPA I configured Google Analytics to monitor Core Web Vitals across browsers. At BofA we maintained a cross-browser test suite — IE11 was still a requirement for business banking customers."*

> **⚡ Pro Tip:** Mention **Playwright** for automated cross-browser testing — it's in the JD and is the modern solution for running the same E2E tests across Chrome, Firefox, and Safari automatically.

---

### Q14: How do you test and optimize a web app for mobile devices?

#### How to Answer

**Testing approach:**

1. **Chrome DevTools** — device emulation with network throttling (3G simulation)
2. **Playwright/Cypress** — set mobile viewport and touch events in tests
3. **Real device testing** — BrowserStack or physical devices for iOS Safari and Android Chrome
4. **Lighthouse** — performance, accessibility, and PWA audits in CI pipeline

**Performance optimizations for mobile:**

```typescript
// 1. Next.js Image optimization — auto-resizes for mobile
import Image from 'next/image';
<Image src="/logo.png" width={200} height={60} sizes="(max-width: 768px) 100vw, 200px" />

// 2. Code splitting — don't load the chart library on mobile account pages
const TransferChart = dynamic(() => import('./TransferChart'), {
  ssr: false,
  loading: () => <ChartSkeleton />
});

// 3. Touch-friendly targets (min 44x44px per Apple HIG)
<button className="min-h-[44px] min-w-[44px] p-3">
  Transfer
</button>
```

**Mobile-specific banking considerations:**
- Biometric auth (Face ID/fingerprint) — use WebAuthn API
- Touch-friendly form inputs — large tap targets, appropriate keyboard types (`inputmode="decimal"` for amounts)
- Offline support — Service Worker caching for account balance viewing without connection
- Reduced data usage — skeleton screens instead of loading spinners, progressive image loading

> **⚡ Pro Tip:** Mention `inputmode="decimal"` for currency input fields — it shows the numeric keyboard on mobile without the browser's date/type validation quirks. This is a real UX detail that shows mobile-first thinking.

---

## Section 6: DevOps & Deployment

---

### Q15: Describe your experience with GitHub Actions or Jenkins. How do you set up a CI/CD pipeline?

#### How to Answer

**Your GitHub Actions experience (PG&E):**

```yaml
# .github/workflows/ci.yml
name: CI/CD Pipeline

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'pnpm'

      - run: pnpm install --frozen-lockfile
      - run: pnpm run lint           # ESLint + TypeScript check
      - run: pnpm run test:coverage  # Jest with coverage threshold
      - run: pnpm run test:e2e       # Cypress/Playwright

      # Quality gate: fail pipeline if coverage < 80%
      - name: Check coverage threshold
        run: pnpm run coverage:check

  build-and-deploy:
    needs: quality
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - name: Build Docker image
        run: docker build -t app:${{ github.sha }} .

      - name: Push to registry
        run: docker push registry.example.com/app:${{ github.sha }}

      - name: Deploy to Kubernetes
        run: |
          kubectl set image deployment/app \
            app=registry.example.com/app:${{ github.sha }}
```

**Jenkins (CloudBees) — relevant to this role:**

```groovy
// Jenkinsfile
pipeline {
  agent any
  stages {
    stage('Install')  { steps { sh 'pnpm install' } }
    stage('Lint')     { steps { sh 'pnpm run lint' } }
    stage('Test')     { steps { sh 'pnpm run test:coverage' } }
    stage('Build')    { steps { sh 'docker build -t app:${BUILD_NUMBER} .' } }
    stage('Deploy')   {
      when { branch 'main' }
      steps { sh 'kubectl apply -f k8s/' }
    }
  }
  post {
    failure { slackSend message: "Build #${BUILD_NUMBER} failed" }
  }
}
```

**Quality gates (critical — mention these):**
- Pipeline **fails** if test coverage drops below threshold
- Pipeline **fails** if TypeScript errors exist
- Snyk security scan — pipeline fails on critical vulnerabilities
- Automated E2E tests must pass before deploy

> **⚡ Pro Tip:** Quality gates are the key senior distinction — the pipeline should **reject** bad code, not just report it. This protects the main branch and is essential for a regulated banking environment.

---

### Q16: What strategies do you use to manage feature branches and pull requests in a team setting?

#### How to Answer

**Branching strategy (GitHub Flow — recommended for this team):**

```
main ←─────────────────────────────── protected, always deployable
  │
  ├── feature/aurora-transfer-form     ← short-lived, daily merges
  ├── feature/aurora-account-summary
  ├── fix/transfer-validation-bug
  └── release/v1.2.0                  ← optional: if releases are batched
```

**PR best practices:**

- **Small PRs** — under 400 lines of diff; easier to review, less merge conflicts
- **Conventional commits** — `feat:`, `fix:`, `chore:`, `test:` prefixes for changelog generation
- **PR template** — checklist: tests added, docs updated, self-reviewed
- **Required reviewers** — at least 1 senior engineer approval before merge
- **Linked Jira ticket** — every PR references its story in the description
- **Branch protection** — require CI to pass, no direct pushes to main

**Code review culture:**
- Review code, not the person — use "nit:" prefix for non-blocking suggestions
- Approve-with-comments for minor issues, request changes only for blockers
- Respond to review comments within 24 hours

**Your resume connection:** *"At PG&E I established Git conventions for the team — conventional commits, PR templates, and required CI checks before merge. This significantly improved code review speed and traceability."*

> **⚡ Pro Tip:** Mention **trunk-based development** awareness — frequent small merges to main reduce merge conflicts and enable continuous deployment, which aligns with Agile/Aurora's delivery cadence.

---

### Q17: What are the benefits of Docker + Kubernetes? How do you troubleshoot in a containerized environment?

#### How to Answer

**Docker benefits:**

| Benefit | Why it matters |
|---|---|
| Environment consistency | "Works on my machine" is eliminated — dev = staging = prod |
| Isolated dependencies | Node 18 for one service, Node 20 for another — no conflicts |
| Fast, reproducible builds | Multi-stage builds produce small, optimized images |
| Easy rollback | `kubectl rollout undo` reverts to previous image tag instantly |

**Kubernetes benefits:**
- **Auto-scaling** (HPA) — scale Transfer API pods under high load automatically
- **Self-healing** — crashed pods restart automatically
- **Rolling updates** — zero-downtime deploys (new pods come up before old ones go down)
- **Service discovery** — services communicate by name, not hardcoded IPs

**Troubleshooting playbook:**

```bash
# 1. Check pod status
kubectl get pods -n banking-app
# Look for: CrashLoopBackOff, ImagePullBackOff, Pending, OOMKilled

# 2. Get logs from crashed container
kubectl logs <pod-name> --previous -n banking-app

# 3. Describe pod for detailed events (scheduling, probe failures)
kubectl describe pod <pod-name> -n banking-app

# 4. Shell into a running container for live debugging
kubectl exec -it <pod-name> -n banking-app -- /bin/sh

# 5. Check resource usage (OOMKilled = exceeded memory limit)
kubectl top pods -n banking-app

# 6. Check recent events
kubectl get events --sort-by='.lastTimestamp' -n banking-app
```

**Common failure modes:**
- `CrashLoopBackOff` — app is crashing on startup; check logs for missing env vars or DB connection errors
- `ImagePullBackOff` — image tag doesn't exist or registry auth failed
- `OOMKilled` — increase memory limits in deployment spec
- `Pending` — insufficient cluster resources; check node capacity

**Your resume connection:** *"At PG&E I containerized all services with Docker and deployed via GitHub Actions to a Kubernetes cluster. When a pod entered CrashLoopBackOff I used `kubectl logs --previous` to identify a missing environment variable that only existed in staging."*

> **⚡ Pro Tip:** The JD mentions **Dynatrace** as a bonus skill. Even saying *"I've worked with Dynatrace for distributed tracing across Kubernetes pods — it's invaluable for tracking a transfer request through the BFF to the backend service"* will stand out.

---

### Q18: What strategies do you use for monitoring and troubleshooting in production?

#### How to Answer

**Observability = Logs + Metrics + Traces (the three pillars):**

**Logging:**

```typescript
// Structured logging — machine-readable, searchable
import winston from 'winston';

const logger = winston.createLogger({
  format: winston.format.json(),
  transports: [new winston.transports.Console()]
});

// Log with context — never log raw account numbers or amounts
logger.info('Transfer initiated', {
  transferId: 'uuid-123',
  userId: 'user-456',
  fromAccount: maskAccount(fromAccountId), // last 4 digits only
  amount: amount,
  requestId: req.headers['x-request-id']
});
```

**Key metrics to monitor for a banking app:**
- Transfer API response time (p50, p95, p99)
- Transfer success/failure rate
- Error rate by endpoint
- DB query duration
- Active user sessions
- Failed auth attempts (security alert)

**Alerting thresholds:**
- P99 response time > 2s — performance alert
- Error rate > 1% — engineering on-call
- Failed auth attempts spike — security team alert

> **⚡ Pro Tip:** Mention **distributed tracing** with correlation IDs — a `x-request-id` header passed through every service call lets you trace a single user's transfer request through the BFF, transfer service, and database in one timeline.

---

## Section 7: Architecture & System Design

---

### Q19: How would you design a secure and scalable internet banking application?

#### How to Answer

**Start with the layered architecture, then drill into security and scalability.**

```
┌─────────────────────────────────────────┐
│           Client Layer                   │
│  Next.js (SSR/RSC) · TailwindCSS        │
│  React · TypeScript                      │
└──────────────────┬──────────────────────┘
                   │ HTTPS only
┌──────────────────▼──────────────────────┐
│              API Gateway                 │
│  Apigee · Rate limiting · Auth          │
│  TLS termination · Request routing      │
└────┬──────────────┬───────────┬─────────┘
     │              │           │
┌────▼───┐  ┌───────▼──┐  ┌────▼──────┐
│  Auth   │  │Transfers │  │ Accounts  │
│ Service │  │ Service  │  │  Service  │
│ (JWT)  │  │ (Node.js)│  │ (Node.js) │
└────┬───┘  └────┬─────┘  └────┬──────┘
     │           │              │
┌────▼───────────▼──────────────▼────────┐
│              Data Layer                  │
│  PostgreSQL (ACID) · Redis (cache/idem) │
│  MongoDB (audit logs) · Kafka (events)  │
└─────────────────────────────────────────┘
```

**Security architecture:**
- All communication TLS 1.2+ — no HTTP
- JWT tokens (15min expiry) + refresh tokens in HttpOnly cookies
- API Gateway enforces auth before requests reach services
- PCI DSS zone segmentation — card data in isolated network zone
- WAF (Web Application Firewall) at the edge

**Scalability:**
- Stateless services — any pod can handle any request (enables horizontal scaling)
- Redis for session storage — not in-memory (survives pod restarts)
- DB connection pooling — PgBouncer for PostgreSQL
- CDN for static assets — Next.js static files served from edge

**Mapping to Project Aurora:** *"This maps directly to Aurora's pod structure — Money Movement, Security, and Retail Enrollment are separate domains. Each pod deploys independently, owns its data, and communicates via APIs — exactly the microservices pattern."*

> **⚡ Pro Tip:** Structure your answer in 3–4 minutes: layers first (30 sec), then pick two areas to go deep — security and scalability are the most relevant for banking. Don't try to cover everything.

---

### Q20: What design patterns have you used in your projects, and why?

#### How to Answer

| Pattern | Where You Used It | Why |
|---|---|---|
| **Repository Pattern** | BofA — switched from MongoDB to PostgreSQL | Abstracts data access so business logic is DB-agnostic |
| **BFF (Backend for Frontend)** | PG&E (Apollo/GraphQL), BofA (Next.js API Routes) | Aggregates multiple APIs, reduces client-side requests |
| **Observer/Pub-Sub** | PG&E — WebSocket + GraphQL subscriptions | Components subscribe to updates without polling |
| **Middleware Chain** | All Node.js work | auth → validate → rate-limit → handler; each step isolated |
| **Strategy Pattern** | Transfer types (ACH, wire, internal) | Select different processing logic at runtime based on type |
| **Singleton** | DB connection pool, Logger | One shared instance across the app |
| **Factory** | Creating different transfer/account types | Single factory function based on type enum |

**Example — Strategy Pattern for transfers:**

```typescript
interface TransferStrategy {
  execute(transfer: TransferDTO): Promise<TransferResult>;
}

class ACHTransfer implements TransferStrategy { /* ... */ }
class WireTransfer implements TransferStrategy { /* ... */ }
class InternalTransfer implements TransferStrategy { /* ... */ }

// Factory + Strategy combined
function createTransferStrategy(type: 'ACH' | 'WIRE' | 'INTERNAL'): TransferStrategy {
  const strategies = {
    ACH: new ACHTransfer(),
    WIRE: new WireTransfer(),
    INTERNAL: new InternalTransfer()
  };
  return strategies[type];
}
```

> **⚡ Pro Tip:** Always give a reason: *"I used the Repository Pattern BECAUSE it let me swap MongoDB for PostgreSQL without touching the service layer."* Reason + result is the senior-level framing.

---

## Section 8: SQL & Data

---

### Q21: How do you optimize SQL queries for performance?

#### How to Answer

**Optimization techniques in priority order:**

**1. Indexing (highest impact):**

```sql
-- Add composite index on the most common query pattern
CREATE INDEX idx_transfers_user_date
  ON transfers (user_id, created_at DESC);

-- The query this optimizes
SELECT * FROM transfers
WHERE user_id = $1
ORDER BY created_at DESC
LIMIT 20;
```

**2. Use EXPLAIN ANALYZE — always before and after optimizing:**

```sql
EXPLAIN ANALYZE
SELECT * FROM transfers WHERE user_id = $1;
-- Look for: Seq Scan (bad on large tables) vs Index Scan (good)
-- Look for: actual rows vs estimated rows (large mismatch = stale stats)
```

**3. Cursor-based pagination over OFFSET:**

```sql
-- ❌ OFFSET pagination — degrades linearly (page 1000 scans 20,000 rows)
SELECT * FROM transfers LIMIT 20 OFFSET 19980;

-- ✅ Cursor-based — constant performance regardless of page
SELECT * FROM transfers
WHERE created_at < $last_cursor
ORDER BY created_at DESC
LIMIT 20;
```

**4. SELECT specific columns:**

```sql
-- ❌ Fetches all columns — unnecessary I/O
SELECT * FROM accounts;

-- ✅ Only what the UI needs
SELECT id, account_number_last4, balance, account_type FROM accounts;
```

**5. N+1 query prevention:**

```typescript
// ❌ N+1 — 1 query for accounts + N queries for balances
const accounts = await db.query('SELECT * FROM accounts WHERE user_id = $1', [userId]);
for (const account of accounts) {
  account.transactions = await db.query('SELECT * FROM transactions WHERE account_id = $1', [account.id]);
}

// ✅ Single JOIN query
const result = await db.query(`
  SELECT a.*, t.* FROM accounts a
  LEFT JOIN transactions t ON t.account_id = a.id
  WHERE a.user_id = $1
`, [userId]);
```

**Your resume connection:** *"At Bank of America I optimized a financial dashboard query from 4.2 seconds to 180ms by adding a composite index on (account_id, created_at) and switching from OFFSET to cursor-based pagination."*

> **⚡ Pro Tip:** Cursor-based pagination is a senior SQL optimization that most developers don't know. Mention it with the reason — OFFSET performance degrades linearly with page number because the DB must scan all preceding rows.

---

### Q22: Can you explain stored procedures and when they are appropriate?

#### How to Answer

**What they are:** Pre-compiled SQL code stored in the database, executed by name with parameters — like functions, but in the DB layer.

```sql
-- Example: Stored procedure for atomic transfer
CREATE OR REPLACE PROCEDURE execute_transfer(
  p_from_account UUID,
  p_to_account UUID,
  p_amount BIGINT,  -- in cents
  p_transfer_id UUID
)
LANGUAGE plpgsql
AS $$
BEGIN
  -- Debit
  UPDATE accounts SET balance = balance - p_amount
  WHERE id = p_from_account AND balance >= p_amount;

  IF NOT FOUND THEN
    RAISE EXCEPTION 'INSUFFICIENT_FUNDS';
  END IF;

  -- Credit
  UPDATE accounts SET balance = balance + p_amount
  WHERE id = p_to_account;

  -- Audit log
  INSERT INTO transfer_audit (transfer_id, from_account, to_account, amount, created_at)
  VALUES (p_transfer_id, p_from_account, p_to_account, p_amount, NOW());

  COMMIT;
END;
$$;
```

**When stored procedures ARE appropriate:**
- Complex atomic operations that must execute as one DB transaction (money transfers)
- Performance-critical operations that benefit from pre-compilation and execution plan caching
- Enforcing data integrity rules at the DB layer (not just application layer)
- Legacy banking systems where business logic lives in the DB (common in financial institutions)

**When to prefer application-layer logic instead:**
- Business logic that changes frequently — stored procedures are harder to version control and deploy
- When you need full TypeScript type safety and unit testability
- Greenfield projects where the team is stronger in TypeScript than SQL

**Your resume connection:** *"The JD mentions SQL stored procedures as a 'plus.' In banking systems I've consumed stored procedures in legacy core banking integrations via the BFF layer — they're common in financial institutions for atomic balance operations."*

> **⚡ Pro Tip:** Acknowledge both sides — stored procedures have real value in banking for atomicity, but can create deployment coupling. Showing you understand the trade-offs is more impressive than a one-sided answer.

---

## Section 9: Behavioral & Leadership

> **Framework for all behavioral questions: STAR**
> **S**ituation → **T**ask → **A**ction → **R**esult (with metrics)

---

### Q23: Describe a time when you led a technical design session. What was the outcome?

#### How to Answer (STAR)

**Situation:** At Bank of America, our team needed to architect a new financial reporting dashboard. Six engineers across frontend, backend, and data teams needed alignment before a single line of code was written.

**Task:** I was asked to lead the design session as the senior frontend engineer — responsible for driving to a concrete architecture decision in 90 minutes.

**Action:**
- Prepared a C4 architecture diagram showing component relationships and data flow
- Researched 3 API options (REST, GraphQL, BFF+REST) and prepared a trade-off matrix
- Ran the session: 15 min context, 30 min discussion of options, 30 min drilling into the winning approach, 15 min action items
- Proposed the BFF pattern to aggregate 3 internal services — reduced client-side complexity significantly
- Aligned the team on folder structure, API contracts, and TypeScript interface definitions before coding began

**Result:** We shipped the dashboard 2 sprints ahead of schedule because architectural alignment before coding eliminated rework. The BFF pattern later allowed us to add a mobile client without changing any backend services.

> **⚡ Pro Tip:** For Project Aurora, you'll be leading design sessions across pods. Show you came *prepared* (diagrams, options), *facilitated* (didn't dictate), and *drove to a decision* (didn't just discuss endlessly).

---

### Q24: How do you handle disagreements in a cross-functional team?

#### How to Answer (STAR)

**Situation:** At NAPA Online, the backend team proposed returning all product data in a single large API response for simplicity. I believed this would cause serious performance issues for catalog pages with 10,000+ SKUs.

**Task:** Advocate for the correct technical approach while maintaining a strong working relationship with the backend team.

**Action:**
- I didn't just argue — I built a quick benchmark: measured response payload size (3.2MB monolithic vs 48KB paginated for the first page)
- Scheduled a 30-minute meeting with both teams, shared the performance data visually
- Acknowledged the backend team's concern (simpler implementation) and proposed a middle ground: paginated endpoint with a generous default page size
- Asked: *"What outcome are we both trying to achieve?"* — this reframed it from a personal disagreement to a shared problem

**Result:** We aligned on pagination. API error rates dropped by 30% (noted in my resume). More importantly, we established a process: future API contracts were drafted and performance-reviewed before implementation.

> **⚡ Pro Tip:** Show you use **data** to resolve disagreements, not just persuasion. Engineers who bring benchmarks or prototypes to disagreements are respected as senior contributors.

---

### Q25: How do you manage your work in an Agile environment?

#### How to Answer

**Your process:**

- **Sprint planning:** Break epics into stories under 8 points. Push back on vague requirements — I ask for the acceptance criteria before committing points.
- **Daily standups:** Update Jira before the standup. Report blockers the same day they're identified — not the next morning.
- **During the sprint:** Update ticket status in real time (In Progress → Code Review → QA → Done). Link PRs directly to Jira tickets.
- **Definition of Done:** Code reviewed, tests passing at 80%+ coverage, deployed to staging, acceptance criteria met. Not just "PR merged."
- **Retrospectives:** Specific, actionable items — not "communicate better" but "add a PR template to reduce review back-and-forth."

**Cross-pod coordination (relevant to Aurora):**
- Aurora has multiple pods (Security, Money Movement, Retail, etc.) — dependency management across pods is the core Agile challenge
- Use dependency mapping in sprint planning; flag inter-pod dependencies early

**Your resume connection:** *"At PG&E I worked in 2-week sprints with a distributed team. I introduced a norm of updating ticket status before standup — it cut our daily sync time by half and made blockers visible earlier. The JD specifically lists GitHub + Jira, which is exactly the stack I use."*

> **⚡ Pro Tip:** Mention **GitHub + Jira** specifically — that's the exact tooling in the JD. Saying you've linked PRs to Jira tickets and used GitHub Actions for CI shows you're ready to contribute from day one.

---

### Q26: Give an example of a time you took ownership of a critical issue and resolved it.

#### How to Answer (STAR)

**Situation:** In production at PG&E, the field operations dashboard was timing out for users with large datasets. Operations teams managing 100K+ record surveys were experiencing 8+ second page loads — directly impacting their ability to coordinate field workers in real time.

**Task:** This wasn't assigned to me — I identified it through user feedback in Slack and took ownership because it was directly affecting mission-critical operations.

**Action:**
- Profiled with Chrome DevTools Performance tab — identified two root causes: (1) rendering all 100K rows in the DOM simultaneously, (2) the GraphQL API was returning the full dataset with no pagination
- Designed and proposed a two-part fix: cursor-based pagination in the GraphQL API, and react-virtual for client-side row virtualization
- Estimated effort, got stakeholder buy-in, and shipped both fixes within 1 sprint
- Added a performance budget test to CI so this class of regression would be caught automatically

**Result:** Page load dropped from 8+ seconds to under 300ms. Zero performance escalations from the operations team after the fix. The pagination pattern I established was later adopted across 3 other data-heavy views.

> **⚡ Pro Tip:** Ownership stories need a **before and after metric**. "I fixed a performance issue" is forgettable. "Page load went from 8 seconds to 300ms and we added a CI performance budget to prevent regression" is what gets remembered — and gets you the offer.

---

## ✅ Final Tips: Do's and Don'ts

### The Senior Answer Framework

Every technical answer should follow this structure:
1. **Name the concept** — "I used the Repository Pattern"
2. **Explain the why** — "because it let me swap databases without touching business logic"
3. **Give your example** — "At Bank of America when we migrated from MongoDB to PostgreSQL..."
4. **State the result** — "only the repository file changed, zero impact on services or controllers"

### Do's ✅

| What to Do | Why It Matters |
|---|---|
| Name design patterns explicitly | Shows senior-level vocabulary |
| Give BEFORE/AFTER metrics from your resume | Concrete evidence beats vague claims |
| Think out loud during live coding | Narration IS the test — they want to see your process |
| Mention idempotency proactively on transfer questions | #1 FinTech API signal |
| Use cents/integers for money — mention float risk | Instantly signals FinTech maturity |
| Tie every answer to banking context | You're interviewing for a BANK |
| Ask 2–3 sharp questions about Project Aurora | Signals genuine interest and preparation |
| Reference your BofA experience for banking credibility | Direct banking experience = major differentiator |

### Don'ts ❌

| What to Avoid | Why It Hurts |
|---|---|
| Vague answers without naming patterns | "I improved performance" — not memorable |
| Silent problem-solving during live coding | They can't assess your thinking |
| Generic answers with no FinTech context | You could be interviewing for any company |
| Overstating experience (K8s, Azure depth) | You will be caught; honesty builds more trust |
| Floats for monetary values | Red flag for any FinTech interviewer |
| Asking no questions at the end | Signals disengagement |
| Heavy prep the night before the interview | Sleep > cramming |

---

## 🚀 Environment Setup Checklist

Run these commands today to confirm your setup:

```bash
# Verify Node.js LTS
node --version   # should be v20.x or v22.x

# Verify pnpm
pnpm --version   # should be v8+

# Create test Next.js project to confirm environment works
pnpm create next-app@latest test-app --typescript --tailwind --app
cd test-app && pnpm dev   # should open on localhost:3000

# Verify TypeScript
tsc --version    # should be 5.x

# Install useful global tools
npm install -g ts-node nodemon
```

---

> **💡 Remember:** You have 6+ years of experience, direct banking experience at Bank of America, and the exact tech stack First Horizon uses. You've built financial dashboards, REST APIs, and full-stack banking features in production.
>
> Walk in confident. You belong in this room. 🏦
