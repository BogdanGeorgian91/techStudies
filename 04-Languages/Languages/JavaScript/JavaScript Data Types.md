In JavaScript, every value is of a certain **data type**. JavaScript is a **dynamically typed** language, meaning you don't have to explicitly declare the data type of a variable when you declare it. The type is determined automatically at runtime based on the value assigned to it.

JavaScript data types can be broadly categorized into two groups: **Primitive Data Types** and **Non-Primitive (or Reference) Data Types**.

### I. Primitive Data Types

Primitive values are immutable (cannot be changed after creation) and are stored directly in the memory where the variable resides. When you assign a primitive value to another variable, a copy of the value is made.

1. **`String`**
    - Represents textual data.
    - Enclosed in single quotes (`'...'`), double quotes (`"..."`), or backticks8 (`` `...` ``). Backticks allow for template literals (embedded expressions and multi-line strings).
    - **Examples:** `'hello'`, `"World"`, `` `My name is ${name}` ``
2. **`Number`**
    - Represents both integer and floating-point numbers.
    - JavaScript uses a single `Number` type, which is a 64-bit floating-point number (IEEE 754 standard).
    - Special `Number` values: `Infinity`, `-Infinity`, `NaN` (Not-a-Number).
    - **Examples:** `10`, `3.14`, `-5`, `0xFF` (hexadecimal), `1e3` (exponential)
3. **`BigInt`**
    - Represents whole numbers larger than `2^53 - 1` (the maximum safe integer that `Number` can reliably represent).
    - Created by appending `n` to the end of an integer.
    - **Examples:** `9007199254740991n`, `10n`13
        
4. **`Boolean`**
    - Represents a logical entity and can have only two values: `true` or `false`.
    - Often used for conditional logic.
    - **Examples:** `true`, `false`
5. **`Undefined`**
    - Represents a variable that has been declared but has not yet been assigned a value.
    - Also, if a function doesn't explicitly return anything, it implicitly returns `undefined`.
    - **Example:** `let x; console.log(x); // undefined`
6. **`Null`**
    - Represents the intentional absence of any object value.
    - It's a primitive value, but `typeof null` returns `'object'` (a long-standing bug in JavaScript that was never fixed for backward compatibility).15
    - **Example:** `let y = null;`
7. **`Symbol`** (Introduced in ES6)
    - Represents a unique and anonymous identifier.
    - Used to create unique property keys that won't clash with other property keys (even if they have the same string value).
    - **Example:** `const id = Symbol('id');`

### II. Non-Primitive (Reference) Data Types

Non-primitive values are mutable (can be changed after creation) and are stored as references in memory. When you assign a non-primitive value to another variable, you're copying the _reference_ to the memory location, not the actual value. Both variables then point to the same underlying data.

1. **`Object`**
    - The most fundamental non-primitive type.
    - Used to store collections of data and more complex entities.
    - Objects are collections of key-value pairs (properties and methods).
        
    - **Examples:**
        - **Plain Objects:** `let person = { name: 'Alice', age: 30 };`
        - **Arrays:** Arrays are a special type of object used to store ordered collections of values. `let colors = ['red', 'green', 'blue'];` (Note: `typeof []` returns `'object'`)
        - **Functions:** Functions are also a special type of object (callable objects). `function greet() { /* ... */ }` (Note: `typeof function() {}` returns `'function'`, but they are still objects underneath).
        - **Dates:** `new Date()`
        - **Regular Expressions:** `/abc/`
        - **Maps, Sets, WeakMaps, WeakSets:** ES6 collections.

**Key Differences Between Primitive and Non-Primitive:**

|                       |                                                                        |                                                                       |
| --------------------- | ---------------------------------------------------------------------- | --------------------------------------------------------------------- |
| **Feature**           | **Primitive Data Types**                                               | **Non-Primitive Data Types**                                          |
| **Storage**           | Stored directly in the variable location.                              | Stored by reference (memory address).                                 |
| **Mutability**        | Immutable (value cannot be changed).                                   | Mutable (can be changed).                                             |
| **Comparison**        | Compared by value (===).                                               | Compared by reference (===).                                          |
| **Assignment**        | Copies the actual value.                                               | Copies the reference (points to same data).                           |
| **`typeof` operator** | Returns the specific type (e.g., `'string'`, `'number'`, `'boolean'`). | Returns `'object'` (except for functions, which return `'function'`). |

**Example of Primitive vs. Reference Assignment/Comparison:**

JavaScript

```
// Primitive example (value comparison, copy by value)
let a = 10;
let b = a; // b gets a copy of 10
a = 20;
console.log(b); // Output: 10 (b is unaffected by change to a)
console.log(a === b); // Output: false

// Non-primitive (Object) example (reference comparison, copy by reference)
let obj1 = { value: 10 };
let obj2 = obj1; // obj2 gets a copy of the *reference* to the same object
obj1.value = 20;
console.log(obj2.value); // Output: 20 (obj2.value changed because it points to the same object)
console.log(obj1 === obj2); // Output: true (they refer to the same object in memory)

let obj3 = { value: 10 }; // A new object, even if content is identical
console.log(obj1 === obj3); // Output: false (different references)
```