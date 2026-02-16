# Section 9: TypeScript

## 108. TypeScript Fundamentals
Expected Answer: Use TypeScript for static typing, better tooling, and early error detection. Hints:
•	Basic types: string, number, boolean, array, tuple, enum, any, unknown
•	type vs interface: interface for object shapes, type for unions
•	Generics for reusable type-safe components
•	Type inference reduces annotation needs

## 109. Advanced TypeScript Features
Expected Answer: Leverage advanced TypeScript features for robust type safety. Hints:
•	Utility types: Partial, Required, Pick, Omit, Record
•	Conditional types: T extends U ? X : Y
•	Mapped types for transforming object types
•	Type guards: typeof, instanceof, custom type predicates
•	keyof and indexed access types

## 110. TypeScript with React
Expected Answer: Type React components, hooks, props, and events properly. Hints:
•	Component props typing with interfaces
•	Event typing: React.ChangeEvent<HTMLInputElement>
•	Hook typing: useState<Type>, useRef<Type>
•	Context typing with createContext<Type | undefined>
•	Generic components for reusability


Here are additional high-value **TypeScript interview questions** tailored for a **Senior Frontend Developer** role (especially in React-heavy environments). These build on your existing Section 9 content by focusing on the most frequently asked advanced/practical topics in 2025–2026 interviews, drawn from real-world senior discussions, LinkedIn posts, GeeksforGeeks, Arc.dev, Reddit, and other recent sources.

I've continued the numbering from your provided items (starting at **111**) and formatted them similarly: question title, expected answer summary, and key hints.

### Section 9: TypeScript (continued)

**111. Differences Between type and interface (Deep Dive)**  
**Expected Answer:** Both define types, but interfaces are better for declaration merging and extending object shapes (especially in libraries), while types excel at unions, intersections, primitives, and more flexible compositions. Prefer interfaces for public APIs/objects that may be extended; use types for complex unions/aliases.  
**Hints:**  
• Interfaces can be reopened/merged; types cannot (error if redeclared).  
• Types support unions (`string | number`), mapped/conditional types more naturally.  
• In React: interfaces often for props (extendable); types for state unions.  
• Senior nuance: Use type for utility/mapped types; interface for class implements/contracts.

**112. any vs unknown (When and Why It Matters in Production)**  
**Expected Answer:** `any` disables type checking (unsafe, avoid in prod); `unknown` is safer — forces explicit checks/casts before operations. Use `unknown` for untrusted inputs (e.g., API data, user input).  
**Hints:**  
• `unknown` requires narrowing (type guards, typeof, etc.) before use.  
• Senior tip: In strict mode (`strict: true`), prefer `unknown` over `any`; use `as` assertions sparingly.  
• Common pitfall: Overusing `any` leads to runtime bugs; `unknown` catches them at compile time.

**113. Discriminated Unions and Narrowing**  
**Expected Answer:** Use discriminated unions for modeling state machines (e.g., API responses: loading/success/error) with a common discriminant property (e.g., `type` or `status`). TypeScript narrows the type automatically in conditionals.  
**Hints:**  
• Example: `type ApiState = { status: 'loading' } | { status: 'success'; data: T } | { status: 'error'; error: string }`  
• Narrow via `if (state.status === 'success') { state.data // typed }`  
• Combine with type predicates for custom narrowing.  
• Very common in React (useReducer, fetch states).

**114. Generics in React Components and Hooks**  
**Expected Answer:** Generics make components/hooks reusable and type-safe (e.g., `<T>`, `useFetch<T>`).  
**Hints:**  
• Generic component: `function List<T>({ items }: { items: T[] })`  
• Hook: `function useFetch<T>(url: string): { data: T | null, ... }`  
• Constraints: `<T extends { id: string }>`  
• Infer from props: `React.FC<Props<T>>`  
• Senior: Use for typed context, custom hooks, or polymorphic components.

**115. Utility Types in Depth (Partial, Required, Pick, Omit, Record, etc.)**  
**Expected Answer:** TypeScript's built-in utility types are global generics that transform types efficiently, reducing duplication and enforcing patterns. They are essential for clean, scalable code in forms, APIs, state, props, and immutability. Senior candidates must know all of them, when to combine them, and real-world trade-offs.  
**Hints (full list with practical React/frontend usage):**  
• `Partial<T>` — Makes **all** properties optional. Ideal for form updates/PATCH requests (`Partial<User>` for editable fields).  
• `Required<T>` — Makes **all** properties required. Useful for validated/submitted forms or mandatory configs.  
• `Readonly<T>` — Makes **all** properties readonly (prevents reassignment). Great for props, immutable state slices, or Redux/Zustand state.  
• `Record<K, T>` — Creates object type with keys from `K` (string | number | symbol | union) and values `T`. Perfect for dictionaries, enum-based maps, form fields (`Record<string, string>` or `Record<'name' | 'email', string>`).  
• `Pick<T, K>` — Selects subset of keys from `T`. Use for previews/minimal props (`Pick<User, 'id' | 'name'>`).  
• `Omit<T, K>` — Excludes specific keys. Common for removing sensitive fields (`Omit<User, 'password'>`) or API payloads.  
• `Exclude<T, U>` — Removes types from union that are assignable to `U`. E.g., `Exclude<'a' | 'b' | 'c', 'b'> = 'a' | 'c'`. Useful for filtering union literals.  
• `Extract<T, U>` — Keeps only types from `T` assignable to `U`. Opposite of Exclude — extract specific variants from unions.  
• `NonNullable<T>` — Removes `null` and `undefined` from type. Great for safe optional chaining or API data after checks.  
• `Parameters<T>` — Extracts tuple of parameter types from function type `T`. Useful for typing callbacks or higher-order functions.  
• `ConstructorParameters<T>` — Gets constructor parameter types (for class factories).  
• `ReturnType<T>` — Extracts return type of function `T`. Very common for typing hook returns or async utils.  
• `InstanceType<T>` — Gets instance type from constructor type `T`.  
• `ThisParameterType<T>` / `OmitThisParameter<T>` — Handles `this` in functions (advanced, callback typing).  
• `Awaited<T>` (TS 4.5+) — Unwraps Promise recursively (like `await`). Ideal for typing async functions/hooks that return Promise<Promise<T>> → T.  
• Senior combinations: `Partial<Pick<T, K>>`, `Readonly<Partial<T>>`, `Record<keyof T, string>` for errors, `Omit<T, keyof U> & U` for merges, Zod `z.infer` + utilities for form schemas.  
Use them to avoid manual type repetition — combine with mapped/conditional for custom utilities.

**116. Conditional Types and Infer**  
**Expected Answer:** Conditional types (`T extends U ? X : Y`) enable powerful type logic; `infer` extracts types from nested structures.  
**Hints:**  
• Example: `type ReturnType<T> = T extends (...args: any[]) => infer R ? R : never`  
• Use for: Extract return type, flatten promises (`Awaited<T>`), or API response typing.  
• Common in advanced React: typing hooks that return different shapes.

**117. Type Guards and Custom Predicates**  
**Expected Answer:** Narrow types at runtime with guards (`typeof`, `instanceof`, in, custom functions returning `arg is Type`).  
**Hints:**  
• Custom: `function isUser(obj: any): obj is User { return 'name' in obj && typeof obj.name === 'string'; }`  
• In React: Narrow event types or props in conditionals.  
• Senior: Combine with discriminated unions for exhaustive checks.

**118. keyof, Indexed Access, and Mapped Types**  
**Expected Answer:** `keyof T` gets union of keys; indexed access `T[K]` gets value type; mapped types `{ [K in keyof T]: ... }` transform objects.  
**Hints:**  
• Enforce form keys: `type FormErrors = { [K in keyof FormData]?: string }`  
• Make readonly: `type Readonly<T> = { readonly [P in keyof T]: T[P] }`  
• Senior use: Dynamic forms, API request builders, or immutable updates.

**7. Generics in React Components and Hooks**  
**Expected Answer:** Generics enable reusable, type-safe abstractions (components, hooks, utilities) that preserve type info across usage.  
**Hints:**  
• Component: `function Table<T>({ data }: { data: T[] })`.  
• Hook: `function useLocalStorage<T>(key: string, initial: T)`.  
• Constraints: `<T extends { id: number }>` or `<T extends string | number>`.  
• Default: `<T = string>`.  
• Senior: Polymorphic components (`as` prop), typed forwardRef, infer generics.

**119. Typing React Context (Advanced)**  
**Expected Answer:** Use generics with `createContext` to avoid `undefined` issues; provide default or use non-null assertion carefully.  
**Hints:**  
• `const MyContext = createContext<MyType | undefined>(undefined);`  
• Custom hook: `function useMyContext() { const ctx = useContext(MyContext); if (!ctx) throw Error(); return ctx; }`  
• Or use `null` + non-null checks.  
• Senior: Generic provider/component patterns.

**120. Enforcing Immutability and Readonly in Large Apps**  
**Expected Answer:** Use `Readonly<T>`, `ReadonlyArray`, or deep readonly utilities; prefer immutable patterns (spread, libraries like Immer) with types.  
**Hints:**  
• `type Immutable<T> = { readonly [K in keyof T]: Immutable<T[K]> }` (recursive).  
• In Redux/ Zustand: Type state slices as readonly.  
• Senior: Prevent accidental mutations in shared state or props.

**121. Handling Third-Party Libraries Without Types (DefinitelyTyped, Module Augmentation)**  
**Expected Answer:** Use `@types/package` if available; declare module for custom; augment existing types via declaration merging.  
**Hints:**  
• `declare module 'some-lib' { interface Options { extra: string; } }`  
• Use `allowJs` + JS files with JSDoc for gradual typing.  
• Senior: Patch broken `@types` via patch-package or custom overrides.

**122. Type-Safe Dynamic Forms / Zod Integration**  
**Expected Answer:** Use Zod schemas to infer types (`z.infer<typeof schema>`) for forms (React Hook Form, etc.); ensures validation matches types.  
**Hints:**  
• `const schema = z.object({ name: z.string(), age: z.number() }); type Form = z.infer<typeof schema>;`  
• Very common in senior React interviews for scalable, validated UIs.

**123. Monorepo Shared Types and Scaling TypeScript**  
**Expected Answer:** Use packages (e.g., `@myorg/types`) or tsconfig paths/references; avoid type duplication across apps.  
**Hints:**  
• `paths`: `"@types/*": ["packages/types/*"]`  
• Prevent bloat: Extract complex generics to shared utils; use `skipLibCheck` wisely.  
• Senior: Discuss tsup/vite for building shared types.
