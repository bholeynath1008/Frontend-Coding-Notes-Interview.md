# Section 5: React Performance & Optimization
# React Performance & Optimization Interview Questions

## Section 5: React Performance & Optimization

- [45. Memory Leaks in React](#45-memory-leaks-in-react)
- [46. Performance Metrics in Production](#46-performance-metrics-in-production)
- [47. App Feels Slow but Lighthouse Good](#47-app-feels-slow-but-lighthouse-good)
- [48. Optimize Images in Large App](#48-optimize-images-in-large-app)
- [49. Render 100k+ Items in React](#49-render-100k-items-in-react)
- [50. How to Improve React Application Performance](#50-how-to-improve-react-application-performance)
- [51. High-Impact Optimizations](#51-high-impact-optimizations)
- [52. Medium-Impact / Supporting Techniques](#52-medium-impact--supporting-techniques)
- [53. Detailed Performance Optimization Techniques](#53-detailed-performance-optimization-techniques)
- [54. React Portals](#54-react-portals)

## 45. Memory Leaks in React
Expected Answer: Prevent memory leaks by cleaning up effects, unsubscribing events, and canceling requests. Hints:
•	Return cleanup function from useEffect
•	Cancel fetch requests with AbortController
•	Remove event listeners in cleanup
•	Clear intervals/timeouts
•	Use React DevTools to detect leaks

## 46. Performance Metrics in Production
Expected Answer: Monitor Core Web Vitals and other metrics for performance optimization. Hints:
•	LCP (Largest Contentful Paint): <2.5s
•	FID/INP (Interaction to Next Paint): <100ms
•	CLS (Cumulative Layout Shift): <0.1
•	Tools: Lighthouse, WebPageTest, Sentry, New Relic
•	Real User Monitoring (RUM) vs synthetic testing

## 47. App Feels Slow but Lighthouse Good
Expected Answer: Address perceived performance and interaction responsiveness issues. Hints:
•	Skeleton screens for loading states
•	Optimistic updates for immediate feedback
•	Debounce rapid user interactions
•	Measure INP for interaction responsiveness
•	Check main thread blocking with Performance tab

## 48. Optimize Images in Large App
Expected Answer: Implement modern image optimization techniques for better performance. Hints:
•	WebP/AVIF formats with fallbacks
•	loading="lazy" for below-fold images
•	srcset and sizes for responsive images
•	CDN with image optimization (resize, compress)
•	Implement blur-up/LQIP technique

## 49. Render 100k+ Items in React
Expected Answer: Use virtualization or pagination to efficiently render large lists. Hints:
•	Libraries: react-window, react-virtualized
•	Only render visible items in viewport
•	Infinite scroll with Intersection Observer
•	Pagination for simpler implementation
•	Trade-off: scroll position restoration

## 50. How to Improve React Application Performance
Expected Answer: Multiple strategies from code splitting to memoization to virtualization. Hints:
•	Bundle optimization: Code splitting, tree shaking
•	Render optimization: Memoization, virtualization
•	Asset optimization: Images, fonts, compression
•	Runtime optimization: Debouncing, web workers

## 51. High-Impact Optimizations
Expected Answer: Code splitting, virtualization, eliminating unnecessary re-renders. Hints:
•	Route-based code splitting with React.lazy
•	Virtualize long lists with react-window
•	Use React.memo, useMemo, useCallback appropriately
•	Optimize images: WebP, lazy loading, responsive

## 52. Medium-Impact / Supporting Techniques
Expected Answer: Bundle analysis, CDN, compression, service workers. Hints:
•	Analyze bundle with webpack-bundle-analyzer
•	CDN for static assets
•	Brotli/gzip compression
•	Service workers for offline/caching

## 53. Detailed Performance Optimization Techniques
Expected Answer: Comprehensive list of specific techniques with implementation details. Hints:
•	Production build: Minification, dead code elimination
•	Avoid inline functions/objects as props
•	Windowing for large lists
•	Web Workers for heavy computations
•	Optimize re-renders with React DevTools Profiler

## 54. React Portals
Expected Answer: Render children into DOM node outside parent hierarchy (modals, tooltips). Hints:
•	ReactDOM.createPortal(child, container)
•	Child mounts into provided DOM node
•	Events bubble through React tree, not DOM tree
•	Use for: Modals, tooltips, dropdowns that need to escape overflow:hidden
