# Section 7: Project Architecture, Build Tools & Deployment
# Project Architecture, Build Tools & Deployment Interview Questions

### Architecture & Project Structure

- [1. Path Aliases in Vite/tsconfig](#1-path-aliases-in-vitetsconfig)
- [2. File & Folder Structure of React + Redux Project](#2-file--folder-structure-of-react--redux-project)
- [3. Feature-Based vs Type-Based Architecture](#3-feature-based-vs-type-based-architecture)
- [4. Monorepo (Nx/Turborepo) for React Apps](#4-monorepo-nxturborepo-for-react-apps)
- [5. Micro Frontends](#5-micro-frontends)
- [6. Atomic Design Methodology](#6-atomic-design-methodology)

### Build Tools & Workflow

- [7. Vite vs Webpack](#7-vite-vs-webpack)
- [8. Build Tools & Workflow](#8-build-tools--workflow)
- [9. Deployment & DevOps](#9-deployment--devops)

## 1. Path Aliases in Vite/tsconfig
Expected Answer: Configure import shortcuts to avoid relative path hell. Hints:
```javascript
// tsconfig.json or jsconfig.json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"],
      "@components/*": ["src/components/*"],
      "@utils/*": ["src/utils/*"]
    }
  }
}
```
•	Vite: Built-in support via resolve.alias in vite.config.js
•	Webpack: Configure in webpack.config.js
•	Absolute imports improve code readability

## 2. File & Folder Structure of React + Redux Project
The project follows a feature-based, scalable architecture that groups code by domain rather than technical role. This modular approach keeps related logic, components, and tests together, making the codebase easy to navigate and maintain as it grows.
```
src/
├── index.js
├── App.jsx
├── api/
│   ├── authApi.js                # API client, axios instances
│   └── ...                       # RTK Query or API helpers
├── app/
│   └── store.js                  # redux store config (RTK)
├── features/
│   ├── auth/
│   │   ├── authSlice.js
│   │   ├── authApi.js
│   │   ├── Login.jsx
│   │   └── _tests_/
│   └── todos/
│       ├── todosSlice.js
│       ├── TodoList.jsx
│       └── components/           # (optional feature‑internal components)
├── components/                   # shared UI components (Button, Modal)
├── hooks/                        # custom hooks (useAuth, useDebounce)
├── routes/                       # route definitions & protected routes
├── utils/                        # helpers, validators
├── assets/                       # images, svgs, fonts
├── styles/                       # global css / tailwind config
└── tests/                        # cypress/e2e or integration tests
```

End to end tests are placed outside src/ at the project root, isolating them from the application code:
```
e2e/                         # or tests/e2e, cypress/, playwright/
├── specs/                   # Test scenarios (auth.spec.js, todos.spec.js)
├── fixtures/                # Test data
├── support/                 # Custom commands, global setup
└── videos/                  # Recorded runs (gitignored)
```
Key Architectural Decisions
•	Feature folders encapsulate everything for a domain (Redux slice, API, components, tests).
•	RTK Query (in authApi.js, etc.) auto-generates hooks and reduces boilerplate.
•	Shared components are highly reusable and UI only (no business logic).
•	Aliases (e.g., @/features, @/components) are configured in Webpack/Vite for clean imports.
•	Consistent naming and index.js exports simplify imports.
•	Test files live next to the code they test (_tests_ folder or .test.js suffix).
•	E2E tests are maintained separately and run in CI against a deployed preview.

## 3. Feature-Based vs Type-Based Architecture
Expected Answer: Organize code by features (auth, dashboard) vs by type (components, services). Hints:
•	Feature-based: All related files in one folder (better for large apps)
•	Type-based: Group by file type across entire app (simpler for small apps)
•	Hybrid approach: Feature folders with internal type organization
•	Choose based on team size, app complexity, maintenance needs

## 4. Monorepo (Nx/Turborepo) for React Apps
Expected Answer: Single repository containing multiple packages/apps sharing code. Hints:
•	Nx: Advanced build system with dependency graph, generators
•	Turborepo: Faster builds with intelligent caching
•	Benefits: Code sharing, consistent tooling, atomic changes
•	Challenges: Complex setup, learning curve, tooling overhead

## 5. Micro Frontends
Expected Answer: Architectural pattern to build apps as composition of independent frontend apps. Hints:
•	Multiple teams work on different parts independently
•	Integration methods: Build-time, server-side, client-side (iframe, Web Components)
•	Frameworks: Module Federation (Webpack 5), Single SPA
•	Benefits: Independent deployment, technology freedom, scalability

## 6. Atomic Design Methodology
Expected Answer: Design system methodology breaking UI into atoms, molecules, organisms, templates, pages. Hints:
•	Atoms: Basic building blocks (buttons, inputs)
•	Molecules: Groups of atoms (search form)
•	Organisms: Complex components (header with nav, search, user menu)
•	Templates: Page-level layouts
•	Pages: Specific instances with real content

## 7. Vite vs Webpack
Expected Answer: Vite offers faster dev server with ES modules, Webpack offers mature ecosystem. Hints:
•	13.1 How it Works: Vite uses native ES modules for dev, Rollup for production
•	13.2 Popular Bundlers: Webpack, Rollup, Parcel, Esbuild, Vite
•	Vite: Faster HMR, simpler config, growing ecosystem
•	Webpack: Extensive plugins, mature, more configuration

## Build Tools & Workflow

## 150. Build Tools & Workflow
Expected Answer: Configure build tools for efficient development and production builds. Hints:
•	Webpack: bundling, loaders, plugins, optimization
•	Vite: faster dev server, ES modules, simpler config
•	Babel: transpilation, polyfills, JSX transform
•	ESLint/Prettier: code quality, consistent formatting
•	Husky/lint-staged: pre-commit hooks

## 151. Deployment & DevOps
CI/CD stands for Continuous Integration and Continuous Delivery/Deployment. It's a software development practice that automates the process of building, testing, and deploying applications.
Hints:
•	CI/CD: GitHub Actions, GitLab CI, Jenkins
•	Deployment: Vercel, Netlify, AWS, Docker
•	Environment variables for configuration
•	Zero-downtime deployments, rollback strategies
•	Monitoring and alerting setup
