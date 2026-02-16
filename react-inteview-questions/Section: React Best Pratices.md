# 3 React Interview Questions with Answers

https://medium.com/@obrm770/best-practices-and-design-patterns-in-react-js-for-high-quality-applications-6b203be747fb

## 1. What are the best practices for building scalable React applications?

**Answer:**

There are several key best practices for building scalable React applications:

**1.1 Folder Structure:** Use a feature-based folder structure instead of organizing by file types. Group components by features to make it easier to find and manage related files.

**1.2 Keep Components Small and Focused:** Each component should have a single responsibility. If a component becomes too large, break it down into smaller, manageable components. For example, split a UserProfile component into ProfilePicture, UserName, and UserBio.

**1.3 Naming Conventions:** Use consistent naming conventions - PascalCase for components (UserProfile.js), camelCase for variables and functions (getUserData()), and UPPERCASE_SNAKE_CASE for constants (API_URL).

**1.4 Separation of Pages and Presentational Components:** Pages (container components) handle data fetching, state management, and logic, while presentational components focus on rendering UI elements. This creates more modular and reusable components.

**1.5 DRY Principle with Array Mapping:** Use arrays and map functions to avoid code repetition, especially for rendering lists of similar elements like navigation links or form inputs.

These practices help create applications that are easier to navigate, understand, maintain, and scale.

---

## 2. When should you use a Custom Hook versus a Service Function?

**Answer:**

The choice between custom hooks and service functions depends on whether the functionality needs to manage state or context:

**2.1 Use Custom Hooks when:**
- You need to manage state or side effects (using useState, useEffect, etc.)
- The functionality needs to access React context
- You want to reuse stateful logic across multiple components
- Examples: useForm for form input handling, useFetch for data fetching with loading states, useUserSettings for managing user preferences with state

**2.2 Use Service Functions when:**
- You need reusable logic that doesn't depend on state or context
- The functionality is task-specific (data validation, formatting, sorting)
- You need standalone functionality that can work across multiple applications
- Examples: API calls (apiService.getUsers()), utility functions (formatDate, calculateSum), data validation (validateEmail)

**2.3 Key Principle:** In practice, custom hooks often use service functions internally. For example, a useFetch custom hook might use an API service function to make the actual HTTP request, while the hook manages the loading state and error handling.

---

## 3. Explain the main Component Design Patterns in React.

**Answer:**

React has several important component design patterns:

**3.1 Higher-Order Components (HOCs):** Functions that take a component and return a new component with additional features. Used for cross-cutting concerns like authentication (requireAuth) or data fetching (withDataFetching). HOCs enable reusable component logic across the application.

**3.2 Render Props:** A pattern where you pass a function as a prop to share code between components. The child component calls this function to determine what to render, giving the parent control over rendering while the child provides functionality. Example: a ProductFetcher component that accepts a render function to display products in different formats.

**3.3 Compound Components:** A pattern that groups related components together, allowing them to share state and control behavior from a parent component. Makes components more intuitive and expressive. Examples: Accordion with AccordionHeader and AccordionContent, or Tabs with Tab and TabContent components.

**3.4 Performance Optimization Patterns:**
- **3.4.1** React.memo for preventing unnecessary re-renders
- **3.4.2** React.lazy for code splitting and lazy loading components

**3.5 State Management Patterns:**
- **3.5.1** Context API (React.createContext) for global state management
- **3.5.2** Reducers (useReducer) for managing complex state predictably

**3.6 List Rendering Best Practice:** Always use unique keys when rendering lists to help React optimize rendering and avoid unnecessary DOM updates.

These patterns help create flexible, reusable, and maintainable components that are easier to test and understand.
