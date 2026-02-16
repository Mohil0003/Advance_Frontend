# React: useDeferredValue Hook

### Overview
`useDeferredValue` is a React Hook that lets you defer updating a non-urgent part of the UI. It acts as a "buffer" between a fast-changing state (like an input) and a slow-rendering component (like a heavy list).

### The "Mental Model"
Imagine a fast-typing user. Instead of the UI freezing on every character while trying to render a massive list, `useDeferredValue` keeps the **old list** on screen while it calculates the **new list** in the background.

---

### 💻 Code Implementation

```javascript
import { useState, useDeferredValue, useMemo } from 'react';

function SearchComponent() {
  const [query, setQuery] = useState("");
  
  // 1. Create a version of 'query' that lags behind
  const deferredQuery = useDeferredValue(query);

  // 2. Wrap heavy work in useMemo linked to the deferred value
  const heavyList = useMemo(() => {
    // This expensive loop only runs when the CPU is idle
    const results = [];
    for (let i = 0; i < 5000; i++) {
      results.push(<div key={i}>Result for {deferredQuery} #{i}</div>);
    }
    return results;
  }, [deferredQuery]);

  return (
    <div>
      <input 
        value={query} 
        onChange={(e) => setQuery(e.target.value)} 
        placeholder="Search 5,000 items..." 
      />

      {/* 3. Visual feedback: Dim the UI when 'stale' */}
      <div style={{ 
        opacity: query !== deferredQuery ? 0.5 : 1,
        transition: 'opacity 0.2s ease'
      }}>
        {heavyList}
      </div>
    </div>
  );
}
