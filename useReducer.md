# useReducer

## What is it?
`useReducer` is a React Hook for managing component state using a reducer function (similar to Redux reducers). It's suited for complex or interrelated state updates.

## Why you need it
- Manage complex state logic and transitions in a predictable way.  
- Centralize update logic for multiple related state values.  
- Improve testability for state transitions.

## Basic syntax
```js
const [state, dispatch] = useReducer(reducer, initialArg, init);
```
- `reducer(state, action)` returns the new state.  
- `dispatch(action)` schedules a state update.

## Examples
### Counter reducer
```js
function reducer(state, action) {
  switch (action.type) {
    case 'increment': return { count: state.count + 1 };
    case 'decrement': return { count: state.count - 1 };
    default: return state;
  }
}
const [state, dispatch] = useReducer(reducer, { count: 0 });
```

### Form-like state
```js
function reducer(state, action) {
  switch (action.type) {
    case 'change': return { ...state, [action.name]: action.value };
    case 'reset': return action.initial;
    default: return state;
  }
}
```

## Rules & best practices
- Keep reducers pure (no side effects).  
- Use action objects (type + payload) for clarity.  
- Prefer `useReducer` over `useState` when state updates are complex or depend on previous state extensively.
- `dispatch` is stable — you don't need to wrap it with `useCallback`.
- Use lazy initialization (`init` third argument) for expensive initial state.

## Common gotchas
- Avoid putting side effects inside reducers — use `useEffect` for side effects.  
- Overengineering: don't use `useReducer` for trivially simple state.

---

**Tip:** combine `useReducer` with context to create a lightweight state manager for a component subtree.