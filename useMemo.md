```markdown
# 💎 React Hook: `useMemo`

### 📝 Definition
`useMemo` is a hook that returns a **memoized value**. It remembers the **result** of a function so you don't have to run expensive calculations every time the component renders.

---

### ❓ Why do we need it?
Some functions are "expensive" (they take a long time to run, like sorting 5,000 items). 
* **The Problem:** React re-runs everything on every render. If you change a theme or a counter, you don't want to re-sort those 5,000 items again.
* **The Solution:** `useMemo` stores the **output** and only re-calculates it if the specific inputs change.



---

### 💻 Code Example
Calculating a factorial is a perfect use case for `useMemo`.

```javascript
import React, { useState, useMemo } from 'react';

export const FactorialDemo = () => {
  const [number, setNumber] = useState(5);
  const [theme, setTheme] = useState(false);

  // 💎 useMemo caches the NUMBER (the result)
  const result = useMemo(() => {
    console.log("Calculating Factorial...");
    let res = 1;
    for (let i = 1; i <= number; i++) res *= i;
    return res;
  }, [number]); // Only re-calculates if 'number' changes

  return (
    <div style={{ background: theme ? '#333' : '#fff' }}>
      <h1>Result: {result}</h1>
      <button onClick={() => setNumber(number + 1)}>Increase Number</button>
      <button onClick={() => setTheme(!theme)}>Toggle Theme</button>
      <p>(Toggling theme won't trigger the calculation!)</p>
    </div>
  );
};
