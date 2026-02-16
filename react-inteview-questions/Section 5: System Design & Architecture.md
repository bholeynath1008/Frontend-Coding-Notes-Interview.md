# Section 8: System Design & Architecture

### Architecture & State Management

- [1. Structure Large-Scale React App](#1-structure-large-scale-react-app)
- [2. Local State vs Context vs Redux](#2-local-state-vs-context-vs-redux)
- [3. Design Reusable Components](#3-design-reusable-components)
- [4. High-Performance Dashboard with Real-Time](#4-high-performance-dashboard-with-real-time)

### Performance & Optimization

- [5. Reduce Global Page Load Time](#5-reduce-global-page-load-time)
- [6. Janky CSS Animation on Mobile](#6-janky-css-animation-on-mobile)

### Migration & Dependency Management

- [7. Dependency Conflicts After Upgrade](#7-dependency-conflicts-after-upgrade)
- [8. Migrate Legacy Frontend](#8-migrate-legacy-frontend)

### Security & Debugging

- [9. Secure Sensitive Data on Client](#9-secure-sensitive-data-on-client)
- [10. Debug Inconsistent UI Across Browsers](#10-debug-inconsistent-ui-across-browsers)

### Production & Testing

- [11. Critical UI Failure During Peak Traffic](#11-critical-ui-failure-during-peak-traffic)
- [12. A/B Testing Implementation](#12-ab-testing-implementation)

## 9. Structure Large-Scale React App
Expected Answer: Organize codebase with feature-based structure, shared utilities, and clear boundaries. Hints:
•	Feature folders vs layer-based organization
•	Shared components, hooks, utilities, constants
•	Config files, environment variables
•	Absolute imports with jsconfig/tsconfig
•	Monorepo with packages for large teams

## 9. Local State vs Context vs Redux
Expected Answer: Choose state management solution based on complexity, team size, and performance needs. Hints:
•	Local state: component-specific, simple
•	Context: app-wide themes/auth, not frequent updates
•	Redux: complex state, time-travel debugging, middleware
•	Zustand/Jotai: simpler alternatives to Redux
•	React Query: server state management

## 10. Design Reusable Components
Expected Answer: Create flexible, composable components with clear APIs and accessibility. Hints:
•	Compound components pattern for flexibility
•	Render props for custom rendering
•	Prop spreading for HTML attribute forwarding
•	TypeScript interfaces for prop documentation
•	Accessibility attributes and keyboard navigation

## 11. High-Performance Dashboard with Real-Time
Expected Answer: Build dashboard with WebSockets, virtualization, and efficient updates. Hints:
•	WebSocket for real-time data
•	Virtualization for large datasets
•	Debounce rapid updates
•	Optimistic UI updates
•	Offline support with local cache

## 12. Reduce Global Page Load Time
Expected Answer: Implement performance optimizations for faster initial page loads. Hints:
•	CDN for global asset delivery
•	Brotli/gzip compression
•	HTTP/2, preconnect, dns-prefetch
•	Critical CSS inlining
•	Code splitting, lazy loading

## 13. Dependency Conflicts After Upgrade
Expected Answer: Resolve version conflicts and ensure compatibility across dependencies. Hints:
•	npm ls to visualize dependency tree
•	package.json resolutions field for forcing versions
•	Check peerDependencies requirements
•	Lock file consistency across team
•	Gradual upgrades, not everything at once

## 14. Migrate Legacy Frontend
Expected Answer: Plan incremental migration with risk mitigation and testing strategies. Hints:
•	Strangler pattern: replace piece by piece
•	Feature flags for gradual rollout
•	Proxy layer between old and new
•	A/B testing during migration
•	Automated tests for critical paths

## 15. Secure Sensitive Data on Client
Expected Answer: Implement security best practices for handling sensitive data in browser. Hints:
•	Tokens in httpOnly cookies, not localStorage
•	Refresh token rotation
•	CSP headers to prevent XSS
•	Input sanitization, output encoding
•	Rate limiting on sensitive endpoints

## 16. Debug Inconsistent UI Across Browsers
Expected Answer: Systematic approach to identify and fix cross-browser compatibility issues. Hints:
•	BrowserStack/CrossBrowserTesting for testing
•	Normalize.css/reset.css for baseline
•	Feature detection with Modernizr
•	Polyfills for missing APIs
•	CSS vendor prefixes, autoprefixer

## 17. Critical UI Failure During Peak Traffic
Expected Answer: Emergency response plan for production issues during high traffic. Hints:
•	Immediate rollback to previous version
•	Feature flag to disable broken feature
•	Canary releases for future deployments
•	Monitoring with alerts
•	Post-mortem analysis

## 18. A/B Testing Implementation
Expected Answer: Design and implement A/B testing with proper analytics and statistical rigor. Hints:
•	Feature flag services (LaunchDarkly, Optimizely)
•	Consistent user assignment (cookie-based)
•	Statistical significance calculation
•	Server-side vs client-side rendering considerations
•	Analytics integration for metrics

## 19. Janky CSS Animation on Mobile
Expected Answer: Optimize animations for smooth performance on mobile devices. Hints:
•	Use transform and opacity (GPU accelerated)
•	Avoid animating height, width, top, left
•	will-change property carefully
•	requestAnimationFrame for JS animations
•	Chrome DevTools Performance tab
