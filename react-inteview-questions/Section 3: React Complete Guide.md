# Section 3: React Complete Guide
## Table of Contents
1. [React Fundamentals](#react-fundamentals)
2. [React Hooks & State Management](#react-hooks--state-management)
3. [React Performance & Optimization](#react-performance--optimization)
4. [React 18 Features](#react-18-features)
5. [Routing & Authentication](#routing--authentication)
6. [API Integration & Data Fetching](#api-integration--data-fetching)
7. [Security & Performance](#security--performance)
8. [Senior Frontend Developer Questions](#senior-frontend-developer-questions)
9. [Scenario Based Interview Questions](#scenario-based-interview-questions)


## Table of Contents

### [React Fundamentals](#react-fundamentals)
1. React vs Angular Comparison
2. What is React?
3. What is SPA (Single Page Application)?
4. Features of React
5. Problem with Real DOM
6. What is Virtual DOM?
7. How React/Virtual DOM Works
8. How React Rendering Works
9. Components & Props Patterns
10. State and Props
11. Why Direct Mutation is a Problem?
12. Stateful and Stateless Components
13. Controlled vs Uncontrolled Components
14. Class-Based vs Functional Components
15. Pure Components in React
16. What is React.memo?
17. Lazy Loading and Suspense
18. Styling in CSS
19. What are Keys?
20. React Strict Mode
21. Fragment in React and Uses

### [React Hooks & State Management](#react-hooks--state-management)
22. Rules of Hooks
23. List of Hooks
24. Component Lifecycle Phases
25. React Lifecycle Comparison: Class vs Functional
26. useState Hook
27. useReducer Hook
28. Context API
29. Ways to Manage State in React
30. When to use useReducer over useState
31. useCallback vs useMemo Hook
32. Custom Hook
33. Higher Order Components (HOC)
34. What is Render in React?
35. useLayoutEffect Hook vs useEffect Hook
36. useEffect Hook
37. useRef Hook, Real use
38. Difference between useState and useReducer
39. Redux vs useReducer + Context API
40. React Error Boundaries, Lifecycle
41. Prevent Performance Issues in Complex Trees
42. React Performance Optimizations
43. React Server Components vs Client Components

### [Redux Concepts](#redux-concepts)
44. What is Redux, why we need Redux?
45. What is basic flow of Redux?
46. Why we need Redux Toolkit?
47. How to connect component with store?
48. What is middleware in Redux?
49. Redux Thunk vs Redux Saga
50. Old Redux vs Redux Toolkit

### [React Performance & Optimization](#react-performance--optimization)
51. Memory Leaks in React
52. Performance Metrics in Production
53. App Feels Slow but Lighthouse Good
54. Optimize Images in Large App
55. Render 100k+ Items in React
56. How to Improve React Application Performance
57. High-Impact Optimizations
58. Medium-Impact / Supporting Techniques
59. Detailed Performance Optimization Techniques
60. React Portals

### [React 18 Features](#react-18-features)
61. Concurrent React
62. Hooks Mentioned in React 18
63. Key Features & Concepts
64. Automatic Batching
65. Transitions (Urgent vs Non-Urgent)
66. Suspense on the Server (Streaming)
67. Strict Mode Updates
68. Migration Steps (Syntax Change)

### [Routing & Authentication](#routing--authentication)
69. React Router
70. Additional Routing Concepts
71. Profilers
72. Protected Routes in React Router v6+
73. Role-Based Authorization in Routing
74. SSO Login (OAuth2/OIDC) in React
75. JWT (JSON Web Token) and Workflow?
76. JWT + Refresh Token Flow
77. User Logged Out Mid-Session
78. Route-Based Code Splitting

### [API Integration & Data Fetching](#api-integration--data-fetching)
79. Interceptors
80. CORS: Cross Origin Resource Sharing
81. REST vs GraphQL
82. Caching & Invalidation
83. Error Handling & Retry Strategies
84. Real-Time Updates
85. Scenario: Real-Time Dashboard

### [Security & Performance](#security--performance)
86. CSRF and XSS
87. Frontend Security Best Practices
88. Performance Optimization Techniques
89. Monitoring & Debugging
90. How to optimize React applications

### [Senior Frontend Developer Questions](#senior-frontend-developer-questions)
91. What is React Fiber and how does it differ from the old reconciliation algorithm?
92. How does React determine when to re-render a component?
93. What are concurrent features in React and how do they help?
94. Explain React's batching behavior and what changed in React 18
95. What is the difference between useMemo and useCallback, and when not to use them?
96. How does Suspense work, and what are some real use cases beyond lazy loading?
97. What is useImperativeHandle and when should you use it?
98. How do you optimize large lists in React?
99. How does useRef differ from useState?
100. How does React handle hydration in SSR, and what problems can arise?

### [Scenario Based Interview Questions](#scenario-based-interview-questions)
101. Your React app suddenly becomes slow after adding new features. How do you find and fix the issue?
102. How do you prevent a login page from being accessible after the user is logged in?
103. How do you handle API errors gracefully in the UI?
104. A component is fetching the same data multiple times unnecessarily. How do you fix it?
105. How do you optimize bundle size in a large React app?
106. How do you handle a form with 20+ fields efficiently?
107. Your React app needs to support dark mode. How do you implement it?
108. How do you prevent XSS (Cross-Site Scripting) in frontend apps?
109. How do you handle offline/online state gracefully in React with Service Workers?
110. Production is not getting the latest deployed update unless in incognito. How do you solve this?
---

## React Fundamentals

### 1. React vs Angular Comparison
Expected Answer: Compare React (library, flexible, JSX) with Angular (framework, opinionated, TypeScript). Hints:
•	React: Library for UI, component-based, virtual DOM, JSX syntax
•	Angular: Full MVC framework, two-way binding, dependency injection, TypeScript
•	React: More flexible, larger ecosystem, steeper learning curve for state management
•	Angular: Batteries included, structured, enterprise-friendly

### 2. What is React?
Expected Answer: React is a JavaScript library for building user interfaces using reusable components. Hints:
•	Declarative: Describe UI for each state, React updates when data changes
•	Component-based: Build encapsulated components managing their own state
•	Learn once, write anywhere: Works on web, mobile (React Native), VR
•	Maintained by Facebook and community

### 3. What is SPA (Single Page Application)?
Expected Answer: Web app that loads single HTML page and updates content dynamically without page reloads. Hints:
•	Initial load downloads HTML, CSS, JS
•	Subsequent navigation happens via JavaScript
•	Better UX: faster transitions, app-like feel
•	Challenges: SEO, initial load time, browser history

### 4. Features of React
Expected Answer: Key features include JSX, Virtual DOM, components, unidirectional data flow, React Native. Hints:
•	JSX: JavaScript + XML syntax, type-safe, compiled to React.createElement()
•	Virtual DOM: In-memory representation, efficient updates via diffing
•	Components: Reusable, composable, manage own state/props
•	Unidirectional data flow: Predictable state changes

### 5. Problem with Real DOM
Expected Answer: Real DOM updates are slow, cause reflows/repaints, inefficient for frequent updates. Hints:
•	Direct DOM manipulation is expensive
•	Each change triggers reflow (layout) and repaint
•	Inefficient for dynamic, complex UIs
•	Manual optimization needed for performance

### 6. What is Virtual DOM?
Expected Answer: Lightweight JavaScript representation of real DOM used for efficient UI updates. Hints:
•	In-memory tree of React elements
•	Diffing algorithm compares virtual trees
•	Batched updates to real DOM
•	Minimizes direct DOM manipulation

### 7. How React/Virtual DOM Works
Expected Answer: React creates virtual DOM, diffs changes, applies minimal updates to real DOM. Hints:
•	Diffing Algorithm: Compares element types, keys, attributes (new and old virtual DOM)
•	Reconciliation: Process of updating DOM to match React elements
•	Keys help identify elements across renders

### 8. How React Rendering Works
React rendering is the process of transforming your components into actual DOM nodes that users can see and interact with. 
Here's how it works: 
1. Component Render Trigger
React triggers a render when:
•	Initial mount: First time loading
•	State changes: useState, useReducer, this.setState
•	Props changes: Parent component re-renders with new props
•	Context changes: Value from Context.Provider changes
•	Hooks changes: useMemo, useCallback dependencies change
2. The Render Phase (Virtual DOM)
Render Phase Steps:
2.1. Call component functions: React calls your component functions
2.2.	Create Virtual DOM tree: Builds a tree of React elements (not real DOM)
2.3.	Diffing algorithm: Compares new Virtual DOM with previous one
2.4.	Reconciliation: Process of updating DOM to match React elements

### 9. Components & Props Patterns
Expected Answer: Design reusable components with proper prop patterns and composition.
•	Controlled vs uncontrolled components
•	React.memo() vs PureComponent
•	Keys for list item identification
•	Fragments for grouping without extra nodes
•	PropTypes or TypeScript for prop validation

### 10. State and Props
Expected Answer: State is internal data that changes, props are external data passed from parent. Hints:
•	State: Managed within component using useState/useReducer
•	Props: Read-only data passed from parent to child
•	Props changes trigger re-render, state changes trigger re-render
•	Lift state up: Share state between components via common ancestor

### 11. Why Direct Mutation is a Problem?
Expected Answer: Direct mutation doesn't trigger re-renders, breaks React's state management. Hints:
•	React uses reference equality for state comparisons
•	Direct mutation: state.items.push(newItem) doesn't create new reference
•	Correct: setItems([...items, newItem]) creates new array
•	Immutable updates ensure predictable rendering

### 12. Stateful and Stateless Components
Expected Answer: Stateful (smart) components manage state, stateless (dumb) components only render props. Hints:
•	Stateful: Class components or functional with hooks, manage data/logic
•	Stateless: Pure functions, receive props, return JSX
•	Container/Presenter pattern: Container manages state, Presenter renders
•	Modern: Hooks blur distinction, all components can have state

### 13. Controlled vs Uncontrolled Components
Expected Answer: Controlled: React manages state; Uncontrolled: DOM manages state via refs. Hints:
•	Controlled: Value prop + onChange handler, single source of truth
•	Uncontrolled: Use ref to access DOM value, form libraries often use
•	Controlled: More React patterns, validation, immediate updates
•	Uncontrolled: Simpler, less code, integrate with non-React code

### 14. Class-Based vs Functional Components
Expected Answer: Class components use ES6 classes, functional components use functions (preferred with hooks). Hints:
•	18.1 Why Functional Components Are Preferred: Simpler, less code, easier testing, hooks
•	Class: Lifecycle methods, this context, more boilerplate
•	Functional: Hooks, no this, easier composition
•	Modern React: Functional components with hooks recommended

### 15. Pure Components in React
Expected Answer: Components that only re-render when props/state change (shallow comparison). Hints:
•	Class: React.PureComponent extends Component with shallow comparison
•	Functional: React.memo() wrapper for functional components
•	Performance optimization, prevent unnecessary re-renders
•	Shallow comparison: compare primitives, object references

### 16. What is React.memo?
Expected Answer: Higher-order component that memoizes functional component based on props. Hints:
•	Wraps component, prevents re-render if props don't change
•	Accepts second arg: custom comparison function
•	Use when: Component renders often with same props, expensive rendering
•	Don't overuse: Memory overhead, premature optimization

### 17. Lazy Loading and Suspense
Expected Answer: Dynamically import components lazily when we need component, don't load every component upfront. to reduce bundle size. Hints:
•	React.lazy() for component-level code splitting
•	Suspense component shows fallback while loading
•	Route-based splitting: Each route loads separately
•	Named exports need default wrapper
Suspense:
Suspense is a powerful tool for managing async operations in React, making your applications feel more responsive and providing better user experience during loading states.

React's Suspense component is primarily used for code splitting and data fetching. Here are its props:
- Fallback (Required): The content to show while children are loading.
- children (Required): The actual content you want to render once it's ready.
- unstable_expectedLoadTime (Experimental, React18+): Tells React how long the loading is expected to take, which helps avoid showing the fallback for very quick loads.
```jsx
<Suspense 
  fallback={<Loading />}
  unstable_expectedLoadTime={5000} // 5 seconds
>
  <HeavyComponent /> // components containing async
</Suspense>
```

### 18. Styling in CSS
Expected Answer: Multiple approaches: CSS Modules, CSS-in-JS, utility classes, styled-components. Hints:
•	CSS Modules: Scoped styles, import styles from './Component.module.css'
•	CSS-in-JS: Styled-components, Emotion, dynamic styles with JavaScript
•	Utility-first: Tailwind CSS, atomic classes
•	Choose based on team preference, project needs

### 19. What are Keys?
Expected Answer: Special string attribute helping React identify which items changed/added/removed. Hints:
•	Required in lists for efficient re-rendering
•	Stable, unique, predictable (not index)
•	Help React match components across renders
•	Wrong keys cause bugs: state mixing, performance issues

### 20. React Strict Mode
Expected Answer: Development tool highlighting potential problems in application. Hints:
•	Wraps app or component tree
•	Detects legacy lifecycle methods, unsafe side effects
•	Double-invokes functions to detect side effects
•	Helps prepare for concurrent features

### 21. Fragment in React and Uses
Expected Answer: Component that lets you group elements without adding extra DOM nodes. Hints:
•	Syntax: <React.Fragment> or <>
•	Useful when returning multiple elements from component
•	Can have key prop when mapping in lists
•	Cleaner DOM structure, no wrapper divs

---

## React Hooks & State Management

### 1. Rules of Hooks
Expected Answer: Only call hooks at top level, only from React functions (not regular JS). Hints:
•	Don't call hooks inside loops, conditions, or nested functions
•	Call from React functional components or custom hooks
•	ESLint plugin eslint-plugin-react-hooks enforces rules
•	Rules ensure hooks called in same order each render

### 23. List of hooks 
•	React built-in hooks
useState, useEffect, useRef, useContext, useReducer, useMemo, useCallback, useLayoutEffect
•	Routing (React Router v6)
useNavigate, useLocation, useParams
•	Redux / RTK
useSelector, useDispatch
•	Custom hooks: to reuse across components
	o	useAuth() – manages authentication state & redirect logic
	o	useDebounce() – debounces rapidly changing values (e.g., search input)
	o	useFetch() / useAxios() – wrapper for API calls with loading/error states
	o	useWindowSize() – tracks window dimensions for responsive behavior
	o	useFormValidation() – (app specific) handles form validation logic

### 24. Component Lifecycle Phases
Expected Answer: Mounting, updating, and unmounting phases with corresponding methods/hooks. Hints:
•	Mounting: Component created and inserted into DOM
•	Updating: Component re-rendered due to props/state changes
•	Unmounting: Component removed from DOM
•	Error handling: Catch errors during render

### 25. React Lifecycle Comparison: Class vs Functional
Expected Answer: Class: lifecycle methods; Functional: hooks in useEffect. Hints:
•	Class: componentDidMount, componentDidUpdate, componentWillUnmount
•	Functional: useEffect with dependency array
•	Mount: useEffect(() => {}, [])
•	Update: useEffect(() => {}, [dependency])
•	Unmount: useEffect(() => { return () => {} }, [])

### 26. useState Hook
Expected Answer: Hook for adding state to functional components, returns state value and setter. Hints:
•	const [state, setState] = useState(initialValue)
•	Can use function for expensive initial state: useState(() => computeInitial())
•	State updates are batched in React 18
•	Functional updates: setState(prev => prev + 1)

### 27. useReducer Hook
Expected Answer: Alternative to useState for complex state logic with actions and reducer. Hints:
•	const [state, dispatch] = useReducer(reducer, initialArg, init)
•	Similar to Redux pattern: (state, action) => newState
•	Good for: Complex state with multiple sub-values, next state depends on previous
•	Use with useContext for global state

### 28. Context API
Expected Answer: Provides way to pass data through component tree without prop drilling. Hints:
•	createContext, Provider, useContext
•	Not optimized for frequent updates
•	Combine with useReducer for Redux-like pattern
•	Use for: Theme, auth, localization data

### 29. Ways to Manage State in React
Expected Answer: Local state, context, state management libraries, server state libraries. Hints:
•	Local: useState, useReducer (component or small tree)
•	Global: Context API (medium apps), Redux/Zustand/Jotai (large apps)
•	Server: React Query, SWR, Apollo
•	Form: Formik, React Hook Form

### 30. When to use useReducer over useState
Expected Answer: Use useReducer when state logic is complex, multiple sub-values, or next state depends on previous. Hints:
•	Complex state transitions
•	State with multiple sub-values that change together
•	Business logic that needs testing
•	When you need to dispatch actions from child components

### 31. useCallback vs useMemo Hook
Expected Answer: useCallback memoizes functions, useMemo memoizes values. Hints:
•	useCallback(fn, deps) returns memoized function
•	useMemo(() => computeValue(), deps) returns memoized value
•	Both prevent unnecessary re-creations
•	Use when: Passing callbacks to optimized child components, expensive calculations

### 32. Custom Hook
Expected Answer: JavaScript function using other hooks to extract reusable component logic. Hints:
•	Name starts with use (convention, not requirement)
•	Can call other hooks
•	Share stateful logic without component hierarchy
•	Examples: useLocalStorage, useFetch, useDebounce

### 34. Higher Order Components (HOC)
Expected Answer: Function that takes component, returns enhanced component with additional props/logic. Hints:
•	Pattern for reusing component logic
•	Wrap component, inject props
•	Can cause prop collision, wrapper hell
•	Modern alternative: Custom hooks

### 35. What is Render in React?
Render = Calling your component function to get React elements (JSX) that describe what should be on the screen. 
Re-render: Calling component functions again

Two Types of Renders:
1.	Virtual DOM Render (Render Phase)
React executes your component function
Output: A tree of React elements (virtual DOM)
Returns: React.createElement('div', null, 'Hello ', props.name)
2.	Actual DOM Update (Commit Phase)
React updates real browser DOM
Output: Actual pixels on screen, happens after render

### 36. useLayoutEffect Hook vs useEffect Hook
Timing difference, both run after render, only timing difference. useLayoutEffect runs synchronously after DOM mutations, before paint.
useEffect hook: Render → Paint to Screen (browser has painted) → useEffect runs (async)
-	Runs after the browser has painted and doesn't block visual updates
-	Use for: API calls, subscriptions, analytics

useLayoutEffect hook: Render → useLayoutEffect runs (sync) → Paint to Screen
-	Run before the browser paints (jsx render), Blocks painting until it completes.
-	Use for: DOM measurements, synchronous state updates

### 37. useEffect Hook
Expected Answer: Perform side effects in functional components (data fetching, subscriptions). Hints:
•	Runs after render, can depend on values in dependency array
•	Cleanup function prevents memory leaks
•	Empty array [] runs once on mount
•	No array runs on every render (usually wrong)

### 38. useRef Hook, Real use
The useRef hook in react is a powerful tool that allows you to create a reference to a value that persists across renders without causing re-renders. 
Use: This is useful for accessing and manipulating DOM elements directly, as well as storing mutable values that don't affect the rendering process. 
•	Returns mutable ref object with .current property, persists across renders
•	Access DOM elements: <div ref={myRef} />
•	Store mutable value without triggering re-render
•	Unlike state, changing .current doesn't cause re-render
•	Use for: Focus management, animation, integration with DOM libraries

### 39. Difference between useState and useReducer
Expected Answer: useState for simple state, useReducer for complex logic with actions. Hints:
•	useState: Simple values, direct setters
•	useReducer: Complex state, predictable transitions with actions
•	useReducer easier to test (pure reducer function)
•	useReducer can dispatch actions from child components

### 40. Redux vs useReducer + Context API
Expected Answer: Redux offers middleware, devtools, ecosystem; Context+Reducer simpler for small apps. Hints:
•	Redux: Time-travel debugging, middleware (thunk/saga), large ecosystem
•	Context+Reducer: Built-in, less boilerplate, good for medium apps
•	Redux Toolkit simplifies Redux setup
•	Choose based on team experience, app complexity

### 41. React Error Boundaries, Lifecycle
Error Boundaries are React components that catch JavaScript errors anywhere in their child component tree, log those errors, and display a fallback UI instead of the component tree that crashed / webpage crash. 
Lifecycle: componentDidCatch, getDerivedStateFormError
•	Class component with getDerivedStateFromError or componentDidCatch
•	Doesn't catch: event handlers, async code, SSR errors, errors in boundary itself
getDerivedStateFromError(error) -> This method is used to render a fallback UI after an error.
componentDidCatch(error, errorInfo) -> This method is used to log error information.

Error boundaries do not catch errors for:
•	Event handlers (use try/catch for those)
•	Asynchronous code (e.g., setTimeout or requestAnimationFrame callbacks)
•	Server side rendering
•	Errors thrown in the error boundary itself (rather than its children)
How to use?
We create a class component that implements one or both above methods and then wrap it around the components that we want to protect, or that have chance of error.

### 42. Prevent Performance Issues in Complex Trees
Expected Answer: Use memoization, state colocation, and code splitting for complex component trees. Hints:
•	React.memo() for component memoization
•	useMemo() for expensive calculations
•	useCallback() for function references
•	Lift state only as needed
•	Code split with React.lazy() and Suspense

### 43. React Performance Optimizations
Expected Answer: Implement strategies to prevent unnecessary re-renders and optimize bundle size. Hints:
•	Avoid inline objects/functions as props
•	Virtualize long lists
•	Code splitting by route/feature, lazy loading
•	Production build with minification, tree shaking
•	Use PureComponent/React.memo appropriately
•	Using proper keys in lists

### 44. React server components vs Client components
React Server Components run exclusively on the server, primarily for data fetching, performance, and security,
Client Components run in the browser to enable interactivity, state management, and access to browser-specific APIs
Feature 	Server Components	Client Components
Execution	Server only	Primarily client (browser), with initial server pre-rendering for performance/SEO
Interactivity	None (cannot use useState, useEffect, event handlers)	Full interactivity with Hooks, state, and event handlers
JavaScript Bundle	Not included in the client-side JavaScript bundle (zero bundle size)	JavaScript code is sent to the client for hydration and execution
Data Access	Can securely access databases, file systems, and secrets directly on the server	Cannot access server-only resources directly; must use API routes/server actions
Performance	Faster initial page loads and improved Time to Interactive (TTI)	Slower initial page load (requires JS download and hydration) but offers highly dynamic UIs post-load
SEO	Excellent for SEO as fully rendered HTML is sent to the crawler	Can be SEO-friendly with server-side rendering (SSR) pre-rendering, but historically challenging

When to Use:
Use Server Components (default) for: 
•	Static or non-interactive content, such as blog posts or layouts.
•	Data fetching from databases or internal APIs to reduce latency and improve security.
•	Reducing client-side JavaScript bundle size for better performance. 
Use Client Components (with "use client" directive) for: 
•	User interaction and event handlers (e.g., onClick, onChange).
•	State management using hooks like useState and useReducer.
•	Browser-specific APIs or web APIs (e.g., window, localStorage, animations).
•	Components that require client-side libraries not supported on the server (e.g., charts, maps). 

### Redux Concepts Explained

### 64. What is Redux, why we need Redux?
Redux is a predictable state management library for JavaScript applications. It serves as a centralized store for managing application state that needs to be accessed by multiple components across your entire app.
Why we need Redux:
•	Single Source of Truth: All application state is stored in one central store
•	Predictable State Updates: State can only be changed in a predictable way through actions and reducers
•	Component Communication: Solves the problem of prop drilling (passing props through multiple levels)
•	Debugging Capabilities: Time-travel debugging and easy state inspection
•	Maintainability: Makes state changes traceable and testable
•	Shared State: When multiple unrelated components need access to the same data

### 65. What is basic flow of Redux?
The Redux flow follows a strict unidirectional data pattern:
1.Action → 2. Reducer → 3. Store → 4. View
Step-by-step flow:
1.	Action Creator → Function that creates an action object (plain JS object with type property)
2.	Dispatch → The action is dispatched to the store
3.	Reducer → Pure function that takes current state and action, returns new state
4.	Store → Updates its state with the new state from reducer
5.	Components → Connected components re-render with new state
Details:
UI triggers an action: The user clicks a button or types in a form. The component calls dispatch() and sends an action — a plain JavaScript object that always has a type (like 'todos/add') and optionally a payload (the new data, e.g. { text: 'Learn Redux' }).

Reducer calculates the new state: The action is sent to a reducer — a pure function that receives the current state and the action object. It looks at the type and decides how to update state. It never changes the original state, it returns a brand new copy of the state with the changes applied.

Store updates and notifies: The Redux store takes the new state returned by the reducer and replaces the old state. Then it tells every part of the app that is listening (subscribers) that the state has changed.

UI re-renders: Components that are connected via useSelector() automatically receive the fresh state. React compares old and new values and re-renders only the components that need to show the updated data.

Bonus – Middleware (for async work like API calls)
Sometimes you need to fetch data from a server. Middleware (like Redux Thunk) sits between dispatch and the reducer. It can "intercept" actions, do async work, and then dispatch new regular actions (like 'FETCH_SUCCESS') when the data arrives. This keeps reducers pure and synchronous.

### 66. Why we need Redux Toolkit? (vs plain Redux or Context)
•	Less boilerplate
Plain Redux forces you to write actions, action creators, switch cases in reducers, and manually combine them.
createSlice generates everything automatically — just write the slice name, initial state, and reducers. Done.
•	Immer integration (mutate safely)
In plain Redux you must never mutate state — you always return a new object using ...spread or Object.assign.
With Redux Toolkit, you write code that looks like mutation (state.value = 5) and Immer automatically converts it to immutable updates. Less mental overhead, fewer bugs.
•	Opinionated best practices
Plain Redux doesn't tell you how to structure your code. RTK encourages feature folders, normalized state, and RTK Query for APIs. This keeps large apps maintainable.
•	RTK Query built-in
Need to fetch data from an API? In plain Redux you'd write thunks, handle pending/fulfilled/rejected, manage loading/error flags manually.
RTK Query gives you auto-generated hooks (useGetTodosQuery) that handle caching, background refetching, and loading states out of the box.
•	Safer patterns
Typos in action type strings ('ADD_TODO' vs 'ADD_TODOS') are common in plain Redux and cause silent failures. RTK's action types are auto-generated from the slice name – no string typos, no copy-paste mistakes.
What about Context?
Context is great for passing down themes or user data, but it re renders all consumers on every update and has no devtools. Redux Toolkit gives you performance optimizations, time travel debugging, and a single store for all global state — without the old Redux complexity.

### 67. How to connect component with store?
Two main approaches:
Using connect() function (Higher-Order Component pattern):
•	Wrap component with connect()
•	Define mapStateToProps to specify which parts of state the component needs
•	Define mapDispatchToProps to specify which actions component can dispatch
•	Returns a new connected component that receives state/dispatch as props
Using Hooks (modern approach):
•	useSelector() hook → extracts data from Redux store state
•	useDispatch() hook → returns reference to dispatch function
•	Components use these hooks to read state and dispatch actions

### 68. What is middleware in Redux, give some examples?
Middleware is a way to extend Redux with custom functionality. It sits between dispatching an action and the moment it reaches the reducer. Middleware can intercept, modify, or delay actions.
Examples:
•	Redux Thunk → Handles async logic by allowing action creators to return functions
•	Redux Saga → Uses generators to handle complex async flows and side effects
•	Redux Logger → Logs actions and state changes to console
•	Redux Persist → Persists and rehydrates Redux store
•	Redux Observable → Uses RxJS observables for handling async logic

### 69. What is basic difference between redux thunk and redux saga?
Redux Thunk:
•	Simpler, less boilerplate
•	Action creators return functions instead of objects
•	Functions receive dispatch and getState as parameters
•	Good for simple async operations (API calls)
•	Less control over complex async flows
Redux Saga:
•	Uses ES6 Generators (function*)
•	More powerful for complex async scenarios
•	Can listen for specific actions (takeEvery, takeLatest)
•	Can cancel or race async operations
•	Better for handling complex side effects, retries, debouncing
•	Steeper learning curve, more boilerplate
Analogy: Thunk is like giving instructions, Saga is like having a dedicated worker following a script.

### 70. What is difference between old Redux and Redux Toolkit?
Old Redux (Traditional/Classic):
•	Lots of boilerplate code
•	Separate files for actions, action types, reducers
•	Manual setup of store with middleware and enhancers
•	Manual immutable updates (spread operators)
•	Multiple packages needed (redux, react-redux, middleware)
•	Complex store configuration
Redux Toolkit (Modern/Recommended):
•	Opinionated defaults and best practices
•	Less boilerplate through createSlice (combines reducers+actions)
•	Built-in middleware (Thunk included by default)
•	Automatic immutable updates with Immer
•	Simplified store setup with configureStore
•	DevTools enabled by default
•	TypeScript friendly
•	Official recommended approach
Key Simplifications:
•	createSlice automatically generates action creators
•	configureStore sets up store with good defaults
•	Write "mutating" logic that gets converted to immutable updates
•	Built-in async handling with createAsyncThunk

---

## React Performance & Optimization

### 45. Memory Leaks in React
Expected Answer: Prevent memory leaks by cleaning up effects, unsubscribing events, and canceling requests. Hints:
•	Return cleanup function from useEffect
•	Cancel fetch requests with AbortController
•	Remove event listeners in cleanup
•	Clear intervals/timeouts
•	Use React DevTools to detect leaks

### 46. Performance Metrics in Production
Expected Answer: Monitor Core Web Vitals and other metrics for performance optimization. Hints:
•	LCP (Largest Contentful Paint): <2.5s
•	FID/INP (Interaction to Next Paint): <100ms
•	CLS (Cumulative Layout Shift): <0.1
•	Tools: Lighthouse, WebPageTest, Sentry, New Relic
•	Real User Monitoring (RUM) vs synthetic testing

### 47. App Feels Slow but Lighthouse Good
Expected Answer: Address perceived performance and interaction responsiveness issues. Hints:
•	Skeleton screens for loading states
•	Optimistic updates for immediate feedback
•	Debounce rapid user interactions
•	Measure INP for interaction responsiveness
•	Check main thread blocking with Performance tab

### 48. Optimize Images in Large App
Expected Answer: Implement modern image optimization techniques for better performance. Hints:
•	WebP/AVIF formats with fallbacks
•	loading="lazy" for below-fold images
•	srcset and sizes for responsive images
•	CDN with image optimization (resize, compress)
•	Implement blur-up/LQIP technique

### 49. Render 100k+ Items in React
Expected Answer: Use virtualization or pagination to efficiently render large lists. Hints:
•	Libraries: react-window, react-virtualized
•	Only render visible items in viewport
•	Infinite scroll with Intersection Observer
•	Pagination for simpler implementation
•	Trade-off: scroll position restoration

### 50. How to Improve React Application Performance
Expected Answer: Multiple strategies from code splitting to memoization to virtualization. Hints:
•	Bundle optimization: Code splitting, tree shaking
•	Render optimization: Memoization, virtualization
•	Asset optimization: Images, fonts, compression
•	Runtime optimization: Debouncing, web workers

### 51. High-Impact Optimizations
Expected Answer: Code splitting, virtualization, eliminating unnecessary re-renders. Hints:
•	Route-based code splitting with React.lazy
•	Virtualize long lists with react-window
•	Use React.memo, useMemo, useCallback appropriately
•	Optimize images: WebP, lazy loading, responsive

### 52. Medium-Impact / Supporting Techniques
Expected Answer: Bundle analysis, CDN, compression, service workers. Hints:
•	Analyze bundle with webpack-bundle-analyzer
•	CDN for static assets
•	Brotli/gzip compression
•	Service workers for offline/caching

### 53. Detailed Performance Optimization Techniques
Expected Answer: Comprehensive list of specific techniques with implementation details. Hints:
•	Production build: Minification, dead code elimination
•	Avoid inline functions/objects as props
•	Windowing for large lists
•	Web Workers for heavy computations
•	Optimize re-renders with React DevTools Profiler

### 54. React Portals
Expected Answer: Render children into DOM node outside parent hierarchy (modals, tooltips). Hints:
•	ReactDOM.createPortal(child, container)
•	Child mounts into provided DOM node
•	Events bubble through React tree, not DOM tree
•	Use for: Modals, tooltips, dropdowns that need to escape overflow:hidden

---

## React 18 Features

### 55. Concurrent React
Expected Answer: Set of features helping React apps stay responsive during rendering. Hints:
•	Interruptible rendering: React can pause, resume, or even abandon low-priority rendering work if something more urgent (e.g., user input) comes in.
•	Priority-based updates: Distinguishes between urgent updates (e.g., typing in an input) and non-urgent ones (e.g., filtering a large list or fetching results), giving higher priority to user interactions.
•	Automatic batching of state updates: Groups multiple state updates into a single re-render (now works in more cases like promises, setTimeout, native event handlers — not just React event handlers).
•	Transition APIs for non-urgent UI updates: Mark certain updates as "transitions" (non-urgent), allowing React to keep the UI responsive while preparing the new state in the background.
useTransition and useDeferredValue are the main ways developers interact with concurrent rendering in React 18.
Which React hooks enable / relate to Concurrent React?
The primary new hooks introduced in React 18 that directly leverage concurrent rendering are:
-Enhanced <Suspense> behavior (especially with transitions and server rendering)
useTransition → Lets you mark state updates as non-urgent "transitions". Returns: [isPending, startTransition] 

Usage example: Wrap slow filter / tab change / search update
es so typing or clicking stays smooth while the heavy render happens in the background.
```javascript
const [isPending, startTransition] = useTransition();
function handleChange(e) {
  startTransition(() => {
    setFilter(e.target.value); // ← this update is now non-urgent
  });
}
```
WorkFlow:
1.	User types in search box
2.	handleChange is called with each keystroke
3.	Inside startTransition, setFilter() is marked non-urgent
4.	Typing stays smooth (urgent updates)
5.	Filtering happens in background
6.	isPending is true while background work runs
7.	Results update when background work completes
useDeferredValue → Defers a value (usually a prop or state) so the UI using the deferred version lags behind the "urgent" value. Very useful for deferring expensive list renders or search results while keeping the input responsive.
```javascript
const deferredQuery = useDeferredValue(query);
// Use deferredQuery in a slow component (e.g. filtered list)
```
1. Initial State:
•	value = "", deferredValue = ""
•	Input: empty, List: empty
2. User types "a":
•	Urgent render: value = "a", deferredValue = "" (old)
•	Input shows "a", List shows empty (filtered with "")
•	Background render starts for "a"
3. User types "b" before background finishes:
•	Urgent render: value = "ab", deferredValue = "a" (old)
•	Input shows "ab", List shows empty or results for "a"
•	Background render for "a" discarded
•	New background render starts for "ab"
4. User types "c" before background finishes:
•	Urgent render: value = "abc", deferredValue = "a" (still old)
•	Input shows "abc", List shows old results
•	Background render for "ab" discarded
•	New background render starts for "abc"
5. User stops typing:
•	Background render for "abc" completes
•	deferredValue updates to "abc"
•	List shows filtered results for "abc"

### 56. Hooks Mentioned in React 18
Expected Answer: New hooks for concurrent features and external store integration.
•	useId: Generate unique IDs stable across server/client
•	useTransition: Mark updates as non-urgent (transitions)
•	useDeferredValue: Defer updating non-urgent parts of UI
•	useSyncExternalStore: Integrate with external stores
•	useInsertionEffect: For CSS-in-JS libraries

### 57. Key Features & Concepts
Expected Answer: Automatic batching, transitions, streaming SSR, new hooks.
•	Concurrent features opt-in via createRoot
•	Strict Mode simulates mounting twice to detect side effects
•	Suspense for data fetching (experimental)
•	Improved performance for apps with many updates

### 58. Automatic Batching
Expected Answer: React groups multiple state updates into single re-render for better performance. Hints:
•	React 17: Only batched inside React event handlers
•	React 18: Batched everywhere (timeouts, promises, native handlers)
•	flushSync() opt-out for immediate updates
•	Reduces unnecessary re-renders

### 60. Transitions (Urgent vs Non-Urgent)
Expected Answer: API to mark certain updates as non-urgent transitions. 
•	startTransition() wraps non-urgent state updates
•	useTransition() hook with isPending state
•	Urgent: Typing, clicking, pressing
•	Non-urgent: Filtering list, loading new page
•	React can interrupt transitions for urgent updates

### 61. Suspense on the Server (Streaming)
Expected Answer: Stream HTML from server as components render, send Suspense fallbacks first. 
•	Server can send HTML in chunks
•	Suspense boundary shows fallback while component loads
•	Client hydration can happen progressively
•	Better perceived performance, Time to First Byte

### 62. Strict Mode Updates
Expected Answer: Development-only checks for identifying side effects, double-invokes functions. 
•	Double-invokes component functions, useEffect, etc.
•	Helps find impure components
•	Prepares for future React features
•	Doesn't affect production builds

### 63. Migration Steps (Syntax Change)
Expected Answer: Update ReactDOM.render to createRoot, enable concurrent features. 
```javascript
// React 17
ReactDOM.render(<App />, document.getElementById('root'));

// React 18
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```
•	Update to new root API
•	Test for breaking changes
•	Gradually adopt concurrent features
•	Check third-party library compatibility

---

## Routing & Authentication

### 130. React Router
Routing is implemented using the React Router library, which allows you to create a single page application with multiple views without reloading the page. npm install react-router-dom 
React Router is the standard library for handling navigation in React applications. It enables Single-Page Applications (SPAs) to have multiple views and bookmarkable URLs without refreshing the entire page.
Core Concepts
•	BrowserRouter: The parent component that stores the current location and keeps your UI in sync with the URL using the HTML5 history API.
•	Routes & Route: Routes acts as a container for your route definitions, while Route defines the mapping between a specific path (e.g., /about) and the component to render.
•	Link & NavLink: Components used for navigation. Unlike standard <a> tags, they prevent full page reloads.
•	Nested Routes: Allows you to nest routes inside others, enabling complex layouts where only a portion of the page (the <Outlet />) changes. 
Key Hooks
•	useNavigate(): Allows for programmatic navigation, such as redirecting a user after they submit a form.
•	useParams(): Extracts dynamic parameters from the URL (e.g., getting 123 from /user/123).
•	useLocation(): Provides access to the current location object, including the path and query strings. 
<BrowserRouter> (or another Router provider) Wraps everything. It provides the routing context for your entire app. Typical placement: In your index.js or main.js wrapping the <App /> component.
<Routes> Wraps all your route definitions. It acts as a smart container that looks through all its children to find the best match for the current URL.
Typical placement: Inside your main layout or App.js.
<Route> Wraps nothing (it's a definition). It is a self-closing tag (or a parent for nested routes) that maps a path to an element.
Syntax: <Route path="/login" element={<LoginPage />} />
Component (e.g., <LoginPage />)
The final destination. This is the actual React component that gets rendered into the UI when the route matches.

### 131. Additional Routing Concepts
Expected Answer: Nested routes, route guards, lazy loading, query parameters. Hints:
•	Nested routes for complex layouts
•	Route guards for authentication
•	Lazy loading routes for code splitting
•	Query parameters with useSearchParams

### 132. Profilers
Expected Answer: React.Profiler component measuring rendering performance. Hints:
•	Wraps component tree, measures render time
•	onRender callback receives metrics
•	Identify performance bottlenecks
•	Use in development, not production

### 133. Protected Routes in React Router v6+
Expected Answer: Create wrapper component checking auth, redirecting if not authenticated. Hints:
```javascript
function ProtectedRoute({ children }) {
  const { user } = useAuth();
  const location = useLocation();
  
  if (!user) {
    return <Navigate to="/login" state={{ from: location }} replace />;
  }
  
  return children;
}
```
•	Check authentication status
•	Redirect to login with return URL
•	Preserve state for post-login redirect

### 134. Role-Based Authorization in Routing
Expected Answer: Check user roles/permissions before allowing route access. Hints:
•	User object contains roles/permissions
•	Route configuration includes required roles
•	Check if user has required role
•	Redirect or show unauthorized

### 135. SSO Login (OAuth2/OIDC) in React
Single Sign On is a authentication method that allows users to access multiple applications and services using a single set of credentials. (username & password).
1.	 Authentication: When user try to login SSO enabled application (websites), it will redirected to Identity Provider (IdP)
–	The application checks if the user is already authenticated.
–	If not it will redirect to the Identity Provider (Idp) eg. Google, Okta, or MS Azure
–	User logs in: The user enter their credentials (username/ passwords) on IdP login page.

2.	Token Generation: Upon IdP verifies user username/ password it creates a signed token (eg. SAML, OAuth)
–	It creates token with user Information.

3.	Token Sharing: (Token sent back to Application): The Idp redirects the user back to the application with the token. And Application validates token with IdP.

4.	Action Granted: Once validated the application grant user access without requiring additional login.
–	No need to login seamless access to another application.
Benefits:
•	Better User Experience
•	Enhance Security: Reduce the risk of weak password
•	Simplified IT management: Admin can manage user with single system credentials.

### 136. JWT (JSON Web Token) and Workflow?
Expected Answer: Compact, URL-safe token containing claims, used for authentication. Hints:
JSON Web Token is a secure way of handling authentication in web react applications. 
•	Structure: Header.Payload.Signature, Username is stored in Payload.
•	Stateless: Server doesn't store session
•	Claims: User ID, roles, expiration
•	Store in httpOnly cookie for security
How JWT Works?
1.	User login (post/ login): When user login with username/ password on login page Client send request to the server.
2.	Server Validates: Server checks if credentials are valid, it creates a JWT containing user secrete key, information and permissions. Then server send JWT token to browser.
3.	Token Storage: The react applications receives the JWT and stores it in local storage or HTTP-Cookie.
4.	Client Requests: For subsequent request for protected routes, the react app includes JWT in the Authorization header.
5.	Token Verification: The server verifies the JWT signature and extract user information for each request.
6.	If token is valid, it send the appropriate response to the client.

### 137. JWT + Refresh Token Flow
Expected Answer: Short-lived access token + long-lived refresh token for security. Hints:
•	Access token: Short-lived (minutes), used for API calls
•	Refresh token: Long-lived (days/weeks), stored securely
•	Refresh endpoint exchanges refresh token for new access token
•	Rotate refresh tokens for security
Token Storage: Security considerations:
•	localStorage: Vulnerable to XSS, but survives page refresh
•	sessionStorage: Vulnerable to XSS, lost on tab close
•	httpOnly cookie: BEST - Immune to XSS, sent automatically
•	Memory only: Most secure, lost on refresh
Flow for refresh token:
1.	User logs in: When user login (/login) POST request, the server validates and generates two tokens.
–	Acess token: short -lived (eg, 30 mins) for Authorization in API request.
–	Refresh token: long lived token (7 days) used to obtain new access token when current one expires
–	Server send access token in response body, while the refresh token is set in an HTTP-only cookie to improve security.
2.	Access token storage: Stores the access token in memory, or in local storage or session storage.
–	Access token is included in Authorization header when making API requests.
3.	Refresh token storage: The refresh token is stored in a secure HTTP-only cookie, which is not accessible to JavaScript and cannot be stolen via XSS attacks.
–	This cookie is automatically sent with requests to the server when needed.
4.	When access token expires: When access token expires, react app detects a 401 unauthorized error when making API request.
–	Instead of automatically logging out the user, the app automatically sends request to /refresh endpoint [refresh] including the refresh token which is stored in an HTTP cookie.
5.	Server validates the refresh token: The server checks if refresh token is valid and has not expired, the server generates new access token and send back to client.
6.	If the refresh token is expired user need to sign it login again. He will logout.

### 138. User Logged Out Mid-Session
Expected Answer: Handle token expiration/invalidation gracefully with redirects. **Hints:`
•	API returns 401 Unauthorized
•	Interceptor catches 401, attempts refresh
•	If refresh fails, redirect to login
•	Clear tokens, show appropriate message

### 139. Route-Based Code Splitting
Expected Answer: Split code by routes so each route loads only needed code. **Hints:`
```javascript
const Home = lazy(() => import('./Home'));
const About = lazy(() => import('./About'));

<Suspense fallback={<Spinner />}>
  <Routes>
    <Route path="/" element={<Home />} />
    <Route path="/about" element={<About />} />
  </Routes>
</Suspense>
```
•	React.lazy() for dynamic import
•	Suspense for loading fallback
•	Reduces initial bundle size

---

## API Integration & Data Fetching

### 140. Interceptors
Expected Answer: Intercept requests/responses to add headers, handle errors, transform data. Hints:
```javascript
// Axios interceptors
axios.interceptors.request.use(config => {
  config.headers.Authorization = `Bearer ${token}`;
  return config;
});

axios.interceptors.response.use(
  response => response,
  error => {
    if (error.response.status === 401) {
      // Handle unauthorized
    }
    return Promise.reject(error);
  }
);
```
•	Add auth tokens automatically
•	Handle common errors globally
•	Transform request/response data
•	Show loading indicators

### 141. CORS: Cross Origin Resource Sharing
Expected Answer: Security mechanism allowing restricted resources on web page from another domain. **Hints:`
•	Browser security feature
•	Server must include CORS headers
•	Preflight requests for non-simple requests
•	Proxy server or backend configuration needed for development

### 142. REST vs GraphQL
Expected Answer: REST: Multiple endpoints, fixed responses; GraphQL: Single endpoint, flexible queries. **Hints:`
•	REST: Resource-based, HTTP methods, over/under-fetching
•	GraphQL: Query language, type system, single endpoint
•	REST: Caching easier, simpler to implement
•	GraphQL: Flexible queries, no versioning

### 143. Caching & Invalidation
Expected Answer: TanStack Query: Automatic caching; Apollo: Normalized cache. **Hints:`
•	TanStack Query: Auto-caching, background updates, optimistic updates
•	Apollo: Normalized cache, GraphQL-specific
•	Cache invalidation strategies: Time-based, mutation-based, manual
•	Stale-while-revalidate pattern

### 144. Error Handling & Retry Strategies
Expected Answer: Implement retry logic with exponential backoff, circuit breakers. **Hints:`
•	Exponential backoff with jitter
•	Circuit breaker pattern prevents cascading failures
•	Different handling for 4xx (client) vs 5xx (server) errors
•	User-friendly error messages

### 145. Real-Time Updates
Expected Answer: WebSocket: bidirectional, low latency; SSE: server to client; Polling: simple. Hints:
•	WebSocket: Full-duplex, persistent connection, complex
•	SSE: Server to client only, simpler, HTTP-based
•	Polling: Request at intervals, simple, inefficient
•	Choose based on: Bidirectional needs, browser support, complexity

### 146. Scenario: Real-Time Dashboard
Expected Answer: Implement dashboard with WebSocket for updates, virtualization for data. Hints:
•	WebSocket connection for live data
•	Virtualized tables/charts for performance
•	Debounce rapid updates
•	Offline support with local cache
•	Graceful degradation if WebSocket fails

---

## Security & Performance

### 147. CSRF and XSS
Cross-Site Request Forgery (CSRF) is a type of cyber attack that occurs when a malicious website, email, link cause a user's web browser to perform an unwanted action on a trusted site when the user is authenticated.
How a CSRF Attack Works?
1.	User Authorization: The user login to trusted website eg. Bank.com and receives session cookie and it is active.
2.	Malicious Action: Then user visits a malicious website or clicks malicious links while he is till authenticated on the trusted site and session is active.
3.	Automatic Action: Malicious sites (attacker) forces the users browser to send a request (eg, POST/ transfer) to bank.com Browser automatically includes the session cookie with the request.
4.	Unauthorized Action: It can leas unauthorized actions like transfer money, password changes, etc.
Prevent It:
1.	Use CSRF tokens: generate a unique token for each user session, include token in every API request.
2.	Use samesite cookies, set cookies with SameSite = Strict
3.	Requires user password to re- enter or user Authentication for critical actions.
XSS (Cross Site Scripting): XSS is a security vulnerability that allows attackers to inject malicious scripts into web pages viewed by users. These scripts can steal sensitive data or manipulate web content.
Scenerio: When a user visits shop.com and use search bar "laptop" to search laptop. Then submit search laptop by clicking search button. The attacker will entered any scripts, or can inject sql queries to manipulate data.
Prevent XSS:
•	Validate & filter user input
•	Use HTTP Only cookies
•	Avoid innerHTML instead use textContent in JS to prevent script execution
•	Validation of input field.

### 148. Frontend Security Best Practices
Expected Answer: Implement security measures to protect against common vulnerabilities. Hints:
•	XSS prevention: sanitize input, escape output, CSP headers
•	CSRF protection: anti-CSRF tokens, SameSite cookies
•	Authentication: secure token storage, refresh rotation
•	Dependency scanning for vulnerabilities
•	Regular security audits

### 149. Performance Optimization Techniques
Expected Answer: Apply performance optimizations for fast loading and smooth interactions. Hints:
•	Bundle optimization: tree shaking, code splitting
•	Image optimization: modern formats, compression
•	Caching strategies: CDN, service workers
•	Critical rendering path optimization
•	Monitoring with Real User Metrics

### 150. Monitoring & Debugging
Expected Answer: Implement monitoring and use debugging tools to maintain application health. Hints:
•	Error tracking with Sentry/LogRocket
•	Performance monitoring with RUM
•	React DevTools for component profiling
•	Chrome DevTools for performance analysis
•	Custom metrics for business goals

### 151. How to optimize React applications
1.	Code Splitting & Lazy Loading Break your app into smaller chunks loaded on demand. - Use React.lazy() + Suspense for dynamic imports. - Reduces initial bundle size → faster Time to Interactive (TTI). - Essential for routes/heavy components (e.g., charts, modals).
2.	React Compiler (Automatic Memoization – React 19+) Build-time tool that automatically memoizes components, values, and functions based on data flow analysis. - Eliminates most need for manual useMemo, useCallback, React.memo. 
3.	React.memo for Memoization Wraps functional components to skip re-renders if props are shallow-equal (unchanged), To make component pure.
1.	useCallback: Memoizes functions (prevents recreation → stable refs for child props).
2.	useMemo: Memoizes expensive computed values.
4.	Avoid inline functions/objects in JSX (they recreate every render → break memoization).
5.	Virtualize Long Lists Render only visible items (e.g., infinite scroll feeds, tables with 1000+ rows). Huge impact on scroll performance. - Libraries: react-window, react-virtualized, or TanStack Table + virtualization.
6.	Use debouncing throttling for rapid invoking function, which cause re-rendering.
7.	Bundle optimization: tree shaking, code splitting
8.	Image optimization: modern formats (webP), compression
9.	Caching strategies: CDN, service workers

---

## Senior Frontend Developer Questions

### React Questions:

#### 1. What is React Fiber and how does it differ from the old reconciliation algorithm? 
React Fiber is a complete rewrite of React's core rendering engine introduced around React 16. Its main job is to break rendering work into small chunks and prioritize tasks based on urgency, which allows smoother rendering — especially in large apps.
In older versions of React, the rendering process was synchronous and blocking. So if the UI update was heavy, the browser could freeze.
But Fiber makes rendering interruptible — React can pause work, do something more urgent like handling user input, then come back and continue where it left off.
👉 In short: Fiber brings asynchronous rendering, prioritization, and better user experience.

#### 2. How does React determine when to re-render a component? 
React re-renders a component when: Its state or props change Its parent component re-renders, unless it's memoized
But here's the twist: React doesn't do deep comparisons by default. So even if an object or array looks "the same", if the reference changes, React treats it as different.
That's why techniques like: React.memo() useMemo() useCallback() are important in performance tuning — they help you avoid unnecessary renders when data hasn't really changed.
🧠 Bonus: The key prop also plays a role in determining if elements inside a list need to be re-rendered or not. 

#### 3. What are concurrent features in React and how do they help? 
Concurrent features — introduced in React 18 — allow React to prepare multiple versions of the UI at the same time.
Let's say a user is typing a search query and you're fetching filtered results. Without concurrency, the UI could feel laggy. But with concurrent rendering, React can keep the UI responsive while preparing the new result list in the background.
📦 Features like startTransition, useDeferredValue, and automatic batching allow this.
So, concurrent features are like giving React multitasking superpowers — it can prioritize urgent updates like user input over non-urgent tasks like data fetching.

#### 4. Explain React's batching behavior and what changed in React 18. 
Batching means React groups multiple state updates into one render to improve performance.
Before React 18, batching only worked inside React event handlers. If you updated state inside a setTimeout or fetch().then(), those updates triggered multiple renders.
React 18 introduced automatic batching — so now, even updates inside promises, setTimeout, or native events are batched by default.
This means fewer renders, better performance, and more predictable behavior across async code.

#### 5. What is the difference between useMemo and useCallback, and when not to use them?
useMemo is used to memoize values useCallback is used to memoize functions
They both prevent re-creation on every render, but people often overuse them.
Here's the deal: if the memoization is more expensive than recalculating the value, you're actually hurting performance. Only use them if: The value/function is heavy to compute It causes unnecessary re-renders in children
Think of them as performance tools, not default habits.

#### 6. How does Suspense work, and what are some real use cases beyond lazy loading?
React.Suspense lets you wait for something — like a lazy-loaded component or even data fetching (when used with libraries like React Query, Relay, or SWR).
You wrap a component with <Suspense fallback={}>, and while the content is loading, React shows the fallback.
Beyond just lazy loading: You can use it with streaming SSR (in Next.js) You can coordinate parallel data loading You can delay transitions to avoid layout shifts
It's about better UX during loading states, not just code splitting.

#### 7. What is useImperativeHandle and when should you use it? 
This one's niche, but powerful.
useImperativeHandle is used with forwardRef to expose custom methods from a child component to a parent, instead of exposing the full DOM ref.
For example: You have a custom Input component and you want to expose a focus() method to the parent. Instead of giving full DOM access, you expose just that method through useImperativeHandle.
🛡️ It's a way to keep encapsulation, while still giving the parent controlled access.

#### 8. How do you optimize large lists in React? 
Rendering thousands of items can crash the browser.
To solve this: Use windowing libraries like react-window or react-virtualized Render only the visible portion of the list Use key props properly Combine with memoization (e.g., React.memo) to avoid re-rendering unchanged items
Think of it like a Netflix carousel — you only render what the user sees. This saves memory, time, and power.

#### 9. How does useRef differ from useState? 
useRef holds a mutable value that doesn't cause re-renders when it changes. useState holds values that do trigger re-renders.
Use useRef for: DOM refs (ref={myRef}) Storing timers, counters, or previous values Avoiding re-renders on updates
So: useState = React cares when it changes useRef = React ignores the change

#### 10. How does React handle hydration in SSR, and what problems can arise? 
Hydration is the process where React attaches event listeners to server-rendered HTML on the client.
The problem is — if the HTML rendered on the server doesn't match what the client renders, you get a hydration mismatch warning.
Common causes: Using random values (like Math.random()) during render Accessing window or localStorage on the server Not wrapping async components in 
Hydration issues are subtle but can break your UI in production — so always test SSR apps thoroughly.

---

## Scenario Based Interview Questions

### Question 1: Your React app suddenly becomes slow after adding new features. How do you find and fix the issue?
Diagnosis:
1.	Chrome DevTools Performance Tab: Record a performance profile to identify slow functions and long tasks.
2.	React DevTools Profiler: Identify components that re-render unnecessarily.
Common Fixes:
1.	Prevent Unnecessary Re-renders: Use React.memo, useCallback, and useMemo.
2.	Avoid Inline Functions/Objects in JSX props to prevent new references on every render.
3.	List Virtualization: Use libraries like react-window or react-virtualized for long lists.
4.	Code Splitting: Use React.lazy and Suspense to split code into smaller chunks.

### Question 2: How do you prevent a login page from being accessible after the user is logged in?
Implementation:
1.	Use Protected/Auth Routes: Create a wrapper component for routes that require authentication.
2.	Logic: Check the user's auth state (e.g., isAuthenticated flag from Context/Redux/store).
3.	Conditional Routing:
–	If user is logged in (isAuthenticated === true) and tries to access /login, redirect them to a private route (e.g., /home).
–	If user is not logged in, they can access the login page.

### Question 3: How do you handle API errors gracefully in the UI?
Strategy:
1.	Categorize Errors: Handle different error types (Network, Server (5xx), Client/Authorization (4xx)).
2.	User-Friendly Feedback:
–	Show clear, non-technical error messages.
–	Use conditional rendering for error states.
–	For network errors, offer a retry button.
–	For auth errors, redirect to login.

### Question 4: A component is fetching the same data multiple times unnecessarily. How do you fix it?
Solutions:
1.	Data Fetching Library: Use React Query (TanStack Query) or SWR for automatic caching, deduplication, and background updates.
2.	State Management: Lift the data to a global store (Context/Redux) and fetch it once at the app level.
3.	Lifting State Up: Move the fetch to the nearest common parent component and pass the data down.
4.	Memoization: Use useMemo to cache the fetched data within the component (less common for fetches).

### Question 5: How do you optimize bundle size in a large React app?
Methods:
1.	Code Splitting: Use React.lazy() with Suspense for route-based and component-based splitting.
2.	Bundle Analysis: Analyze the bundle using source-map-explorer or Webpack Bundle Analyzer.
3.	Dependency Optimization:
–	Use dynamic imports (import()) for heavy libraries.
–	Remove unused imports and code (Tree Shaking in Vite/Webpack).
–	Audit and replace large dependencies with lighter alternatives.
4.	Asset Optimization: Compress images (WebP format) and other static assets.

### Question 6: How do you handle a form with 20+ fields efficiently?
Solutions:
1.	Form Libraries: Use Formik or React Hook Form for state management, validation, and submission.
2.	Step Logic / Sections: Break the form into logical sections/steps (multi-step form).
3.	Controlled Components: Manage form state efficiently using controlled components.
4.	Conditional/Dynamic Rendering: Only render fields that are relevant based on previous inputs.
5.	Validation: Use schema validation libraries like Zod or Yup for robust validation.

### Question 7: Your React app needs to support dark mode. How do you implement it?
Implementation:
1.	Theme State Management: Use Context API or Redux to manage theme state globally.
2.	CSS Implementation: Use CSS Variables (custom properties) or design tokens for theming.
3.	System Preference: Detect user's system preference with prefers-color-scheme media query.
4.	Persistence: Save user's theme preference in localStorage.
5.	Toggle Component: Create a theme toggle switch that updates context and CSS classes.

### Question 8: How do you prevent XSS (Cross-Site Scripting) in frontend apps?
Prevention Methods:
1.	Avoid dangerouslySetInnerHTML: Use it sparingly and sanitize any HTML content.
2.	Sanitize User Input: Use libraries like DOMPurify to sanitize HTML before rendering.
3.	Escape Content: Use React's built-in escaping (automatic for {variable} in JSX).
4.	Secure Data Attributes: Be cautious with dynamic attribute values.
5.	Content Security Policy (CSP): Implement CSP headers on the server side.

### Question 9: How do you handle offline/online state gracefully in React with Service Workers?
Implementation:
1.	Detect Connection: Use navigator.onLine API and online/offline event listeners.
2.	Service Workers: Implement service workers for caching (Cache API) and offline functionality.
3.	UI Feedback: Show clear messages when user is offline ("You are offline").
4.	Queue Actions: Queue user actions and sync when back online.
5.	PWA Features: Implement as a Progressive Web App (PWA) with offline support.

### Question 10: Production is not getting the latest deployed update unless in incognito. How do you solve this?
Solution (Service Worker Caching Issue):
1.	Service Worker Update Strategy: Implement proper versioning and update flow in service worker.
2.	Cache Control Headers: Set appropriate headers on server:
–	Cache-Control: no-cache, must-revalidate
–	Version assets with unique filenames or query strings
3.	Force Update: Add logic to prompt users to update when new version is available.
4.	Skip Waiting: Use skipWaiting() in service worker to activate new version immediately.
5.	App Version Tracking: Track app version and force refresh if outdated.
