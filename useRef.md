# useRef

## What is it?
`useRef` returns a mutable ref object whose `.current` property persists across renders. It's used for accessing DOM nodes or storing mutable values that do not trigger re-renders.

## Why you need it
- Access DOM elements (focus, measurements).  
- Keep mutable values (timers, previous props) without causing re-renders.  
- Create stable references to values across renders.

## Basic syntax
```js
const ref = useRef(initialValue);
// read/write: ref.current
```

## Examples
### DOM access
```jsx
function TextInput() {
  const inputRef = useRef(null);
  useEffect(() => { inputRef.current.focus(); }, []);
  return <input ref={inputRef} />;
}
```

### Mutable value that doesn't re-render
```js
const latestValue = useRef(value);
useEffect(() => { latestValue.current = value; }, [value]);
// read latestValue.current inside callbacks without triggering re-renders
```

### Storing timers
```js
const timerRef = useRef(null);
useEffect(() => {
  timerRef.current = setTimeout(...);
  return () => clearTimeout(timerRef.current);
}, []);
```

## Rules & best practices
- Mutating `ref.current` does NOT cause a re-render — use state if the UI must update.
- Use `useRef` for instance-like storage or DOM refs; prefer `useState` for UI-driven values.
- For components that accept refs, use `forwardRef` to pass refs down.

## Common gotchas
- Don't rely on refs for controlled data flow — refs are imperative tools.
- Reading `ref.current` during server-side rendering may be `null`.

---

**Tip:** use `useRef` to keep the latest value for callbacks to avoid stale closures without forcing re-renders.