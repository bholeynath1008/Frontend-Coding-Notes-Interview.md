# Section 9: TypeScript

## 1. TypeScript Fundamentals
Expected Answer: Use TypeScript for static typing, better tooling, and early error detection. Hints:
•	Basic types: string, number, boolean, array, tuple, enum, any, unknown
•	type vs interface: interface for object shapes, type for unions
•	Generics for reusable type-safe components
•	Type inference reduces annotation needs

## 2. Advanced TypeScript Features
Expected Answer: Leverage advanced TypeScript features for robust type safety. Hints:
•	Utility types: Partial, Required, Pick, Omit, Record
•	Conditional types: T extends U ? X : Y
•	Mapped types for transforming object types
•	Type guards: typeof, instanceof, custom type predicates
•	keyof and indexed access types

## 3. TypeScript with React
Expected Answer: Type React components, hooks, props, and events properly. Hints:
•	Component props typing with interfaces
•	Event typing: React.ChangeEvent<HTMLInputElement>
•	Hook typing: useState<Type>, useRef<Type>
•	Context typing with createContext<Type | undefined>
•	Generic components for reusability

# TypeScript Interview Guide

## Complete Table of Contents

### Section 1: Primitive Types
- [1. What are the basic primitive data types in TypeScript?](#1-what-are-the-basic-primitive-data-types-in-typescript)
- [2. What are the special utility-like primitive-ish types?](#2-what-are-the-special-utility-like-primitive-ish-types)

### Section 2: Composite Types
- [3. What are the composite/structural types?](#3-what-are-the-compositestructural-types)

### Section 3: Literal Types
- [4. What are literal types?](#4-what-are-literal-types)

### Section 4: Type Composition
- [5. What are union and intersection types?](#5-what-are-union-and-intersection-types)

### Section 5: Type Definition Mechanisms
- [6. What's the difference between `type` and `interface`?](#6-whats-the-difference-between-type-and-interface)
- [7. What are type aliases?](#7-what-are-type-aliases)
- [8. What are index signatures?](#8-what-are-index-signatures)

### Section 6: Advanced Type Operators
- [9. What is `keyof` and how is it used?](#9-what-is-keyof-and-how-is-it-used)
- [10. What is indexed access type (`T[K]`)?](#10-what-is-indexed-access-type-tk)
- [11. What is `typeof` in type context?](#11-what-is-typeof-in-type-context)

### Section 7: Utility Types
- [12. What are all the built-in utility types?](#12-what-are-all-the-built-in-utility-types)
- [13. Advanced utility combinations](#13-advanced-utility-combinations)

### Section 8: Conditional Types & Mapped Types
- [14. What are conditional types?](#14-what-are-conditional-types)
- [15. What is the `infer` keyword?](#15-what-is-the-infer-keyword)
- [16. What are mapped types?](#16-what-are-mapped-types)

### Section 9: Type Guards & Narrowing
- [17. What are type guards?](#17-what-are-type-guards)
- [18. What are custom type predicates?](#18-what-are-custom-type-predicates)

### Section 10: Advanced Patterns
- [19. What are template literal types?](#19-what-are-template-literal-types)
- [20. What are declaration merging and module augmentation?](#20-what-are-declaration-merging-and-module-augmentation)
- [21. What are assertion functions?](#21-what-are-assertion-functions)

### Section 11: React-Specific TypeScript
- [22. How do you type React components?](#22-how-do-you-type-react-components)
- [23. How do you type React hooks?](#23-how-do-you-type-react-hooks)
- [24. How do you handle events in React with TypeScript?](#24-how-do-you-handle-events-in-react-with-typescript)

### Section 12: Advanced React Patterns
- [25. How do you type higher-order components (HOCs)?](#25-how-do-you-type-higher-order-components-hocs)
- [26. What are render props and how do you type them?](#26-what-are-render-props-and-how-do-you-type-them)

### Section 13: Configuration & Best Practices
- [27. What are important TypeScript compiler options?](#27-what-are-important-typescript-compiler-options)
- [28. What are performance best practices for large TypeScript codebases?](#28-what-are-performance-best-practices-for-large-typescript-codebases)

### Section 14: Integration & Ecosystem
- [29. How do you integrate TypeScript with testing frameworks?](#29-how-do-you-integrate-typescript-with-testing-frameworks)
- [30. How do you work with external data sources (APIs, GraphQL)?](#30-how-do-you-work-with-external-data-sources-apis-graphql)

### Section 15: Migration & Legacy Code
- [31. How do you migrate a JavaScript codebase to TypeScript?](#31-how-do-you-migrate-a-javascript-codebase-to-typescript)
- [32. How do you handle common migration challenges?](#32-how-do-you-handle-common-migration-challenges)

### Section 16: TypeScript with Modern Frameworks
- [33. How do you use TypeScript with Next.js?](#33-how-do-you-use-typescript-with-nextjs)
- [34. How do you use TypeScript with Redux/Zustand?](#34-how-do-you-use-typescript-with-reduxzustand)
---

## Section 1: Primitive Types

**1. What are the basic primitive data types in TypeScript?**

**Answer:**
TypeScript includes all JavaScript primitives with type safety:

| Type | Description | Example |
|------|-------------|---------|
| **`string`** | Textual data | `"hello"`, `'world'`, `` `Hi ${name}` `` |
| **`number`** | All numeric values (integers, floats, hex, binary, octal) | `42`, `3.14`, `0xFF`, `0b1010` |
| **`boolean`** | Logical values | `true`, `false` |
| **`bigint`** | Arbitrary-precision integers | `123n`, `9007199254740991n` |
| **`symbol`** | Unique, immutable identifiers | `Symbol('key')`, `Symbol.iterator` |
| **`null`** | Intentional absence of value | `null` |
| **`undefined`** | Uninitialized variable | `undefined` |

---

**2. What are the special utility-like primitive-ish types?**

**Answer:**
These behave like primitives but have special semantics in the type system:

| Type | Definition | Use Case | Example |
|------|------------|----------|---------|
| **`any`** | Disables all type checking | Migration, legacy code (avoid in production) | `let value: any = 42; value = 'string';` |
| **`unknown`** | Type-safe counterpart to `any` | API responses, user input | `let data: unknown = JSON.parse(json);` |
| **`never`** | Type that never occurs | Functions that throw, exhaustive checks | `function throwError(): never { throw new Error(); }` |
| **`void`** | Function returns nothing | Event handlers, side effects | `function log(message: string): void { console.log(message); }` |

---

## Section 2: Composite Types

**3. What are the composite/structural types?**

**Answer:**
These types describe complex data structures and shapes:

| Type | Description | Syntax | Example |
|------|-------------|--------|---------|
| **`object`** | Any non-primitive value | `object` | `{ name: 'John' }`, `[]`, `new Date()` |
| **`array`** | Ordered lists | `T[]` or `Array<T>` | `string[]`, `Array<number>` |
| **`tuple`** | Fixed-length with specific types | `[type1, type2]` | `[string, number]` for CSV data |
| **`enum`** | Named constants | `enum Name { ... }` | `enum Color { Red, Green, Blue }` |
| **`function`** | Callable signatures | `(param: type) => returnType` | `(name: string) => string` |

---

## Section 3: Literal Types

**4. What are literal types?**

**Answer:**
Exact values that act as types—powerful for narrowing and exhaustive checking:

| Type | Description | Example |
|------|-------------|---------|
| **String literal** | Specific string values | `"success" | "error" | "loading"` |
| **Number literal** | Specific numbers | `1 | 2 | 3 | 404 | 500` |
| **Boolean literal** | `true` or `false` | `true | false` |
| **Bigint literal** | Specific big integers | `100n | 200n | 500n` |
| **Template literal** | Pattern-based strings (TS 4.1+) | `` `user_${string}` `` |

---

## Section 4: Type Composition

**5. What are union and intersection types?**

**Answer:**
Ways to combine multiple types into one:

| Construct | Symbol | Purpose | Example |
|-----------|--------|---------|---------|
| **Union** | `|` | Value can be any of the types | `string | number | boolean` |
| **Intersection** | `&` | Value must satisfy all types | `User & Admin` combines properties |
| **Discriminated Union** | `|` with discriminant | Type-safe state machines | `{ type: 'success'; data: T } | { type: 'error' }` |

---

## Section 5: Type Definition Mechanisms

**6. What's the difference between `type` and `interface`?**

**Answer:**
Both define types but have different capabilities:

| Feature | `interface` | `type` |
|---------|-------------|--------|
| **Declaration merging** | ✅ Yes | ❌ No |
| **Extends classes** | ✅ Yes | ⚠️ Limited |
| **Implement by class** | ✅ Yes | ✅ Yes (object types) |
| **Union types** | ❌ No | ✅ Yes |
| **Tuple types** | ❌ No | ✅ Yes |
| **Mapped types** | ❌ No | ✅ Yes |
| **Computed properties** | ❌ No | ✅ Yes |
| **Performance** | Better for object shapes | Better for complex types |

---

**7. What are type aliases?**

**Answer:**
Names for any type—primitives, unions, tuples, functions, or objects:

```typescript
// Primitive alias
type ID = string | number;

// Union alias
type Status = 'draft' | 'published' | 'archived';

// Function type alias
type Callback<T> = (data: T) => void;

// Object type alias
type User = {
  id: ID;
  name: string;
  status: Status;
};

// Generic alias
type Result<T> = {
  data: T;
  error?: string;
  loading: boolean;
};
```

---

**8. What are index signatures?**

**Answer:**
Define types for dynamic object properties:

```typescript
// Dictionary pattern
interface StringMap {
  [key: string]: string;
}

// Mixed types with index signature
interface UserData {
  name: string;
  age: number;
  [key: string]: string | number; // Additional dynamic props
}

// Readonly index
interface ReadonlyMap {
  readonly [key: string]: number;
}

// Template literal index (TS 4.4+)
interface EventMap {
  [key: `on${string}`]: (event: Event) => void;
}
```

---

## Section 6: Advanced Type Operators

**9. What is `keyof` and how is it used?**

**Answer:**
`keyof` produces a union of all property keys of a type:

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

// "id" | "name" | "email"
type UserKeys = keyof User;

// Type-safe property access
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

// With mapped types
type Optional<T> = {
  [K in keyof T]?: T[K];
};
```

---

**10. What is indexed access type (`T[K]`)?**

**Answer:**
Retrieves the type of a specific property:

```typescript
interface User {
  id: number;
  profile: {
    name: string;
    age: number;
  };
  tags: string[];
}

// number
type IdType = User['id'];

// { name: string; age: number; }
type ProfileType = User['profile'];

// string[]
type TagsType = User['tags'];

// string (nested access)
type NameType = User['profile']['name'];

// Union of multiple properties
type IdOrName = User['id' | 'profile'];
```

---

**11. What is `typeof` in type context?**

**Answer:**
Extracts the type from a runtime value:

```typescript
const user = {
  name: 'John',
  age: 30
};

// { name: string; age: number; }
type UserType = typeof user;

// With functions
function createUser(name: string) {
  return { id: Date.now(), name };
}

// (name: string) => { id: number; name: string; }
type CreateUserType = typeof createUser;

// With class instances
class DataService {
  fetch() {}
}

// DataService
type ServiceType = typeof DataService;
```

---

## Section 7: Utility Types (Complete Reference)

**12. What are all the built-in utility types?**

**Answer:**
TypeScript provides these global utilities for type transformation:

| Utility | Purpose | Example | Use Case |
|---------|---------|---------|----------|
| **`Partial<T>`** | All properties optional | `Partial<User>` | Form updates, PATCH requests |
| **`Required<T>`** | All properties required | `Required<Config>` | Validated configs |
| **`Readonly<T>`** | No property reassignment | `Readonly<State>` | Immutable state |
| **`Record<K,T>`** | Key-value object | `Record<string, number>` | Dictionaries, maps |
| **`Pick<T,K>`** | Select specific keys | `Pick<User, 'id' | 'name'>` | Preview components |
| **`Omit<T,K>`** | Exclude specific keys | `Omit<User, 'password'>` | Remove sensitive data |
| **`Exclude<T,U>`** | Remove from union | `Exclude<'a'|'b', 'a'>` → `'b'` | Filter union types |
| **`Extract<T,U>`** | Keep from union | `Extract<'a'|'b', 'a'>` → `'a'` | Extract specific types |
| **`NonNullable<T>`** | Remove null/undefined | `NonNullable<string | null>` → `string` | Safe optional values |
| **`Parameters<T>`** | Function parameter types | `Parameters<(a: string) => void>` → `[string]` | Callback typing |
| **`ConstructorParameters<T>`** | Constructor params | `ConstructorParameters<typeof Date>` | Class factories |
| **`ReturnType<T>`** | Function return type | `ReturnType<() => string>` → `string` | Hook return values |
| **`InstanceType<T>`** | Class instance type | `InstanceType<typeof Date>` → `Date` | Instance typing |
| **`ThisParameterType<T>`** | `this` parameter type | `ThisParameterType<(this: User) => void>` → `User` | Method contexts |
| **`OmitThisParameter<T>`** | Remove `this` type | `OmitThisParameter<(this: User) => void>` → `() => void` | Callback binding |
| **`ThisType<T>`** | Contextual `this` type | `ThisType<User>` | Object method contexts |
| **`Awaited<T>`** (TS 4.5+) | Unwrap Promise | `Awaited<Promise<string>>` → `string` | Async utilities |
| **`Capitalize<T>`** | Capitalize string | `Capitalize<'hello'>` → `'Hello'` | String formatting |
| **`Uncapitalize<T>`** | Uncapitalize string | `Uncapitalize<'Hello'>` → `'hello'` | String formatting |
| **`Uppercase<T>`** | Uppercase string | `Uppercase<'hello'>` → `'HELLO'` | Case conversion |
| **`Lowercase<T>`** | Lowercase string | `Lowercase<'HELLO'>` → `'hello'` | Case conversion |

---

**13. Advanced utility combinations:**

```typescript
// Deep partial
type DeepPartial<T> = {
  [K in keyof T]?: T[K] extends object ? DeepPartial<T[K]> : T[K];
};

// Mutable (opposite of Readonly)
type Mutable<T> = {
  -readonly [K in keyof T]: T[K];
};

// Nullable
type Nullable<T> = T | null;

// Maybe (including undefined)
type Maybe<T> = T | null | undefined;

// Async return type
type AsyncReturnType<T> = T extends (...args: any[]) => Promise<infer U> ? U : never;

// Union to intersection
type UnionToIntersection<U> = 
  (U extends any ? (k: U) => void : never) extends ((k: infer I) => void) ? I : never;

// Keys with specific value type
type KeysOfType<T, U> = { [K in keyof T]: T[K] extends U ? K : never }[keyof T];
```

---

## Section 8: Conditional Types & Mapped Types

**14. What are conditional types?**

**Answer:**
Types that select between two possibilities based on a condition:

```typescript
// Basic conditional
type IsArray<T> = T extends any[] ? true : false;
// IsArray<string[]> → true
// IsArray<number> → false

// Nested conditionals
type TypeName<T> = 
  T extends string ? 'string' :
  T extends number ? 'number' :
  T extends boolean ? 'boolean' :
  T extends undefined ? 'undefined' :
  T extends Function ? 'function' :
  'object';

// Distributive conditional types
type ToArray<T> = T extends any ? T[] : never;
// ToArray<string | number> → string[] | number[]

// Filtering unions
type FilterString<T> = T extends string ? T : never;
// FilterString<string | number | boolean> → string
```

---

**15. What is the `infer` keyword?**

**Answer:**
`infer` declares a type variable within a conditional type to extract and use a subtype:

```typescript
// Extract element type from array
type ElementType<T> = T extends (infer U)[] ? U : never;
// ElementType<string[]> → string

// Extract return type
type ReturnType<T> = T extends (...args: any[]) => infer R ? R : never;

// Extract promise resolved type
type PromiseType<T> = T extends Promise<infer U> ? U : never;

// Extract first parameter
type FirstArg<T> = T extends (first: infer F, ...args: any[]) => any ? F : never;

// Extract nested type
type DeepPromise<T> = T extends Promise<infer U> ? DeepPromise<U> : T;

// Function composition helper
type Compose<F, G> = F extends (a: infer A) => infer B
  ? G extends (b: B) => infer C
    ? (a: A) => C
    : never
  : never;
```

---

**16. What are mapped types?**

**Answer:**
Mapped types transform each property of an existing type:

```typescript
// Basic mapped type
type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};

// Optional properties
type Partial<T> = {
  [P in keyof T]?: T[P];
};

// Nullable properties
type Nullable<T> = {
  [P in keyof T]: T[P] | null;
};

// With property modifiers
type Mutable<T> = {
  -readonly [P in keyof T]: T[P];
};

type Required<T> = {
  [P in keyof T]-?: T[P];
};

// With renaming (as clause, TS 4.1+)
type Getters<T> = {
  [K in keyof T as `get${Capitalize<string & K>}`]: () => T[K];
};

// Filtering keys
type Methods<T> = {
  [K in keyof T as T[K] extends Function ? K : never]: T[K];
};
```

---

## Section 9: Type Guards & Narrowing

**17. What are type guards?**

**Answer:**
Runtime checks that inform TypeScript's type system:

```typescript
// typeof guard
function process(value: string | number) {
  if (typeof value === 'string') {
    return value.toUpperCase(); // TypeScript knows: string
  }
  return value.toFixed(2); // TypeScript knows: number
}

// instanceof guard
class APIError extends Error {}
class ValidationError extends Error {}

function handleError(error: Error) {
  if (error instanceof APIError) {
    // Handle API error
  } else if (error instanceof ValidationError) {
    // Handle validation error
  }
}

// 'in' guard
interface User { name: string; age: number }
interface Product { title: string; price: number }

function log(item: User | Product) {
  if ('name' in item) {
    console.log(item.name); // TypeScript knows: User
  } else {
    console.log(item.price); // TypeScript knows: Product
  }
}

// Truthiness narrowing
function getFirst(list?: string[]) {
  return list?.[0]; // Returns string | undefined
}
```

---

**18. What are custom type predicates?**

**Answer:**
User-defined type guard functions with `param is Type` syntax:

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  role: 'admin' | 'user';
}

// Basic predicate
function isUser(obj: any): obj is User {
  return obj 
    && typeof obj.id === 'number'
    && typeof obj.name === 'string'
    && typeof obj.email === 'string'
    && (obj.role === 'admin' || obj.role === 'user');
}

// Generic predicate
function isArrayOf<T>(arr: any, check: (item: any) => item is T): arr is T[] {
  return Array.isArray(arr) && arr.every(check);
}

// Async predicate (type predicate + promise)
async function isValidUser(id: string): Promise<boolean> {
  const user = await fetchUser(id);
  return isUser(user);
}

// Assertion functions
function assertIsUser(obj: any): asserts obj is User {
  if (!isUser(obj)) {
    throw new Error('Not a user');
  }
}
```

---

## Section 10: Advanced Patterns

**19. What are template literal types? (TS 4.1+)**

**Answer:**
Types that manipulate string literals using template literal syntax:

```typescript
// Basic template literal
type EventName = `on${string}`;
// Matches: "onClick", "onChange", "onSubmit"

// Union combinations
type Size = 'small' | 'medium' | 'large';
type Color = 'red' | 'green' | 'blue';
type SizeColor = `${Size}-${Color}`;
// "small-red" | "small-green" | "small-blue" | ...

// Intrinsic string manipulation
type Greeting = 'hello' | 'world';
type Capitalized = Capitalize<Greeting>; // 'Hello' | 'World'

// Parsing with infer
type ExtractRouteParams<T extends string> = 
  T extends `${string}:${infer Param}/${infer Rest}`
    ? { [K in Param | keyof ExtractRouteParams<`/${Rest}`>]: string }
    : T extends `${string}:${infer Param}`
      ? { [K in Param]: string }
      : {};

// Example: "/users/:id/posts/:postId"
// → { id: string; postId: string; }
```

---

**20. What are declaration merging and module augmentation?**

**Answer:**
Extending existing types and interfaces:

```typescript
// Interface merging
interface User {
  name: string;
}

interface User {
  age: number;
}

// User now has both name and age

// Namespace merging
namespace Validation {
  export interface StringValidator {
    isValid(s: string): boolean;
  }
}

namespace Validation {
  export class EmailValidator implements StringValidator {
    isValid(s: string) { return s.includes('@'); }
  }
}

// Module augmentation
declare module 'express' {
  interface Request {
    user?: {
      id: string;
      role: string;
    };
  }
}

// Global augmentation
declare global {
  interface Window {
    __ENV__: {
      API_URL: string;
    };
  }
}

// Adding static methods to classes
interface DateConstructor {
  nowInUTC(): number;
}

Date.nowInUTC = () => {
  const now = new Date();
  return Date.UTC(
    now.getUTCFullYear(),
    now.getUTCMonth(),
    now.getUTCDate()
  );
};
```

---

**21. What are assertion functions?**

**Answer:**
Functions that narrow types by throwing when conditions aren't met:

```typescript
// Basic assertion
function assertString(value: unknown): asserts value is string {
  if (typeof value !== 'string') {
    throw new Error('Expected string');
  }
}

// Usage
function process(value: unknown) {
  assertString(value);
  value.toUpperCase(); // TypeScript knows: string
}

// Multiple assertions
function assertUser(obj: any): asserts obj is User {
  assertString(obj.name);
  if (typeof obj.age !== 'number') {
    throw new Error('User must have age');
  }
}

// Assertion with condition
function assert(condition: any, message?: string): asserts condition {
  if (!condition) {
    throw new Error(message || 'Assertion failed');
  }
}

// Usage with custom message
function divide(a: number, b: number) {
  assert(b !== 0, 'Cannot divide by zero');
  return a / b;
}
```

---

## Section 11: React-Specific TypeScript

**22. How do you type React components?**

**Answer:**
Multiple approaches with different tradeoffs:

```typescript
// Function component with explicit return
function Button({ label, onClick }: { label: string; onClick: () => void }) {
  return <button onClick={onClick}>{label}</button>;
}

// Using React.FC (discouraged in modern React)
const Button: React.FC<ButtonProps> = ({ label, onClick }) => {
  return <button onClick={onClick}>{label}</button>;
};

// Best practice: explicit children
interface CardProps {
  title: string;
  children: React.ReactNode;
  className?: string;
}

function Card({ title, children, className }: CardProps) {
  return (
    <div className={className}>
      <h2>{title}</h2>
      {children}
    </div>
  );
}

// Generic component
function List<T>({ 
  items, 
  renderItem 
}: { 
  items: T[]; 
  renderItem: (item: T) => React.ReactNode 
}) {
  return <div>{items.map(renderItem)}</div>;
}

// Polymorphic component (as prop)
type PolymorphicProps<T extends React.ElementType> = {
  as?: T;
  children: React.ReactNode;
} & React.ComponentPropsWithoutRef<T>;

function Box<T extends React.ElementType = 'div'>({
  as,
  children,
  ...props
}: PolymorphicProps<T>) {
  const Component = as || 'div';
  return <Component {...props}>{children}</Component>;
}
```

---

**23. How do you type React hooks?**

**Answer:**
Complete typing for custom hooks:

```typescript
// useState with complex type
function useUser() {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(false);
  
  const fetchUser = async (id: string) => {
    setLoading(true);
    try {
      const response = await api.getUser(id);
      setUser(response);
    } finally {
      setLoading(false);
    }
  };
  
  return { user, loading, fetchUser };
}

// Generic hook
function useLocalStorage<T>(key: string, initialValue: T) {
  const [stored, setStored] = useState<T>(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch {
      return initialValue;
    }
  });
  
  const setValue = (value: T | ((val: T) => T)) => {
    try {
      const valueToStore = value instanceof Function ? value(stored) : value;
      setStored(valueToStore);
      window.localStorage.setItem(key, JSON.stringify(valueToStore));
    } catch (error) {
      console.log(error);
    }
  };
  
  return [stored, setValue] as const;
}

// useReducer with discriminated unions
type State = {
  count: number;
  lastAction: string;
};

type Action = 
  | { type: 'increment'; payload: number }
  | { type: 'decrement'; payload: number }
  | { type: 'reset' };

function reducer(state: State, action: Action): State {
  switch (action.type) {
    case 'increment':
      return { count: state.count + action.payload, lastAction: 'increment' };
    case 'decrement':
      return { count: state.count - action.payload, lastAction: 'decrement' };
    case 'reset':
      return { count: 0, lastAction: 'reset' };
    default:
      return state;
  }
}

// useContext with safety
function createSafeContext<T>() {
  const Context = React.createContext<T | undefined>(undefined);
  
  function useContext() {
    const context = React.useContext(Context);
    if (context === undefined) {
      throw new Error('useContext must be used within Provider');
    }
    return context;
  }
  
  return [Context.Provider, useContext] as const;
}
```

---

**24. How do you handle events in React with TypeScript?**

**Answer:**
Proper typing for all React events:

```typescript
// Form events
function Form() {
  const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    // Handle submission
  };
  
  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    console.log(e.target.value);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input type="text" onChange={handleChange} />
    </form>
  );
}

// Mouse events
function Button() {
  const handleClick = (e: React.MouseEvent<HTMLButtonElement>) => {
    console.log(e.clientX, e.clientY);
  };
  
  const handleDoubleClick = (e: React.MouseEvent) => {
    console.log('double clicked');
  };
  
  return <button onClick={handleClick} onDoubleClick={handleDoubleClick}>Click</button>;
}

// Keyboard events
function Input() {
  const handleKeyDown = (e: React.KeyboardEvent<HTMLInputElement>) => {
    if (e.key === 'Enter') {
      console.log('Enter pressed');
    }
  };
  
  return <input onKeyDown={handleKeyDown} />;
}

// Generic event handler type
type EventHandler<T = Element> = (event: React.SyntheticEvent<T>) => void;

// With event mapping
interface EventMap {
  click: React.MouseEvent;
  change: React.ChangeEvent;
  submit: React.FormEvent;
}

function createHandler<T extends keyof EventMap>(
  type: T,
  handler: (event: EventMap[T]) => void
) {
  return handler;
}
```

---

## Section 12: Advanced React Patterns

**25. How do you type higher-order components (HOCs)?**

**Answer:**
Type-safe HOC patterns:

```typescript
// Simple HOC
function withLogger<T extends object>(Component: React.ComponentType<T>) {
  return function WithLogger(props: T) {
    useEffect(() => {
      console.log('Component rendered with props:', props);
    }, [props]);
    
    return <Component {...props} />;
  };
}

// HOC that adds props
interface WithIdProps {
  id: string;
}

function withId<T extends object>(Component: React.ComponentType<T & WithIdProps>) {
  return function WithId(props: T) {
    const id = useId();
    return <Component {...props as T} id={id} />;
  };
}

// HOC with prop injection (advanced)
type Subtract<T, K> = Omit<T, keyof K>;

function withUser<T extends { user: User }>(
  Component: React.ComponentType<T>
) {
  return function WithUser(props: Subtract<T, { user: User }>) {
    const user = useUser();
    return <Component {...props as T} user={user} />;
  };
}

// Generic HOC
function withLoading<T extends object>(
  Component: React.ComponentType<T>
) {
  return function WithLoading({ loading, ...props }: T & { loading: boolean }) {
    if (loading) {
      return <div>Loading...</div>;
    }
    return <Component {...props as T} />;
  };
}
```

---

**26. What are render props and how do you type them?**

**Answer:**
Typing render prop patterns:

```typescript
// Basic render prop
interface DataProviderProps<T> {
  render: (data: T) => React.ReactNode;
  url: string;
}

function DataProvider<T>({ render, url }: DataProviderProps<T>) {
  const [data, setData] = useState<T | null>(null);
  
  useEffect(() => {
    fetch(url)
      .then(res => res.json())
      .then(setData);
  }, [url]);
  
  return data ? render(data) : null;
}

// Multiple render props
interface MouseTrackerProps {
  render: (position: { x: number; y: number }) => React.ReactNode;
  children?: never;
}

function MouseTracker({ render }: MouseTrackerProps) {
  const [position, setPosition] = useState({ x: 0, y: 0 });
  
  return (
    <div onMouseMove={e => setPosition({ x: e.clientX, y: e.clientY })}>
      {render(position)}
    </div>
  );
}

// Function as child pattern
interface ListProps<T> {
  children: (item: T, index: number) => React.ReactNode;
  items: T[];
}

function List<T>({ children, items }: ListProps<T>) {
  return (
    <ul>
      {items.map((item, index) => (
        <li key={index}>{children(item, index)}</li>
      ))}
    </ul>
  );
}
```

---

## Section 13: Configuration & Best Practices

**27. What are important TypeScript compiler options?**

**Answer:**
Key `tsconfig.json` options for production apps:

```json
{
  "compilerOptions": {
    // Strictness
    "strict": true,                    // Enable all strict checks
    "noImplicitAny": true,              // Error on implicit any
    "strictNullChecks": true,           // Handle null/undefined properly
    "strictFunctionTypes": true,        // Check function parameter bivariance
    "strictBindCallApply": true,        // Check bind/call/apply
    "strictPropertyInitialization": true, // Check class property init
    "noImplicitThis": true,              // Error on this with implicit any
    "alwaysStrict": true,                // Use strict mode
    
    // Module resolution
    "module": "esnext",                  // Modern module system
    "moduleResolution": "node",           // Node-style resolution
    "target": "es2020",                   // Target JS version
    "lib": ["dom", "dom.iterable", "esnext"], // Include type definitions
    
    // Output
    "outDir": "./dist",                   // Output directory
    "rootDir": "./src",                    // Source directory
    "declaration": true,                   // Generate .d.ts files
    "declarationMap": true,                 // Maps for .d.ts files
    "sourceMap": true,                      // Generate source maps
    
    // Interop
    "esModuleInterop": true,               // Better CommonJS interop
    "allowSyntheticDefaultImports": true,   // Allow default imports
    "forceConsistentCasingInFileNames": true, // Case-sensitive imports
    
    // Advanced
    "skipLibCheck": true,                   // Skip node_modules checks
    "resolveJsonModule": true,               // Import JSON files
    "isolatedModules": true,                  // Required for Babel
    "noUnusedLocals": true,                    // Error on unused locals
    "noUnusedParameters": true,                 // Error on unused params
    "noFallthroughCasesInSwitch": true,         // Error on fallthrough
    "noUncheckedIndexedAccess": true            // Add undefined to index access
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

---

**28. What are performance best practices for large TypeScript codebases?**

**Answer:**
Strategies for scaling TypeScript:

```typescript
// 1. Project references for incremental builds
// tsconfig.json (root)
{
  "references": [
    { "path": "./packages/core" },
    { "path": "./packages/utils" },
    { "path": "./packages/ui" }
  ]
}

// 2. Const assertions for literals
const routes = {
  home: '/',
  users: '/users',
  settings: '/settings'
} as const;

// Type: '/home' | '/users' | '/settings'
type Route = typeof routes[keyof typeof routes];

// 3. Use interface for public APIs (better performance)
// Prefer interface over type for object shapes
interface User {
  id: string;
  name: string;
}

// 4. Avoid complex recursive types in hot paths
// ❌ Expensive
type DeepReadonly<T> = {
  readonly [K in keyof T]: DeepReadonly<T[K]>;
};

// ✅ Better for hot paths (flatten if possible)
interface FlatState {
  users: readonly User[];
  posts: readonly Post[];
}

// 5. Use type-only imports
import type { User } from './types';

// 6. Enable skipLibCheck
{
  "compilerOptions": {
    "skipLibCheck": true
  }
}

// 7. Use incremental compilation
{
  "compilerOptions": {
    "incremental": true,
    "tsBuildInfoFile": ".tsbuildinfo"
  }
}
```

---

## Section 14: Integration & Ecosystem

**29. How do you integrate TypeScript with testing frameworks?**

**Answer:**
Type-safe testing patterns:

```typescript
// Jest with TypeScript
import { describe, it, expect, jest } from '@jest/globals';

// Typed mocks
jest.mock('./api');
import { fetchUser } from './api';

const mockFetchUser = fetchUser as jest.MockedFunction<typeof fetchUser>;

describe('UserService', () => {
  it('should fetch user', async () => {
    mockFetchUser.mockResolvedValue({ id: 1, name: 'John' });
    
    const result = await getUser(1);
    expect(result.name).toBe('John');
  });
});

// React Testing Library
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';

test('Button click', async () => {
  const handleClick = jest.fn();
  render(<Button onClick={handleClick}>Click</Button>);
  
  await userEvent.click(screen.getByText('Click'));
  expect(handleClick).toHaveBeenCalledTimes(1);
});

// Custom assertions
expect.extend({
  toBeWithinRange(received: number, floor: number, ceiling: number) {
    const pass = received >= floor && received <= ceiling;
    return {
      message: () => `expected ${received} to be within range ${floor} - ${ceiling}`,
      pass
    };
  }
});

declare global {
  namespace jest {
    interface Matchers<R> {
      toBeWithinRange(floor: number, ceiling: number): R;
    }
  }
}
```

---

**30. How do you work with external data sources (APIs, GraphQL)?**

**Answer:**
Type-safe data handling:

```typescript
// Fetch API with generics
async function fetchAPI<T>(url: string): Promise<T> {
  const response = await fetch(url);
  if (!response.ok) {
    throw new Error('Network error');
  }
  return response.json();
}

interface User {
  id: number;
  name: string;
}

// Usage
const user = await fetchAPI<User>('/api/users/1');

// Axios with interceptors
import axios, { AxiosInstance } from 'axios';

interface ApiConfig {
  baseURL: string;
  token?: string;
}

class ApiClient {
  private client: AxiosInstance;
  
  constructor(config: ApiConfig) {
    this.client = axios.create({
      baseURL: config.baseURL,
      headers: config.token ? { Authorization: `Bearer ${config.token}` } : {}
    });
  }
  
  async get<T>(path: string): Promise<T> {
    const response = await this.client.get<T>(path);
    return response.data;
  }
  
  async post<T, D = unknown>(path: string, data: D): Promise<T> {
    const response = await this.client.post<T>(path, data);
    return response.data;
  }
}

// GraphQL with codegen
import { gql } from '@apollo/client';

// This would be auto-generated from schema
interface GetUserQuery {
  user: {
    id: string;
    name: string;
    posts: Array<{
      id: string;
      title: string;
    }>;
  };
}

const GET_USER = gql`
  query GetUser($id: ID!) {
    user(id: $id) {
      id
      name
      posts {
        id
        title
      }
    }
  }
`;

// Usage with typed hooks
function UserProfile({ userId }: { userId: string }) {
  const { data, loading } = useQuery<GetUserQuery>(GET_USER, {
    variables: { id: userId }
  });
  
  if (loading) return <div>Loading...</div>;
  return <div>{data?.user.name}</div>;
}
```

---

## Section 15: Migration & Legacy Code

**31. How do you migrate a JavaScript codebase to TypeScript?**

**Answer:**
Phased migration strategy:

**Phase 1: Initial Setup**
```json
// tsconfig.json (lenient)
{
  "compilerOptions": {
    "allowJs": true,
    "checkJs": false,
    "noImplicitAny": false,
    "strictNullChecks": false,
    "strict": false,
    "outDir": "./dist",
    "rootDir": "./src"
  },
  "include": ["src/**/*"]
}
```

**Phase 2: Add JSDoc comments incrementally**
```javascript
// @ts-check

/**
 * @typedef {Object} User
 * @property {number} id
 * @property {string} name
 * @property {string} email
 */

/**
 * @param {User} user
 * @returns {string}
 */
function formatUser(user) {
  return `${user.name} (${user.email})`;
}
```

**Phase 3: Rename files to .ts gradually**
```bash
# Rename in batches
find src -name "*.js" | grep -v "node_modules" | xargs -I {} git mv {} {}.ts
```

**Phase 4: Tighten compiler options**
```json
{
  "compilerOptions": {
    "allowJs": false,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "strict": true
  }
}
```

**Migration tools:**
```bash
# TypeScript migration tool
npx ts-migrate src/

# Airbnb's TS migration tool
npx @airbnb/ts-migrate init

# Convert JSDoc to TypeScript
npx ts-migrate-jsdoc src/
```

---

**32. How do you handle common migration challenges?**

**Answer:**
Solutions for typical migration problems:

```typescript
// Challenge 1: Missing types from dependencies
// Solution: Install @types or declare modules
declare module 'untyped-library' {
  export function doSomething(): void;
}

// Challenge 2: Dynamic requires
// Solution: Use import() type
const moduleName = getModuleName();
const module = await import(moduleName) as typeof import('./types');

// Challenge 3: Global variables
// Solution: Declare global
declare global {
  interface Window {
    MY_GLOBAL: string;
  }
}

// Challenge 4: Complex dynamic objects
// Solution: Use index signatures
interface DynamicConfig {
  [key: string]: string | number | boolean;
}

// Challenge 5: Function overloads from JS patterns
// Solution: Use union types or overloads
function format(input: string): string;
function format(input: number): string;
function format(input: Date): string;
function format(input: string | number | Date): string {
  if (input instanceof Date) {
    return input.toISOString();
  }
  return String(input);
}

// Challenge 6: Mixin patterns
// Solution: Use intersection types
type Constructor<T = {}> = new (...args: any[]) => T;

function Timestamped<TBase extends Constructor>(Base: TBase) {
  return class extends Base {
    timestamp = Date.now();
  };
}

const UserWithTimestamp = Timestamped(User);
```

---

## Section 16: TypeScript with Modern Frameworks

**33. How do you use TypeScript with Next.js?**

**Answer:**
Complete Next.js TypeScript patterns:

```typescript
// pages/index.tsx
import { GetServerSideProps, GetStaticProps, NextPage } from 'next';

interface HomeProps {
  users: User[];
  timestamp: number;
}

const Home: NextPage<HomeProps> = ({ users, timestamp }) => {
  return (
    <div>
      <h1>Users ({timestamp})</h1>
      {users.map(user => (
        <div key={user.id}>{user.name}</div>
      ))}
    </div>
  );
};

// Static Generation
export const getStaticProps: GetStaticProps<HomeProps> = async () => {
  const users = await getUsers();
  return {
    props: {
      users,
      timestamp: Date.now()
    },
    revalidate: 60 // ISR
  };
};

// Server-side Rendering
export const getServerSideProps: GetServerSideProps<HomeProps> = async (context) => {
  const { req, query } = context;
  const users = await getUsers(query.page as string);
  
  return {
    props: {
      users,
      timestamp: Date.now()
    }
  };
};

// API Routes
// pages/api/users/[id].ts
import { NextApiHandler } from 'next';

const handler: NextApiHandler = async (req, res) => {
  const { id } = req.query;
  
  if (req.method === 'GET') {
    const user = await getUserById(id as string);
    res.status(200).json(user);
  } else if (req.method === 'PUT') {
    const updates = req.body;
    // Type-safe body
    const updated = await updateUser(id as string, updates);
    res.status(200).json(updated);
  }
};

export default handler;

// Next.js specific types
import type { 
  GetStaticPaths, 
  InferGetStaticPropsType,
  NextApiRequest,
  NextApiResponse 
} from 'next';

// Type-safe paths
export const getStaticPaths: GetStaticPaths = async () => {
  const users = await getUsers();
  const paths = users.map(user => ({ params: { id: user.id.toString() } }));
  
  return {
    paths,
    fallback: 'blocking'
  };
};

// Infer props type
type Props = InferGetStaticPropsType<typeof getStaticProps>;
```

---

**34. How do you use TypeScript with Redux/Zustand?**

**Answer:**
State management patterns:

```typescript
// Redux Toolkit
import { configureStore, createSlice, PayloadAction } from '@reduxjs/toolkit';
import { TypedUseSelectorHook, useDispatch, useSelector } from 'react-redux';

interface UserState {
  users: User[];
  loading: boolean;
  error: string | null;
}

const initialState: UserState = {
  users: [],
  loading: false,
  error: null
};

const userSlice = createSlice({
  name: 'users',
  initialState,
  reducers: {
    fetchStart: (state) => {
      state.loading = true;
      state.error = null;
    },
    fetchSuccess: (state, action: PayloadAction<User[]>) => {
      state.users = action.payload;
      state.loading = false;
    },
    fetchError: (state, action: PayloadAction<string>) => {
      state.loading = false;
      state.error = action.payload;
    }
  }
});

// Store setup
export const store = configureStore({
  reducer: {
    users: userSlice.reducer
  }
});

// Typed hooks
export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;

export const useAppDispatch = () => useDispatch<AppDispatch>();
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;

// Zustand (simpler alternative)
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

interface BearState {
  bears: number;
  increase: () => void;
  decrease: () => void;
  reset: () => void;
}

const useBearStore = create<BearState>()(
  persist(
    (set) => ({
      bears: 0,
      increase: () => set((state) => ({ bears: state.bears + 1 })),
      decrease: () => set((state) => ({ bears: state.bears - 1 })),
      reset: () => set({ bears: 0 })
    }),
    { name: 'bear-storage' }
  )
);

// Middleware with types
interface LoggerState {
  logs: string[];
  addLog: (message: string) => void;
}

const useLoggerStore = create<LoggerState>()((set, get) => ({
  logs: [],
  addLog: (message) => {
    console.log('Previous logs:', get().logs);
    set((state) => ({ logs: [...state.logs, message] }));
  }
}));
```

---
