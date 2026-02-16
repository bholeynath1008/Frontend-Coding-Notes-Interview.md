# JavaScript Interview Questions

## Section 2: JavaScript

- [1. Execution Context & Memory Management](#1-execution-context--memory-management)
- [2. Scope, Hoisting, TDZ, var/let/const](#2-scope-hoisting-tdz-varletconst)
- [3. 'use strict' Mode](#3-use-strict-mode)
- [4. Closures](#4-closures)
- [5. IIFE](#5-iife)
- [6. Call, Apply, Bind Methods](#6-call-apply-bind-methods)
- [7. Arrow Function vs Regular Function](#7-arrow-function-vs-regular-function)
- [8. Anonymous Functions & Higher-Order Functions](#8-anonymous-functions--higher-order-functions)
- [9. Functional Programming Concepts](#9-functional-programming-concepts)
- [10. Event Loop & Concurrency Model](#10-event-loop--concurrency-model)
- [11. Callbacks vs Promises vs Async/Await](#11-callbacks-vs-promises-vs-asyncawait)
- [12. Promise Methods & Error Handling](#12-promise-methods--error-handling)
- [13. Synchronous vs Asynchronous Patterns](#13-synchronous-vs-asynchronous-patterns)
- [14. 'this' Keyword & Binding](#14-this-keyword--binding)
- [15. Prototypal Inheritance & ES6 Classes](#15-prototypal-inheritance--es6-classes)
- [16. Deep vs Shallow Copy](#16-deep-vs-shallow-copy)
- [17. Object Methods](#17-object-methods)
- [18. Null vs Undefined](#18-null-vs-undefined)
- [19. Pass by Value vs Pass by Reference](#19-pass-by-value-vs-pass-by-reference)
- [20. Set vs Map vs Array vs Object](#20-set-vs-map-vs-array-vs-object)
- [21. Event Delegation & Propagation](#21-event-delegation--propagation)
- [22. CORS](#22-cors)
- [23. Web Request Lifecycle](#23-web-request-lifecycle)
- [24. ES6+ Modern Features](#24-es6-modern-features)
- [25. Factory Functions](#25-factory-functions)
- [26. Polyfills & Feature Detection](#26-polyfills--feature-detection)
- [27. Security Best Practices](#27-security-best-practices)
- [28. Debouncing vs Throttling](#28-debouncing-vs-throttling)
- [29. AbortController](#29-abortcontroller)
- [30. Generator Function](#30-generator-function)
- [31. Browser Parse and Render Process](#31-browser-parse-and-render-process)
- [32. Client-Side Storage Options](#32-client-side-storage-options)
- [33. Browser APIs & Capabilities](#33-browser-apis--capabilities)
- [34. Common Algorithm Patterns](#34-common-algorithm-patterns)
- [35. Output Prediction Questions](#35-output-prediction-questions)

## 1. Execution Context & Memory Management
Expected Answer: Understand how JavaScript executes code, manages memory, and handles variable lifecycles. Hints:
•	Execution context: global, function, eval
•	Memory phases: allocation, use, release (garbage collection)
•	Stack (primitive values) vs Heap (objects, functions)
•	Common memory leaks: event listeners, timers, closures, DOM references

## 2. Scope, Hoisting, (TDZ), Difference of var, let, const 

Scope: accessibility and visibility of variables, functions, and objects in a particular part of the code.

Lexical Scope and Chaining: JavaScript uses the scope chain for variable lookup: it starts in the current scope (block or function), moves outward through parent scopes, reaches the global scope (outside of all scope), and throws a ReferenceError if not found.

Hoisting: is JavaScript's behavior of moving declarations to the top of their scope during the Memory Phase of execution context creation.
TDZ: It is stage/ time between hoisting and actual declaration, variables (let, const) exists in memory but cannot be accessed. Accessing them throws References error. Console.log(a)//TDZ, let a = 10;

•	JS creates Execution Context; it has Memory Phase and Execution Phase. 
•	In memory phase, variable and function declaration are hoisted. 
•	Let/ const are hoisted but stored in TDZ until execution reaches them.

Var, let, const Difference: Scope, Hoisting, Re-declaration (var only), Reassignment (var, let)
•	var: Scope (Functional), hoisted and initialized with undefined
•	let/const ES6: Scope (Block), hoisted but not initialized (TDZ until declaration)
•	Function declarations: fully hoisted (can be called before declaration)
•	Function expressions: follow variable hoisting rules

## 3. 'use strict' Mode
Expected Answer: Strict mode enables stricter parsing and error handling in JavaScript. Hints:
•	Eliminates silent errors by throwing exceptions
•	Prevents accidental global variables
•	Makes assignments that would silently fail throw errors
•	Disables confusing or problematic features
•	Can be file-wide or function-scoped

## Functions & Scope

## 4. Closures
Expected Answer: Functions that remember and can access variables from their lexical scope even when executed outside that scope. Hints:
•	Created when function is defined, not when called
•	Practical uses: data privacy, partial application, event handlers
•	Stale closures in React: capture old state values
•	Memory considerations: closures can prevent garbage collection

## 5. IIFE (Immediately Invoked Function Expression)
Expected Answer: Self-executing function that runs immediately after creation. Hints:
•	Creates private scope, avoids global pollution
•	Used for initialization, module pattern before ES6 modules
•	Syntax: (function() { /* code */ })(); or arrow function variant
•	Modern alternative: ES6 modules, block scope with let/const

## 6. Call, Apply, Bind Methods
Expected Answer: call, apply and bind are methods in JS that are very useful for explicitly setting the "this" context in js while calling function. Hints:
•	call(thisArg, arg1, arg2...): invoke immediately with individual args
•	apply(thisArg, [argsArray]): same as call but takes array of args
•	bind(thisArg, arg1, arg2...): returns new bound function without invoking
•	Real use cases: method borrowing from another object, partial application, bind with this in react class based components,  bind with event handlers

## 7. Arrow function, Difference between arrow vs regular function
Arrow functions (introduced in ES6) provide a concise/ simple syntax (() => {}) for writing functions in JavaScript, lacking their own this, arguments, or prototype,
Arrow function: inherit this from the surrounding lexical scope, cannot be used as constructors, and are not hoisted.  
Difference between arrow vs regular function:
•	this Binding: Regular functions define their own this context based on how they are called (caller-dependent). Arrow functions do not bind their own this; they inherit it from the parent scope.
•	Constructors: Regular functions can be used as constructors with the new keyword. Arrow functions cannot and will throw an error if used this way.
•	Syntax: Arrow functions allowing for implicit returns in single-line expressions, whereas regular functions require function keyword and return.
•	arguments Object: Regular functions have access to the arguments array-like object. Arrow functions do not; they must use rest parameters (...args).
•	Hoisting: Regular function declarations are hoisted, allowing them to be called before they are defined. Arrow functions must be defined before use. 
When to Use Which
•	Use Arrow Functions: For callbacks, array methods (map, filter), or when you want to retain the this context of the surrounding code. Requires this binding for context.
•	Use Regular Functions: For object methods (so this refers to the object), constructor functions, or when you need dynamic this binding. 

## 8. Anonymous Functions & Higher-Order Functions
Expected Answer: Functions without names and functions that operate on other functions. 
•	Anonymous functions: function expressions without name, often used as callbacks
•	Higher-order functions: take functions as arguments or return them
•	Examples: map, filter, reduce, setTimeout callbacks
•	Arrow functions often used for anonymous functions

## 9. Functional Programming Concepts
Expected Answer: Programming paradigm using pure functions, immutability, and function composition. Hints:
•	Pure functions: same input → same output, no side effects
•	Immutability: create new data instead of mutating existing
•	Higher-order functions: map, filter, reduce
•	Function composition: combining simple functions
•	Currying: transforming multi-argument functions into sequence of single-argument functions

## Asynchronous JavaScript

## 10. Event Loop & Concurrency Model
Event loop: It is a single threaded mechanism that handles asynchronous operations by executing synchronous code first, then processing all microtask queue, followed by one microtask queue.
Event loop continuously checks if call stack is empty to process queued tasks
Expected Answer: JavaScript's single-threaded, non-blocking execution model using event loop. Hints:
•	Call stack → Microtask queue → Render → Task queue cycle
•	Microtasks (Priority queue): Promises, queueMicrotask(), MutationObserver
•	Macrotasks (callback queue): setTimeout, setInterval, I/O, UI rendering

## 11. Callbacks vs Promises vs Async/Await, Error handling, (3 ways to handle async task
Asynchronous JavaScript is a programming approach that enables the non-blocking execution of tasks, allowing concurrent operations, improved responsiveness.
Three ways to handle promises: callbacks, promises, async/ await
Promise: Promise is an object that may not produce a value immediately but will produce value in sometime in future with either a resolved value or a reason why it is not resolved. 
Three states: Fulfilled, rejected or pending.
Promise allows us to write asynchronous code in more synchronous way making it easier to read and maintain. Better error handling with .catch() chaining, Promise.all() for parallelism
```javascript
fetch('/data').then(res=> res.json()).catch(err => ... )
```
Callbacks hell: callback hell is antipattern, pyramid shape, hard to read, maintain and debug, error handling issues.
Callback: in JavaScript is a function passed as an argument to another function, which is then executed (or "called back") at a later point in time or when a specific condition is met.
async/await: is syntactic sugar on top of Promises, synchronous-looking syntax, await waits for a promise to resolve and lets you write async code in a synchronous style. try/catch error handling. await should be used inside Async func definition only, async function always return promise. 
async/ await Internally it still uses Promises, ie. convert to promises under the hood.

## 12. Ways to create Promise, handle Promise, Promise Methods & Error Handling
Ways to create Promise: 
1) new Promise() Constructor, 2) Promise.resolve(value) 
3) Promise.reject(reason) 4) Async Functions: Using the async keyword automatically returns a promise.
Handle Promises: Promise chaining, handle success: .then(), handle errors: .catch(), Executes code regardless of the outcomes: finally()
•	try Block: Contains the code that is executed first. It is monitored for errors.
•	catch Block: Executed only if an exception is thrown in the try block. It receives an exception object containing details about the error.
Promise methods:
•	Promise.all(): fails fast if any promise rejects
•	Promise.allSettled(): waits for all, returns results for each
•	Promise.race(): resolves/rejects with first settled promise
•	Promise.any(): resolves with first fulfilled promise

## 13. Synchronous vs Asynchronous Patterns
Expected Answer: Understanding blocking vs non-blocking code execution strategies. Hints:
•	Synchronous: sequential execution, blocks until completion
•	Asynchronous: continues execution, handles completion via callbacks/promises
•	Blocking operations: avoid in main thread (CPU-intensive tasks)
•	Web Workers for true parallelism

## Objects & Prototypes

## 14. 'this' Keyword & Binding
Expected Answer: this refers to execution context, with multiple binding rules. Hints:
•	Default binding: global/window (non-strict mode)
•	Implicit binding: object method invocation
•	Explicit binding: call(), apply(), bind()
•	New binding: constructor invocation
•	Arrow functions: lexical this (from surrounding scope)

## 15. Prototypal Inheritance & ES6 Classes
Prototype Inheritance  is a feature in Javascript that allows/ is used to inherit properties and methods from the other object. 
Every object has a hidden internal property called [[prototype]] that points to another object this is called prototype. 
When you try to access the property of an object, if the property doesn't exist on that object, JS will automatically check the object's prototype to see if the property exists there, It will look for the end of chain is Prototype Chain. Prototype chain ends with null.
```javascript
Employee.__proto__ = person // properties
Animal.prototype.sound = () => console.log("Hi") // method
```
Expected Answer: JavaScript's prototype-based inheritance model. Hints:
•	Every object has prototype chain ending with null
•	ES6 classes are syntactic sugar over prototypes
•	extends sets up prototype chain, super() calls parent constructor
•	Object.create() for pure prototypal inheritance
•	instanceof vs Object.getPrototypeOf()

## 16. Deep vs Shallow Copy
Expected Answer: Different approaches to copying objects with varying depth. Hints:
•	Shallow copy: {...obj}, Object.assign({}, obj), slice(), concat()
•	Deep copy: JSON.parse(JSON.stringify(obj)) (loses functions), structuredClone()
•	Libraries: Lodash _.cloneDeep() handles edge cases
•	Circular references need special handling

## 17. Object Methods:
Ways to Create JavaScript Objects:
-	Object Literals: Objects are defined using curly braces {} with key-value pairs.
-	Constructor Functions with new: This approach defines an object type (a blueprint) using a function, and the new keyword creates an instance of that object
-	ES6 Classes: Modern JavaScript offers a cleaner syntax using the class keyword
-	Object.create(): This method creates a new object using an existing object.
-	Object.assign(): This method copies properties from one or more source objects to a target object.
Object Methods:
•	Object.assign(target, source): Copies properties from one or more source objects to a target object.
•	Object.create(proto): Creates a new object, using an existing object as the prototype of the newly created object.
•	Object.keys(obj): Returns an array of an object's own enumerable string property names (keys).
•	Object.values(obj): Returns an array of an object's own enumerable property values.
•	Object.entries(obj): Returns an array of an object's own enumerable string property [key, value] pairs.
•	Object.freeze(obj): Freezes an object, preventing new properties from being added, existing properties from being removed, and the enumerability, writability, or configurability of existing properties from being changed.
•	Object.getPrototypeOf(obj): Returns the prototype (internal [[Prototype]] property) of the specified object. 

## Data Types & Structures

## 18. Null vs Undefined
Expected Answer: Different meanings and use cases for two "empty" values. Hints:
•	undefined: variable declared but not assigned, function no return value
•	null: intentional absence of value, must be explicitly assigned
•	Typeof: typeof undefined = "undefined", typeof null = "object" (historical bug)
•	Use strict equality (===) for checking, not loose equality (==)

## 19. Pass by Value vs Pass by Reference
Expected Answer: How JavaScript passes different types to functions. Hints:
•	Primitives (string, number, boolean, null, undefined, symbol, bigint): passed by value (copy)
•	Objects (including arrays, functions): passed by reference (memory address)
•	Object copying creates new reference, not deep copy
•	Immutable patterns help avoid unintended mutations

## 20. Set vs Map vs Array vs Object
Expected Answer: Choosing appropriate data structures based on use case. Hints:
•	Set: stores unique values, O(1) lookup
•	Map: key-value pairs, keys can be any type (not just strings)
•	Array: ordered collection, allows duplicates, index-based access
•	Object: key-value pairs, keys are strings or symbols
•	WeakSet/WeakMap: hold weak references, don't prevent garbage collection

## DOM & Browser APIs

## 21. Event Delegation & Propagation
Both (event delegation and propagation) Efficient event handling using DOM event flow.
Event Propagation: The process by which an event travels through the DOM tree. Consists three phases:
1.	Capturing Phase: The event travels from the root (window) down to the target element.
2.	Target Phase: The event reaches the actual element that was interacted with.
3.	Bubbling Phase: The event travels back up from the target element to the root.
Event Delegation: By attaching one listener to a parent, you "catch" events as they bubble up from child elements. You then use the event.target property to identify and handle the specific child that triggered the event
•	Event phases: capturing → target → bubbling
•	Event delegation: attach single listener to parent, use event.target
•	stopPropagation(): stops event from propagating further
•	preventDefault(): prevents default browser action, stopping a submit event from refreshing the page, handling menu items.
•	Benefits: performance, dynamic elements, less memory usage

## 22. CORS (Cross-Origin Resource Sharing)
Expected Answer: Security mechanism for cross-domain HTTP requests. Hints:
•	Browser enforces same-origin policy
•	Server must include Access-Control-Allow-Origin header
•	Preflight requests (OPTIONS) for non-simple requests
•	Credentials require Access-Control-Allow-Credentials: true
•	Common solutions: CORS headers, proxy server, JSONP (legacy)

## 23. Web Request Lifecycle
Expected Answer: Complete journey of HTTP request from browser to server. Hints:
•	DNS lookup → TCP handshake → SSL/TLS → HTTP request → Response
•	Status codes: 2xx success, 3xx redirect, 4xx client error, 5xx server error
•	Headers: request/response metadata, caching directives
•	Performance optimizations: keep-alive, compression, HTTP/2, CDN

## Modern JavaScript & Best Practices

## 24. ES6+ Modern Features
Key modern JavaScript syntax improvements. 
•	let/const: block scoping, TDZ, no hoisting to undefined
•	Arrow functions: lexical this, concise syntax, implicit return
•	Template literals: multi-line strings, interpolation, tagged templates
•	Destructuring: extract values from objects/arrays
•	Spread/rest: ... operator for copying, merging, arguments
•	Modules: import/export, named vs default exports
•	Promises, async await, Classes

## 25. Factory functions - why & difference vs normal functions
Factory function: a function that creates and returns objects, often encapsulates private state via closures.
Return Value: While a normal function might return undefined or a specific result, a factory function always returns an object instance intended for use elsewhere in the application.
```javascript
function createUser(name) {
  return {
    getName: () => name, // 'name' is trapped in a closure
    setName: (n) => {name = n;}
  };
}
const u = createUser('Pashupatinath');
console.log(u.name); // undefined (it's private!)
console.log(u.getName()); // 'Shruti'
```
Why Factory Functions?
•	Encapsulation: They allow for truly private variables (like name above) that cannot be accessed outside the function scope.
•	Simplicity: No need for the new keyword or complex binding issues.
•	Flexibility: They can return any object structure and easily incorporate mixins or composition.

## 26. Polyfills & Feature Detection
Supporting older browsers while using modern features. 
•	Polyfill: code that provides modern functionality in older environments
•	@supports in CSS, if (window.fetch) in JavaScript
•	Babel for transpilation, polyfill.io for automatic polyfills
•	Implementation: Custom Map, Filter using polyfills

## 27. Security Best Practices
•	XSS prevention: sanitize user input, use textContent not innerHTML
•	CSRF tokens for state-changing requests
•	Secure authentication: HTTPS, secure cookies, JWT storage
•	Content Security Policy (CSP) headers
•	Regular dependency updates for security patches

## 28. Debouncing vs Throttling
Debouncing and throttling are techniques used to optimize the performance of functions triggered by events that occur frequently, such as scrolling, resizing, or typing.
Throttling in JavaScript is a technique used to limit how often a function can be called within a specified time period, regardless of how many times the triggering event occurs.
Debouncing: Techniques to limit the number of times a function gets executed. (call function after pause)
Expected Answer: Implement rate limiting techniques for events like resize, scroll, and input. Hints:
•	Debounce: wait for pause (search input)
•	Throttle: execute at most once per period (scroll/resize)
•	Implementation with closure and setTimeout
•	Lodash _.debounce and _.throttle
```javascript
window.addEventListener('input', _.debounce(mySearchFunction, 300));
window.addEventListener('scroll', _.throttle(myScrollFunction, 100));
```

## 29. AbortController 
AbortController is a built-in JavaScript API that allows you to cancel one or more asynchronous operations, such as fetch requests, event listeners, or custom promises.
How it works: The AbortController API consists of two main parts:
•	AbortController: An object with an abort() method. Calling this method signals that associated operations should be canceled.
•	AbortSignal: A read-only property (.signal) of the controller instance. This signal is passed to asynchronous APIs (like fetch()). It has an aborted boolean property and fires an abort event when the controller's abort() method is called. 

## 30. Generator Function
A generator function in JavaScript is a special type of function that can pause its execution and resume later, producing a sequence of values on demand. and return a single value. Generators can manage asynchronous operations in a more sequential, readable way. 
•	function* syntax: Generator functions are defined using function*.
•	yield keyword: The yield keyword pauses the function's execution and produces a value to the caller. The function's state is preserved until it is resumed.
•	Generator object: When a generator function is called, it does not execute its body immediately. Instead, it returns a special generator object, which is an iterator.
•	.next() method: The generator object's .next() method is used to resume execution. Each call to .next() runs the code until the next yield statement (or return or end of function) and returns an object in the format { value: ..., done: boolean }.
	o	value: The yielded value (or the final return value when done is true).
	o	done: A boolean indicating if the generator has finished executing. 
```javascript
function* numberGenerator() {
  yield 1;
  yield 2;
  yield 3;
  return 'Finished'; 
}
const gen = numberGenerator(); // Calling function returns a generator object
console.log(gen.next()); // Output: { value: 1, done: false }
console.log(gen.next()); // Output: { value: 2, done: false }
```

## 31. Browser Parse and Render Process
Expected Answer: Understand critical rendering path from HTML to painted pixels. Hints:
•	HTML parsing → DOM tree
•	CSS parsing → CSSOM tree
•	Render tree → Layout → Paint → Composite
•	Repaint vs reflow (layout)
•	Optimize by minimizing critical resources

## 32. Client-Side Storage Options
Expected Answer: Choose appropriate storage mechanism based on data size, persistence needs, and structure. 
•	localStorage: 5-10MB, synchronous, string only
•	sessionStorage: tab lifetime
•	IndexedDB: large, structured, async, transactions
•	Cookies: 4KB, sent with requests, httpOnly for security
•	Cache API: for network responses

## 33. Browser APIs & Capabilities
Native browser API:  DOM API, Fetch API, Web storage API, Geolocation API
Capabilities: 
•	Service Workers: offline support, push notifications
•	Web Workers: background processing
•	Geolocation, Camera, Microphone access
•	Clipboard, Drag and Drop APIs
•	Intersection Observer for visibility detection

## Problem Solving & Algorithms

## 34. Common Algorithm Patterns
Expected Answer: Typical JavaScript coding interview problems. Hints:
•	Array flattening: recursion, iteration, reduce() with concat
•	Memorization
•	Inline Reverse of an array
•	String manipulation: first repeating character, palindrome check
•	Object/array transformations: grouping, frequency counting
•	Currying and function composition
•	Implementing polyfills for common methods (map, filter)

## 35. Output Prediction Questions
Expected Answer: Predicting code output considering JavaScript's quirks. Hints:
•	Event loop timing: synchronous → microtasks → macrotasks
•	this binding in different contexts
•	Closure variable capture
•	Promise chaining and error propagation
•	Type coercion with == vs ===
