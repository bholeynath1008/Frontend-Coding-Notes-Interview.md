# Section 4: React Hooks & State Management
## React Hooks & State Management Interview Questions

- [1. Rules of Hooks](#1-rules-of-hooks)
- [2. List of Hooks](#2-list-of-hooks)
- [3. Component Lifecycle Phases](#3-component-lifecycle-phases)
- [4. React Lifecycle Comparison: Class vs Functional](#4-react-lifecycle-comparison-class-vs-functional)
- [5. useState Hook](#5-usestate-hook)
- [6. useReducer Hook](#6-usereducer-hook)
- [7. Context API](#7-context-api)
- [8. Ways to Manage State in React](#8-ways-to-manage-state-in-react)
- [9. When to use useReducer over useState](#9-when-to-use-usereducer-over-usestate)
- [10. useCallback vs useMemo Hook](#10-usecallback-vs-usememo-hook)
- [11. Custom Hook](#11-custom-hook)
- [12. Higher Order Components (HOC)](#12-higher-order-components-hoc)
- [13. What is Render in React?](#13-what-is-render-in-react)
- [14. useLayoutEffect Hook vs useEffect Hook](#14-uselayouteffect-hook-vs-useeffect-hook)
- [15. useEffect Hook](#15-useeffect-hook)
- [16. useRef Hook, Real use](#16-useref-hook-real-use)
- [17. Difference between useState and useReducer](#17-difference-between-usestate-and-usereducer)
- [18. Redux vs useReducer + Context API](#18-redux-vs-usereducer--context-api)
- [19. React Error Boundaries, Lifecycle](#19-react-error-boundaries-lifecycle)
- [20. Prevent Performance Issues in Complex Trees](#20-prevent-performance-issues-in-complex-trees)
- [21. React Performance Optimizations](#21-react-performance-optimizations)
- [22. React Server Components vs Client Components](#22-react-server-components-vs-client-components)

## Redux Concepts

- [23. What is Redux, why we need Redux?](#23-what-is-redux-why-we-need-redux)
- [24. What is basic flow of Redux?](#24-what-is-basic-flow-of-redux)
- [25. Why we need Redux Toolkit?](#25-why-we-need-redux-toolkit)
- [26. How to connect component with store?](#26-how-to-connect-component-with-store)
- [27. What is middleware in Redux?](#27-what-is-middleware-in-redux)
- [28. Redux Thunk vs Redux Saga](#28-redux-thunk-vs-redux-saga)
- [29. Old Redux vs Redux Toolkit](#29-old-redux-vs-redux-toolkit)

## 22. Rules of Hooks
Expected Answer: Only call hooks at top level, only from React functions (not regular JS). Hints:
•	Don't call hooks inside loops, conditions, or nested functions
•	Call from React functional components or custom hooks
•	ESLint plugin eslint-plugin-react-hooks enforces rules
•	Rules ensure hooks called in same order each render

## 23. List of hooks 
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

## 24. Component Lifecycle Phases
Expected Answer: Mounting, updating, and unmounting phases with corresponding methods/hooks. Hints:
•	Mounting: Component created and inserted into DOM
•	Updating: Component re-rendered due to props/state changes
•	Unmounting: Component removed from DOM
•	Error handling: Catch errors during render

## 25. React Lifecycle Comparison: Class vs Functional
Expected Answer: Class: lifecycle methods; Functional: hooks in useEffect. Hints:
•	Class: componentDidMount, componentDidUpdate, componentWillUnmount
•	Functional: useEffect with dependency array
•	Mount: useEffect(() => {}, [])
•	Update: useEffect(() => {}, [dependency])
•	Unmount: useEffect(() => { return () => {} }, [])

## 26. useState Hook
Expected Answer: Hook for adding state to functional components, returns state value and setter. Hints:
•	const [state, setState] = useState(initialValue)
•	Can use function for expensive initial state: useState(() => computeInitial())
•	State updates are batched in React 18
•	Functional updates: setState(prev => prev + 1)

## 27. useReducer Hook
Expected Answer: Alternative to useState for complex state logic with actions and reducer. Hints:
•	const [state, dispatch] = useReducer(reducer, initialArg, init)
•	Similar to Redux pattern: (state, action) => newState
•	Good for: Complex state with multiple sub-values, next state depends on previous
•	Use with useContext for global state

## 28. Context API
Expected Answer: Provides way to pass data through component tree without prop drilling. Hints:
•	createContext, Provider, useContext
•	Not optimized for frequent updates
•	Combine with useReducer for Redux-like pattern
•	Use for: Theme, auth, localization data

## 29. Ways to Manage State in React
Expected Answer: Local state, context, state management libraries, server state libraries. Hints:
•	Local: useState, useReducer (component or small tree)
•	Global: Context API (medium apps), Redux/Zustand/Jotai (large apps)
•	Server: React Query, SWR, Apollo
•	Form: Formik, React Hook Form

## 30. When to use useReducer over useState
Expected Answer: Use useReducer when state logic is complex, multiple sub-values, or next state depends on previous. Hints:
•	Complex state transitions
•	State with multiple sub-values that change together
•	Business logic that needs testing
•	When you need to dispatch actions from child components

## 31. useCallback vs useMemo Hook
Expected Answer: useCallback memoizes functions, useMemo memoizes values. Hints:
•	useCallback(fn, deps) returns memoized function
•	useMemo(() => computeValue(), deps) returns memoized value
•	Both prevent unnecessary re-creations
•	Use when: Passing callbacks to optimized child components, expensive calculations

## 32. Custom Hook
Expected Answer: JavaScript function using other hooks to extract reusable component logic. Hints:
•	Name starts with use (convention, not requirement)
•	Can call other hooks
•	Share stateful logic without component hierarchy
•	Examples: useLocalStorage, useFetch, useDebounce

## 34. Higher Order Components (HOC)
Expected Answer: Function that takes component, returns enhanced component with additional props/logic. Hints:
•	Pattern for reusing component logic
•	Wrap component, inject props
•	Can cause prop collision, wrapper hell
•	Modern alternative: Custom hooks

## 35. What is Render in React?
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

## 36. useLayoutEffect Hook vs useEffect Hook
Timing difference, both run after render, only timing difference. useLayoutEffect runs synchronously after DOM mutations, before paint.
useEffect hook: Render → Paint to Screen (browser has painted) → useEffect runs (async)
-	Runs after the browser has painted and doesn't block visual updates
-	Use for: API calls, subscriptions, analytics

useLayoutEffect hook: Render → useLayoutEffect runs (sync) → Paint to Screen
-	Run before the browser paints (jsx render), Blocks painting until it completes.
-	Use for: DOM measurements, synchronous state updates

## 37. useEffect Hook
Expected Answer: Perform side effects in functional components (data fetching, subscriptions). Hints:
•	Runs after render, can depend on values in dependency array
•	Cleanup function prevents memory leaks
•	Empty array [] runs once on mount
•	No array runs on every render (usually wrong)

## 38. useRef Hook, Real use
The useRef hook in react is a powerful tool that allows you to create a reference to a value that persists across renders without causing re-renders. 
Use: This is useful for accessing and manipulating DOM elements directly, as well as storing mutable values that don't affect the rendering process. 
•	Returns mutable ref object with .current property, persists across renders
•	Access DOM elements: <div ref={myRef} />
•	Store mutable value without triggering re-render
•	Unlike state, changing .current doesn't cause re-render
•	Use for: Focus management, animation, integration with DOM libraries

## 39. Difference between useState and useReducer
Expected Answer: useState for simple state, useReducer for complex logic with actions. Hints:
•	useState: Simple values, direct setters
•	useReducer: Complex state, predictable transitions with actions
•	useReducer easier to test (pure reducer function)
•	useReducer can dispatch actions from child components

## 40. Redux vs useReducer + Context API
Expected Answer: Redux offers middleware, devtools, ecosystem; Context+Reducer simpler for small apps. Hints:
•	Redux: Time-travel debugging, middleware (thunk/saga), large ecosystem
•	Context+Reducer: Built-in, less boilerplate, good for medium apps
•	Redux Toolkit simplifies Redux setup
•	Choose based on team experience, app complexity

## 41. React Error Boundaries, Lifecycle
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

## 42. Prevent Performance Issues in Complex Trees
Expected Answer: Use memoization, state colocation, and code splitting for complex component trees. Hints:
•	React.memo() for component memoization
•	useMemo() for expensive calculations
•	useCallback() for function references
•	Lift state only as needed
•	Code split with React.lazy() and Suspense

## 43. React Performance Optimizations
Expected Answer: Implement strategies to prevent unnecessary re-renders and optimize bundle size. Hints:
•	Avoid inline objects/functions as props
•	Virtualize long lists
•	Code splitting by route/feature, lazy loading
•	Production build with minification, tree shaking
•	Use PureComponent/React.memo appropriately
•	Using proper keys in lists

## 44. React server components vs Client components
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

## Redux Concepts Explained

## 64. What is Redux, why we need Redux?
Redux is a predictable state management library for JavaScript applications. It serves as a centralized store for managing application state that needs to be accessed by multiple components across your entire app.
Why we need Redux:
•	Single Source of Truth: All application state is stored in one central store
•	Predictable State Updates: State can only be changed in a predictable way through actions and reducers
•	Component Communication: Solves the problem of prop drilling (passing props through multiple levels)
•	Debugging Capabilities: Time-travel debugging and easy state inspection
•	Maintainability: Makes state changes traceable and testable
•	Shared State: When multiple unrelated components need access to the same data

## 65. What is basic flow of Redux?
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

## 66. Why we need Redux Toolkit? (vs plain Redux or Context)
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

## 67. How to connect component with store?
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

## 68. What is middleware in Redux, give some examples?
Middleware is a way to extend Redux with custom functionality. It sits between dispatching an action and the moment it reaches the reducer. Middleware can intercept, modify, or delay actions.
Examples:
•	Redux Thunk → Handles async logic by allowing action creators to return functions
•	Redux Saga → Uses generators to handle complex async flows and side effects
•	Redux Logger → Logs actions and state changes to console
•	Redux Persist → Persists and rehydrates Redux store
•	Redux Observable → Uses RxJS observables for handling async logic

## 69. What is basic difference between redux thunk and redux saga?
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

## 70. What is difference between old Redux and Redux Toolkit?
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
