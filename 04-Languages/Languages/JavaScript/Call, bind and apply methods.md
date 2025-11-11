In JavaScript, `call()`, `apply()`, and `bind()` are methods used to control the `this` context of a function and how arguments are passed:

- **`call()`**: Invokes a function immediately with a specified `this` context and arguments passed individually.
  ```js
  function greet(name) {
    console.log(`Hello, ${name}! My name is ${this.name}.`);
  }
  const person = { name: 'Alice' };
  greet.call(person, 'Bob'); // Hello, Bob! My name is Alice.
  ```
- **`apply()`**: Similar to `call()`, but arguments are passed as an array.
  ```js
  function sum(a, b) {
    return a + b;
  }
  const numbers = [1, 2];
  sum.apply(null, numbers); // 3
  ```
- **`bind()`**: Returns a new function with a bound `this` context and optionally preset arguments, but does not call the function immediately.
  ```js
  function greet(name) {
    console.log(`Hello, ${name}! My name is ${this.name}.`);
  }
  const person = { name: 'Alice' };
  const greetPerson = greet.bind(person);
  greetPerson('Bob'); // Hello, Bob! My name is Alice.
  ```

### Summary of Differences:
| Method  | When It Executes       | Arguments Format          | Returns          |
|---------|-----------------------|--------------------------|------------------|
| `call`  | Immediately            | List of arguments        | Result of function call |
| `apply` | Immediately            | Array of arguments       | Result of function call |
| `bind`  | Later (when invoked)   | List of arguments (preset possible) | New bound function |

Use `call` or `apply` to invoke functions immediately with a specific `this` context; use `bind` to create a new function with bound context for later use.
