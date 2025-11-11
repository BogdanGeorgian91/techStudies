The native `Promise.all` takes an iterable (like an array) of Promises and returns a single Promise.

- This single Promise **resolves** when all of the input Promises have resolved. It resolves with an array of the resolved values, in the same order as the input Promises.
- This single Promise **rejects** as soon as _any_ of the input Promises rejects. It rejects with the reason of the first Promise that rejected.

The **`Promise.all()`** static method takes an iterable of promises as input and returns a single [`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise). This returned promise fulfills when all of the input's promises fulfill (including when an empty iterable is passed), with an array of the fulfillment values. It rejects when any of the input's promises rejects, with this first rejection reason.

The **`Promise`** object represents the eventual completion (or failure) of an asynchronous operation and its resulting value.

Our implementation will try to mimic this behavior.

JavaScript

```
function promiseAll(promises) {
  // Return a new Promise that encapsulates the logic
  return new Promise((resolve, reject) => {
    // Check if the input is an iterable (like an array).
    // If not, we should reject or throw an error, similar to native Promise.all
    if (!Array.isArray(promises)) {
      // For simplicity, we'll convert non-arrays to arrays, as Promise.all expects iterables.
      // A more strict implementation might throw a TypeError here.
      promises = Array.from(promises);
    }

    const results = []; // Array to store the resolved values
    let completed = 0;  // Counter for resolved promises
    const total = promises.length;

    // Handle the edge case of an empty array immediately
    if (total === 0) {
      resolve([]);
      return;
    }

    // Iterate over each promise in the input array
    promises.forEach((promise, index) => {
      // Ensure that 'promise' is actually a Promise.
      // If it's a non-Promise value, Promise.resolve wraps it.
      Promise.resolve(promise)
        .then(value => {
          results[index] = value; // Store the value at its original index
          completed++;           // Increment the counter

          // If all promises have completed, resolve the main promise
          if (completed === total) {
            resolve(results);
          }
        })
        .catch(reason => {
          // If any promise rejects, immediately reject the main promise
          reject(reason);
        });
    });
  });
}

// --- Test Cases ---

// Test Case 1: All promises resolve
const p1 = new Promise(res => setTimeout(() => res(1), 100));
const p2 = new Promise(res => setTimeout(() => res(2), 50));
const p3 = new Promise(res => setTimeout(() => res(3), 200));

promiseAll([p1, p2, p3])
  .then(values => {
    console.log("Test 1 (All resolved):", values); // Expected: [1, 2, 3]
  })
  .catch(error => {
    console.error("Test 1 (Should not error):", error);
  });

// Test Case 2: One promise rejects
const p4 = new Promise(res => setTimeout(() => res("Hello"), 100));
const p5 = new Promise((_, rej) => setTimeout(() => rej("Error in p5"), 20)); // Rejects quickly
const p6 = new Promise(res => setTimeout(() => res("World"), 500));

promiseAll([p4, p5, p6])
  .then(values => {
    console.log("Test 2 (Should not resolve):", values);
  })
  .catch(error => {
    console.error("Test 2 (One rejected):", error); // Expected: "Error in p5"
  });

// Test Case 3: Empty array
promiseAll([])
  .then(values => {
    console.log("Test 3 (Empty array):", values); // Expected: []
  })
  .catch(error => {
    console.error("Test 3 (Should not error for empty):", error);
  });

// Test Case 4: Non-promise values
promiseAll([1, Promise.resolve(2), "three"])
  .then(values => {
    console.log("Test 4 (Mixed values):", values); // Expected: [1, 2, "three"]
  })
  .catch(error => {
    console.error("Test 4 (Should not error for mixed):", error);
  });

// Test Case 5: All promises reject (should catch the first one)
const p7 = new Promise((_, rej) => setTimeout(() => rej("Error P7"), 100));
const p8 = new Promise((_, rej) => setTimeout(() => rej("Error P8"), 50)); // Rejects first
const p9 = new Promise((_, rej) => setTimeout(() => rej("Error P9"), 200));

promiseAll([p7, p8, p9])
  .then(values => {
    console.log("Test 5 (Should not resolve):", values);
  })
  .catch(error => {
    console.error("Test 5 (Multiple rejections, catches first):", error); // Expected: "Error P8"
  });
```

### Explanation of the `promiseAll` Implementation:

1. return new Promise((resolve, reject) => { ... });:
    
    The promiseAll function returns a new Promise. This is the central Promise that will eventually resolve with all results or reject with the first error. The resolve and reject functions passed to its executor control its final state.
    
2. Input Validation/Conversion:
    
    if (!Array.isArray(promises)) { promises = Array.from(promises); }
    
    The native Promise.all expects an iterable. While our simplified version primarily tests with arrays, a robust implementation might handle other iterables or throw a TypeError if the input isn't iterable, mirroring the native behavior more closely. We've opted for Array.from for basic iterable support.
    
3. **`results = []`, `completed = 0`, `total = promises.length`**:
    
    - `results`: An array to store the resolved values. We use this to maintain the order of the results, as promises can resolve in any order.
    - `completed`: A counter to track how many promises have successfully resolved.
    - `total`: The total number of promises to wait for.
4. if (total === 0):
    
    An important edge case: if an empty array of promises is passed, Promise.all should immediately resolve with an empty array. We handle this directly.
    
5. promises.forEach((promise, index) => { ... });:
    
    We iterate over each item in the input promises array.
    
6. Promise.resolve(promise):
    
    This is a crucial step that mimics Promise.all's behavior. If an item in the input array is not already a Promise, Promise.resolve() wraps it in a new Promise that immediately resolves with that value. This allows promiseAll to work with mixed arrays of Promises and regular values.
    
7. **`.then(value => { ... })`**:
    
    - When a promise resolves, its value is stored in the `results` array at its original `index`. This ensures the output array maintains the input order.
    - The `completed` counter is incremented.
    - If `completed` equals `total`, it means all promises have successfully resolved, so we `resolve(results)` the main `promiseAll` Promise.
8. **`.catch(reason => { ... })`**:
    
    - This is the "fail fast" mechanism. If _any_ promise in the input array rejects, we immediately `reject(reason)` the main `promiseAll` Promise with that specific reason. No further resolutions or rejections from other promises will matter to the main promise once it's rejected.

This implementation captures the core logic and behavior of the native `Promise.all`, demonstrating how you would build such an asynchronous control flow mechanism from scratch.