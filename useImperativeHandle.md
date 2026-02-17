# useImperativeHandle

## What is it?
`useImperativeHandle` lets a function component customize the instance value that is exposed to parent components when using `ref`. It's used together with `forwardRef` to expose imperative methods.

## Why you need it
- Provide a controlled, limited imperative API (e.g., `focus`, `reset`) from a child component.  
- Hide internal implementation details while still enabling imperative operations.

## Basic syntax
```js
useImperativeHandle(ref, () => ({
  focus: () => inputRef.current.focus()
}), [/* deps */]);
```
- Must be used inside a component wrapped with `forwardRef`.

## Example
```jsx
const FancyInput = forwardRef((props, ref) => {
  const inputRef = useRef();
  useImperativeHandle(ref, () => ({
    focus: () => inputRef.current.focus(),
    clear: () => (inputRef.current.value = '')
  }), []);
  return <input ref={inputRef} {...props} />;
});

// parent
const ref = useRef();
<FancyInput ref={ref} />;
ref.current.focus();
```

## Rules & best practices
- Use only when imperative access is necessary (avoid encouraging imperative patterns).  
- Expose a small, stable API surface — don't leak internal state.  
- Keep the returned object stable using dependencies to avoid surprising changes.

## Common gotchas
- `useImperativeHandle` does NOT replace props/state-driven behavior — prefer declarative props where possible.
- Only works with `forwardRef` — calling it in a normal component will not make the parent ref receive the object.

---

**Tip:** prefer exposing a couple of clear methods (focus, reset) instead of exposing the entire DOM node unless needed.