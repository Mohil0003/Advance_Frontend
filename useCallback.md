# ⚡ React Hook: `useCallback`

### 📝 Definition
`useCallback` is a hook that returns a **memoized version of a function**. It "locks" a function in memory so it isn't recreated every time a component re-renders.

---

### ❓ Why do we need it?
In React, functions are objects. Every time a component renders, functions inside it are created from scratch. 
* **The Problem:** If you pass a "new" function to a child component, the child thinks its props have changed and re-renders, even if the logic is exactly the same.
* **The Solution:** `useCallback` ensures the function maintains the same **referential identity**.



---

### 💻 Code Example
This example shows a parent passing a stable function to a memoized child.

```javascript
import React, { useState, useCallback } from 'react';

// Child is memoized - it only re-renders if props change
const ChildButton = React.memo(({ handleClick }) => {
  console.log("Child rendered!");
  return <button onClick={handleClick}>Click Me</button>;
});

export const ParentComponent = () => {
  const [count, setCount] = useState(0);

  // This function is "cached"
  const memoizedFn = useCallback(() => {
    console.log("Button clicked");
  }, []); // Empty array = never changes

  return (
    <>
      <h1>Count: {count}</h1>
      <button onClick={() => setCount(count + 1)}>Increase Count</button>
      
      {/* Child won't re-render when count changes! */}
      <ChildButton handleClick={memoizedFn} />
    </>
  );
};
