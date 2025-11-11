React, being a library for building user interfaces, has adopted and evolved several design patterns to help developers write more maintainable, scalable, and efficient code. While React itself is opinionated about its core concepts (components, props, state), the way these concepts are combined and organized often follows established patterns.

Here are some of the most common and important design patterns in React:

1. **Component-Based Architecture:** This is the foundational pattern of React. The entire UI is broken down into small, independent, and reusable components. Each component is responsible for rendering a specific part of the UI and manages its own state and lifecycle.
    
2. **Presentational and Container Components:**
    
    - **Presentational Components (or "Dumb" Components):** These components are concerned with _how_ things look. They receive data and callbacks via props and render UI. They typically have no internal state or business logic and are focused purely on presentation.
        
    - **Container Components (or "Smart" Components):** These components are concerned with _what_ things do. They handle data fetching, state management, and business logic. They pass data and callbacks down to presentational components as props. This separation of concerns improves reusability and testability.
        
3. **Higher-Order Components (HOCs):**
    
    - HOCs are functions that take a component as an argument and return a new component with enhanced functionality. They are used for reusing component logic (e.g., authentication, logging, data fetching) across multiple components without duplicating code.
        
    - _Note:_ While still a valid pattern, the rise of React Hooks has often made HOCs less necessary for sharing stateful logic, as custom Hooks often provide a more direct and flexible solution.
        
4. **Render Props Pattern:**
    
    - This pattern involves a component that takes a function as a prop, and that function is responsible for rendering something. The component provides data or logic to the render prop, allowing for flexible and reusable behavior.
        
    - It's another way to share code between components, often used for things like sharing mouse coordinates, window size, or other dynamic data.
        
5. **Provider Pattern (Context API):**
    
    - The Provider pattern, often implemented using React's Context API, is used for sharing "global" data across a component tree without manually passing props down through every level (known as "prop drilling").
        
    - This is useful for data like user authentication status, theme settings, or locale, which might be needed by many components at different levels of the tree.
        
6. **Compound Components:**
    
    - This pattern allows multiple components to work together to achieve a shared functionality, where the parent component implicitly shares state and logic with its children. This creates a more expressive and versatile API for complex UI elements (e.g., a `Tabs` component with `TabList`, `Tab`, and `TabPanel` sub-components).
        
7. **Custom Hooks:**
    
    - While not a "design pattern" in the traditional GoF sense, custom Hooks have become a fundamental pattern for reusing stateful logic in functional components. They allow you to extract component logic into reusable functions, making components cleaner and more focused on rendering. They often replace the need for HOCs and render props in many scenarios.
        
8. **Controlled and Uncontrolled Components:**
    
    - **Controlled Components:** Form elements (like inputs, textareas) whose values are controlled by React state. Every state update re-renders the component, making the form data easily accessible and manageable within React.
        
    - **Uncontrolled Components:** Form elements whose values are managed by the DOM itself. You access their values when needed (e.g., on form submission) using refs. They are simpler for very basic forms but offer less control.
        
9. **Conditional Rendering:**
    
    - A straightforward but essential pattern where components or elements are rendered based on certain conditions (e.g., showing a loading spinner while data is fetched, displaying different UI based on user roles).
        
10. **Layout Components:**
    
    - These components are primarily responsible for arranging other components on a page, defining the overall structure and layout. They often accept `children` as a prop and apply styling or structural elements to them.
        

Understanding and applying these design patterns can significantly improve the quality, readability, and maintainability of your React applications. The choice of which pattern to use often depends on the specific problem you're trying to solve and the complexity of your application.