# React Interview Notes
## Comprehensive Study Guide

________________________________________

## Table of Contents

### SECTION 1: React Fundamentals
1. React VS Angular
2. What is React?
3. What is SPA (Single Page Application)?
4. Features of React
5. Problem with Real DOM
6. What is Virtual DOM?
7. How React/Virtual DOM Works
   7.1. Diffing Algorithm (What Changed?)
   7.2. Reconciliation

### SECTION 2: Project Architecture & Build Tools
8. Path Aliases in Vite/tsconfig
9. Feature-Based vs Type-Based Architecture
10. Monorepo (Nx/Turborepo) for React Apps
11. Micro Frontends
12. Atomic Design Methodology
13. Vite vs Webpack
   13.1. How it Works (General Steps)
   13.2. Popular Bundlers

### SECTION 3: Components & Props
14. State and Props
15. Why Direct Mutation is a Problem?
16. Stateful and Stateless Components
17. Controlled vs Uncontrolled Components
18. Class-Based vs Functional Components
   18.1. Why Functional Components Are Preferred
19. Pure Components in React
20. What is React.memo?
21. Lazy Loading
22. Styling in CSS
23. What are Keys?
24. React Strict Mode
25. Fragment in React and Uses

### SECTION 4: Hooks
26. Rules of Hooks
27. Component Lifecycle Phases
28. React Lifecycle Comparison: Class Components vs Functional Components
29. useState Hook
30. useReducer Hook
31. Context API
32. Ways to Manage State in React
33. When to use useReducer over useState
34. useCallback vs useMemo Hook
35. Custom Hook
36. Higher Order Components (HOC)
37. useEffect Hook
38. useRef Hook
39. Difference between useState and useReducer
40. Redux vs useReducer + Context API

### SECTION 5: Performance Optimization
41. How to Improve React Application Performance
42. High-Impact Optimizations
43. Medium-Impact / Supporting Techniques
44. Detailed Performance Optimization Techniques
45. React Portals
46. Error Boundaries

### SECTION 6: React 18 Features
47. Concurrent React - React 18 Interview Cheat Sheet
48. Hooks Mentioned in React 18
   A. useId
   B. useTransition
   C. useDeferredValue
   D. useSyncExternalStore
   E. useInsertionEffect
49. Key Features & Concepts
   - Automatic Batching
   - Transitions (Urgent vs Non-Urgent)
   - Suspense on the Server (Streaming)
50. Strict Mode Updates
51. Migration Steps (Syntax Change)
52. Summary for Interview

### SECTION 7: Testing
53. React Testing and Its Importance
54. Purpose of Unit Testing in React
55. Advantages of Using Jest for Unit Testing
56. Shallow Rendering and Full Rendering in React Testing
57. Test Asynchronous Code in React
58. Snapshot Testing in React
59. What to Test in React Component Test
60. Component Integration Testing Steps
61. Steps of Testing using RTL
62. Queries in React Testing Library
63. Mock Function in Jest
64. Simulate Interactions
65. Test Lifecycle Hooks
66. Test Component that uses useState
67. Testing Initial Load (API Calls) or useEffect
68. Testing Dependencies
69. Why Use Mock Props in React Testing Library?
70. Why Testing with Mock Data Similar to Component API Call/Data?
71. Jest + RTL vs Vitest + RTL

### SECTION 8: Routing
72. React Router

### Additional Concepts
73. Profilers

### SECTION 9: Routing & Authentication
74. Protected Routes in React Router v6+
75. Role-Based Authorization in Routing
76. SSO Login (OAuth2/OIDC) in React
   76.1. SSO
77. JWT
78. JWT + Refresh Token Flow
79. User Logged Out Mid-Session - Graceful Redirect
80. Route-Based Code Splitting

### SECTION 10: API Integration & Data Fetching
81. Interceptors
82. CORS: Cross Origin Resource Sharing
83. REST vs GraphQL
84. Caching & Invalidation with TanStack Query vs Apollo
85. Error Handling & Retry Strategies
86. Real-Time Updates: WebSocket vs SSE vs Polling
87. Scenario: Real-Time Dashboard

________________________________________

## SECTION 1: React Fundamentals

**1. React VS Angular**

| Aspect | React (Library) | Angular (Framework) |
|--------|-----------------|---------------------|
| Type | UI library (you add what you need) | Complete framework (batteries included) |
| Developer | Meta (Facebook) | Google |
| Primary Language | JavaScript + JSX (TypeScript optional & very common) | TypeScript (mandatory) |
| Data Binding | One-way (unidirectional) | Two-way (default) + one-way options |
| DOM Handling | Virtual DOM + reconciliation | Real DOM + change detection (optimized with Ivy) |
| Learning Curve | Easier entry (especially if you know JS) | Steeper (TypeScript, RxJS, DI, modules, decorators) |
| State Management | External (Zustand, Redux, Recoil, Context + useReducer) | Built-in (Services + RxJS) |
| Routing | External (React Router) | Built-in (Angular Router) |
| Forms | External / custom (Formik, React Hook Form) | Built-in powerful forms (template-driven / reactive) |
| Performance | Excellent for dynamic UIs (Virtual DOM + memoization + Server Components in React 19+) | Very good for large apps (AOT compilation, tree-shaking, zoneless in newer versions) |
| Bundle Size | Smaller starting point (add libraries as needed) | Larger out-of-the-box (but optimized well) |
| Tooling / CLI | Create React App / Vite (very fast) | Angular CLI (powerful, but heavier) |

**2. What is React?**
React is an open-source JavaScript library for building user interfaces (UIs), primarily for web applications. It is used to build single page applications and allows you to create reusable UI components.

**3. What is SPA (Single Page Application)?**
SPA is a web application that have only one initial webpage/HTML page (index.html).
- Loads single HTML page initially. Whenever user do some actions on website (any updates), then response content is dynamically updated without refreshing or loading a new page.
- After that initial load, all interactions happen dynamically without full page refreshes.

Why this matters:
- While there's only one physical HTML file, the application can have many logical "pages" or views (like /home, /profile, /settings) that are handled by JavaScript using the browser's History API.
- The URL changes and it feels like different pages to the user, but no new HTML file is downloaded from the server.

**4. Features of React**
a. JSX: Allows writing HTML-like syntax directly in JavaScript code within JavaScript functions.
b. Components: Build UI using reusable and independent components, enhancing code maintainability and reusability.
c. Virtual DOM: React uses Virtual DOM to minimize direct manipulation of the actual DOM, improving performance through efficient updates.
d. One Way Data Flow: Data flows unidirectionally (parent to child), making it easier to track changes and debug applications.
e. Large Community & Ecosystem: Extensive ecosystem with abundant tools, libraries, and community support resources available.

**5. Problem with Real DOM**
a. Even small changes in the application cause the entire layout to re-render
b. For example: Changing just one element (like a title tag) forces the browser to recalculate and re-render the entire DOM tree.
c. This process is very time-consuming and increases page load time. Results in slow performance and poor user experience.

**6. What is Virtual DOM?**
- Virtual DOM is a lightweight representation of the real DOM. It is an in-memory JavaScript object that represents the real DOM structure
- React uses Virtual DOM to efficiently update the user interface without re-rendering the entire page

Benefits of Virtual DOM:
- Improved Performance: Updates only the necessary parts instead of the entire page
- Batch Updates: Multiple state updates are batched into a single render cycle
- Better User Experience: Faster updates lead to smoother interactions and enhanced user satisfaction

**7. How React/Virtual DOM Works**
Step 1: When we create a React application, it creates a Virtual DOM (lightweight representation of real DOM)
Step 2: When changes occur in component state or props, or when something in your app changes. React creates a (2nd) new Virtual DOM with the updated changes. React uses virtual DOM to keep track of what needs to be updated on real DOM.
Step 3: Diffing Algorithm: React now does a "diffing" process. React compares the new Virtual DOM with the previous one to identify what changed
Step 4: Reconciliation: Once it knows the specific changes, React goes to the real DOM (the actual webpage) and, in a process called "reconciliation," it only updates those tiny, specific elements that need to change.
Step 5: It often batches multiple changes together into a single update cycle. The real DOM is updated with minimal changes in a single render cycle, making React very fast and efficient.

**7.1. Diffing Algorithm (What Changed?)**
It's the process of comparing the new Virtual DOM tree with the previous one to pinpoint the exact nodes (elements, components) that have changed.
It calculates difference between old and new virtual DOM and determine the minimum set of changes need to update of real dom.

**7.2. Reconciliation**
The process where react updates the real DOM by comparing old and new component trees. Priorities important tasks to keep the app responsive.
React Fiber: React updated its engine since (React 16) for smoother updates. Its priorities important tasks to keep the app responsive.

// Simplified example of priorities:
1. Immediate - Button clicks, text input (needs < 100ms response)
2. High - Animations, transitions
3. Low - Data fetching, background calculations

How React Fiber works:
- Splits work into small chunks (fibers).
- Can pause low-priority tasks (eg. Background loading) to focus on urgent ones (eg. Button click)
- Makes App feel faster, especially for animations.

Example: A button click updates instantly while a big list loads in the background

________________________________________

## SECTION 2: Project Architecture & Build Tools

**8. Path Aliases in Vite/tsconfig**

What are path aliases?
Path aliases allow you to create shortcuts for import paths instead of using relative paths.

Absolute Path: defines a location from the root directory independent of current directory (/ or C:)
Relative path specifies a location relative to the current folder, using syntax like ../ (parent)

Configuration:
```typescript
// vite.config.ts
import { defineConfig } from 'vite';
import path from 'path';

export default defineConfig({
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
      '@components': path.resolve(__dirname, './src/components'),
      '@utils': path.resolve(__dirname, './src/utils'),
      '@hooks': path.resolve(__dirname, './src/hooks'),
    }
  }
});

// tsconfig.json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"],
      "@components/*": ["src/components/*"],
      "@utils/*": ["src/utils/*"],
      "@hooks/*": ["src/hooks/*"]
    }
  }
}
```

Why use them:
- Cleaner imports: `import Button from '@components/Button'` instead of `import Button from '../../../components/Button'`
- Easier refactoring: Move files without updating all import paths
- Better readability: Clear indication of what you're importing
- Avoid deep nesting: No need to count ../ levels

Potential pitfalls:
1. Build tool mismatch: Vite and TypeScript configs must match or you'll get runtime vs compile-time errors
2. Jest/Testing issues: Need to configure Jest to understand aliases:
```javascript
// jest.config.js
moduleNameMapper: {
  '^@/(.*)$': '<rootDir>/src/$1',
  '^@components/(.*)$': '<rootDir>/src/components/$1'
}
```
3. ESLint import resolution: May need eslint-import-resolver-alias plugin
4. Team confusion: New developers may not know where @ points to
5. Over-aliasing: Too many aliases can be as confusing as deep paths
6. Absolute vs relative trade-off: Sometimes relative imports (./Button) are clearer for nearby files

Best practices:
- Use @ for src root only, keep it simple
- Document aliases in README
- Limit to 3-5 commonly used paths

**9. Feature-Based vs Type-Based Architecture**

Type-Based (Traditional) Architecture:
```
src/
├── components/
│   ├── Button.tsx
│   ├── Card.tsx
│   └── Modal.tsx
├── hooks/
│   ├── useAuth.tsx
│   └── useFetch.tsx
├── pages/
│   ├── Home.tsx
│   └── Profile.tsx
├── utils/
│   └── api.ts
└── store/
    └── authSlice.ts
```

Pros:
- Easy to understand for beginners and good for small apps

Cons:
- Hard to scale (files scattered across folders)
- Team conflicts (everyone touches same folders)

Feature-Based (Domain-Driven) Architecture:
```
src/
├── features/
│   ├── auth/
│   │   ├── components/
│   │   │   ├── LoginForm.tsx
│   │   │   └── SignupForm.tsx
│   │   ├── hooks/
│   │   │   └── useAuth.ts
│   │   ├── api/
│   │   │   └── authApi.ts
│   │   ├── store/
│   │   │   └── authSlice.ts
│   │   └── index.ts
│   ├── products/
│   │   ├── components/
│   │   ├── hooks/
│   │   └── api/
│   └── cart/
├── shared/
│   ├── components/  (Button, Modal)
│   ├── hooks/  (useDebounce, useLocalStorage)
│   └── utils/
└── pages/
    ├── HomePage.tsx
    └── ProductPage.tsx
```

Pros:
- Scalability: Works well for large-scale applications as features are decoupled and can grow independently without significant interference.
- Maintainability: Easier to manage, update, or remove a feature because all related code is in one place.
- Team Collaboration: Multiple developers/teams can work on different features with fewer merge conflicts.
- Discoverability: Developers can quickly find all relevant files for a specific functionality.

Cons:
- Overhead for Small Projects: Can introduce unnecessary complexity and empty folders in small applications.
- Need to identify "shared" vs "feature" code
- Requires discipline to maintain boundaries

When to choose each:
Type-Based:
- Small apps (<5 features, <20 components)
- Simple CRUD apps

Feature-Based:
- Medium to large apps (>10 features)
- Long-term maintenance expected
- Microservices/microfrontend architecture
- Apps that will scale

Hybrid Approach (Recommended):
```
src/
├── features/  (business logic)
├── shared/    (reusable utilities)
├── pages/     (route components)
└── layouts/   (app shells)
```

**10. Monorepo (Nx/Turborepo) for React Apps**

What is a monorepo?
A single repository containing multiple related projects/packages with shared dependencies and tooling.

When to use a monorepo:
1. Multiple related apps:
   - Customer app + Admin app + Marketing site
   - Web app + Mobile app (React Native)
   - Multiple related React apps sharing components, Shared business logic or utilities
2. Shared component library:
   - Design system used across multiple apps
   - Common utilities/hooks shared between projects
3. Microservices architecture:
   - Backend services + Frontend apps
   - All services need to stay in sync
4. Team structure:
   - Multiple teams working on related products, Team coordination across multiple projects
   - Need atomic commits across projects

Benefits:
- Code sharing: Easy to share components, utils, configs
- Atomic commits: Change multiple apps in single commit
- Unified versioning: All apps use same dependency versions
- Better refactoring: Change shared code and see impact across all apps
- Consistent tooling: One ESLint, Prettier, TypeScript config
- Faster CI/CD: Smart caching and parallel execution
- Easier onboarding: One repo to clone, one setup

Tools:
Nx:
- Advanced with code generation, affected commands
- Great for large enterprises

Turborepo:
- Simpler, better for small team, Excellent caching
- Easy migration from existing repos
- Faster build times with caching

Example structure:
```
monorepo/
├── apps/
│   ├── web/          (Next.js app)
│   ├── admin/        (React app)
│   └── mobile/       (React Native)
├── packages/
│   ├── ui/           (shared components)
│   ├── utils/        (shared utilities)
│   ├── config/       (eslint, tsconfig)
│   └── api-client/   (shared API layer)
├── package.json
└── turbo.json / nx.json
```

When NOT to use:
- Single small app
- No code sharing between projects
- Team unfamiliar with monorepo concepts
- No CI/CD infrastructure for caching

**11. Micro Frontends**

What are micro frontends?
Architectural style where a frontend app is decomposed into individual, semi-independent "micro apps" that can be developed, tested, and deployed independently.

Key concepts:
- Each team owns a complete feature (frontend + backend)
- Apps can use different frameworks (React, Vue, Angular)
- Independent deployment
- Runtime integration

Implementation approaches:
1. Build-time integration (NPM packages):
```javascript
// Each microfrontend published as package
import { ProductList } from '@company/products-mfe';
import { Cart } from '@company/cart-mfe';
```

2. Runtime integration (Module Federation):
```javascript
// webpack.config.js
module.exports = {
  plugins: [
    new ModuleFederationPlugin({
      name: 'host',
      remotes: {
        products: 'products@http://products.example.com/remoteEntry.js',
        cart: 'cart@http://cart.example.com/remoteEntry.js'
      }
    })
  ]
};
```

3. iframe-based:
```html
<iframe src="https://products.example.com"></iframe>
```

4. Web Components:
```javascript
// products-mfe exposes web component
<product-list theme="dark"></product-list>
```

Why it's important:
1. Team autonomy: Teams can work independently
2. Technology freedom: Use different React versions or frameworks
3. Independent deployment: Deploy features without full app rebuild
4. Scalability: Scale teams horizontally
5. Fault isolation: One MFE crashes, others keep working

Benefits:
- Faster development: Parallel team work
- Better performance: Load only needed features
- Easier maintenance: Smaller codebases per team
- Technology evolution: Migrate incrementally
- Clear ownership: Each team owns their domain

Challenges:
- Complexity: More infrastructure needed
- Shared state: Hard to manage across MFEs
- Consistency: UI/UX can diverge
- Bundle duplication: Each MFE might bundle React
- Performance overhead: Loading multiple bundles
- Testing: Integration testing more complex
- Debugging: Cross-MFE issues harder to trace

Best practices:
- Use shared design system
- Centralized routing
- Single source for auth/user data
- Version shared dependencies carefully
- Monitor performance (bundle sizes)
- Clear API contracts between MFEs

When to use:
- Large applications (>50 developers)
- Multiple teams working independently
- Different release cycles needed
- Long-term product suite
- Need to scale teams

When NOT to use:
- Small apps (<10 developers)
- Single team
- Tight feature coupling
- Simple deployment requirements

**12. Atomic Design Methodology**

What is Atomic Design?
A methodology for creating design systems with five distinct levels, inspired by chemistry.

The 5 levels:
1. Atoms (Smallest building blocks): HTML elements like buttons, input fields, labels, or color palettes.
2. Molecules (Groups of atoms): Simple, group of atoms (e.g., a search form = input field + label + button).
3. Organisms (Complex UI components): Distinct, complex components composed of groups of molecules and/or atoms, such as a header, navigation bar, or footer. (Header, ProductCard)
4. Templates (Page layouts): Page-level objects that structure organisms into a layout, focusing on the content structure rather than final content. Page-level wireframes
5. Pages (Actual instances): Specific, final instances of templates that showcase the UI with real, representative content.

Pros for production:
- Reusability: Build once, use everywhere
- Consistency: Enforced design patterns
- Scalability: Easy to add new features
- Testing: Test atoms independently
- Documentation: Clear component hierarchy
- Collaboration: Designers and devs speak same language
- Maintenance: Changes propagate automatically

Cons for production:
- Learning curve: Team needs training, not suitable for small app
- Abstraction overhead: Too many layers
- Naming challenges: What level is this component?

Modified approach for production:
Many teams use a simplified 3-level approach:
```
components/
├── primitives/   (Atoms: Button, Input, Icon)
├── components/   (Molecules/Organisms: Card, Modal, Header)
└── layouts/      (Templates: PageLayout, DashboardLayout)
```

Best practices:
1. Start small: Build atoms as needed, don't over-engineer
2. Use Storybook: Document each level
3. Version components: Semantic versioning for design system
4. Automate testing: Visual regression tests for atoms

**13. Vite vs Webpack**

Bundling in React is the process of combining an application's many individual source files and their dependencies into one or more optimized files, called "bundles" or "chunks," that the browser can efficiently load and execute.

React applications often consist of hundreds of files (JavaScript, JSX, CSS, images, etc.) that use modern features like JSX syntax or ES modules which most browsers cannot execute directly.

Without bundling the browser would have to make separate HTTP requests for every single file, leading to slow load times and poor user experience.

**13.1. How it Works (General Steps):**
The bundling process generally involves a few key steps:

Step 1: Dependency Graph Creation: The bundler starts from an entry point (e.g., your main index.js file) and analyzes all import and export statements to build a "module tree" or dependency graph, which maps out how every file in the project relates to others.

Step 2: Transpilation: The bundler uses tools (like Babel) to transform modern code (e.g., JSX or TypeScript) into standard JavaScript that all browsers can understand. Babel will provide Legacy browser support.

Step 3: Bundling: The transpiled modules are merged into a single file or a few optimized chunks.
Step 4: Optimization: The final bundles are minified and prepared for a production environment

HMR stands for Hot Module Replacement, a feature that allows developers to see changes to their code reflected in a running application instantly, without requiring a full page reload or losing the application's current state. Benefits: This significantly speeds up the development process

**13.2. Popular Bundlers**
Several tools are used for bundling React applications:

Webpack: Traditionally the most popular and widely used bundler, notably used by the now-legacy Create React App (CRA).

Vite: A modern build tool that uses Rollup for production builds and provides an extremely fast development environment with instant hot reloading.

Rollup: Often recommended for bundling libraries due to its efficient output structure.

Parcel: Known for its zero-configuration approach and support for React Server Components.

Why choose Vite over Webpack in modern React projects?

Key Differences:
1. Dev Server Speed:
Vite:
- Instant server start (no bundling in dev)
- Uses native ES modules
- Only transforms files on request
- Start time: ~100-500ms

Webpack:
- Bundles entire app before serving
- Start time: 5-30 seconds (large apps)
- Must rebuild on every change (in dev environment too)

Example:
```javascript
// Vite serves this as-is (ES modules)
import React from 'react';
import { Button } from './Button';

// Webpack bundles everything first
```

2. Hot Module Replacement (HMR):
Vite:
- Instant HMR (<50ms)
- Only updates changed module
- Maintains app state
- Fast regardless of app size

Webpack:
- HMR slows as app grows
- Often requires full reload
- Can take 1-5 seconds in large apps

3. Production Bundler:
Vite:
- Uses Rollup for production
- Better tree-shaking
- Smaller bundle sizes
- Simpler config

Webpack:
- Uses Webpack for production
- More configuration needed
- Larger bundles (generally)

4. Configuration:
Vite:
```typescript
// vite.config.ts - Simple and clean
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()], // Automatically handles HMR and Fast Refresh
  build: {
    sourcemap: true, // Generate source maps for production
  },
});
```

Webpack:
```javascript
// webpack.config.js - Complex
module.exports = {
    entry: './src/index.tsx',
    mode: 'development',
    devtool: 'eval-source-map', // Best for development speed
    devServer: {
        hot: true, // Enables HMR
    },
    plugins: ['react-refresh/babel'], // Required for Fast Refresh
    optimization: {
    usedExports: true, // Marks unused exports in the bundle
};
```

How Tree-Shaking Works in Vite:
Automatic in production (uses Rollup):
```javascript
// Bundlers will include 'Button' but shake out 'Slider', 'Card', etc.
import { Button } from '@mui/material'; 
// ❌ AVOID: Importing everything (unless needed)
import * as MUI from '@mui/material'; 
```

Dev vs Production Builds in Vite:
Development Server: During development (vite dev), Vite serves files using native ES Modules file like as component (Product.js) without bundling, relying on the browser's import system, which is incredibly fast.

Development:
- No bundling (serves ES modules directly)
- Fast HMR via native ESM
- Source maps enabled
- No minification
- Environment: mode: 'development'

Production Build: For production (vite build), Vite uses Rollup under the hood.
- Full bundling with Rollup
- Minification + tree-shaking
- Code splitting
- Optimized assets
- Environment: mode: 'production'

Build command:
```bash
# Development
vite dev

# Production
vite build
vite preview  # Preview production build
```

Environment Variables in Vite:
VITE_ prefix is required:
```bash
// .env
VITE_API_URL=https://api.example.com // Expose .env with VITE prefix
// use
apiUrl: import.meta.env.VITE_API_URL,
```

Different env files:
```
.env                # All environments
.env.local          # Local overrides (gitignored)
.env.development    # Development only
.env.production     # Production only
```

Babel/esbuild Role in Vite:
esbuild (default):
- Extremely fast (Go-based)
- Transpiles TypeScript/JSX
- Used in dev mode
- 10-100x faster than Babel

Babel (optional):
```typescript
// vite.config.ts
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [
    react({
      babel: {
        plugins: ['babel-plugin-styled-components']
      }
    })
  ]
});
```

When to use Babel:
- Need specific Babel plugins
- Custom transformations
- Legacy browser support
- Emotion/styled-components

Scenario: Slow HMR in Large App
Webpack problems:
1. Rebuilds entire dependency graph
2. HMR speed degrades with app size
3. Circular dependencies slow everything

How Vite fixes it:
1. No bundling in dev: Uses native ES modules
2. On-demand compilation: Only transforms requested files
3. Module graph optimization: Vite pre-bundles dependencies
4. Fast refresh: Only updates changed module

Example:
Large app (1000+ modules):
Webpack HMR: 3-5 seconds
Vite HMR: <50ms (60-100x faster)

Vite's approach:
```javascript
// When you save Button.tsx
// Vite only:
1. Transforms Button.tsx (esbuild)
2. Sends update to browser
3. Browser hot-swaps module

// Webpack:
1. Rebuilds dependency graph
2. Re-bundles all dependencies
3. Sends entire bundle
```

Summary - Choose Vite when:
- New projects
- Need fast dev experience
- Team values DX
- Modern browsers only
- Want simpler config

Stick with Webpack when:
- Legacy project (migration cost)
- Need specific Webpack plugins
- Complex build requirements
- IE11 support needed

________________________________________

## SECTION 3: Components & Props

**14. State and Props**

State:
- State is a built-in JavaScript object in React that holds data used by components.
- It allows a component to keep track of its data and update it when needed.
- State is mutable - it can be updated using the useState, useReducer Hook and redux state is store.
- In class-based components, state is declared using this.state
- State is used within components to track:
  - User input, API responses, Toggle states, Component-specific data

Props:
- Props is a JavaScript object used to pass data from one component to another (parent to child)
- Props are immutable - they are read-only and cannot be modified by the component that receives them
- Props allow components to be reusable and configurable.

**15. Why Direct Mutation is a Problem?**
React relies on the concept of immutability for efficient state management.
When state changes in an immutable way (by creating a copy rather than modifying the existing one), React can easily detect that the state has changed and trigger a re-render of components.
If we mutate state directly (like using array.push() or modifying object properties), React may not recognize that the state has been updated. This happens because the reference to the state object/array hasn't changed - only its internal content was modified.
This can result in a failure to re-render the component, meaning the UI may not reflect the actual state, causing bugs and inconsistencies.

**16. Stateful and Stateless Components**

Stateless Components
- Components that do not manage or maintain their own state
- They render only the props they receive
- Also called presentational components or dumb components
- Before Hooks, functional components were stateless components

Stateful Components
- Components that manage and update their own state
- With the useState Hook, functional components can become stateful
- Class-based components have been stateful in React from the beginning
- Also called smart components or container components

**17. Controlled vs Uncontrolled Components**

Controlled Components
Components where forms data is handled/managed by react state.
- The form field's value is tied to a state variable. Input value = state value, controlled via value prop.
- Changes handled via onChange handler, and update state using onChange handler.
- Used with interactive elements: text input, radio, checkbox, select dropdown, password, file upload.

Uncontrolled Components
Components that form data are handled by dom directly, accessed using refs. React doesn't manage values in state. They are faster no-re render required.

When to Use:
- Controlled components: When you need full control over form data, validation, and passing values as props
- Uncontrolled components: When you need direct DOM access/manipulation and don't need state management.

**18. Class-Based vs Functional Components**

| Aspect | Class-Based Components | Functional Components |
|--------|----------------------|----------------------|
| Definition | ES6 class extending React.Component | JavaScript function |
| State Management | this.state and this.setState() | useState, useReducer hooks |
| Access Props via | this.props | Function parameters |
| Lifecycle Methods | componentDidMount, componentDidUpdate | useEffect hook |
| Hooks Support | Not supported | Fully supported |
| Code Style | More boilerplate code | Simpler, cleaner syntax |
| State Initialization | Constructor required | Direct with useState |

**18.1. Why Functional Components Are Preferred:**
- Simpler & More Readable: Cleaner code with less boilerplate
- Better State Management: Hooks provide improved state and side effects handling
- No this Binding: Avoids confusion with this keyword
- Easier Testing & Maintenance: Straightforward logic and dependencies
- Modern Standard: React team's recommended approach with full feature support

**19. Pure Components in React**

Pure Functional Components:
- It is a component that always renders the same output for the same props and state. When no props and state changes it will give same output.
- Has no side effects (no API calls, state updates, or DOM manipulations)
- Use React.memo to prevent unnecessary re-renders when props don't change

Example:
```javascript
const MyComponent = React.memo((props) => { 
  return <div>{props.name}</div>; 
});

// Or
export default memo(MyComponent);
```

Pure Class Components:
- Extend React.PureComponent instead of React.Component
- Automatically implements shouldComponentUpdate with shallow prop/state comparison

Key Benefits:
- Performance optimization by avoiding unnecessary re-renders
- Predictable behavior with same input = same output

**20. What is React.memo?**
- React.memo is a Higher-Order Component (HOC) that makes functional components pure.
- It Prevents unnecessary re-renders when props remain the same and improves performances.

How it works:
- Memorization: Caches the component's rendered output
- Reuses cache if props haven't changed
- Shallow comparison: Compares current vs previous props
- Primitives (string, number, boolean): It compares values for Primitives data types.
- Objects/Arrays: It compares references (memory addresses), not content.

Important Consideration:
- If passing callback functions as props, use useCallback hook
- Without useCallback, new function references trigger re-renders even with React.memo.

**21. Lazy Loading**
It is a technique used to optimize the performance of application by loading components, images, and other resources when they needed, rather than loading everything upfront. It Improves initial load time and user experience.

Implementation:
- Use React's React.lazy() for code-splitting components
- Wrap lazy components with Suspense for fallback UI during loading

Syntax:
```javascript
const MyComponent = React.lazy(() => import('./MyComponent'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <MyComponent />
    </Suspense>
  );
}
```

Use Cases:
- Route-based splitting (Home, About pages)
- Conditional components (modals, tabs)
- Heavy components with complex logic

Image Lazy Loading:
```html
<img src="image.jpg" loading="lazy" alt="description" />
```

Benefits:
- Faster initial load
- Better performance
- Efficient resource usage

How React.lazy() works?
i. Initial render → Promise pending → shows "Loading…"
This returns a Promise that resolves to the component
```javascript
const LazyComponent = React.lazy(() => import('./MyComponent'));
```
ii. Returns: Promise resolves → component loads → renders actual content
```javascript
// While Promise is pending → shows fallback
// When Promise resolves → shows actual component
```
iii. Subsequent renders → uses cached component

**22. Styling in CSS**
1. Inline CSS
- Style elements directly with style attribute
- Value must be a JavaScript object
- CSS Stylesheets: Global scope, potential naming conflicts
- Example: `<div style={{ color: 'red', fontSize: '16px' }}>`

2. CSS Stylesheet
- Create separate .css file
- Import into component file
- Styles are global by default
- CSS Modules: Local scope, avoids conflicts

3. CSS Modules
- File naming: [name].module.css
- Scopes CSS to specific components
- Prevents global naming conflicts
- Classes are locally scoped
- Inline CSS: Component-specific, no external files

**23. What are Keys?**
- Key is a special string attribute used when rendering/creating lists of elements in React.
- Helps React identify which items have changed, added, or removed

Problem (Use Index as key): If we use an index as key and order of item is changed, react may associate the wrong state with wrong items, like if we delete items on row, it will assign index to another item.

When Keys are Used:
- When rendering arrays of elements using map(), ie. dynamically generating lists of components
- When list items can be reordered, added, or removed, to track elements in collections/lists
- To optimize the rendering process and improve performance

Benefits of Keys:
- To track elements and identify which items are added, modified, or removed
- Allow React to efficiently update only the changed parts of UI and improve performance.

Problems Without Keys:
- Unnecessary re-renders of all list items
- Instead of updating only changed items, React re-renders everything when key is not provided.
- Unexpected behavior and state mixing between components
- Slow performance with large lists

**24. React Strict Mode**
- React Strict Mode is a development tool that helps identify potential bugs and issues early in the development process. It does not affect production builds.
- In development mode, it intentionally invokes certain lifecycle methods and side effects (like useEffect/API calls) twice to help detect improper handling of side effects and ensure code follows React's best practices.

**25. Fragment in React and Uses**
- It is a way to group multiple elements together without adding an extra node to the DOM.
- They help avoid unnecessary DOM nodes, keep the DOM structure clean,
- It prevents unnecessary styling or layout issues of elements.
- For example, using a div (which is a block-level element with default 100% width) as a wrapper can sometimes break CSS layouts or table structures, which fragments solve by providing an invisible wrapper.

________________________________________

## SECTION 4: Hooks

**26. Rules of Hooks**
- Call hooks at the top level of your component - not inside loops, conditions, or nested functions for consistent call order to track.
- Only call hooks from React functional components or custom hooks - not from regular JavaScript functions.
- Custom hooks must start with use prefix (e.g., useLocalStorage), while HOCs (Higher-Order Components) typically start with with (e.g., withAuth)

**27. Component Lifecycle Phases**
1. Mounting - Component is being created and inserted into the DOM
   - Constructor → render → componentDidMount (class) / useEffect with empty dependencies (function)
2. Initialization - Setting up initial state and props
3. Updating - Component re-renders due to state or props changes
   - render → componentDidUpdate (class) / useEffect with dependencies (function)
4. Unmounting - Component is being removed from the DOM
   - componentWillUnmount (class) / useEffect cleanup (function)

**28. React Lifecycle Comparison: Class Components vs. Functional Components**

1. Mounting
- Class Component: componentDidMount
- Functional Component: useEffect(…, [])
- Description & When It Runs: Runs once, after the component is initially rendered and inserted into the DOM.
- Typical Use Cases: Fetching data, setting up subscriptions, timers, or performing DOM measurements.

2. Updating
- Class Component: componentDidUpdate
- Functional Component: useEffect(…, [dep1, dep2])
- Description & When It Runs: Runs after every re render caused by changes in props or state (but not on the initial mount).
- Typical Use Cases: Reacting to prop/state changes, performing side effects, or additional DOM updates.

3. Unmounting
- Class Component: componentWillUnmount
- Functional Component: Cleanup function returned from useEffect
- Description & When It Runs: Runs once, immediately before the component is removed from the DOM.
- Typical Use Cases: Cleaning up timers, cancelling fetch requests, unsubscribing from subscriptions, or removing event listeners.

4. Render
- Class Component: render() method
- Functional Component: Function body (returns JSX)
- Description & When It Runs: Runs every time the component needs to produce or update its output (on mount and every update).
- Typical Use Cases: Describing what should be shown on the screen (pure UI description).

Additional Advanced Lifecycle Methods/Hooks

Pre Render
- Class Component: getDerivedStateFromProps
- Functional Component: (No direct equivalent)
- Description: Runs before render on both mount and update. It returns an object to update the component's state based on props.
- Typical Use Cases: Rarely used; strictly for syncing state with props.

Update Optimization
- Class Component: shouldComponentUpdate
- Functional Component: React.memo (for components), useMemo, useCallback (for values/functions)
- Description: Runs before render on update. It returns true (re render) or false (skip re render).
- Typical Use Cases: Performance optimization by preventing unnecessary re renders.

Layout/Sync Effects
- Class Component: (N/A)
- Functional Component: useLayoutEffect
- Description: Identical signature to useEffect, but fires synchronously after all DOM mutations and before the browser paints.
- Typical Use Cases: DOM measurements (like scrolling) or mutations that must be seen before paint.

Error Handling
- Class Component: componentDidCatch (Class Components Only)
- Functional Component: (Class Components Only)
- Description: Catches JavaScript errors in the child component tree.
- Typical Use Cases: Logging errors and displaying a fallback UI (Error Boundary).

**29. useState Hook**
```javascript
const [state, setState] = useState(initialValue);
```
- Arguments: It accepts initial state value as arguments
- Returns: Array with current state, and function to update state.
- Why use: It is used to manages local component-level state.

**30. useReducer Hook**
```javascript
const [state, dispatch] = useReducer(reducer, initialState);
```
Arguments: It accepts reducer and initialState as argument and return current state and dispatch.
- reducer – It is a pure function takes current state and an action and return new state.
  - Signature: (state, action) => newState
- initialState - Initial state value or lazy initializer function initFunction(optional)

When to use useReducer:
- Complex state logic
- Large state objects: When state is an object with many properties
- Predictable state management: When you want to centralize state update logic in a reducer function
- It is better to use in certain component tree level to manage states with context API.

Flow: Dispatch Action → Reducer → New State → Re-render
1. Dispatch Action – When user events/click events occurs, it triggers state change by calling dispatch ({type: 'ACTION_TYPE', payload: data}) with an action object containing type and optional payload.
2. Reducer Processes - Pure function takes current state and action, then returns new state based on action type through switch cases or conditional logic.
3. State Updates - React replaces the old state with the new state returned by reducer, ensuring immutable updates
4. Component Re-renders - Component and its children automatically re-render with the updated state, reflecting changes in the UI

**31. Context API**
- The context API in react is a way to share data across the component tree without having to pass props manually.
- It avoids prop drilling. No need to pass props manually at multiple component levels/trees.

How Context API Works:
1. Create Context: `const MyContext = createContext();`
2. Provide Context: Wrap components with `<MyContext.Provider value={value}>`
3. Consume Context: `const value = useContext(MyContext);`

**32. Ways to Manage State in React**
Component-Level State:
- useState Hook - Local component state

Global State:
- Context API - Built-in global state management
- useReducer + Context - Complex global state
- Third-party libraries: Redux, MobX, Recoil, Zustand

**33. When to use useReducer over useState**
- Complex state with multiple properties
- When state updates depend on previous state
- Need predictable and centralized state updates
- Managing large state objects

**34. useCallback vs useMemo Hook**

useCallback:
Purpose: Returns a memoized version of the callback function that only changes if one of the dependencies has changed. Passing callbacks to child components (esp. with React.memo). (Return: memorized function)

Example: When callback function is passed to child as a props. When child has a large list of tables or expensive calculations it will impact performance of application while continuously re-render. Now that function gets created only once when the parent first mounts. Every re-render after that? Same exact function reference gets passed down. Child sees the prop is same/identical → skips re-rendering. It just does a shallow comparison — basically a quick surface-level check.

Imagine we have got a parent component with a counter or some form input that keeps changing. Every time you type or click, the parent re-renders. Inside that parent, you've defined a simple function like addRow =>{}, And addRow function is passed down as a prop to a child component, the child is a big fancy table or list that shows thousands of rows, maybe with sorting filtering, pagination. That child is wrapped in React.memo so it doesn't re-render unless its props change.

Why it matters: In React, every render creates a new function instance if you write onClick={() => ...} or const handleClick = () => .... When you pass this new function to a child component wrapped in React.memo (or memo), the child sees it as a "new prop" → child re-renders even if nothing meaningful changed.

Signature:
```javascript
const memoizedCallback = useCallback(() => {
  // your function body
}, [dependency1, dependency2]);   // ← deps array
```

Empty Dependencies: It means with usememo/useCallback function with empty dependencies it is not dependent on any values from component scope, So, the function will be only created once when component mounts. It stays the same reference, Child doesn't re-render when parent re-renders for unrelated reasons.

useMemo:
Purpose: Memoizes the value/result of a calculation/function call. It will memorize the result of computation which is based on its props. It runs the function only when dependencies change, otherwise returns the previous cached value. Avoiding re-render by maintaining reference of objects or arrays. (Return: memorized value)

Why it matters: Useful when you have expensive computations (filtering large arrays, complex math, creating big objects) that is based with the props, parent state, that you don't want to repeat on every render when props are not changing.

Example: When child components depend on parent state and has expensive calculation based on it props coming from parent usememo in the child component to prevent that expensive calculation from being repeated on every render when props are not changed. Preventing new reference → avoids child/child-list re-renders.

The child component gets some props straight from the parent — maybe a big list of users or some data the parent is holding in state. Inside the child, you're doing a heavy calculation based on those props — like filtering thousands of items, sorting them, maybe even doing some math or formatting on each one. Every single time the parent re-renders, the child re-renders too, that same expensive calculation runs all over again, even though the props didn't change at all. That's where it starts feeling slow.

Signature:
```javascript
const memoizedValue = useMemo(() => {
  // expensive computation
  return computedResult;
}, [dependency1, dependency2]);
```

**35. Custom Hook**
Custom hooks are JavaScript functions that:
- Start with use (naming convention)
- Can call other React hooks inside them (useState, useEffect, useContext, etc.)
- Let you extract and reuse stateful logic across multiple components without duplicating code.
- Use Cases: Data Fetching, Form Handling (useForm), Local Storage, Authentication

Benefits of Custom Hooks
- Reusability — Write logic once, use it in many components.
- Abstraction / Cleaner components — Components focus on UI + what to do, not how to do it → easier to read and maintain.
- Better separation of concerns — Logic lives in one place (the hook), not scattered in components.
- Easier testing — You can test the hook logic independently (using @testing-library/react-hooks or similar), without rendering full components.
- Encapsulation — Hide implementation details (e.g., how fetching works, retry logic, caching, etc.).

Important Rules & Best Practices
1. Naming: Must start with use (e.g., useFetch, useAuth).
2. Only call hooks at the top level of the custom hook (no loops, conditions, nested functions).
3. State management:
   - Define all related state inside the hook (loading, data, error, etc.).
   - Don't let the component manage hook-related state — pass inputs (like URL, initial value) as arguments.
4. Inputs & Outputs:
   - Accept parameters (e.g., URL, options, initial value).
   - Return what the component needs (usually an object or array): { data, loading, error, refetch } or [data, { loading, error }].

**36. Higher Order Components (HOC)**
A Higher-Order Component is an advanced pattern in React (not part of the core API) for reusing component logic.

It is a component/function that:
- Takes a component as an argument (called the "wrapped" or "base" component)
- Returns a new enhanced component with added props, behavior, state, or rendering logic
- Naming convention: HOCs usually start with with (e.g., withAuth, withTheme, withData)

Use Case: Authentication/Authorization, Theme/Styling injection, data fetching, loading, Conditional Rendering

```javascript
// Basic structure
function withSomething(WrappedComponent) {
  return function EnhancedComponent(props) {
    // Add logic, props, conditions, etc.
    return <WrappedComponent {...props} extraProp="value" />;
  };
}

// Usage
const UserProfileWithAuth = withAuth(UserProfile);
```

Why Use HOCs?
They solve similar problems to custom hooks — reusing logic across components — but in a different way.

Main benefits:
- Reusing logic — e.g., data fetching, authentication checks, logging, conditional rendering
- Adding / injecting props — Pass new data or functions to the wrapped component
- Enhancing behavior — Wrap with loading spinners, error boundaries, permissions, styling, etc.
- Non-invasive — Original component stays unchanged and reusable
- Composition — You can stack multiple HOCs (though it can lead to "wrapper hell")

**37. useEffect Hook**
UseEffect Hooks: A React Hook that allows you to perform side effects in functional components.
- Side Effects: Data fetching (API calls), subscriptions, timers, manually changing the DOM, logging. (Subscriptions: like WebSockets, event emitters, or third-party libraries like RxJS)
- Execution Order: The component renders first (JSX runs), then useEffect executes after the render is complete.

Syntax
It accepts a callback function (containing the side effect) and a dependency array.
```javascript
useEffect(() => {
  // Side Effect Logic here (e.g., API call)
}, [dependencies]); // Dependency Array
```

The Dependency Array (The 4 Cases)
The behavior of the effect depends entirely on what you pass as the second argument.

Case 1: No Array Provided
Runs on every single render.
```javascript
useEffect(() => {
  console.log("I run after every render");
});
```

Case 2: Empty Array []
Runs only once after the first render (mount).
```javascript
useEffect(() => {
  console.log("I run only once when component mounts");
}, []);
```

Case 3: Props or State Values [prop, state]
Runs only when specific values state/prop change.

Lifecycle Steps:
1. First Mount: Component mounts, renders initial JSX, then executes inside useEffect hooks. (Note: The effect does not create JSX again; the component function creates JSX).
2. State/Prop Change: When props change, the component re-runs to create new JSX.
3. DOM Update: React compares previous DOM with new JSX and updates the DOM based on the new JSX (diffing).
4. Effect Execution: After the DOM update, React executes inside useEffect (if dependencies changed).

```javascript
useEffect(() => {
  console.log("I run when 'count' changes");
}, [count]);
```

Case 4: Effect Cleanup
Used for effects that require cleanup (like removing event listeners or clearing timers). The cleanup function runs before the next effect and when the component unmounts.

```javascript
useEffect(() => {
  // Setup (e.g., add event listener)
  const timer = setInterval(() => console.log('tick'), 1000);

  // Cleanup function
  return () => {
    clearInterval(timer); // Cleanup logic
  };
}, []);
```

How Cleanup Works
The cleanup function runs in two specific scenarios:
1. Before Re-render: If the dependency changes, the previous effect's cleanup runs before the new effect runs.
2. On Unmount: When the component is removed from the screen.

Note: On the very first render, the cleanup does not run because there is nothing to clean up yet.

The Lifecycle Flow of useEffect Cleanup:
1. First Render (Mount): Component mounts & renders JSX.
   - useEffect runs (Setup phase).
   - Cleanup: (No Cleanup) — Why? Because there is no previous effect to clean up.
2. Every Update (Re-render): Component re-renders (due to prop/state change).
   - Step 1: Cleanup function from Previous render runs.
   - Step 2: useEffect for the New render runs.
3. Unmount: Component is about to be removed from the screen.
   - Cleanup function runs (removes listeners/timers).
   - Effect: (No Effect) — Why? Because the component is gone.

Dependencies Guide
- Include: Props, state, context values, functions, refs, external variables used inside the effect.
- Avoid (in the array): Static values or values created/initialized inside the useEffect itself. Including these can cause infinite loops because the reference changes every render.

Infinite Loops ⚠️
This happens when the component re-renders, useEffect triggers, updates state, and causes another re-render in an endless cycle.

Cause 1: Updating State that is a Dependency
```javascript
// ❌ BAD: Infinite Loop
useEffect(() => {
  setCount(count + 1); // Updates state
}, [count]); // Dependency is that state -> Triggers effect again
```

Cause 2: Incorrect Dependencies
Forgetting to list dependencies or listing objects/arrays created inside the render (which change reference on every render). It will cause infinite loops, in each render reference will changed. To prevent indefinite loop: create/define Global Variables or wrap list with useMemo.

Cause 3: Unconditional State Updates
Calling setState directly in the component body (outside of useEffect or event handlers).
```javascript
// ❌ BAD: Runs on every render
function Component() {
  setCount(count + 1); // Causes infinite loop immediately
  return <div>{count}</div>;
}
```

Cause 4: Misuse of useMemo or useCallback
If these hooks are not correctly memoizing dependencies passed to useEffect, the effect may re-run unnecessarily.

Common useEffect Mistakes

| Mistake | Why it's bad | Fix |
|---------|--------------|-----|
| Missing Dependencies | Linter warnings; effect runs with stale data. | Add all variables used inside effect to the array. |
| Including Objects/Arrays | [ { data: 1 } ] creates a new reference every render, causing infinite loops. | Wrap in useMemo or move outside component. |
| Async Functions | useEffect(() => async () => ...) is not allowed directly. | Define async function inside the effect callback. |
| Cleanup Logic | Forgetting to unsubscribe from events/APIs causes memory leaks. | Always return a cleanup function when needed. |

**38. useRef Hook**
useRef is React hook that is used to persists values between/across component re-renders without triggering re-renders when the value changes. (ref.current = newValue)

Usage:
1. Access Dom Elements directly: We can use useRef to reference a Dom element directly, This useful when we interact with an element like focusing an input without triggering re-renders.
2. Persist values between two renders: useRef can store a mutable value that persists across renders. It updates the .current property of the ref, won't triggers re-render.

How to use useRef:
Step 1: Create Reference:
The useRef hook is called to create a reference object of DOM, which has a .current property.
```javascript
const inputRef = useRef(null);      // DOM reference
const timerRef = useRef(null);      // Timer ID storage
const counterRef = useRef(0);       // Value persistence
```

Step 2: Accessing DOM Elements:
Use inputRef to get direct access to DOM elements. Attach the ref to a DOM element to get direct access to it.
```javascript
<input ref={inputRef} type="text" />
{/* Access stored value */}
<p>Render count: {renderCountRef.current}</p>
```

Step 3: Persist value between renders:
Store mutable values that persist across re-renders without triggering re-renders.
```javascript
// Store timer ID (persists across renders)
timerRef.current = setInterval(() => {
  console.log('tick');
}, 1000);

// Count renders (doesn't cause re-render)
renderCountRef.current += 1;
```

Key Characteristics
- Mutable: Can modify .current property
- Persistent: Value survives re-renders
- Non-reactive: Changing .current does NOT trigger re-render

Use Cases:
DOM Manipulation: Focus management, Text selection, Media playback control, Animation triggers
Persisting Values: Timer/interval IDs, Previous state/props, Mutable variables, Third-party library instances (like maps)
Integration with Libraries: D3.js, Three.js, ArcGIS, Google Maps, Video players, Audio players, Charting libraries

**39. Difference between useState and useReducer**

| | useState | useReducer |
|---|---|---|
| State Complexity | Manage simple state: number, string, boolean, simple object/array | Manage complex state: object/array with multiple values |
| Update Method | Directly call the setter function to update (e.g. setCount(count + 1)) | Requires defining actions and reducer function to update states. |
| Logic Location | Logic is handled directly in the component (event handlers) | Logic is centralized in a separate reducer function (pure, predictable) |
| Boilerplate | Minimal Boilerplate of code (just declare + setter) | More Boiler plate: define reducer function + action types/objects |
| Performance | Passing as props drilling setter can cause unnecessary re-renders in nested trees cause performance issues. | dispatch is stable (never changes), safe to pass down without memoization issues |
| Use Cases | Eg. Toggling a modal, counter, simple form input, theme switch | Eg. Shopping cart (add/remove/update quantities), multi-step form wizard, complex filters/search |

**40. Redux vs useReducer + Context API**

| | Redux / Redux Toolkit | useReducer + context API |
|---|---|---|
| Type | It is a 3rd-party library for global state management | Built-in React hook |
| Scope | Global state management for the entire app | Component level or small tree level state management |
| Middleware | Supports built in middleware (Redux Thunk, Saga, RTK Query for data fetching) | No built-in middleware — you write your own async logic |
| DevTools | Integrated with Redux DevTools (time-travel, action replay, state diff) | No built-in devtools |
| Boilerplate | More boilerplate with Redux Toolkit (createSlice, createAsyncThunk) | Less boilerplate code — just Context + useReducer |
| Performance | Best for very large/frequent updates (with proper selectors + memoization) | Good enough for medium apps; can cause extra renders if not optimized |
| Use Cases | Use: Large-scale apps, complex async flows, teams that want strict patterns, need time-travel debugging, heavy data fetching/caching | Use: Medium/small apps, theming, auth user, modals, shopping cart in a section, forms spanning a few pages |

________________________________________

## SECTION 5: Performance Optimization

**41. How to Improve React Application Performance**

Quick Prioritization for improving performance:

1. Measure First → Use React Profiler + Lighthouse + Web Vitals
Always start here. Run Lighthouse in Chrome DevTools or PageSpeed Insights, enable React DevTools Profiler tab, and add the web-vitals library to production for real-user metrics (CLS, LCP, INP). → Identifies actual bottlenecks (e.g., render thrashing vs slow network vs large bundle). Don't optimize blindly.

2. Enable React Compiler + Adopt Server Components (if using Next.js)
React 19's compiler auto-memoizes most things → massive reduction in manual work and re-renders. Server Components (RSC) offload rendering to the server → dramatically smaller client bundles (often 30–70% less JS shipped) and no hydration cost for static parts. → Highest impact for modern apps in 2026; enables streaming + partial hydration.

3. Code Splitting + Lazy Loading + Eliminate Waterfalls
Use React.lazy() + Suspense, route-based splitting (Next.js App Router or React Router), and parallelize fetches (e.g., Promise.all). → Reduces initial bundle by 50%+ in many apps; fixes waterfalls (sequential loading) that kill TTI.

4. Virtualize Lists + Lazy Media
react-window / react-virtualized for long lists/tables. loading="lazy" on images + next/image (auto-optimizes format/size). → Prevents slow scrolling/jank on data-heavy UIs; huge for mobile.

5. Memoization (Compiler First, Manual as Needed)
Rely on React Compiler for automatic useMemo/useCallback/React.memo. Use manual hooks only when profiler shows issues (e.g., heavy props drilling or expensive calcs). → Over-memoizing hurts readability more than it helps in 2026.

6. Bundle Analysis + Tree Shaking + Compression + Minification
- Analyze: webpack-bundle-analyzer, rollup-plugin-visualizer, or Vite's built-in stats. Identify heavy deps (e.g., replace moment → date-fns, lodash → native).
- Tree shaking: Ensure ES modules + "sideEffects": false in package.json where safe. Modern bundlers (Vite/Rspack) do this well.
- Minification: Automatic in production builds (Terser in Webpack/Vite).
- Compression — This is done on the server/CDN when sending to the browser (frontend):
  - Static assets (JS/CSS from build): Pre-compress during build (e.g., .js.br + .js.gz files) or let CDN/server compress on-the-fly.
  - Dynamic responses (SSR HTML, API payloads): Server compresses before sending (e.g., Next.js/Vercel auto-handles).
  - Browser uncompresses automatically on receipt → no manual uncompression needed in frontend code.
  - Best in 2026: Prefer Brotli (15–25%+ better than Gzip for JS/CSS/HTML text) for static files (widely supported: 97%+ browsers). Use Gzip as fallback for dynamic/edge cases.
  - Enable via:
    - Vercel/Netlify/CDN (often default or toggle).
    - Node/Express: compression middleware with Brotli support.
    - Nginx/Apache: Configure brotli_static on; or similar.
  - → Reduces transferred size by 60–80% (e.g., 500 KB → 130–150 KB). Huge for slow networks.

7. Debounce/Throttle + Avoid Inline Cruft
Debounce inputs/scrolls (e.g., lodash.debounce or custom hook). Avoid inline styles/functions/objects in JSX (breaks memoization). Prefer CSS modules/Tailwind. → Good for event-heavy UIs but lower overall impact than the above.

**42. High-Impact Optimizations**
1. Code Splitting & Lazy Loading
Break your app into smaller chunks loaded on demand. - Use React.lazy() + Suspense for dynamic imports. - Reduces initial bundle size → faster Time to Interactive (TTI). - Essential for routes/heavy components (e.g., charts, modals).

2. React Compiler (Automatic Memoization – React 19+)
Build-time tool that automatically memoizes components, values, and functions based on data flow analysis. - Eliminates most need for manual useMemo, useCallback, React.memo. - Handles cases manual hooks can't (e.g., after early returns, conditionals). - Often 20–50% faster renders in production.

3. React.memo for Memoization
Wraps functional components to skip re-renders if props are shallow-equal (unchanged), To make component pure.

4. useCallback & useMemo Hooks
- useCallback: Memoizes functions (prevents recreation → stable refs for child props).
- useMemo: Memoizes expensive computed values.
- Avoid inline functions/objects in JSX (they recreate every render → break memoization).

5. Virtualize Long Lists
Render only visible items (e.g., infinite scroll feeds, tables with 1000+ rows). Huge impact on scroll performance. - Libraries: react-window, react-virtualized, or TanStack Table + virtualization.

**43. Medium-Impact / Supporting Techniques**
1. Lazy Load Media (Images, etc.)
- Use loading="lazy" on `<img>` (native browser support).
- For advanced: next/image (Next.js) or libraries like react-lazy-load-image-component.
- Reduces initial page weight → better LCP (Largest Contentful Paint).

2. Debouncing & Throttling
Limit expensive operations (e.g., search input, scroll/resize events). - Use lodash.debounce or custom hook.
```javascript
const debouncedSearch = useCallback(debounce(handleSearch, 300), []);
```

3. Avoid Inline Styles
- Inline styles recalculate on every render (minor hit).
- Prefer CSS classes/modules or Tailwind for better caching/performance.

4. Bundle Size Optimization
- Tree shaking (remove unused code via modern bundlers like Vite/Rspack).
- Analyze: webpack-bundle-analyzer or rollup-plugin-visualizer.
- Minify JS/CSS, enable Gzip/Brotli compression on server.
- Remove heavy deps (e.g., moment → date-fns).

5. Use CDN & Asset Optimization
- Serve static assets (JS, CSS, images) via CDN → lower latency.
- Modern formats: WebP/AVIF for images, font subsetting.
- Compression: Enable Gzip/Brotli compression for faster transfers

6. CSS Optimization
- Minify CSS.
- Remove unused rules (PurgeCSS / Tailwind's JIT).
- Combine/compress stylesheets.

**44. Detailed Performance Optimization Techniques**
1. List Virtualization (Windowing)
- Rendering large lists all at once can harm performance and consume significant memory.
- Virtualization solves this by rendering only a subset of list items currently visible in the viewport.
- As you scroll, it dynamically replaces previous items with new ones (visible items) while maintaining the visible amount. Virtualization renders only the visible items in the viewport and recycles DOM elements as the user scrolls.
- This dramatically improves scroll performance and reduces initial render time. (react-virtualized)

2. Lazy Loading Images
- Instead of loading all images at once, lazy loading defers loading until images are required or about to be displayed in the viewport.
- Typically loads a placeholder, lower-resolution, or smaller-sized thumbnail initially.
- Images are loaded only as the user scrolls the page and they come into view.
- Can be implemented using libraries like React Lazy Load or use native HTML `<img loading="lazy">`

3. Memoization
Prevent unnecessary re-renders and expensive calculations. - React.memo: Wrap components to skip re-renders if props are shallow-equal (great for pure components). - useMemo: Cache computed values (e.g., filtered lists, complex derivations). - useCallback: Memoize functions to stabilize references (prevents child re-renders when passed as props). - React Compiler (introduced in React 19), many manual memoizations become automatic—React analyzes and optimizes re-renders for you. Use it first; then fall back to (memo, usememo, usecallback) hooks only where needed.

4. Throttling and Debouncing Events
High-frequency events (scroll, resize, mousemove, input keystrokes) can trigger excessive function calls. Controls function execution to ensure it runs after a set minimum time, managing rapid invocations.
- Throttling: A function is repeatedly invoked at specified intervals. Limit calls to once every X ms (great for scroll or resize handlers)
- Debouncing: A function is invoked only after a period of inactivity, useful in scenarios like search input or window resizing. Delay execution until inactivity stops.

5. Code Splitting
- Break your large JavaScript bundle into smaller chunks loaded on demand.
- Use React.lazy() + Suspense for component-level splitting (e.g., routes, modals, heavy features).
- Tools like webpack/Vite handle route-based or dynamic import splitting automatically.
- Result: Faster initial load, better Time to Interactive (TTI), especially on mobile.

6. React Fragments
- Prevents unnecessary extra DOM elements from rendering. Avoid unnecessary wrapper `<div>` elements when returning multiple children from a component.
- Use `<React.Fragment>` (or shorthand `<>...</>`) to group elements without adding extra DOM nodes.
- This reduces DOM depth, slightly improves render speed, and helps with styling/layout (e.g., flex/grid parents).

7. Web Workers
- Executes multiple tasks in the background, reducing the load on the main thread.
- Handles long-running processes, improving overall performance and responsiveness of the application. CPU-intensive tasks (e.g., data parsing, image processing, complex calculations) from the main thread to background Web Workers. This prevents UI freezes and keeps the app responsive.

8. useTransition Hook
- Call useTransition at the top level of your component to mark specific state updates as transitions.
- When multiple state updates occur, React will group them and apply updates efficiently.
- Renders the component once after state updates instead of updating multiple times, enhancing performance.
- Mark non-urgent state updates as "transitions" so React can interrupt them for higher-priority work (e.g., user input).
- Keeps UI responsive during heavy re-renders (e.g., filtering large lists while typing). Pair with useDeferredValue for deferring values (e.g., search query).
- In React 19, async actions + transitions handle pending/error/optimistic states automatically.

Profile first — Use React DevTools Profiler + browser Performance tab to find real bottlenecks before optimizing.
React Compiler — Enable it (via babel-plugin-react-compiler) to auto-memoize most cases—reduces manual useMemo/useCallback boilerplate.

**45. React Portals**
React Portals is React methods that provide a way to render HTML outside the parent component's DOM hierarchy, that need to break out of their container's layout.

Useful for components like: modals, tooltips, popovers and dialogs

How to create React Portals:
To use a React Portal, you use the createPortal method from react-dom.
1. children: The React node (JSX, component, etc.) that you want to render.
2. domNode: The target DOM element where the children will be physically placed

First, ensure your index.html (or equivalent file) has a separate mount point for the portal content, for example:
```html
<!-- public/index.html -->
<div id="root"></div>
<div id="modal-root"></div> <!-- The target for our portal -->
</body>
```

**46. Error Boundaries**
Error boundaries are React components that catch JavaScript errors in their child component tree during rendering, in lifecycle methods, and in constructors. They log these errors and display a fallback UI instead of crashing the entire application, which prevents a poor user experience.

Limitations of Error Boundaries
Error boundaries do not catch errors in event handlers, asynchronous code, server-side rendering, or errors within the error boundary component itself. Use try/catch for event handlers and asynchronous operations.

Why Class based components for Error Boundary?
Error boundaries in React must be class components because they are the only components that have access to the specific lifecycle methods required to catch errors: static getDerivedStateFromError() and/or componentDidCatch().
- getDerivedStateFromError(error): This method is used to update the component's state after an error allowing the component to display a fallback UI (e.g., an error message) instead of the crashed component tree.
- componentDidCatch(error, info): This method is used to log error information or perform side effects (like sending error reports)

Use the Error Boundary: Wrap components that might throw errors with the Error Boundary.

________________________________________

## SECTION 6: React 18 Features

**47. Concurrent React - React 18 Interview Cheat Sheet**
Core Focus: Performance improvements and Concurrent Rendering (a new behind-the-scenes mechanism).
Goal: Prepare for React 18 interviews by understanding the "Why," "How," and the specific Hooks/APIs.

1. The Big Concept: Concurrent Rendering
Think of React as a multitasking assistant now.
React 18: March 2022
Focus: Performance improvements, updating the rendering engine, and foundation for Concurrent Rendering.

The Analogy (The Phone Call):
- React 17 (Non-Concurrent): You are calling Alice. You must finish that conversation before you can call Bob. If Alice puts you on hold for 5 minutes, you are frozen for 5 minutes.
- React 18 (Concurrent): You are calling Alice. She puts you on hold. React sees you are free, so it pauses the Alice call, lets you quickly call Bob (urgent interaction), finishes with Bob, and then resumes the Alice call.

What this means for your App: React can now interrupt, pause, or abandon a render.
- It prioritizes urgent updates (typing, clicking) over heavy background updates (rendering a long list).
- This makes the app feel significantly faster/smoother to the user.

**48. Hooks Mentioned in React 18**
- useId
- useTransition
- useDeferredValue
- useSyncExternalStore
- useInsertionEffect
- Transitions features: startTransition, useTransition

A. useId
- Problem: In Server-Side Rendering (SSR), generating unique IDs on the server can mismatch (clash) with IDs generated on the client, causing errors.
- Solution: useId generates a unique ID that is stable between server and client.
- Use Case: Connecting HTML labels to inputs (`<label htmlFor={id}>`) or for accessibility attributes.

Code:
```javascript
const id = useId();
return (
  <div>
    <label htmlFor={id}>Email</label>
    <input id={id} type="text" />
  </div>
);
```

B. useTransition
Concept: Separating UI updates into two categories (Urgent vs. Non-Urgent)
1. Urgent: Direct interactions (typing, clicking, pressing). Need immediate feedback.
2. Non-Urgent (Transitions): UI updates that can wait (filtering a large list, rendering a search result).

Returns: An array [isPending, startTransition].
- isPending: Boolean that is true while the non-urgent update is processing (great for showing a loading spinner).
- startTransition: A function used to wrap the slow state update.

Use Case: A search filter or a typeahead where typing must be fast, but the list of results can load slightly slower.

Code:
```javascript
const [isPending, startTransition] = useTransition();
const [input, setInput] = useState("");
const [list, setList] = useState([]);

const handleChange = (e) => {
  const value = e.target.value;

  // 1. Urgent Update (Typing feels instant)
  setInput(value);

  // 2. Non-Urgent Update (Filtering the list waits)
  startTransition(() => {
    setList(filterBigList(value));
  });
};

return <input value={input} onChange={handleChange} />;
```

C. useDeferredValue
Concept: A simpler alternative to useTransition. It tells React to "lag" behind a specific value.
How it works: You pass it a state (e.g., search text). It returns a "deferred" version of that text that stays one step behind. It only updates the heavy component when the user stops typing.
Use Case: Re-rendering a heavy list based on user input.

Code:
```javascript
const [text, setText] = useState("");

// deferredText waits until the user pauses typing
const deferredText = useDeferredValue(text);

return (
  <div>
    <input value={text} onChange={(e) => setText(e.target.value)} />
    {/* This heavy list only re-renders for deferredText */}
    <HeavyList data={deferredText} />
  </div>
);
```

D. useSyncExternalStore
Problem: Third-party stores (like Redux, MobX, or browser APIs) can cause "tearing" in React 18 if they update outside of React's control.
Solution: This hook allows external stores to subscribe to React updates safely. It ensures that multiple reads of the store value always return the same data, preventing UI glitches.
Use Case: Library authors creating state management libraries or subscribing to window.matchMedia.

E. useInsertionEffect
Problem: CSS-in-JS libraries (like styled-components) need to inject styles into the DOM before the browser paints the layout.
Solution: This hook runs before React modifies the DOM but after React has determined what changes to make.
Use Case: Injecting dynamic `<style>` tags. It is specifically for CSS-in-JS libraries, not typically for application developers.

**49. Key Features & Concepts**

Automatic Batching
In React:
- React 17: Only batched updates inside standard event handlers (e.g., onClick). It did not batch inside fetch, setTimeout, or promises.
- React 18: Batches everything (fetches, timeouts, promises).
- Result: Fewer re-renders, Better performance.
- Opt-out: If you need code to run immediately after a state update (e.g., measure DOM), use flushSync.

Transitions (Urgent vs. Non-Urgent)
- Urgent: Direct interactions like typing, clicking, pressing buttons. Users expect immediate feedback.
- Transitions (Non-Urgent): UI views like searching a list or rendering a chart. These can wait a few milliseconds without the user noticing.
- Difference from Debounce/setTimeout:
  - setTimeout has a fixed delay (e.g., 500ms).
  - startTransition is adaptive. It only waits as long as necessary to keep the UI responsive. If the device is fast, it happens instantly. If the device is slow, it waits.

Suspense on the Server (Streaming)
- Old Way (All or Nothing): If you rendered a page on the server, you had to load everything (slow components + fast components) before sending a single byte of HTML to the user.
- React 18 Way (Streaming): You can wrap a slow component in `<Suspense>`.
  - The server sends the HTML for the fast parts immediately.
  - It sends a placeholder (loading spinner) for the slow part.
  - When the slow part finishes, the server streams the rest of the HTML.
- Benefit: The user sees the page skeleton much faster and feels the app is snappier.

**50. Strict Mode Updates**
- Behavior: In React 18, Strict Mode simulates unmounting and remounting the component immediately after mount.
- Why? To prepare for a future feature where React preserves state (e.g., if you close a tab and come back, the component might resume where you left off).
- Interview Takeaway: Ensure your useEffect cleanup functions are robust because they will be invoked twice in development mode.

**51. Migration Steps (Syntax Change)**
To use Concurrent Features, you must update the root API.

React 17:
```javascript
import ReactDOM from 'react-dom';
ReactDOM.render(<App />, document.getElementById('root'));
```

React 18:
```javascript
import { createRoot } from 'react-dom/client';
const container = document.getElementById('root');
const root = createRoot(container);
root.render(<App />);
```

**52. Summary for Interview**
- React 18 introduces Concurrent Rendering, allowing React to interrupt heavy tasks to handle user input first.
- Automatic Batching is now enabled for async operations (promises/timeouts).
- New Hooks:
  - useId: For unique IDs.
  - useTransition & useDeferredValue: For prioritizing urgent updates over slow ones.
  - useSyncExternalStore: For reading external state safely.
  - useInsertionEffect: For CSS-in-JS libraries.
- Server Suspense allows streaming HTML so users see content faster.

________________________________________

## SECTION 7: Testing

**53. React Testing and Its Importance**
React testing is the process of verifying the behavior and functionality of React components and applications. It involves creating and executing automated tests to ensure that the code behaves as expected, and meets the requirements.

React testing is important for several reasons:
- It helps identify and prevent defects, bugs, and regressions in the code.
- It helps developers catch issues early in the development cycle, reducing the cost and time required to fix them. By having a comprehensive set of tests, developers can confidently make changes to the codebase.

**54. Purpose of Unit Testing in React**
The purpose of unit testing in React is to test individual units or components of an application in isolation to ensure they work as expected. Unit testing helps to catch errors early in the development process, promotes code quality and maintainability, and provides confidence in the codebase.

**55. Advantages of Using Jest for Unit Testing**
It is easy to use, fast execution speed, built-in mocking capabilities, support for snapshot testing, and integration with other tools like Enzyme.

**56. Shallow Rendering and Full Rendering in React Testing**
Shallow rendering only renders the current component without rendering its child components, while full rendering renders the current component along with all its child components.

**57. Test Asynchronous Code in React**
Test asynchronous code in React by using asynchronous test functions, mocking asynchronous dependencies, or using the async/await syntax.

**58. Snapshot Testing in React**
Snapshot testing is a testing technique in which a snapshot of the component's rendered output is saved and compared against future renders to ensure that the component's behavior has not changed unexpectedly.

**59. What to Test in React Component Test**
1. Render tests: Ensure that components render correctly without crashing.
2. Props handling: Test if component properly render with props.
3. User interactions: Test user actions like click, input change, event handlers working properly.
4. API Calls: Ensure API calls work correctly without failing, and behavior when data is empty, undefined, error.
5. Conditional Component rendering like in some edge cases like prop is false/true.
6. Component Integration: Testing parent child communication. The primary goal is to ensure data flows correctly between components and that they react as expected to each other's state changes or triggered events.

**60. Component Integration Testing Steps**
1. Render the Parent Component: The entire component tree is rendered within the test environment.
2. Simulate User Interaction: The test code finds the "Submit" button in the parent component and simulates a click event.
3. Verify Child Component's State: The test asserts that the child component's content or state has updated to reflect the success message.

**61. Steps of Testing using RTL**

Simple:
Define test block using test or it method which will take two arguments first description message, second callback function.
Callback function will test logic:
1. Render the component.
2. Query the DOM using Screen
3. Make assertions using expects.

Details:
1. Import necessary tools
Import render and screen from @testing-library/react, component, assertion libraries like @testing-library/jest-dom.

2. Define the test block:
Wrap logic in a test('description', () => { … }) or it(…). It/test will take two arguments first description message and second callback function.

3. Render the component
Use the render() function to create a virtual version of your component in the testing environment.

4. Locate the element (Querying)
Use screen methods to find the element you want to test.
- Priority 1: getByRole (e.g., button, heading).
- Priority 2: getByLabelText (for inputs).
- Priority 3: getByText (for paragraphs or spans).

5. Simulate user interaction (Optional)
If the test requires a click or typing, use await userEvent.click(element) or await userEvent.type(element, 'text').

6. Make the assertion
Use expect(element)… to verify the state. Common assertions include:
- toBeInTheDocument()
- toBeVisible()
- toHaveValue('some text')

7. Handle Asynchronous updates (Optional)
If you are waiting for an API call or a timer, swap getBy for await screen.findByText(…)

**62. Queries in React Testing Library**
In React Testing Library:
Synchronous queries (getBy…, queryBy, getByRole, getByTestId) are used for elements present immediately in the DOM
- getBy* → throws error if element not found
- queryBy* → returns null if element not found (useful for asserting non-existence)

Asynchronous queries (findBy…, FindByText, FindByRole, FindByTestId) and the waitFor utility are used to test elements that appear or change after an asynchronous operation.

waitFor(): wait for condition to meet, to appear in DOM.

Mistake: Forgetting await on findBy* queries

**63. Mock Function in Jest**
1. Mocks with jest.fn()
This is typically used for dependency injection, where you pass the mock as a callback to the function being tested.
- Usage: `const myMock = jest.fn();`
- Implementation: You can pass a function to provide logic: `jest.fn(x => 42 + x)`.
- Assertions: Check calls and arguments using `expect(myMock).toHaveBeenCalledWith(args)`.

2. Spying on Methods with jest.spyOn()
Use this to track or replace a method on an existing object or global (like fetch or Math) without destroying the original implementation initially.
- Usage: `jest.spyOn(object, 'methodName')`.
- Mocking Implementation: After spying, you can override the behavior: `.mockImplementation(() => 'mocked value')`.
- Restoration: You can restore the original function later with `.mockRestore()`.

3. Mocking Modules with jest.mock()
This is the standard way to handle imported dependencies like axios or local files.
- Automatic Mocking: `jest.mock('./myModule')` replaces all exports with mock functions.
- Module Factories: Pass a second argument to return a custom object:
```javascript
jest.mock('./myModule', () => ({
  myFunction: jest.fn(() => 'mocked value'),
}));
```
- Partial Mocking: Use jest.requireActual() inside the factory to mock only specific parts of a module while keeping others intact.

Mock Control Methods
Once you have a mock, you can control its response dynamically:
- Static values: `.mockReturnValue(value)` or `.mockReturnValueOnce(value)`.
- Promises/Async: `.mockResolvedValue(value)` or `.mockRejectedValue(error)`.
- Full Logic: `.mockImplementation(fn)` for more complex behavior.

To avoid "test pollution," always clean up using `jest.clearAllMocks()` or `jest.restoreAllMocks()` in an afterEach block.

**64. Simulate Interactions**
userEvent (Recommended)
- Behavior: Simulates full interactions rather than single events. For example, userEvent.type() triggers focus, keyDown, keyPress, and keyUp for every character.

fireEvent
- Behavior: A low-level wrapper around the browser's dispatchEvent API that triggers a single, specific DOM event.

**65. Test Lifecycle Hooks**
beforeAll: A function or annotation that runs exactly once before any of the tests in a specific file or block begin.
afterAll: A function or annotation that runs exactly once after all the tests in that file or block have finished.

**66. Test Component that uses useState**
1. Render the component.
2. Find the interactive element (button) and the display element (text).
3. Fire an event (click) to trigger the useState setter.
4. Assert that the DOM updated correctly.

```javascript
// Counter.test.js
import { render, screen, fireEvent } from '@testing-library/react';
import Counter from './Counter';

test('increments count when button is clicked', () => {
  render(<Counter />);
  
  const button = screen.getByText('Increment');
  const display = screen.getByTestId('count-val');

  // Initial state check
  expect(display).toHaveTextContent('0');

  // Trigger state change
  fireEvent.click(button);

  // Assert state updated in the UI
  expect(display).toHaveTextContent('1');
});
```

Custom Hooks: If your state logic is complex and reused, extract it into a custom hook and use renderHook from the React Testing Library Hooks to test the logic in isolation.

**67. Testing Initial Load (API Calls) or useEffect**
When useEffect fetches data on mount, use asynchronous queries like findBy to wait for the UI to update.

Tips: Mock your APIs data.

```javascript
// Component: Fetches a username on mount
useEffect(() => {
  fetchUser().then(data => setName(data.name));
}, []);

// Test
test('loads and displays name', async () => {
  render(<UserComponent />);
  // findBy queries wait for the element to appear (up to 1000ms by default)
  const nameElement = await screen.findByText(/john doe/i);
  expect(nameElement).toBeInTheDocument();
});
```

**68. Testing Dependencies**
If your effect runs when a prop or state changes, trigger that change in your test (e.g., by clicking a button or re-rendering with new props) and verify the outcome.
- Action: Use fireEvent or userEvent to trigger the change.
- Rerender: Use the rerender function returned by render to test effect changes based on prop updates.

**69. Why Use Mock Props in React Testing Library?**
Mock props in RTL is a common practice, to isolate and test components efficiently.
1. Isolate component testing: to make sure its behavior is not influenced by it parent component or external dependency.
2. Control over Component behavior testing: By passing mock props we can test different scenarios like empty data, loading states or error conditions.
3. Faster test Execution: Mock props eliminates the need of fetch or generate real data, which can slow down tests.
4. Avoiding side effects: When testing avoid side effects from external dependencies.
5. Easy to debug if test case failed.

**70. Why Testing with Mock Data Similar to Component API Call/Data?**
1. To test it with Real API behaviour, to test on actual conditions under which component run.
2. Accurate testing
3. Error testing like real scenarios component behaviour.

**71. Jest + RTL vs Vitest + RTL**

Jest + RTL: Traditional React testing setup
Vitest + RTL: Modern, faster alternative

1. Jest
Purpose: Full-featured JavaScript testing framework (Test framework)
Key features:
- Test runner, assertion library, and mocking built-in
- Snapshot testing
- JSDOM for browser environment simulation

```javascript
// Jest example
test('adds 1 + 2 to equal 3', () => {
  expect(1 + 2).toBe(3);
});
```

2. Vitest
Purpose: Modern, Vite-native test runner (Test framework)
Key features:
- Built on Vite (faster than Jest)
- Same API as Jest (mostly compatible)
- Native ES modules support
- Built-in TypeScript support

```javascript
import { describe, it, expect } from 'vitest';

describe('sum', () => {
  it('adds numbers', () => {
    expect(1 + 2).toBe(3);
  });
});
```

3. React Testing Library (RTL)
Purpose: DOM testing utilities for React components (Testing utilities)
Key features:
- Encourages testing like a user would interact
- DOM queries (getBy, findBy, queryBy)
- User event simulation

________________________________________

## SECTION 8: Routing

**72. React Router**
React Router is a library for routing in React applications. It enables navigation between different components in react.

How React Router Works?
1. Browser Router:
`<BrowserRouter>` is the main wrapper component that enables routing. It provides the context for routing and keep track of the history ie. url of application

2. Routes:
`<Routes>` component is the container for defining all the routes in your application.
Note: `<Switch>` was there before react router v6.
It ensures first matching `<Route>` is rendered. Switch also do same like render first matching route.

3. Route:
`<Route>` defines a path and the component to render when the path matches the current URL.

________________________________________

## Additional Concepts

**73. Profilers**
React Profiler is a tool for measuring the performance of React components and identifying performance bottlenecks.

________________________________________

## SECTION 9: Routing & Authentication

**74. Protected Routes in React Router v6+**

What are protected routes?
Routes that require authentication/authorization before allowing access.

Implementation Steps using React Router v6+:
1. Create an Authentication Context/State: First, establish a way to track the user's authentication status (e.g., a state variable or a context API).

2. Create the ProtectedRoute Component: This component acts as a wrapper that contains the logic to check the authentication status.

```jsx
// ProtectedRoute.jsx
import { Navigate, Outlet } from 'react-router-dom';

const ProtectedRoute = ({ isAuthenticated }) => {
  return isAuthenticated ? <Outlet /> : <Navigate to="/login" replace />;
};
export default ProtectedRoute;
```

2.1. : Renders the child routes if the user is authenticated.
2.1. : Redirects the user to the login page if not authenticated, and the replace prop prevents the protected route from being saved in the browser history.

3. Integrate the Component into Your Router Setup: Wrap the routes you want to protect within a parent Route that uses the ProtectedRoute component as its element.

```jsx
// Example in App.js or a separate AuthContext.js
import { useState } from 'react';

function App() {
  const [isAuthenticated, setIsAuthenticated] = useState(false);
  // ... other components
}
```

Implementation:
Method 1: Protected Route Component
```typescript
// ProtectedRoute.tsx
import { Navigate, Outlet } from 'react-router-dom';
import { useAuth } from '@/hooks/useAuth';

interface ProtectedRouteProps {
  redirectPath?: string;
  children?: React.ReactNode;
}

const ProtectedRoute = ({ 
  redirectPath = '/login',
  children 
}: ProtectedRouteProps) => {
  const { isAuthenticated, isLoading } = useAuth();

  if (isLoading) {
    return <LoadingSpinner />;
  }

  if (!isAuthenticated) {
    return <Navigate to={redirectPath} replace />;
  }

  return children ? <>{children}</> : <Outlet />;
};

// App.tsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        {/* Public routes */}
        <Route path="/login" element={<LoginPage />} />
        <Route path="/signup" element={<SignupPage />} />

        {/* Protected routes */}
        <Route element={<ProtectedRoute />}>
          <Route path="/dashboard" element={<Dashboard />} />
          <Route path="/profile" element={<Profile />} />
          <Route path="/settings" element={<Settings />} />
        </Route>

        {/* 404 */}
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
}
```

Method 2: Higher-Order Component
```typescript
// withAuth.tsx
import { Navigate } from 'react-router-dom';
import { useAuth } from '@/hooks/useAuth';

export const withAuth = <P extends object>(
  Component: React.ComponentType<P>,
  requiredRole?: string
) => {
  return (props: P) => {
    const { isAuthenticated, user, isLoading } = useAuth();

    if (isLoading) return <LoadingSpinner />;
    if (!isAuthenticated) return <Navigate to="/login" replace />;
    if (requiredRole && user?.role !== requiredRole) {
      return <Navigate to="/unauthorized" replace />;
    }

    return <Component {...props} />;
  };
};

// Usage
const ProtectedDashboard = withAuth(Dashboard);
const AdminPanel = withAuth(AdminPanelComponent, 'admin');
```

Method 3: Loader-based (React Router v6.4+)
```typescript
// router.tsx
import { createBrowserRouter, redirect } from 'react-router-dom';

const checkAuth = () => {
  const token = localStorage.getItem('token');
  if (!token) {
    throw redirect('/login');
  }
  return null;
};

export const router = createBrowserRouter([
  {
    path: '/login',
    element: <LoginPage />
  },
  {
    path: '/dashboard',
    element: <Dashboard />,
    loader: checkAuth  // Runs before rendering
  }
]);
```

**75. Role-Based Authorization in Routing**

Implementation:
```typescript
// types.ts
type Role = 'user' | 'admin' | 'moderator';

interface User {
  id: string;
  email: string;
  role: Role;
  permissions: string[];
}

// RoleProtectedRoute.tsx
interface RoleProtectedRouteProps {
  allowedRoles: Role[];
  children?: React.ReactNode;
}

const RoleProtectedRoute = ({ 
  allowedRoles, 
  children 
}: RoleProtectedRouteProps) => {
  const { user, isAuthenticated, isLoading } = useAuth();

  if (isLoading) return <LoadingSpinner />;
  if (!isAuthenticated) return <Navigate to="/login" replace />;
  
  if (!user || !allowedRoles.includes(user.role)) {
    return <Navigate to="/unauthorized" replace />;
  }

  return children ? <>{children}</> : <Outlet />;
};

// App.tsx
<Routes>
  {/* Admin only */}
  <Route element={<RoleProtectedRoute allowedRoles={['admin']} />}>
    <Route path="/admin" element={<AdminPanel />} />
    <Route path="/users" element={<UserManagement />} />
  </Route>

  {/* Any authenticated user */}
  <Route element={<RoleProtectedRoute allowedRoles={['user', 'admin', 'moderator']} />}>
    <Route path="/dashboard" element={<Dashboard />} />
  </Route>
</Routes>
```

Permission-based approach:
```typescript
// PermissionProtectedRoute.tsx
interface PermissionProtectedRouteProps {
  requiredPermissions: string[];
  requireAll?: boolean;  // AND vs OR logic
}

const PermissionProtectedRoute = ({ 
  requiredPermissions,
  requireAll = true 
}: PermissionProtectedRouteProps) => {
  const { user } = useAuth();

  const hasPermission = requireAll
    ? requiredPermissions.every(p => user?.permissions.includes(p))
    : requiredPermissions.some(p => user?.permissions.includes(p));

  if (!hasPermission) {
    return <Navigate to="/unauthorized" replace />;
  }

  return <Outlet />;
};

// Usage
<Route element={<PermissionProtectedRoute requiredPermissions={['posts:write', 'posts:delete']} />}>
  <Route path="/posts/manage" element={<PostManagement />} />
</Route>
```

Granular component-level authorization:
```typescript
// Can.tsx - Authorization component
interface CanProps {
  permissions: string[];
  children: React.ReactNode;
  fallback?: React.ReactNode;
}

export const Can = ({ permissions, children, fallback = null }: CanProps) => {
  const { user } = useAuth();
  
  const hasPermission = permissions.every(p => 
    user?.permissions.includes(p)
  );

  return hasPermission ? <>{children}</> : <>{fallback}</>;
};

// Usage in components
<Can permissions={['posts:delete']}>
  <button onClick={deletePost}>Delete</button>
</Can>
```

**76. SSO Login (OAuth2/OIDC) in React**

**76.1. SSO:**
Single Sign On is a authentication method that allows users to access multiple applications and services using a single set of credentials. (username & password).

1. Authentication: When user try to login SSO enabled application (websites), it will redirected to Identity Provider (IdP)
   - The application checks if the user is already authenticated.
   - If not it will redirect to the Identity Provider (Idp) eg. Google, Okta, or MS Azure
   - User logs in: The user enter their credentials (username/ passwords) on IdP login page.

2. Token Generation: Upon IdP verifies user username/ password it creates a signed token (eg. SAML, OAuth)
   - It creates token with user Information.

3. Token Sharing: (Token sent back to Application): The Idp redirects the user back to the application with the token. And Application validates token with IdP.

4. Action Granted: Once validated the application grant user the access without requiring additional login.
   - No need to login seamless access to another application.

Benefits:
- Better User Experience
- Enhance Security: Reduce the risk of weak password
- Simplified IT management: Admin can manage user with single system credentials.

OAuth2/OIDC Flow:
1. User clicks "Login with Google/GitHub"
2. Redirect to OAuth provider
3. User authenticates
4. Provider redirects back with authorization code
5. Exchange code for tokens
6. Store tokens and user info

Implementation:
Using PKCE (Proof Key for Code Exchange) - Recommended:
```typescript
// authConfig.ts
export const oauthConfig = {
  clientId: import.meta.env.VITE_OAUTH_CLIENT_ID,
  redirectUri: `${window.location.origin}/callback`,
  authorizationEndpoint: 'https://accounts.google.com/o/oauth2/v2/auth',
  tokenEndpoint: 'https://oauth2.googleapis.com/token',
  scope: 'openid profile email',
};

// utils/pkce.ts
export const generateCodeVerifier = () => {
  const array = new Uint8Array(32);
  crypto.getRandomValues(array);
  return base64URLEncode(array);
};

export const generateCodeChallenge = async (verifier: string) => {
  const encoder = new TextEncoder();
  const data = encoder.encode(verifier);
  const hash = await crypto.subtle.digest('SHA-256', data);
  return base64URLEncode(new Uint8Array(hash));
};

// LoginPage.tsx
const handleSSOLogin = async () => {
  // 1. Generate PKCE values
  const codeVerifier = generateCodeVerifier();
  const codeChallenge = await generateCodeChallenge(codeVerifier);
  
  // 2. Store verifier for later
  sessionStorage.setItem('code_verifier', codeVerifier);
  
  // 3. Build authorization URL
  const params = new URLSearchParams({
    client_id: oauthConfig.clientId,
    redirect_uri: oauthConfig.redirectUri,
    response_type: 'code',
    scope: oauthConfig.scope,
    code_challenge: codeChallenge,
    code_challenge_method: 'S256',
    state: generateRandomState()  // CSRF protection
  });
  
  // 4. Redirect to OAuth provider
  window.location.href = `${oauthConfig.authorizationEndpoint}?${params}`;
};

// CallbackPage.tsx - Handle redirect back
const CallbackPage = () => {
  const navigate = useNavigate();
  const [searchParams] = useSearchParams();

  useEffect(() => {
    const handleCallback = async () => {
      const code = searchParams.get('code');
      const state = searchParams.get('state');
      const error = searchParams.get('error');

      if (error) {
        console.error('OAuth error:', error);
        navigate('/login?error=' + error);
        return;
      }

      if (!code) {
        navigate('/login?error=no_code');
        return;
      }

      // Verify state (CSRF protection)
      const savedState = sessionStorage.getItem('oauth_state');
      if (state !== savedState) {
        navigate('/login?error=invalid_state');
        return;
      }

      try {
        // Exchange code for tokens
        const codeVerifier = sessionStorage.getItem('code_verifier');
        const tokens = await exchangeCodeForTokens(code, codeVerifier);
        
        // Store tokens
        localStorage.setItem('access_token', tokens.access_token);
        localStorage.setItem('refresh_token', tokens.refresh_token);
        
        // Get user info
        const userInfo = await getUserInfo(tokens.access_token);
        
        // Update auth state
        setAuthState({ user: userInfo, isAuthenticated: true });
        
        // Redirect to app
        navigate('/dashboard');
      } catch (err) {
        console.error('Token exchange failed:', err);
        navigate('/login?error=exchange_failed');
      } finally {
        // Cleanup
        sessionStorage.removeItem('code_verifier');
        sessionStorage.removeItem('oauth_state');
      }
    };

    handleCallback();
  }, [searchParams, navigate]);

  return <LoadingSpinner />;
};

// api/auth.ts
const exchangeCodeForTokens = async (code: string, codeVerifier: string) => {
  const response = await fetch(oauthConfig.tokenEndpoint, {
    method: 'POST',
    headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
    body: new URLSearchParams({
      grant_type: 'authorization_code',
      code,
      client_id: oauthConfig.clientId,
      redirect_uri: oauthConfig.redirectUri,
      code_verifier: codeVerifier
    })
  });

  if (!response.ok) {
    throw new Error('Token exchange failed');
  }

  return response.json();
};
```

Using a library (Recommended for production):
```typescript
// Using @auth0/auth0-react
import { Auth0Provider, useAuth0 } from '@auth0/auth0-react';

// index.tsx
<Auth0Provider
  domain={import.meta.env.VITE_AUTH0_DOMAIN}
  clientId={import.meta.env.VITE_AUTH0_CLIENT_ID}
  authorizationParams={{
    redirect_uri: window.location.origin
  }}
>
  <App />
</Auth0Provider>

// LoginPage.tsx
const LoginPage = () => {
  const { loginWithRedirect } = useAuth0();
  
  return (
    <button onClick={() => loginWithRedirect()}>
      Login with SSO
    </button>
  );
};

// ProtectedRoute.tsx
const ProtectedRoute = () => {
  const { isAuthenticated, isLoading } = useAuth0();
  
  if (isLoading) return <LoadingSpinner />;
  if (!isAuthenticated) return <Navigate to="/login" />;
  
  return <Outlet />;
};
```

**77. JWT**
JSON Web Token is a secure way of handling authentication in web react applications.
It is created by signing the base64url-encoded header and payload with a secret or private key using algorithms like HS256 or RS256.

Key Aspects of JWT Signatures
- Structure: A JWT is composed of Header.Payload.Signature. Username is stored in Payload.
- Generation: The signature is generated by Algorithm(base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)
- Verification: Upon receiving a JWT, the server recalculates the signature using the payload, header, and its secret key to verify that the token has not been altered in transit.

How JWT Works?
1. User login (post/ login): When user login with username/ password on login page Client send request to the server.
2. Server Validates: Server checks if credentials are valid, it creates a JWT containing user secrete key, information and permissions. Then server send JWT token to browser.
3. Token Storage: The react applications receives the JWT and stores it in local storage or HTTP-Cookie.
4. Client Requests: For subsequent request for protected routes, the react app includes JWT in the Authorization header.
5. Token Verification: The server verifies the JWT signature and extract user information for each request.
6. If token is valid, it send the appropriate response to the client.

**78. JWT + Refresh Token Flow**

Token Storage:
Security considerations:
- localStorage: Vulnerable to XSS, but survives page refresh
- sessionStorage: Vulnerable to XSS, lost on tab close
- httpOnly cookie: BEST - Immune to XSS, sent automatically
- Memory only: Most secure, lost on refresh

Flow for refresh token:
1. User logs in: When user login (/login) POST request, the server validates and generates two tokens.
   - Acess token: short -lived (eg, 30 mins) for Authorization in API request.
   - Refresh token: long lived token (7 days) used to obtain new access token when current one expires
   - Server send access token in response body, while the refresh token is set in an HTTP-only cookie to improve security.
2. Access token storage: Stores the access token in memory, or in local storage or session storage.
   - Access token is included in Authorization header when making API requests.
3. Refresh token storage: The refresh token is stored in a secure HTTP-only cookie, which is not accessible to JavaScript and cannot be stolen via XSS attacks.
   - This cookie is automatically sent with requests to the server when needed.
4. When access token expires: When access token expires, react app detects a 401 unauthorized error when making API request.
   - Instead of automatically logging out the user, the app automatically sends request to /refresh endpoint [refresh] including the refresh token which is stored in an HTTP cookie.
5. Server validates the refresh token: The server checks if refresh token is valid and has not expired, the server generates new access token and send back to client.
6. If the refresh token is expired user need to sign it login again. He will logout.

```javascript
// Inside your response interceptor
if (error.response.status === 401 && error.config.url === '/refresh') {
localStorage.removeItem('accessToken'); // Cleanup
navigate('/login'); // Redirect to login
}
```

Recommended approach:
```typescript
// Store refresh token in httpOnly cookie (set by backend)
// Store access token in memory or sessionStorage

interface AuthTokens {
  accessToken: string;
  refreshToken?: string;  // Only if not in httpOnly cookie
  expiresIn: number;
}

class TokenManager {
  private accessToken: string | null = null;
  private refreshToken: string | null = null;
  private expiresAt: number | null = null;

  setTokens(tokens: AuthTokens) {
    this.accessToken = tokens.accessToken;
    this.expiresAt = Date.now() + tokens.expiresIn * 1000;
    
    // Only store refresh token if backend doesn't use httpOnly cookie
    if (tokens.refreshToken) {
      this.refreshToken = tokens.refreshToken;
      // Or store in sessionStorage for persistence
      sessionStorage.setItem('refresh_token', tokens.refreshToken);
    }
  }

  getAccessToken() {
    return this.accessToken;
  }

  isAccessTokenExpired() {
    if (!this.expiresAt) return true;
    return Date.now() >= this.expiresAt - 60000; // Refresh 1 min before expiry
  }

  clearTokens() {
    this.accessToken = null;
    this.refreshToken = null;
    this.expiresAt = null;
    sessionStorage.removeItem('refresh_token');
  }
}

export const tokenManager = new TokenManager();
```

Complete flow:
```typescript
// api/client.ts
import axios from 'axios';

const api = axios.create({
  baseURL: import.meta.env.VITE_API_URL
});

// Request interceptor - Attach access token
api.interceptors.request.use(
  (config) => {
    const token = tokenManager.getAccessToken();
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error) => Promise.reject(error)
);

// Response interceptor - Handle token refresh
let isRefreshing = false;
let failedQueue: Array<{
  resolve: (value?: unknown) => void;
  reject: (reason?: unknown) => void;
}> = [];

const processQueue = (error: Error | null, token: string | null = null) => {
  failedQueue.forEach(prom => {
    if (error) {
      prom.reject(error);
    } else {
      prom.resolve(token);
    }
  });
  failedQueue = [];
};

api.interceptors.response.use(
  (response) => response,
  async (error) => {
    const originalRequest = error.config;

    // If error is not 401 or already retried, reject
    if (error.response?.status !== 401 || originalRequest._retry) {
      return Promise.reject(error);
    }

    // If already refreshing, queue this request
    if (isRefreshing) {
      return new Promise((resolve, reject) => {
        failedQueue.push({ resolve, reject });
      })
        .then(token => {
          originalRequest.headers.Authorization = `Bearer ${token}`;
          return api(originalRequest);
        })
        .catch(err => Promise.reject(err));
    }

    originalRequest._retry = true;
    isRefreshing = true;

    try {
      // Attempt token refresh
      const response = await axios.post(
        `${import.meta.env.VITE_API_URL}/auth/refresh`,
        {},
        {
          withCredentials: true  // Send httpOnly cookie
        }
      );

      const { accessToken, expiresIn } = response.data;
      tokenManager.setTokens({ accessToken, expiresIn });

      // Update original request and retry
      originalRequest.headers.Authorization = `Bearer ${accessToken}`;
      processQueue(null, accessToken);

      return api(originalRequest);
    } catch (refreshError) {
      processQueue(refreshError as Error, null);
      tokenManager.clearTokens();
      window.location.href = '/login';
      return Promise.reject(refreshError);
    } finally {
      isRefreshing = false;
    }
  }
);

export default api;
```

Login flow:
```typescript
// authService.ts
export const login = async (email: string, password: string) => {
  const response = await axios.post(
    `${import.meta.env.VITE_API_URL}/auth/login`,
    { email, password },
    { withCredentials: true }  // Receive httpOnly cookie
  );

  const { accessToken, expiresIn, user } = response.data;
  
  tokenManager.setTokens({ accessToken, expiresIn });
  
  return user;
};

export const logout = async () => {
  try {
    await axios.post(
      `${import.meta.env.VITE_API_URL}/auth/logout`,
      {},
      { withCredentials: true }
    );
  } finally {
    tokenManager.clearTokens();
  }
};
```

Secure storage summary:

| Method | XSS Vulnerable | CSRF Vulnerable | Persists | Best For |
|--------|----------------|-----------------|----------|----------|
| localStorage | ✅ Yes | ❌ No | ✅ Yes | Not recommended |
| sessionStorage | ✅ Yes | ❌ No | Tab only | Short sessions |
| httpOnly cookie | ❌ No | ✅ Yes (use CSRF tokens) | ✅ Yes | Refresh tokens |
| Memory only | ❌ No | ❌ No | ❌ No | Access tokens |

Best practice:
- Access token: Memory/sessionStorage (short-lived, 15 min)
- Refresh token: httpOnly cookie (long-lived, 7 days)
- Use CSRF tokens with cookies
- Implement token rotation

**79. User Logged Out Mid-Session - Graceful Redirect**

Scenario: User's session expires while they're actively using the app.

Implementation:
```typescript
// useAuthMonitor.ts
import { useEffect } from 'react';
import { useNavigate } from 'react-router-dom';

export const useAuthMonitor = () => {
  const navigate = useNavigate();
  const { isAuthenticated, checkAuth } = useAuth();

  useEffect(() => {
    // Check auth status periodically
    const interval = setInterval(() => {
      checkAuth().catch(() => {
        // Session expired
        handleSessionExpired();
      });
    }, 60000); // Every minute

    // Listen for 401 responses globally
    const handleUnauthorized = (event: CustomEvent) => {
      handleSessionExpired();
    };

    window.addEventListener('unauthorized', handleUnauthorized as EventListener);

    return () => {
      clearInterval(interval);
      window.removeEventListener('unauthorized', handleUnauthorized as EventListener);
    };
  }, []);

  const handleSessionExpired = () => {
    // Show toast notification
    toast.error('Your session has expired. Please log in again.');
    
    // Save current location for redirect after login
    const currentPath = window.location.pathname + window.location.search;
    sessionStorage.setItem('redirect_after_login', currentPath);
    
    // Clear auth state
    logout();
    
    // Redirect to login
    navigate('/login', { replace: true });
  };
};

// api/client.ts - Emit event on 401
api.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response?.status === 401) {
      window.dispatchEvent(new CustomEvent('unauthorized'));
    }
    return Promise.reject(error);
  }
);

// App.tsx
function App() {
  useAuthMonitor();  // Monitor auth status globally
  
  return <Routes>...</Routes>;
}

// LoginPage.tsx - Redirect after login
const LoginPage = () => {
  const navigate = useNavigate();
  
  const handleLogin = async (credentials) => {
    await login(credentials);
    
    // Check for saved redirect path
    const redirectPath = sessionStorage.getItem('redirect_after_login');
    sessionStorage.removeItem('redirect_after_login');
    
    navigate(redirectPath || '/dashboard', { replace: true });
  };
  
  return <LoginForm onSubmit={handleLogin} />;
};
```

Advanced: Modal instead of full redirect
```typescript
const SessionExpiredModal = () => {
  const [isOpen, setIsOpen] = useState(false);
  
  useEffect(() => {
    const handler = () => setIsOpen(true);
    window.addEventListener('session-expired', handler);
    return () => window.removeEventListener('session-expired', handler);
  }, []);
  
  const handleRelogin = () => {
    // Open login modal or redirect
    window.location.href = '/login';
  };
  
  return (
    <Modal open={isOpen} onClose={() => {}}>
      <h2>Session Expired</h2>
      <p>Your session has expired. Please log in again to continue.</p>
      <button onClick={handleRelogin}>Log In</button>
    </Modal>
  );
};
```

**80. Route-Based Code Splitting**

Why code split?
- Reduce initial bundle size
- Faster first page load
- Load routes on demand

Implementation:
```typescript
// routes/index.tsx
import { lazy, Suspense } from 'react';
import { createBrowserRouter } from 'react-router-dom';

// Lazy load route components
const HomePage = lazy(() => import('@/pages/HomePage'));
const Dashboard = lazy(() => import('@/pages/Dashboard'));
const ProfilePage = lazy(() => import('@/pages/ProfilePage'));
const AdminPanel = lazy(() => import('@/pages/AdminPanel'));

// Loading component
const RouteLoadingFallback = () => (
  <div className="loading-container">
    <Spinner />
    <p>Loading...</p>
  </div>
);

export const router = createBrowserRouter([
  {
    path: '/',
    element: (
      <Suspense fallback={<RouteLoadingFallback />}>
        <HomePage />
      </Suspense>
    )
  },
  {
    path: '/dashboard',
    element: (
      <Suspense fallback={<RouteLoadingFallback />}>
        <Dashboard />
      </Suspense>
    )
  },
  {
    path: '/profile',
    element: (
      <Suspense fallback={<RouteLoadingFallback />}>
        <ProfilePage />
      </Suspense>
    )
  }
]);

// OR with layout wrapper
const AppLayout = ({ children }: { children: React.ReactNode }) => (
  <div>
    <Header />
    <Suspense fallback={<RouteLoadingFallback />}>
      {children}
    </Suspense>
    <Footer />
  </div>
);

export const routes = (
  <Routes>
    <Route element={<AppLayout />}>
      <Route path="/" element={<HomePage />} />
      <Route path="/dashboard" element={<Dashboard />} />
      <Route path="/profile" element={<ProfilePage />} />
    </Route>
  </Routes>
);
```

Prefetching routes:
```typescript
// Prefetch on link hover
const PrefetchLink = ({ to, children }: { to: string; children: React.ReactNode }) => {
  const handleMouseEnter = () => {
    // Dynamically import the component
    import(`@/pages/${to}Page`);
  };
  
  return (
    <Link to={to} onMouseEnter={handleMouseEnter}>
      {children}
    </Link>
  );
};
```

Analyzing bundle splits:
```bash
# Build and analyze
npm run build
npx vite-bundle-visualizer
```

Webpack magic comments (for Webpack):
```javascript
const Dashboard = lazy(() => 
  import(
    /* webpackChunkName: "dashboard" */
    /* webpackPrefetch: true */
    '@/pages/Dashboard'
  )
);
```

Best practices:
- Split by route (most common)
- Shared components in main bundle
- Lazy load heavy features (charts, editors)
- Show meaningful loading states
- Error boundaries for failed chunk loads

```typescript
// Error boundary for chunk load failures
class ChunkLoadError extends Error {
  constructor(message: string) {
    super(message);
    this.name = 'ChunkLoadError';
  }
}

const RouteErrorBoundary = () => {
  return (
    <div>
      <h2>Failed to load page</h2>
      <p>Please refresh the page to try again.</p>
      <button onClick={() => window.location.reload()}>
        Refresh
      </button>
    </div>
  );
};
```

________________________________________

## SECTION 10: API Integration & Data Fetching

**81. Interceptors:**
Interceptors are functions that are used to modify or intercept HTTP requests or response before they are handled.

In axios there are two interceptors:
1. Request Interceptors: They are functions that run before the HTTP request is sent. Use to modify requests, like adding authorization token, modifying header
2. Response Interceptors: These run after the response is received before handling by user. Use to handle errors globally, modify the response data, or manage loading states.

**82. CORS: Cross origin resource sharing**
CORS is a security feature that allows or rejects web applications hosted on one domain to make requests like API calls to resources hosted on different domain.

Scenerio:
- We have frontend react app hosted on example.com and an API on example-api.com
- When example.com tries to make request to api-example.com the browser will check CORS header to see if it is allowed.
- The server should include a special HTTP Headers like (Access – control- allow – origin) in the response to indicate which origin resources to be permitted to access its resources.
Like: Access – control-Allow-Orign , https: api-example.com for all origin
Benefits: Prevents malicious websites for making unauthorized request to API.

Cross-Site Request Forgery (CSRF) is a type of cyber attack that occurs when a malicious website, email, link cause a user's web browser to perform an unwanted action on a trusted site when the user is authenticated.

How a CSRF Attack Works?
1. User Authorization: The user login to trusted website eg. Bank.com and receives session cookie and it is active.
2. Malicious Action: Then user visits a malicious website or clicks malicious links while he is till authenticated on the trusted site and session is active.
3. Automatic Action: Malicious sites (attacker) forces the users browser to send a request (eg, POST/ transfer) to bank.com Browser automatically includes the session cookie with the request.
4. Unauthorized Action: It can leas unauthorized actions like transfer money, password changes, etc.

Prevent It:
1. Use CSRF tokens: generate a unique token for each user session, include token in every API request.
2. Use samesite cookies, set cookies with SameSite = Strict
3. Requires user password to re- enter or user Authentication for critical actions.

XSS (Cross Site Scripting):
XSS is a security vulnerability that allows attackers to inject malicious scripts into web pages viewed by users. These scripts can steal sensitive data or manipulate web content.

Scenerio:
When a user visits shop.com and use search bar "laptop" to search laptop. Then submit search laptop by clicking search button. The attacker will entered any scripts, or can inject sql queries to manipulate data.

Prevent XSS:
- Validate & filter user input
- Use HTTP Only cookies
- Avoid innerHTML instead use textContent in JS to prevent script execution
- Validation of input field.

**83. REST vs GraphQL**

When to choose each:
REST (REpresentational State Transfer)
Choose REST when:
- Simple CRUD operations
- Public APIs (easier for third parties)
- Caching is critical (HTTP caching works great)
- Team familiar with REST
- Microservices with clear boundaries
- File uploads/downloads

Pros:
- Simple and well-understood
- HTTP caching built-in
- Tooling everywhere
- Stateless
- Easy to version (v1, v2)

Cons:
- Over-fetching (get unnecessary data)
- Under-fetching (need multiple requests)
- No type safety by default
- Versioning can be complex

Example:
```javascript
// REST - Multiple requests for related data
const user = await fetch('/api/users/123');
const posts = await fetch('/api/users/123/posts');
const comments = await fetch('/api/posts/456/comments');
```

GraphQL
Choose GraphQL when:
- Complex data relationships
- Mobile apps (minimize data transfer)
- Rapid frontend iteration
- Need precise data fetching
- Real-time subscriptions
- Microservices aggregation

Pros:
- Fetch exactly what you need
- Single request for complex data
- Strong typing (schema)
- Great tooling (Apollo, Relay)
- Real-time subscriptions
- Introspection

Cons:
- Steeper learning curve
- Caching more complex
- Can be overkill for simple apps
- Backend complexity
- N+1 query problem
- Rate limiting harder

Example:
```javascript
// GraphQL - Single request for all data
const query = gql`
  query GetUserData($id: ID!) {
    user(id: $id) {
      name
      email
      posts {
        title
        comments {
          text
          author {
            name
          }
        }
      }
    }
  }
`;
```

Comparison:

| Aspect | REST | GraphQL |
|--------|------|---------|
| Data Fetching | Multiple endpoints | Single endpoint |
| Over/Under Fetching | Common | Rare |
| Caching | HTTP cache | Custom logic needed |
| Learning Curve | Easy | Medium |
| Versioning | URL-based | Schema evolution |
| Real-time | WebSockets/SSE | Built-in subscriptions |
| File Upload | Native | Requires extra setup |

**84. Caching & Invalidation with TanStack Query vs Apollo**

TanStack Query (React Query)

Setup:
```typescript
// main.tsx
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { ReactQueryDevtools } from '@tanstack/react-query-devtools';

const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 5 * 60 * 1000, // 5 minutes
      gcTime: 10 * 60 * 1000,   // 10 minutes (formerly cacheTime)
      retry: 3,
      refetchOnWindowFocus: false,
    },
  },
});

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <AppContent />
      <ReactQueryDevtools initialIsOpen={false} />
    </QueryClientProvider>
  );
}
```

Basic usage:
```typescript
// hooks/useProducts.ts
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';

// Fetch products
export const useProducts = () => {
  return useQuery({
    queryKey: ['products'],
    queryFn: async () => {
      const res = await fetch('/api/products');
      if (!res.ok) throw new Error('Failed to fetch');
      return res.json();
    },
    staleTime: 5 * 60 * 1000, // Consider data fresh for 5 min
  });
};

// Create product
export const useCreateProduct = () => {
  const queryClient = useQueryClient();
  
  return useMutation({
    mutationFn: async (newProduct: Product) => {
      const res = await fetch('/api/products', {
        method: 'POST',
        body: JSON.stringify(newProduct),
      });
      return res.json();
    },
    onSuccess: () => {
      // Invalidate and refetch products
      queryClient.invalidateQueries({ queryKey: ['products'] });
    },
  });
};

// Component
const ProductList = () => {
  const { data, isLoading, error } = useProducts();
  const createMutation = useCreateProduct();
  
  if (isLoading) return <Spinner />;
  if (error) return <Error error={error} />;
  
  return <div>{data.map(product => ...)}</div>;
};
```

Cache invalidation strategies:
```typescript
// 1. Invalidate specific query
queryClient.invalidateQueries({ queryKey: ['products'] });

// 2. Invalidate multiple related queries
queryClient.invalidateQueries({ queryKey: ['products'] });
queryClient.invalidateQueries({ queryKey: ['categories'] });

// 3. Optimistic updates
const useUpdateProduct = () => {
  const queryClient = useQueryClient();
  
  return useMutation({
    mutationFn: updateProduct,
    onMutate: async (updatedProduct) => {
      // Cancel outgoing refetches
      await queryClient.cancelQueries({ queryKey: ['products'] });
      
      // Snapshot previous value
      const previousProducts = queryClient.getQueryData(['products']);
      
      // Optimistically update
      queryClient.setQueryData(['products'], (old: Product[]) =>
        old.map(p => p.id === updatedProduct.id ? updatedProduct : p)
      );
      
      return { previousProducts };
    },
    onError: (err, updatedProduct, context) => {
      // Rollback on error
      queryClient.setQueryData(['products'], context.previousProducts);
    },
    onSettled: () => {
      // Always refetch after error or success
      queryClient.invalidateQueries({ queryKey: ['products'] });
    },
  });
};

// 4. Manual cache update (no refetch)
queryClient.setQueryData(['products'], (old: Product[]) => {
  return [...old, newProduct];
});

// 5. Prefetching
const prefetchProduct = (id: string) => {
  queryClient.prefetchQuery({
    queryKey: ['product', id],
    queryFn: () => fetchProduct(id),
  });
};
```

Apollo Client (GraphQL)

Setup:
```typescript
// apolloClient.ts
import { ApolloClient, InMemoryCache, ApolloProvider } from '@apollo/client';

const client = new ApolloClient({
  uri: 'https://api.example.com/graphql',
  cache: new InMemoryCache({
    typePolicies: {
      Query: {
        fields: {
          products: {
            merge(existing, incoming) {
              return incoming;
            },
          },
        },
      },
    },
  }),
});

// App.tsx
<ApolloProvider client={client}>
  <App />
</ApolloProvider>
```

Usage:
```typescript
import { useQuery, useMutation, gql } from '@apollo/client';

const GET_PRODUCTS = gql`
  query GetProducts {
    products {
      id
      name
      price
    }
  }
`;

const CREATE_PRODUCT = gql`
  mutation CreateProduct($input: ProductInput!) {
    createProduct(input: $input) {
      id
      name
      price
    }
  }
`;

const ProductList = () => {
  const { data, loading, error } = useQuery(GET_PRODUCTS);
  const [createProduct] = useMutation(CREATE_PRODUCT, {
    refetchQueries: [{ query: GET_PRODUCTS }],
    // Or manually update cache
    update(cache, { data: { createProduct } }) {
      cache.modify({
        fields: {
          products(existing = []) {
            return [...existing, createProduct];
          },
        },
      });
    },
  });
  
  if (loading) return <Spinner />;
  if (error) return <Error error={error} />;
  
  return <div>{data.products.map(product => ...)}</div>;
};
```

Comparison:

| Feature | TanStack Query | Apollo Client |
|---------|----------------|---------------|
| Use Case | REST APIs | GraphQL |
| Bundle Size | ~13KB | ~33KB |
| Learning Curve | Easier | Steeper |
| Caching | Automatic | Normalized cache |
| Devtools | Excellent | Excellent |
| Offline Support | Via plugins | Built-in |
| Subscriptions | Manual | Built-in |

**85. Error Handling & Retry Strategies**

TanStack Query error handling:
```typescript
const useProducts = () => {
  return useQuery({
    queryKey: ['products'],
    queryFn: fetchProducts,
    retry: (failureCount, error) => {
      // Don't retry on 404
      if (error.status === 404) return false;
      // Retry up to 3 times
      return failureCount < 3;
    },
    retryDelay: (attemptIndex) => {
      // Exponential backoff: 1s, 2s, 4s
      return Math.min(1000 * 2 ** attemptIndex, 30000);
    },
    onError: (error) => {
      toast.error(`Failed to load products: ${error.message}`);
    },
  });
};

// Component-level error handling
const ProductList = () => {
  const { data, error, isError, refetch } = useProducts();
  
  if (isError) {
    return (
      <ErrorBoundary>
        <div>
          <p>Error: {error.message}</p>
          <button onClick={() => refetch()}>Retry</button>
        </div>
      </ErrorBoundary>
    );
  }
  
  return <div>...</div>;
};
```

Global error boundary for API errors:
```typescript
// ErrorBoundary.tsx
class APIErrorBoundary extends React.Component<
  { children: React.ReactNode },
  { hasError: boolean; error: Error | null }
> {
  state = { hasError: false, error: null };

  static getDerivedStateFromError(error: Error) {
    return { hasError: true, error };
  }

  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    // Log to error reporting service
    console.error('API Error:', error, errorInfo);
    
    // Report to Sentry/DataDog
    Sentry.captureException(error, {
      contexts: { react: errorInfo },
    });
  }

  render() {
    if (this.state.hasError) {
      return (
        <div className="error-container">
          <h2>Something went wrong</h2>
          <p>{this.state.error?.message}</p>
          <button onClick={() => window.location.reload()}>
            Reload Page
          </button>
        </div>
      );
    }

    return this.props.children;
  }
}

// Usage
<APIErrorBoundary>
  <QueryClientProvider client={queryClient}>
    <App />
  </QueryClientProvider>
</APIErrorBoundary>
```

Axios interceptor for global error handling:
```typescript
import axios from 'axios';

api.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response) {
      // Server responded with error
      switch (error.response.status) {
        case 400:
          toast.error('Invalid request');
          break;
        case 401:
          toast.error('Unauthorized');
          window.location.href = '/login';
          break;
        case 403:
          toast.error('Forbidden');
          break;
        case 404:
          toast.error('Resource not found');
          break;
        case 500:
          toast.error('Server error. Please try again.');
          break;
        default:
          toast.error('An error occurred');
      }
    } else if (error.request) {
      // Request made but no response
      toast.error('Network error. Check your connection.');
    } else {
      // Error in request setup
      toast.error('Request failed');
    }
    
    return Promise.reject(error);
  }
);
```

**86. Real-Time Updates: WebSocket vs SSE vs Polling**

WebSocket
When to use:
- Bidirectional communication needed
- Low latency critical (gaming, chat)
- High-frequency updates

Pros:
- Full-duplex (send/receive simultaneously)
- Low latency
- Efficient for frequent updates

Cons:
- More complex to implement
- Harder to scale
- Connection management overhead

Implementation:
```typescript
// useWebSocket.ts
import { useEffect, useRef, useState } from 'react';

export const useWebSocket = (url: string) => {
  const [data, setData] = useState(null);
  const [status, setStatus] = useState<'connecting' | 'connected' | 'disconnected'>('connecting');
  const ws = useRef<WebSocket | null>(null);

  useEffect(() => {
    ws.current = new WebSocket(url);

    ws.current.onopen = () => {
      console.log('WebSocket connected');
      setStatus('connected');
    };

    ws.current.onmessage = (event) => {
      const message = JSON.parse(event.data);
      setData(message);
    };

    ws.current.onerror = (error) => {
      console.error('WebSocket error:', error);
    };

    ws.current.onclose = () => {
      console.log('WebSocket disconnected');
      setStatus('disconnected');
      // Reconnect logic
      setTimeout(() => {
        console.log('Reconnecting...');
        setStatus('connecting');
      }, 3000);
    };

    return () => {
      ws.current?.close();
    };
  }, [url]);

  const sendMessage = (message: any) => {
    if (ws.current?.readyState === WebSocket.OPEN) {
      ws.current.send(JSON.stringify(message));
    }
  };

  return { data, status, sendMessage };
};

// Usage
const Dashboard = () => {
  const { data, status, sendMessage } = useWebSocket('wss://api.example.com');
  
  return (
    <div>
      <p>Status: {status}</p>
      <p>Data: {JSON.stringify(data)}</p>
      <button onClick={() => sendMessage({ type: 'PING' })}>
        Send Message
      </button>
    </div>
  );
};
```

Server-Sent Events (SSE)
When to use:
- One-way server-to-client updates
- Simpler than WebSocket
- Automatic reconnection

Pros:
- Simpler than WebSocket
- Automatic reconnection
- Works over HTTP
- Built-in event types

Cons:
- One-way only (server → client)
- Limited browser support (IE)
- Connection limits (6 per domain)

Implementation:
```typescript
// useSSE.ts
export const useSSE = (url: string) => {
  const [data, setData] = useState(null);
  const [error, setError] = useState<Error | null>(null);

  useEffect(() => {
    const eventSource = new EventSource(url);

    eventSource.onmessage = (event) => {
      setData(JSON.parse(event.data));
    };

    eventSource.onerror = (err) => {
      console.error('SSE error:', err);
      setError(new Error('Connection failed'));
      eventSource.close();
    };

    return () => {
      eventSource.close();
    };
  }, [url]);

  return { data, error };
};

// Usage
const Notifications = () => {
  const { data } = useSSE('/api/notifications/stream');
  
  return <div>{data?.message}</div>;
};
```

Polling
When to use:
- Simple requirements
- Infrequent updates
- Compatibility critical

Pros:
- Simple to implement
- Works everywhere
- No special server setup

Cons:
- Inefficient (constant requests)
- Higher latency
- Server load

Implementation:
```typescript
// usePolling.ts
export const usePolling = (fetchFn: () => Promise<any>, interval: number = 5000) => {
  const [data, setData] = useState(null);
  const [error, setError] = useState<Error | null>(null);

  useEffect(() => {
    const poll = async () => {
      try {
        const result = await fetchFn();
        setData(result);
      } catch (err) {
        setError(err as Error);
      }
    };

    // Initial fetch
    poll();

    // Set up polling
    const intervalId = setInterval(poll, interval);

    return () => clearInterval(intervalId);
  }, [fetchFn, interval]);

  return { data, error };
};

// Usage
const Dashboard = () => {
  const { data } = usePolling(
    () => fetch('/api/dashboard/stats').then(r => r.json()),
    10000 // Poll every 10 seconds
  );
  
  return <div>{JSON.stringify(data)}</div>;
};
```

Comparison:

| Feature | WebSocket | SSE | Polling |
|---------|-----------|-----|---------|
| Direction | Bidirectional | Server → Client | Client request |
| Latency | Low | Low | High |
| Efficiency | High | Medium | Low |
| Complexity | High | Medium | Low |
| Browser Support | Modern | Modern (no IE) | All |
| Use Case | Chat, gaming | Notifications, feeds | Simple updates |

**87. Scenario: Real-Time Dashboard**

Architecture for real-time dashboard with frequent updates:
```typescript
// 1. Connection Management
const DashboardProvider = ({ children }: { children: React.ReactNode }) => {
  const [stats, setStats] = useState<DashboardStats | null>(null);
  const ws = useRef<WebSocket | null>(null);

  useEffect(() => {
    // Establish WebSocket connection
    ws.current = new WebSocket('wss://api.example.com/dashboard');

    ws.current.onmessage = (event) => {
      const update = JSON.parse(event.data);
      
      // Update state based on message type
      setStats(prev => ({
        ...prev,
        ...update
      }));
    };

    // Cleanup
    return () => ws.current?.close();
  }, []);

  return (
    <DashboardContext.Provider value={stats}>
      {children}
    </DashboardContext.Provider>
  );
};

// 2. Optimized Components (React.memo)
const StatCard = React.memo(({ title, value, trend }: StatCardProps) => {
  console.log(`Rendering ${title}`); // Debug re-renders
  
  return (
    <div className="stat-card">
      <h3>{title}</h3>
      <p>{value}</p>
      <span>{trend}%</span>
    </div>
  );
});

// 3. Selective Updates
const Dashboard = () => {
  const stats = useDashboardStats();
  
  return (
    <div className="dashboard">
      {/* Only re-renders when specific stat changes */}
      <StatCard title="Users" value={stats.users} trend={stats.usersTrend} />
      <StatCard title="Revenue" value={stats.revenue} trend={stats.revenueTrend} />
      <Chart data={stats.chartData} />
    </div>
  );
};

// 4. Throttle/Debounce Updates
const useThrottledUpdates = (data: any, delay: number = 1000) => {
  const [throttledData, setThrottledData] = useState(data);

  useEffect(() => {
    const handler = setTimeout(() => {
      setThrottledData(data);
    }, delay);

    return () => clearTimeout(handler);
  }, [data, delay]);

  return throttledData;
};

// 5. Background Sync Fallback
const useDashboardData = () => {
  const { data: wsData, status } = useWebSocket('wss://api.example.com/dashboard');
  const { data: pollData } = usePolling(fetchDashboard, 30000); // Fallback every 30s

  // Use WebSocket data if available, else polling
  return status === 'connected' ? wsData : pollData;
};
```

________________________________________

## DevOps & System Design Section

**Devops:**
CI/CD: It is a set of software development practices that aims to improves process of integrating code changes and deploying applications, making more efficient development , making more efficient development and fast

CI: Continuous Integration - The practice of merging all developers' working copies to a shared mainline several times a day.

CD: Continuous Delivery/Deployment - The practice of automatically deploying code changes to production after passing automated tests.

GitHub Actions pipeline? A: Checkout → setup Node → install → lint/test → build → deploy S3.
Q2: Docker multi-stage? A: Builder stage (Node build) → runtime (Nginx serve).
Nginx role? A: Static serving, caching headers, gzip.
Deployment strategies? A: Blue-green (zero downtime), canary (gradual).
S3 + CloudFront vs EC2? A: S3: Serverless/scalable. EC2: More control.

How it works,
Sonarqube
Testcase where it test
- Describe a CI/CD pipeline you have built for a front-end application. (Expect to discuss stages: linting, unit testing, building, linting, deployment).
- How do you handle environment-specific configurations (Dev, Staging, Production)?
How do you automate the deployment process (e.g., using GitHub Actions, Jenkins, or GitLab CI)?.

Containerization (Docker)
- What are the benefits of Dockerizing a front-end application?
- How do you optimize Docker images for front-end apps to keep them small? (Mention multi-stage builds).
- How do you serve a static frontend application using Nginx in a container

Performance & Infrastructure
- How do you configure a CDN (Content Delivery Network) to optimize asset delivery?.
- What is the role of a web server like Nginx or Apache in front-end deployments?

Monitoring & Troubleshooting
- How do you monitor the performance and errors of a front-end application in production? (Mention Sentry, cloudwatch and others).
- What steps do you take if a deployment fails in production? (Expect to discuss rollback strategies
- 

Collaboration
Explain your experience with Infrastructure as Code (IaC) tools like Terraform

**System Design**
Q1: Real-time dashboard? A: Frontend: WebSocket + TanStack store. Backend: Pub/Sub.
Q2: Scaling WebSocket? A: Horizontal scaling, sticky sessions or Redis broadcast.
Q3: Polling vs SSE vs WebSocket? A: As above trade-offs.
Q4: Large-scale state? A: RTK Query + slices.
Q5: AI search design? A: Debounced calls, streaming responses.

**Modern React (React 18/19+, Concurrent, Custom Hooks)**
Q1: Key concurrent features in React 18+? A: Automatic batching, Transitions, Suspense everywhere, useDeferredValue.
Q2: useTransition and startTransition — example? A: Mark non-urgent updates: startTransition(() => setTab(newTab)) → keeps UI responsive.
Q3: useDeferredValue for performance? A: Defers heavy re-renders (e.g., search results while typing).
Q4: Suspense for data fetching? A: Wrap components that fetch → show fallback while loading.
Q5: Automatic batching vs React 17? A: Batches all setState (even in promises/timeouts) → fewer re-renders.
Q6: Best practices for custom hooks? A: Prefix use, single responsibility, handle cleanup/errors, return object/array.
Q7: Make custom hooks testable/reusable? A: Export logic separately if needed, use @testing-library/react-hooks.
Q8: Common patterns (useFetch, useForm)? A: useFetch: { data, error, loading }. useForm: Handle values/validation/submit.
Q9: Pitfalls of custom hooks? A: Wrong deps array → stale closures. Missing cleanup → leaks.
Q10: Scenario: Search box lags UI — fix? A: useDeferredValue for results list.
Q11: What's new in React 19? (As of 2026) A: Actions, better server components (if using Next), improved Suspense.

Global error boundaries for API errors.
What are the security risks of storing tokens in localStorage vs httpOnly cookies?
