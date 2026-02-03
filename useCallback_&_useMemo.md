# 🧠 Master React Performance: `useCallback` vs `useMemo`

This guide is designed to help you understand the "Why", "How", and "When" of React's primary optimization hooks.

---

## 📑 Quick Reference Table

| Hook | What it Caches | Return Value | Main Purpose |
| :--- | :--- | :--- | :--- |
| **`useCallback`** | A **Function** | The actual function | Stable function references for Child Components. |
| **`useMemo`** | A **Value** | The result of a calculation | Preventing expensive logic from running every render. |

---

## 1. useCallback: Caching the "Action" ⚡

Every time a component re-renders, React re-creates all functions inside it. Even if the logic is the same, the "memory address" is new. `useCallback` stops this by "locking" the function in memory.



### ❓ Use Case
Used primarily when passing functions to child components that are wrapped in `React.memo` to prevent those children from re-rendering needlessly.

### 💻 Code Snippet
```javascript
const handleSave = useCallback(() => {
  console.log("Saving data...");
}, [dependency]); // Only changes if 'dependency' changes




## 2. useMemo: Caching the "Result" 💎

`useMemo` runs a function and stores the **output**. It will only run that function again if the dependencies change. Think of it as a "save game" for expensive math.



### ❓ Use Case
When you have a heavy calculation (like sorting 10,000 items, filtering a large array, or calculating factorials) that you don't want to repeat every time a unrelated state (like a theme toggle) updates.

### 💻 Code Snippet
```javascript
import React, { useState, useMemo } from 'react';

const ProductList = ({ items }) => {
  const [filter, setFilter] = useState('');

  // 💎 useMemo caches the RESULT of the filter
  // It won't re-run unless 'items' or 'filter' changes.
  const filteredItems = useMemo(() => {
    console.log("Filtering items..."); // Only logs when necessary
    return items.filter(item => item.name.includes(filter));
  }, [items, filter]); 

  return (
    <div>
      <input onChange={(e) => setFilter(e.target.value)} />
      <ul>
        {filteredItems.map(item => <li key={item.id}>{item.name}</li>)}
      </ul>
    </div>
  );
};
