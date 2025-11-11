
# JavaScript Object Methods - Complete Guide

Object methods in JavaScript fall into different categories based on their purpose and how they're accessed. Understanding these methods is essential for effective object manipulation and data processing.

## Property Enumeration Methods

### Object.keys()

The Object.keys() method returns an array of a given object's own enumerable property names, in the same order as a normal loop.

```javascript
const person = {
    name: 'Alice',
    age: 30,
    city: 'New York'
};

const keys = Object.keys(person);
console.log(keys); // ['name', 'age', 'city']

// Practical example: checking if object has properties
if (Object.keys(person).length > 0) {
    console.log('Object has properties');
}
```

### Object.values()

The Object.values() method returns an array of a given object's own enumerable property values.

```javascript
const scores = {
    math: 95,
    science: 87,
    english: 92
};

const values = Object.values(scores);
console.log(values); // [95, 87, 92]

// Calculate average score
const average = values.reduce((sum, score) => sum + score, 0) / values.length;
console.log(average); // 91.33
```

### Object.entries()

The Object.entries() method returns an array of a given object's own enumerable property [key, value] pairs.

```javascript
const product = {
    name: 'Laptop',
    price: 999,
    category: 'Electronics'
};

const entries = Object.entries(product);
console.log(entries); 
// [['name', 'Laptop'], ['price', 999], ['category', 'Electronics']]

// Convert back to object (useful for transformations)
const doubled = Object.fromEntries(
    entries.map(([key, value]) => [key, typeof value === 'number' ? value * 2 : value])
);
console.log(doubled); // { name: 'Laptop', price: 1998, category: 'Electronics' }
```

### Object.getOwnPropertyNames()

The Object.getOwnPropertyNames() method returns an array of all properties found directly in a given object, including non-enumerable properties.

```javascript
const obj = {};
Object.defineProperty(obj, 'hiddenProp', {
    value: 'secret',
    enumerable: false // This won't show up in Object.keys()
});
obj.visibleProp = 'visible';

console.log(Object.keys(obj)); // ['visibleProp']
console.log(Object.getOwnPropertyNames(obj)); // ['hiddenProp', 'visibleProp']
```

## Object Creation and Manipulation

### Object.create()

The Object.create() method creates a new object with the specified prototype object and properties.

```javascript
// Create an object with a specific prototype
const personPrototype = {
    greet() {
        return `Hello, I'm ${this.name}`;
    },
    getAge() {
        return this.age;
    }
};

const alice = Object.create(personPrototype);
alice.name = 'Alice';
alice.age = 30;

console.log(alice.greet()); // "Hello, I'm Alice"

// Create object with null prototype (no inherited properties)
const pureObject = Object.create(null);
pureObject.data = 'some data';
console.log(pureObject.toString); // undefined (no inherited methods)
```

### Object.assign()

The Object.assign() method copies all enumerable own properties from one or more source objects to a target object.

```javascript
const target = { a: 1, b: 2 };
const source1 = { b: 3, c: 4 };
const source2 = { c: 5, d: 6 };

// Properties in source objects overwrite properties in the target
const result = Object.assign(target, source1, source2);
console.log(result); // { a: 1, b: 3, c: 5, d: 6 }
console.log(target); // target is modified: { a: 1, b: 3, c: 5, d: 6 }

// Common use: cloning objects (shallow copy)
const original = { name: 'John', details: { age: 25 } };
const clone = Object.assign({}, original);
// Note: this is a shallow copy - nested objects are still referenced
```

### Object.fromEntries()

The Object.fromEntries() method transforms a list of key-value pairs into an object.

```javascript
const entries = [['name', 'Bob'], ['age', 25], ['city', 'Boston']];
const person = Object.fromEntries(entries);
console.log(person); // { name: 'Bob', age: 25, city: 'Boston' }

// Practical example: filtering object properties
const scores = { math: 85, science: 92, english: 78, history: 88 };
const highScores = Object.fromEntries(
    Object.entries(scores).filter(([subject, score]) => score >= 85)
);
console.log(highScores); // { math: 85, science: 92, history: 88 }
```

## Property Definition and Descriptors

### Object.defineProperty()

The Object.defineProperty() method defines a new property directly on an object, or modifies an existing property with precise control over its behavior.

```javascript
const user = {};

Object.defineProperty(user, 'name', {
    value: 'Alice',
    writable: true,    // Can the value be changed?
    enumerable: true,  // Will it show up in for...in loops and Object.keys()?
    configurable: true // Can the property be deleted or its descriptor modified?
});

Object.defineProperty(user, 'id', {
    value: 12345,
    writable: false,   // This property cannot be changed
    enumerable: false, // This won't appear in Object.keys()
    configurable: false
});

console.log(user.name); // 'Alice'
user.name = 'Bob'; // This works because writable is true
console.log(user.name); // 'Bob'

user.id = 54321; // This silently fails because writable is false
console.log(user.id); // 12345
```

### Object.defineProperties()

The Object.defineProperties() method defines new or modifies existing properties directly on an object, returning the object.

```javascript
const product = {};

Object.defineProperties(product, {
    name: {
        value: 'Smartphone',
        writable: true,
        enumerable: true
    },
    price: {
        value: 599,
        writable: true,
        enumerable: true
    },
    internalId: {
        value: 'PROD_001',
        writable: false,
        enumerable: false // Hidden from normal enumeration
    }
});

console.log(Object.keys(product)); // ['name', 'price'] - internalId is not enumerable
```

### Object.getOwnPropertyDescriptor()

The Object.getOwnPropertyDescriptor() method returns an object describing the configuration of a specific property on a given object.

```javascript
const obj = { regularProp: 'value' };

Object.defineProperty(obj, 'specialProp', {
    value: 'special',
    writable: false,
    enumerable: false,
    configurable: true
});

const regularDesc = Object.getOwnPropertyDescriptor(obj, 'regularProp');
console.log(regularDesc);
// { value: 'value', writable: true, enumerable: true, configurable: true }

const specialDesc = Object.getOwnPropertyDescriptor(obj, 'specialProp');
console.log(specialDesc);
// { value: 'special', writable: false, enumerable: false, configurable: true }
```

### Object.getOwnPropertyDescriptors()

The Object.getOwnPropertyDescriptors() method returns all own property descriptors of a given object.

```javascript
const obj = {
    prop1: 'value1',
    prop2: 'value2'
};

Object.defineProperty(obj, 'hiddenProp', {
    value: 'hidden',
    enumerable: false
});

const descriptors = Object.getOwnPropertyDescriptors(obj);
console.log(descriptors);
// Shows descriptors for all properties including non-enumerable ones
```

## Object State Control

### Object.freeze()

The Object.freeze() method freezes an object, preventing new properties from being added and existing properties from being removed or modified.

```javascript
const config = {
    apiUrl: 'https://api.example.com',
    timeout: 5000
};

Object.freeze(config);

// These operations will fail silently (or throw in strict mode)
config.apiUrl = 'https://malicious.com'; // Doesn't change
config.newProp = 'new value'; // Doesn't get added
delete config.timeout; // Doesn't get deleted

console.log(config); // Original object unchanged
console.log(Object.isFrozen(config)); // true

// Note: freeze is shallow - nested objects can still be modified
const user = { name: 'Alice', settings: { theme: 'dark' } };
Object.freeze(user);
user.settings.theme = 'light'; // This still works!
console.log(user.settings.theme); // 'light'
```

### Object.seal()

The Object.seal() method seals an object, preventing new properties from being added and marking all existing properties as non-configurable.

```javascript
const user = {
    name: 'Bob',
    age: 25
};

Object.seal(user);

// You can still modify existing properties
user.name = 'Robert'; // This works
user.age = 26; // This works

// But you cannot add or delete properties
user.email = 'bob@example.com'; // Doesn't get added
delete user.age; // Doesn't get deleted

console.log(Object.isSealed(user)); // true
console.log(user); // { name: 'Robert', age: 26 }
```

### Object.preventExtensions()

The Object.preventExtensions() method prevents new properties from ever being added to an object, but allows modification and deletion of existing properties.

```javascript
const obj = { existing: 'property' };

Object.preventExtensions(obj);

obj.existing = 'modified'; // This works
delete obj.existing; // This also works
obj.newProp = 'new'; // This doesn't work

console.log(Object.isExtensible(obj)); // false
```

## Prototype Methods

### Object.getPrototypeOf()

The Object.getPrototypeOf() method returns the prototype of the specified object.

```javascript
const parent = { parentMethod() { return 'from parent'; } };
const child = Object.create(parent);

console.log(Object.getPrototypeOf(child) === parent); // true

// Get prototype of built-in objects
console.log(Object.getPrototypeOf([])); // Array.prototype
console.log(Object.getPrototypeOf({})); // Object.prototype
```

### Object.setPrototypeOf()

The Object.setPrototypeOf() method sets the prototype of a specified object to another object or null.

```javascript
const animal = {
    speak() {
        return 'Some generic animal sound';
    }
};

const dog = {
    name: 'Buddy'
};

Object.setPrototypeOf(dog, animal);
console.log(dog.speak()); // 'Some generic animal sound'

// Warning: This is slower than Object.create() for performance
// Better to use Object.create() when creating new objects
```

### Object.hasOwnProperty() (Instance Method)

The hasOwnProperty() method returns a boolean indicating whether the object has the specified property as its own property.

```javascript
const person = { name: 'Alice', age: 30 };

console.log(person.hasOwnProperty('name')); // true
console.log(person.hasOwnProperty('toString')); // false (inherited from Object.prototype)

// More reliable way (in case hasOwnProperty is overridden)
console.log(Object.prototype.hasOwnProperty.call(person, 'name')); // true
```

## Comparison and Testing

### Object.is()

The Object.is() method determines whether two values are the same value, providing a more precise comparison than === in some edge cases.

```javascript
// Normal cases work like ===
console.log(Object.is(25, 25)); // true
console.log(Object.is('hello', 'hello')); // true
console.log(Object.is(25, '25')); // false

// Special cases where Object.is() differs from ===
console.log(Object.is(NaN, NaN)); // true (=== would be false)
console.log(Object.is(-0, +0)); // false (=== would be true)
console.log(Object.is(0, -0)); // false (=== would be true)

// Object comparison (still reference-based)
const obj1 = { a: 1 };
const obj2 = { a: 1 };
console.log(Object.is(obj1, obj2)); // false (different references)
console.log(Object.is(obj1, obj1)); // true (same reference)
```

## Instance Methods (Available on All Objects)

### toString()

The toString() method returns a string representing the object. Most objects override this method for more meaningful output.

```javascript
const obj = { name: 'Alice', age: 30 };

console.log(obj.toString()); // '[object Object]'

// Custom toString implementation
const person = {
    name: 'Bob',
    age: 25,
    toString() {
        return `Person: ${this.name}, age ${this.age}`;
    }
};

console.log(person.toString()); // 'Person: Bob, age 25'
console.log(String(person)); // Also calls toString()
```

### valueOf()

The valueOf() method returns the primitive value of the specified object. JavaScript calls this method automatically when an object is used in a context where a primitive value is expected.

```javascript
const numberObject = {
    value: 42,
    valueOf() {
        return this.value;
    }
};

console.log(numberObject + 8); // 50 (valueOf() is called automatically)
console.log(Number(numberObject)); // 42

// Built-in objects have their own valueOf implementations
const date = new Date();
console.log(date.valueOf()); // Returns timestamp (number of milliseconds)
```

### propertyIsEnumerable()

The propertyIsEnumerable() method returns a Boolean indicating whether the specified property is enumerable and is the object's own property.

```javascript
const obj = { prop1: 'value1' };

Object.defineProperty(obj, 'prop2', {
    value: 'value2',
    enumerable: false
});

console.log(obj.propertyIsEnumerable('prop1')); // true
console.log(obj.propertyIsEnumerable('prop2')); // false
console.log(obj.propertyIsEnumerable('toString')); // false (inherited)
```

## Advanced Utility Methods

### Object.getOwnPropertySymbols()

The Object.getOwnPropertySymbols() method returns an array of all symbol properties found directly upon a given object.

```javascript
const symbolKey1 = Symbol('key1');
const symbolKey2 = Symbol('key2');

const obj = {
    regularProp: 'value',
    [symbolKey1]: 'symbol value 1',
    [symbolKey2]: 'symbol value 2'
};

console.log(Object.keys(obj)); // ['regularProp'] - symbols don't appear
console.log(Object.getOwnPropertySymbols(obj)); // [Symbol(key1), Symbol(key2)]
```

Understanding these object methods will dramatically improve your ability to work with JavaScript objects effectively. Each method serves specific purposes in object manipulation, property control, and data transformation. The key to mastering these methods is understanding when to use static methods versus instance methods, and how different methods affect object mutability and property visibility.
