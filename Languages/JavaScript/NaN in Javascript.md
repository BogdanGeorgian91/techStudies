## What is NaN in JavaScript?

**NaN** stands for "Not a Number." It's a special numeric value returned when a mathematical operation fails or when JavaScript cannot convert a value into a valid number—for example, multiplying non-numeric strings or dividing zero by zero[1][2][4]. NaN is of type `number` but represents an invalid number result[3][5].

## How to Check for NaN

- Use the global `isNaN()` function to check if a value is NaN or cannot be converted to a number:

  ```js
  isNaN(NaN); // true
  isNaN("hello"); // true
  isNaN(123); // false
  ```

- For a stricter check (only true for actual NaN, not for values that just can't be converted), use `Number.isNaN()`:

  ```js
  Number.isNaN(NaN); // true
  Number.isNaN("hello"); // false
  ```

- Note: `NaN` is the only value in JavaScript that is not equal to itself (`NaN !== NaN` is true)[6].

In summary, use `isNaN()` for general checks and `Number.isNaN()` for strict NaN detection.

