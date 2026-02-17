# useContext

## What is it?
`useContext` is a React Hook that lets you subscribe to React Context values from a function component. It returns the current context value for the nearest matching Provider above in the tree.

## Why you need it
- Share data across many components without prop drilling (theme, auth, locale, etc.).
- Read context values directly inside components without wrapping with `<Context.Consumer>`.

## Basic syntax
```js
const value = useContext(MyContext);
```
- `MyContext` is created with `React.createContext(defaultValue)`.
- The returned `value` is whatever the nearest `<MyContext.Provider value={...}>` supplies.

## Examples
```jsx
const ThemeContext = createContext('light');

function Toolbar() {
  const theme = useContext(ThemeContext);
  return <div className={theme}>...</div>;
}
```

## Rules & best practices
- Call `useContext` at the top level of your component (Rules of Hooks).
- Context updates cause all consuming components to re-render — avoid putting rapidly changing values in context.
- Use multiple contexts for independent concerns instead of a single large context.
- Wrap access in a small custom hook (e.g., `useTheme()`) to centralize validation and defaults.

## Common gotchas
- `useContext(SomeContext)` will return the default value if no Provider is present.
- Don't use context as global mutable state for frequently changing data; prefer local state or external stores.

## Quick reference
- Use for cross-cutting data (theme, locale, auth).  
- Combine with `useMemo`/`useCallback` on Provider `value` to avoid unnecessary re-renders.

---

**Tip:** create a small custom hook (e.g., `useAuth()`) that wraps `useContext(AuthContext)` for clearer intent and easier testing.