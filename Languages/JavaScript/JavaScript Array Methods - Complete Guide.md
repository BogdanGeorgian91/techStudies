# JavaScript Array Methods - Complete Guide

Understanding array methods is fundamental to JavaScript programming. These methods fall into several categories based on their behavior and purpose.

## Mutating Methods (Modify the Original Array)

### push()

The push method adds one or more elements to the end of an array and returns the new length of the array.

```javascript
const fruits = ['apple', 'banana'];
const newLength = fruits.push('orange', 'grape');
console.log(fruits); // ['apple', 'banana', 'orange', 'grape']
console.log(newLength); // 4
```

### pop()

The pop method removes the last element from an array and returns that element. If the array is empty, it returns undefined.

```javascript
const fruits = ['apple', 'banana', 'orange'];
const removedFruit = fruits.pop();
console.log(fruits); // ['apple', 'banana']
console.log(removedFruit); // 'orange'
```

### unshift()

The unshift method adds one or more elements to the beginning of an array and returns the new length.

```javascript
const numbers = [2, 3, 4];
const newLength = numbers.unshift(0, 1);
console.log(numbers); // [0, 1, 2, 3, 4]
console.log(newLength); // 5
```

### shift()

The shift method removes the first element from an array and returns that element. This method changes the length of the array.

```javascript
const colors = ['red', 'green', 'blue'];
const firstColor = colors.shift();
console.log(colors); // ['green', 'blue']
console.log(firstColor); // 'red'
```

### splice()

The splice method changes the contents of an array by removing or replacing existing elements and/or adding new elements in place.

```javascript
const animals = ['cat', 'dog', 'elephant', 'fox'];
// Remove 2 elements starting at index 1, and insert 'bird' and 'fish'
const removed = animals.splice(1, 2, 'bird', 'fish');
console.log(animals); // ['cat', 'bird', 'fish', 'fox']
console.log(removed); // ['dog', 'elephant']
```

### sort()

The sort method sorts the elements of an array in place and returns the sorted array. By default, it converts elements to strings and compares them.

```javascript
const fruits = ['banana', 'apple', 'cherry'];
fruits.sort();
console.log(fruits); // ['apple', 'banana', 'cherry']

// For numbers, provide a compare function
const numbers = [10, 5, 40, 25, 1000, 1];
numbers.sort((a, b) => a - b); // Ascending order
console.log(numbers); // [1, 5, 10, 25, 40, 1000]
```

### reverse()

The reverse method reverses an array in place, with the first array element becoming the last and the last array element becoming the first.

```javascript
const letters = ['a', 'b', 'c', 'd'];
letters.reverse();
console.log(letters); // ['d', 'c', 'b', 'a']
```

### fill()

The fill method changes all elements in an array to a static value, from a start index to an end index.

```javascript
const numbers = [1, 2, 3, 4, 5];
numbers.fill(0, 2, 4); // Fill with 0 from index 2 to 4 (exclusive)
console.log(numbers); // [1, 2, 0, 0, 5]
```

## Non-Mutating Methods (Return New Arrays or Values)

### concat()

The concat method merges two or more arrays and returns a new array without changing the existing arrays.

```javascript
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = arr1.concat(arr2, [7, 8]);
console.log(combined); // [1, 2, 3, 4, 5, 6, 7, 8]
console.log(arr1); // [1, 2, 3] - unchanged
```

### slice()

The slice method returns a shallow copy of a portion of an array into a new array, selected from start to end (end not included).

```javascript
const animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];
const sliced = animals.slice(2, 4);
console.log(sliced); // ['camel', 'duck']
console.log(animals); // unchanged
```

### join()

The join method creates and returns a new string by concatenating all elements in an array, separated by a specified separator.

```javascript
const words = ['Hello', 'world', 'from', 'JavaScript'];
const sentence = words.join(' ');
console.log(sentence); // 'Hello world from JavaScript'

const csvData = ['apple', 'banana', 'cherry'];
console.log(csvData.join(',')); // 'apple,banana,cherry'
```

## Iteration Methods (Higher-Order Functions)

### forEach()

The forEach method executes a provided function once for each array element. It doesn't return a new array.

```javascript
const numbers = [1, 2, 3, 4, 5];
numbers.forEach((num, index) => {
    console.log(`Index ${index}: ${num * 2}`);
});
// Index 0: 2
// Index 1: 4
// Index 2: 6
// Index 3: 8
// Index 4: 10
```

### map()

The map method creates a new array populated with the results of calling a provided function on every element.

```javascript
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(num => num * 2);
console.log(doubled); // [2, 4, 6, 8, 10]
console.log(numbers); // [1, 2, 3, 4, 5] - unchanged

// More complex mapping
const users = [
    { name: 'Alice', age: 25 },
    { name: 'Bob', age: 30 }
];
const userNames = users.map(user => user.name);
console.log(userNames); // ['Alice', 'Bob']
```

### filter()

The filter method creates a new array with all elements that pass the test implemented by the provided function.

```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
const evenNumbers = numbers.filter(num => num % 2 === 0);
console.log(evenNumbers); // [2, 4, 6, 8, 10]

// Filtering objects
const products = [
    { name: 'Laptop', price: 1000, inStock: true },
    { name: 'Phone', price: 500, inStock: false },
    { name: 'Tablet', price: 300, inStock: true }
];
const availableProducts = products.filter(product => product.inStock);
console.log(availableProducts); // [Laptop and Tablet objects]
```

### reduce()

The reduce method executes a reducer function on each element of the array, resulting in a single output value.

```javascript
const numbers = [1, 2, 3, 4, 5];
const sum = numbers.reduce((accumulator, current) => accumulator + current, 0);
console.log(sum); // 15

// More complex reduction
const words = ['Hello', 'world', 'this', 'is', 'JavaScript'];
const wordLengths = words.reduce((acc, word) => {
    acc[word] = word.length;
    return acc;
}, {});
console.log(wordLengths); // { Hello: 5, world: 5, this: 4, is: 2, JavaScript: 10 }
```

### reduceRight()

The reduceRight method applies a function against an accumulator and each value of the array (from right-to-left) to reduce it to a single value.

```javascript
const numbers = [1, 2, 3, 4, 5];
const result = numbers.reduceRight((acc, current) => acc - current);
console.log(result); // 3 (5 - 4 - 3 - 2 - 1)
```

## Search and Test Methods

### find()

The find method returns the first element in the array that satisfies the provided testing function.

```javascript
const users = [
    { id: 1, name: 'Alice', age: 25 },
    { id: 2, name: 'Bob', age: 30 },
    { id: 3, name: 'Charlie', age: 35 }
];
const user = users.find(user => user.age > 28);
console.log(user); // { id: 2, name: 'Bob', age: 30 }
```

### findIndex()

The findIndex method returns the index of the first element in the array that satisfies the provided testing function.

```javascript
const numbers = [5, 12, 8, 130, 44];
const index = numbers.findIndex(num => num > 10);
console.log(index); // 1 (the index of 12)
```

### indexOf()

The indexOf method returns the first index at which a given element can be found in the array, or -1 if it is not present.

```javascript
const fruits = ['apple', 'banana', 'cherry', 'banana'];
console.log(fruits.indexOf('banana')); // 1
console.log(fruits.indexOf('grape')); // -1
```

### lastIndexOf()

The lastIndexOf method returns the last index at which a given element can be found in the array, or -1 if it is not present.

```javascript
const fruits = ['apple', 'banana', 'cherry', 'banana'];
console.log(fruits.lastIndexOf('banana')); // 3
```

### includes()

The includes method determines whether an array includes a certain value among its entries, returning true or false.

```javascript
const numbers = [1, 2, 3, 4, 5];
console.log(numbers.includes(3)); // true
console.log(numbers.includes(6)); // false
```

## Test Methods (Return Boolean)

### some()

The some method tests whether at least one element in the array passes the test implemented by the provided function.

```javascript
const numbers = [1, 2, 3, 4, 5];
const hasEvenNumber = numbers.some(num => num % 2 === 0);
console.log(hasEvenNumber); // true

const ages = [12, 15, 18, 21];
const hasAdult = ages.some(age => age >= 18);
console.log(hasAdult); // true
```

### every()

The every method tests whether all elements in the array pass the test implemented by the provided function.

```javascript
const numbers = [2, 4, 6, 8, 10];
const allEven = numbers.every(num => num % 2 === 0);
console.log(allEven); // true

const ages = [25, 30, 35, 40];
const allAdults = ages.every(age => age >= 18);
console.log(allAdults); // true
```

## Modern Array Methods

### flat()

The flat method creates a new array with all sub-array elements concatenated into it recursively up to the specified depth.

```javascript
const nestedArray = [1, 2, [3, 4, [5, 6]]];
const flattened = nestedArray.flat();
console.log(flattened); // [1, 2, 3, 4, [5, 6]]

const deepFlattened = nestedArray.flat(2);
console.log(deepFlattened); // [1, 2, 3, 4, 5, 6]
```

### flatMap()

The flatMap method first maps each element using a mapping function, then flattens the result into a new array.

```javascript
const sentences = ['Hello world', 'How are you'];
const words = sentences.flatMap(sentence => sentence.split(' '));
console.log(words); // ['Hello', 'world', 'How', 'are', 'you']
```

### from() (Static Method)

The Array.from() static method creates a new, shallow-copied Array instance from an array-like or iterable object.

```javascript
// From string
const chars = Array.from('hello');
console.log(chars); // ['h', 'e', 'l', 'l', 'o']

// From Set
const uniqueNumbers = Array.from(new Set([1, 2, 2, 3, 3, 4]));
console.log(uniqueNumbers); // [1, 2, 3, 4]

// With mapping function
const doubled = Array.from([1, 2, 3], x => x * 2);
console.log(doubled); // [2, 4, 6]
```

### isArray() (Static Method)

The Array.isArray() static method determines whether the passed value is an Array.

```javascript
console.log(Array.isArray([1, 2, 3])); // true
console.log(Array.isArray('hello')); // false
console.log(Array.isArray({ length: 3 })); // false
```

### of() (Static Method)

The Array.of() static method creates a new Array instance from a variable number of arguments.

```javascript
const arr1 = Array.of(7); // [7]
const arr2 = Array.of(1, 2, 3); // [1, 2, 3]

// Compare with Array constructor
const arr3 = Array(7); // [empty × 7]
const arr4 = Array(1, 2, 3); // [1, 2, 3]
```

Understanding these array methods will significantly improve your JavaScript programming skills. Each method has its specific use case, and mastering when to use mutating versus non-mutating methods is crucial for writing predictable, maintainable code.