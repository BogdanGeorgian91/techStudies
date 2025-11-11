Both `React.memo` and `useMemo()` are powerful tools in React (and React Native) for **optimizing performance by memoization**, but they serve different purposes and operate at different levels.

### 1. `React.memo`

- **What it does:** `React.memo` is a **Higher-Order Component (HOC)**. It's used to memoize an entire **functional component** itself.
- **How it works:** ==It prevents a functional component from re-rendering if its props have not changed since the last render. It performs a **shallow comparison** of the component's props.==
- **When to use it:** When you have a functional component that:
    - Renders frequently.
    - Receives the same props most of the time.
    - Has a relatively high rendering cost (i.e., it takes time to compute its output).
    - Is wrapped by a parent component that re-renders often, but the child's props don't change.
- **Signature:** `React.memo(Component, [areEqual])`
    - `Component`: The functional component you want to memoize.
    - `areEqual` (optional): A custom comparison function. If provided, `React.memo` will use this instead of the default shallow comparison. It should return `true` if `prevProps` and `nextProps` are equal (i.e., the component _should not_ re-render), and `false` otherwise.

**Example:**
JavaScript
```
import React from 'react';

// This component will only re-render if 'name' or 'age' props change
const UserCard = React.memo(({ name, age }) => {
  console.log('UserCard re-rendered'); // You'll see this less often
  return (
    <div>
      <h3>{name}</h3>
      <p>Age: {age}</p>
    </div>
  );
});

function App() {
  const [count, setCount] = React.useState(0);
  const user = { name: 'Alice', age: 30 }; // This object reference remains constant

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>
        Increment Count: {count}
      </button>
      {/* UserCard's props (name, age) are primitive strings/numbers,
          and the object 'user' itself isn't being passed as prop,
          so UserCard will NOT re-render when count changes. */}
      <UserCard name={user.name} age={user.age} />

      {/* If you pass an object directly or a function that changes reference: */}
      {/*
      <UserCard user={user} /> // Problematic if 'user' object is re-created on parent re-renders
      <UserCard greet={() => console.log('Hi')} /> // Problematic, new function reference every render
      */}
    </div>
  );
}
```

**Important consideration:** `React.memo` performs a shallow comparison. If a prop is an object or an array, `React.memo` will only check if the _reference_ to that object/array has changed, not if its _contents_ have changed. This is where `useMemo()` and `useCallback()` often come into play _within_ the parent component to provide stable references.

### 2. `useMemo()` Hook
- **What it does:** ==`useMemo()` is a **Hook** that memoizes a **value** (the result of a computation).==
- **How it works:** It runs a function only when one of its specified dependencies has changed. It stores the _last computed value_ and returns it if the dependencies haven't changed.
- **When to use it:** ==When you have an expensive, pure computation within a component that needs to be re-run only when its specific inputs (dependencies) change. This prevents re-calculating the same value on every render if the relevant data hasn't changed.==
- **Signature:** `useMemo(factoryFunction, dependenciesArray)`
    - `factoryFunction`: The function that performs the expensive computation and returns the value you want to memoize.
    - `dependenciesArray`: An array of values (props, state, etc.). The `factoryFunction` will only re-execute if any value in this array has changed. If the array is empty (`[]`), the `factoryFunction` will run only once on the initial mount.

**Example:**
JavaScript
```
import React, { useState, useMemo } from 'react';

// Simulates an expensive computation
const calculateFilteredItems = (items, filter) => {
  console.log('Calculating filtered items...'); // You'll see this less often
  return items.filter(item => item.name.includes(filter));
};

function ItemList({ allItems }) {
  const [filter, setFilter] = useState('');
  const [count, setCount] = useState(0); // For demonstrating re-renders

  // The 'filteredItems' value will only be re-calculated if 'allItems' or 'filter' changes.
  // Changes to 'count' will NOT trigger this re-calculation.
  const filteredItems = useMemo(() => {
    return calculateFilteredItems(allItems, filter);
  }, [allItems, filter]); // Dependencies: re-run if allItems or filter change

  return (
    <div>
      <input
        type="text"
        value={filter}
        onChange={e => setFilter(e.target.value)}
        placeholder="Filter items"
      />
      <button onClick={() => setCount(count + 1)}>
        Rerender Parent (Count: {count})
      </button>

      <h4>Filtered Items:</h4>
      <ul>
        {filteredItems.map(item => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
    </div>
  );
}

// Example usage
function AppParent() {
  const items = useMemo(() => [
    { id: 1, name: 'Apple' },
    { id: 2, name: 'Banana' },
    { id: 3, name: 'Orange' },
  ], []); // Memoize the items array itself to ensure stable reference

  return <ItemList allItems={items} />;
}
```

### Key Differences Summarized

|   |   |   |
|---|---|---|
|**Feature**|**React.memo**|**useMemo()**|
|**What it memoizes**|An entire **functional component**|The **result of a computation (a value)**|
|**Type**|Higher-Order Component (HOC)|React Hook|
|**Syntax**|Wraps a component: `React.memo(MyComponent)`|Called inside a component: `useMemo(() => value, [deps])`|
|**Purpose**|Prevents **unnecessary component re-renders** based on prop changes.|Prevents **expensive re-calculations** within a component.|
|**Returns**|A memoized React component.|The memoized value.|
|**Dependencies**|Compares `props` (shallow by default). Can use custom `areEqual` function.|Explicit `dependenciesArray` you provide.|
|**Usage**|When passing props to a child component.|For expensive calculations, filtering, sorting, or providing stable object/array/function references to children.|

### When to Use Which (and in React Native)

The principles apply identically to both React for the web and React Native for mobile.

- **Use `React.memo` when:**
    - Your functional component is rendering unnecessarily, despite its props not changing.
    - The component's rendering logic is complex or computationally intensive.
    - You are passing stable props (primitives, or memoized objects/arrays/functions from `useMemo`/`useCallback`) to this component.
- **Use `useMemo()` when:**
    - You have a computationally expensive calculation (e.g., complex filtering, sorting, data transformation) inside your component that you only want to re-run when its specific inputs change.
    - You need to provide a **stable reference** to an object, array, or function to a child component that is wrapped in `React.memo` (or a `PureComponent` if it's a class component). This prevents the child from re-rendering just because a new object/array/function reference is created on every parent render. For functions, specifically, `useCallback()` is generally preferred as a specialized `useMemo` for functions.

**Caution:** Don't over-optimize! Memoization itself has a cost (for comparisons and memory). Use these tools strategically where you identify actual performance bottlenecks, rather than applying them everywhere. React's default rendering is often fast enough.