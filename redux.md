# Redux — What it is, What it's for, Why you need it, and a Single Basic Example

## What is Redux?

Redux is a predictable state container for JavaScript apps. It centralizes application state in a single store, enforces unidirectional data flow, and makes state changes explicit and testable.

## What is it used for?

- Managing global/shared state across many components.
- Coordinating complex UI state and async data flows.
- Keeping a single source of truth that can be time-traveled and inspected (devtools).

Use Redux when multiple parts of your app need to read or update the same state, or when you want predictable, testable state transitions.

## Why / When you need Redux

- Your app has cross-cutting state (auth, theme, cart, user data).
- You need coordinated updates from many components.
- You require easy debugging (Redux DevTools) or predictable undo/redo flows.

Not every app needs Redux — for small or isolated UI state, prefer component-local `useState` / `useReducer`.

## Quick prerequisites

Install the minimal dependencies:

```bash
npm install @reduxjs/toolkit react-redux
```

## Single basic example (minimal, copy-paste friendly)

This example uses Redux Toolkit for brevity. It shows a slice, store, Provider wiring, and a small component.

1) Slice (counter)

```js
// features/counter/counterSlice.js
import { createSlice } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => { state.value += 1; },
    decrement: (state) => { state.value -= 1; },
  },
});

export const { increment, decrement } = counterSlice.actions;
export default counterSlice.reducer;
```

2) Store configuration

```js
// store/Store.js
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from '../features/counter/counterSlice';

export const store = configureStore({
  reducer: { counter: counterReducer },
});

export default store;
```

3) App entry — Provider

```jsx
// main.jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import { Provider } from 'react-redux';
import App from './App';
import { store } from './store/Store';

ReactDOM.createRoot(document.getElementById('root')).render(
  <Provider store={store}>
    <App />
  </Provider>
);
```

4) Component using hooks

```jsx
// components/Counter.jsx
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { increment, decrement } from '../features/counter/counterSlice';

export default function Counter() {
  const value = useSelector((state) => state.counter.value);
  const dispatch = useDispatch();

  return (
    <div>
      <h3>Counter: {value}</h3>
      <button onClick={() => dispatch(decrement())}>-</button>
      <button onClick={() => dispatch(increment())}>+</button>
    </div>
  );
}
```

Drop these files into the matching paths and run your app. The example is intentionally minimal — replace paths if your repo uses a different layout.

## Short benefits summary

- Single source of truth for shared state.
- Predictable updates and better debugging (devtools).
- Less boilerplate when using Redux Toolkit.

---

