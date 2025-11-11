Hoisting in JavaScript is a behavior where variable and function declarations are moved to the top of their containing scope (script or function) before the code is executed.

This means you can use variables and functions before you declare them — but how they behave depends on how they were declared.
⸻⸻⸻⸻⸻⸻⸻⸻⸻⸻⸻⸻⸻
# Hoisting with var, let, and const

✅ var is hoisted:

console.log(a); // undefined
var a = 5;

Explanation:
	•	The declaration var a; is hoisted.
	•	But the assignment a = 5 stays in place.
	•	So it’s as if:

var a;
console.log(a); // undefined
a = 5;

⚠️ let and const are hoisted, but in a Temporal Dead Zone (TDZ):

console.log(b); // ReferenceError
let b = 10;

Explanation:
	•	let b is hoisted, but not initialized.
	•	You cannot access it before the line where it’s declared (TDZ).
⸻⸻⸻⸻⸻⸻⸻⸻⸻⸻⸻⸻⸻
# Hoisting with Functions

✅ Function Declarations are fully hoisted:

greet(); // "Hello"

function greet() {
  console.log("Hello");
}

Explanation:
	•	The entire function is hoisted, so you can call it before its definition.

⚠️ Function Expressions are NOT hoisted the same way:

sayHi(); // TypeError: sayHi is not a function

var sayHi = function () {
  console.log("Hi");
};

Explanation:
	•	var sayHi is hoisted (as undefined), but the assignment happens later.
	•	So you try to call undefined().

⸻⸻⸻⸻⸻⸻⸻⸻⸻⸻⸻
# Summary

Declaration Type	Hoisted?	Can Use Before Declaration?
var	✅ Yes	⚠️ Yes, but value is undefined
let / const	✅ Yes	❌ No (TDZ error)
Function Declaration	✅ Yes	✅ Yes
Function Expression	❌ Only var name hoisted	❌ No

⸻⸻⸻⸻⸻⸻⸻⸻⸻⸻⸻⸻
⸻⸻⸻⸻⸻⸻⸻⸻⸻⸻⸻⸻⸻
⸻⸻⸻⸻⸻⸻⸻⸻⸻⸻⸻⸻

Hoisting in JavaScript is a behavior where **declarations of variables, functions, and classes are conceptually moved to the top of their containing scope during the compilation phase, before the code is actually executed.**

It's important to understand that the code isn't physically moved; rather, the JavaScript engine parses and understands the declarations first, then executes the code. This "pre-processing" is what gives the _appearance_ of declarations being "hoisted" to the top.

Here's a breakdown of how hoisting works with different types of declarations:

**1. `var` Variable Hoisting:**

- **Declaration is hoisted, initialization is not.**
- When a variable is declared with `var`, its declaration is moved to the top of its scope (either global or function scope).
- However, its _assignment_ or _initialization_ stays exactly where it was written in the code.
- If you try to access a `var` variable before its assignment, it will have a value of `undefined`.

**Example with `var`:**

JavaScript

```
console.log(myVar); // Output: undefined
var myVar = 10;
console.log(myVar); // Output: 10
```

**What the JavaScript engine "sees" (conceptually):**

JavaScript

```
var myVar;           // Declaration is hoisted and initialized to undefined
console.log(myVar);  // myVar is undefined at this point
myVar = 10;          // Assignment happens here
console.log(myVar);  // myVar is now 10
```

**2. Function Declaration Hoisting:**

- **Both the function declaration and its definition are fully hoisted.**
- This means you can call a function declared with the `function` keyword _before_ its actual declaration in the code.

**Example with Function Declaration:**

JavaScript

```
sayHello(); // Output: Hello!

function sayHello() {
  console.log("Hello!");
}
```

**What the JavaScript engine "sees" (conceptually):**

JavaScript

```
function sayHello() { // Entire function is hoisted
  console.log("Hello!");
}

sayHello(); // Function is available to be called
```

**3. `let` and `const` Variable Hoisting (and the Temporal Dead Zone - TDZ):**

- **Declarations are hoisted, but they are not initialized.**
- `let` and `const` variables are hoisted to the top of their block scope.
- However, unlike `var`, they are not given a default value of `undefined`.
- Instead, they enter a state called the **Temporal Dead Zone (TDZ)** from the beginning of their block until their declaration line is executed.
- Attempting to access a `let` or `const` variable within its TDZ will result in a `ReferenceError`.

**Example with `let`:**

JavaScript

```
console.log(myLet); // ReferenceError: Cannot access 'myLet' before initialization
let myLet = 20;
console.log(myLet);
```

**What the JavaScript engine "sees" (conceptually):**

JavaScript

```
// myLet is in the Temporal Dead Zone from here until its declaration
console.log(myLet); // Accessing myLet here throws a ReferenceError
let myLet = 20;     // myLet is initialized here, exiting the TDZ
console.log(myLet);
```

**Example with `const`:**

The behavior is identical to `let` regarding hoisting and the TDZ.

JavaScript

```
console.log(myConst); // ReferenceError: Cannot access 'myConst' before initialization
const myConst = 30;
console.log(myConst);
```

**4. Function Expressions:**

- Function expressions are treated like `var` variables. Only the variable declaration is hoisted, not the function definition itself.

**Example with Function Expression:**

JavaScript

```
sayGoodbye(); // TypeError: sayGoodbye is not a function
var sayGoodbye = function() {
  console.log("Goodbye!");
};
```

**What the JavaScript engine "sees" (conceptually):**

JavaScript

```
var sayGoodbye; // sayGoodbye is hoisted and initialized to undefined
sayGoodbye();   // Attempting to call undefined as a function results in a TypeError
sayGoodbye = function() {
  console.log("Goodbye!");
};
```

**Why is Hoisting Important?**

- **Understanding Code Execution:** It helps you predict how your JavaScript code will behave, especially when variables and functions are used before their literal declarations.
- **Avoiding Bugs:** Misunderstanding hoisting can lead to unexpected `undefined` values or `ReferenceError`s, especially with `var` and `let`/`const`.
- **Code Organization:** While generally not recommended to rely on hoisting for code organization, it explains why certain patterns (like placing function calls at the top and declarations at the bottom) might still work.

**Best Practices:**

- **Always declare your variables and functions before you use them.** This is the most reliable way to avoid confusion and potential bugs related to hoisting.
- **Prefer `let` and `const` over `var`.** They provide block-scoping and the TDZ, which makes variable access more predictable and helps catch errors earlier.
- **Declare variables at the top of their scope.** This makes your code cleaner and easier to read, regardless of hoisting.

In essence, hoisting is a characteristic of JavaScript's compilation phase. While it can seem counter-intuitive at first, understanding its nuances is crucial for writing robust and predictable JavaScript code.

---
# REACT

Hoisting isn't a React-specific feature; it's a fundamental behavior of **JavaScript itself**. However, because React applications are built entirely with JavaScript, understanding hoisting helps explain why certain patterns work (or don't work) in your React code.

In essence, **hoisting is JavaScript's default behavior of moving declarations to the top of their containing scope during the compilation phase, before code execution.** It doesn't move the _assignments_ or _initializations_, just the declarations.

Let's break down its relevance in React contexts:

### 1. Functional Components and Local Variables/Functions

Functional components are just JavaScript functions. The rules of hoisting apply to everything you declare inside them.

- **`const` and `let` (Variables for State, Props, Local Data):**
    - **Declarations are hoisted, but they enter the Temporal Dead Zone (TDZ).**
    - This means you **cannot access a `const` or `let` variable before its declaration line** within a functional component. Attempting to do so will result in a `ReferenceError`.
    - This is generally a good thing, as it encourages declaring variables before use, leading to more readable and predictable code.
    
    JavaScript
    
    ```
    function MyComponent(props) {
      // console.log(data); // ReferenceError: Cannot access 'data' before initialization
      const [data, setData] = React.useState(null); // 'data' is declared with const
    
      // console.log(localVariable); // ReferenceError: Cannot access 'localVariable' before initialization
      let localVariable = "Hello";
    
      // ... rest of the component logic
      return <div>{localVariable}</div>;
    }
    ```
    
    **Relevance in React:** You naturally define your `useState`, `useRef`, and other Hook variables at the top of your functional component because you need them for the logic that follows. Hoisting (and the TDZ) perfectly aligns with this best practice, preventing accidental early access.
    
- **Named Function Declarations (e.g., helper functions):**
    
    - **Fully hoisted.** Both the declaration and the definition are moved to the top.
    - You **can call a named helper function before its definition** within your functional component.
    
    JavaScript
    
    ```
    function MyComponent(props) {
      doSomething(); // This works! Output: Doing something...
    
      function doSomething() { // Function declaration
        console.log("Doing something...");
      }
    
      return <button onClick={doSomething}>Click Me</button>;
    }
    ```
    
    **Relevance in React:** While possible, it's generally good practice for readability to define helper functions before they are called, even if hoisting allows otherwise.
    
- **Arrow Function Expressions (e.g., event handlers, memoized callbacks):**
    
    - **Treated like `const` or `let` variables.** Only the variable declaration is hoisted, not the function definition.
    - You **cannot call an arrow function before its definition** if it's assigned to a `const` or `let` variable (due to the TDZ).
    
    JavaScript
    
    ```
    function MyComponent(props) {
      // handleClick(); // ReferenceError: Cannot access 'handleClick' before initialization
      const handleClick = () => { // Arrow function expression assigned to a const
        console.log("Button clicked!");
      };
    
      return <button onClick={handleClick}>Click Me</button>;
    }
    ```
    
    **Relevance in React:** This is why you always define your event handlers (e.g., `const handleClick = () => { ... };`) before you attach them to elements in JSX (e.g., `<button onClick={handleClick}>`). You're naturally following the rule of not accessing a `const` before its initialization.
    

### 2. Class Components and Methods/Properties

While less common with modern React (due to Hooks), hoisting also applies to class components.

- **Class Declarations:**
    
    - `class MyClass { ... }` declarations are partially hoisted, but similar to `let` and `const`, they also exhibit a TDZ. You cannot access a class before its declaration.
    - **Relevance:** You always define your class component (`class MyComponent extends React.Component { ... }`) before you try to render it.
- **Methods (Traditional Syntax):**
    
    - Methods defined using the traditional `methodName() { ... }` syntax are attached to the prototype and are not individually hoisted in the same way regular functions are within their scope. They become available once the class itself is defined.
    
    JavaScript
    
    ```
    class MyComponent extends React.Component {
      // In constructor, you can call methods that are defined later in the class
      constructor(props) {
        super(props);
        this.state = { count: 0 };
        this.logMessage(); // This works even though logMessage is defined later
      }
    
      logMessage() {
        console.log("Message from method");
      }
    
      render() {
        return <button onClick={this.logMessage}>Log</button>;
      }
    }
    ```
    
    **Relevance:** This allows for a flexible order of method definitions within a class, though again, defining them before use is generally clearer.
    
- **Class Properties (Arrow Function Methods):**
    
    - When you define an arrow function as a class property (`myHandler = () => { ... }`), it's essentially an instance property, initialized when the class instance is created.
    - The property name itself is "hoisted" in a sense (you can refer to `this.myHandler` in the constructor), but the _value_ (the arrow function) is assigned during instance creation.
    - **Relevance:** This is the preferred way to define event handlers in class components because it automatically binds `this` correctly, avoiding the need for `this.method.bind(this)` in the constructor. From a hoisting perspective, you'd still define the property before attempting to use it in `render()`.
    
    JavaScript
    
    ```
    class MyComponent extends React.Component {
      state = { count: 0 };
    
      handleClick = () => { // Class property assigned an arrow function
        this.setState(prevState => ({ count: prevState.count + 1 }));
      };
    
      render() {
        // You reference handleClick after it's defined as a property
        return <button onClick={this.handleClick}>Increment</button>;
      }
    }
    ```
    

### Why Hoisting is Less "Visible" (or problematic) in Modern React

1. **`let` and `const` Dominance:** Most modern React code uses `let` and `const` for variable declarations (state, props, local variables). The TDZ associated with these makes it a best practice (and a requirement) to declare variables _before_ you use them, naturally aligning with logical code flow and effectively sidestepping the more confusing `var` hoisting behavior.
2. **Functional Components and Hooks:** The linear execution of a functional component from top to bottom (on each render) means you're almost always defining your state, effects, and memoized callbacks (`useState`, `useEffect`, `useCallback`, `useMemo`) at the very beginning of the component function, before you need to reference them in JSX or other logic.
3. **Arrow Functions for Callbacks:** Arrow functions, when assigned to `const` or `let` variables, inherit the TDZ behavior, again reinforcing the "declare before use" pattern for event handlers and other callbacks.

While hoisting is a core JavaScript mechanism, its implications for React developers are often mitigated by modern JavaScript best practices (using `const`/`let`), the top-to-bottom execution model of functional components, and the common patterns for defining components and handlers. 
You rarely explicitly rely on hoisting in React; instead, understanding it helps you avoid unexpected `ReferenceError`s if you accidentally try to use a `const` or `let` variable before its definition.