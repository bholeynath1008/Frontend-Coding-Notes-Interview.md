# Section 6: React 18 Features
# React 18 Features Interview Questions

## Section 6: React 18 Features

- [55. Concurrent React](#55-concurrent-react)
- [56. Hooks Mentioned in React 18](#56-hooks-mentioned-in-react-18)
- [57. Key Features & Concepts](#57-key-features--concepts)
- [58. Automatic Batching](#58-automatic-batching)
- [59. Transitions (Urgent vs Non-Urgent)](#59-transitions-urgent-vs-non-urgent)
- [60. Suspense on the Server (Streaming)](#60-suspense-on-the-server-streaming)
- [61. Strict Mode Updates](#61-strict-mode-updates)
- [62. Migration Steps (Syntax Change)](#62-migration-steps-syntax-change)
## 55. Concurrent React
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

## 56. Hooks Mentioned in React 18
Expected Answer: New hooks for concurrent features and external store integration.
•	useId: Generate unique IDs stable across server/client
•	useTransition: Mark updates as non-urgent (transitions)
•	useDeferredValue: Defer updating non-urgent parts of UI
•	useSyncExternalStore: Integrate with external stores
•	useInsertionEffect: For CSS-in-JS libraries

## 57. Key Features & Concepts
Expected Answer: Automatic batching, transitions, streaming SSR, new hooks.
•	Concurrent features opt-in via createRoot
•	Strict Mode simulates mounting twice to detect side effects
•	Suspense for data fetching (experimental)
•	Improved performance for apps with many updates

## 58. Automatic Batching
Expected Answer: React groups multiple state updates into single re-render for better performance. Hints:
•	React 17: Only batched inside React event handlers
•	React 18: Batched everywhere (timeouts, promises, native handlers)
•	flushSync() opt-out for immediate updates
•	Reduces unnecessary re-renders

## 60. Transitions (Urgent vs Non-Urgent)
Expected Answer: API to mark certain updates as non-urgent transitions. 
•	startTransition() wraps non-urgent state updates
•	useTransition() hook with isPending state
•	Urgent: Typing, clicking, pressing
•	Non-urgent: Filtering list, loading new page
•	React can interrupt transitions for urgent updates

## 61. Suspense on the Server (Streaming)
Expected Answer: Stream HTML from server as components render, send Suspense fallbacks first. 
•	Server can send HTML in chunks
•	Suspense boundary shows fallback while component loads
•	Client hydration can happen progressively
•	Better perceived performance, Time to First Byte

## 62. Strict Mode Updates
Expected Answer: Development-only checks for identifying side effects, double-invokes functions. 
•	Double-invokes component functions, useEffect, etc.
•	Helps find impure components
•	Prepares for future React features
•	Doesn't affect production builds

## 63. Migration Steps (Syntax Change)
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
