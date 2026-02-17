# useState

## What is it?
`useState` is a React Hook that lets you add local state to function components. It returns a state value and a function to update that value.

## Why you need it
- Store component-specific data that changes over time (form inputs, toggles, counters).
- Trigger re-renders when state changes so the UI stays in sync with data.

## Basic syntax
```js
const [state, setState] = useState(initialValue);
```
- `initialValue` can be a value or a function for lazy initialization: `useState(() => expensiveInit())`.
- `setState` replaces the state with the provided value (or you can use the functional form to derive from previous state).

## Examples
### 1) Simple counter
```jsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(c => c + 1)}>Increment</button>
    </div>
  );
}
```

### 2) Updating object state (immutable)
```jsx
const [user, setUser] = useState({ name: 'Ana', age: 25 });

// update name:
setUser(prev => ({ ...prev, name: 'Alex' }));
```

### 3) Updating array state
```jsx
const [items, setItems] = useState([]);
setItems(prev => [...prev, newItem]);
setItems(prev => prev.filter(x => x.id !== idToRemove));
```

### 4) Lazy initialization (avoid expensive work on every render)
```jsx
const [data] = useState(() => computeInitialData());
```

## Rules & best practices
- Rules of Hooks: call hooks only at the top level and only from React function components or custom hooks.
- Prefer the functional updater `setState(prev => next)` when the new state depends on previous state — this prevents stale closures.
- Do not mutate state directly. Always return a new object/array.
- Keep state minimal: don't store values that can be derived from props or other state.
- Use multiple `useState` calls for independent pieces of state rather than one large object (unless values are tightly related).
- For complex state logic or many related updates, consider `useReducer`.

## Common gotchas
- setState is asynchronous — don't rely on the state value immediately after calling `setState`.
- Event handlers or timers capturing stale state — use functional updates or refs.

## Quick reference
- Returns: [value, setValue]
- When to use: local UI state (form fields, toggles, counters)
- Alternatives: `useReducer` (complex state), `useRef` (mutable value that doesn't cause re-render)

---

**Example one-line tip:** use the functional form (`setState(prev => ...)`) when updating based on the previous value to avoid race conditions.  

© Hooks Cheat Sheet — concise guide for `useState` usage.