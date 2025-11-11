
**Definition & Usage**

- `undefined` means a variable has been declared but has not been assigned a value. It also appears when you access a non-existent property of an object or a function returns nothing implicitly[1][3][6].
- `null` is an explicit assignment by the programmer to indicate "no value" or "empty." It is used when you intentionally want to clear a variable or property[1][3][6].

**Type**

- `typeof undefined` returns `"undefined"`.
- `typeof null` returns `"object"` (this is a long-standing bug in JavaScript)[4][6].

**Assignment**

- `undefined` is set automatically by JavaScript when a variable is declared but not initialized, but you can also set it manually.
- `null` must be explicitly assigned by the programmer[1][4][6].

**Typical Use Cases**

- Use `undefined` to represent uninitialized variables or missing object properties.
- Use `null` to intentionally indicate the absence of any object value[1][3][4].

**Equality Comparison**

- `null == undefined` is `true` (loose equality).
- `null === undefined` is `false` (strict equality)[5][6].

## Summary Table

| Feature        | `undefined`                         | `null`                   |
| -------------- | ----------------------------------- | ------------------------ |
| Set by         | JavaScript (by default)             | Programmer (explicitly)  |
| Type           | `"undefined"`                       | `"object"`               |
| Meaning        | Variable not initialized or missing | Explicitly no value      |
| Equality (==)  | Equal to `null`                     | Equal to `undefined`     |
| Equality (===) | Not equal to `null`                 | Not equal to `undefined` |

Both are falsy values and represent "no value," but in different contexts and with different intent.


