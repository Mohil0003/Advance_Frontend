# useEffect

## What is it?
`useEffect` is a React Hook that lets you perform side effects in function components (data fetching, subscriptions, DOM mutations, timers, etc.). Effects run after render and can optionally clean up when dependencies change or the component unmounts.

## Why you need it
- Run code that interacts with the outside world (network, browser APIs).  
- Keep imperative behavior separate from render logic.  
- React will re-run the effect only when its dependencies change.

## Basic syntax
```js
useEffect(() => {
  // side effect
  return () => {
    // optional cleanup
  };
}, [dep1, dep2]);
```
- No dependency array: runs after every render.  
- Empty array (`[]`): runs once after mount (and cleanup on unmount).  
- Include dependencies to re-run when they change.

## Examples
### Data fetch with cleanup
```jsx
useEffect(() => {
  let mounted = true;
  fetch(url)
    .then(res => res.json())
    .then(data => mounted && setData(data));
  return () => { mounted = false; };
}, [url]);
```

### Timer with cleanup
```jsx
useEffect(() => {
  const id = setInterval(() => setTick(t => t + 1), 1000);
  return () => clearInterval(id);
}, []);
```

## Rules & best practices
- Never cause side effects directly inside render — use `useEffect` instead.
- Always declare effect dependencies; use the functional updater (`setState(prev => ...)`) to avoid stale values when appropriate.
- Clean up subscriptions, timers, or listeners in the returned function to avoid memory leaks.
- Use multiple `useEffect` calls to separate unrelated effects.
- Let ESLint (react-hooks/exhaustive-deps) guide dependency lists; add stable references (useCallback/useMemo) when needed.

## Common gotchas
- Missing dependencies can cause stale closures or bugs.  
- Putting non-primitive objects or inline functions in dependency arrays will re-run the effect unless stabilized.
- Long-running effects should be cancelable (abort controller, mounted flag).

## Quick reference
- Runs after render; cleans up before next run/unmount.  
- Use for I/O, subscriptions, timers, manual DOM updates.

---

**Tip:** prefer small, focused effects and keep dependency arrays explicit to avoid unexpected re-runs.