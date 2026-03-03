# Redux — Basic Setup, Needs, Benefits, and Examples

This short guide shows a minimal Redux setup using Redux Toolkit and React hooks (`useSelector`, `useDispatch`). It explains what you need, how to solve common problems, benefits, and includes concise code snippets.

## What you need

- Node dependency: `@reduxjs/toolkit` and `react-redux`.
- Optional: `redux-devtools-extension` (RTK includes devtools support by default).

Install with npm:

```bash
npm install @reduxjs/toolkit react-redux
```

Or with yarn:

```bash
yarn add @reduxjs/toolkit react-redux
```

## Basic file structure (recommended)

- `src/store/Store.js` — configure and export the store
- `src/features/counter/counterSlice.js` — example slice
- `src/main.jsx` — wrap the app with `<Provider>`
- `src/components/Counter.jsx` — using hooks to read & dispatch

## Minimal setup (code snippets)

1) Create a slice with Redux Toolkit

```js
// src/features/counter/counterSlice.js
import { createSlice } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => { state.value += 1; },
    decrement: (state) => { state.value -= 1; },
    addAmount: (state, action) => { state.value += action.payload; },
  },
});

export const { increment, decrement, addAmount } = counterSlice.actions;
export default counterSlice.reducer;
```

2) Configure the store

```js
// src/store/Store.js
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from '../features/counter/counterSlice';

export const store = configureStore({
  reducer: {
    counter: counterReducer,
  },
});

export default store;
```

3) Wrap your app with `Provider`

```jsx
// src/main.jsx
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

4) Use Redux in components with hooks

```jsx
// src/components/Counter.jsx
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { increment, decrement, addAmount } from '../features/counter/counterSlice';

export default function Counter() {
  const value = useSelector((state) => state.counter.value);
  const dispatch = useDispatch();

  return (
    <div>
      <h3>Counter: {value}</h3>
      <button onClick={() => dispatch(decrement())}>-</button>
      <button onClick={() => dispatch(increment())}>+</button>
      <button onClick={() => dispatch(addAmount(5))}>+5</button>
    </div>
  );
}
```

## Handling async logic

- Use `createAsyncThunk` (RTK) to create thunks that handle pending/fulfilled/rejected states.
- Alternatively, use RTK Query for data fetching with caching and automatic re-fetching.

Example skeleton with `createAsyncThunk`:

```js
import { createAsyncThunk } from '@reduxjs/toolkit';

export const fetchSomething = createAsyncThunk(
  'items/fetchSomething',
  async (arg, thunkAPI) => {
    const resp = await fetch('/api/items');
    return resp.json();
  }
);
```

Then handle `extraReducers` in your slice to respond to `.pending/.fulfilled/.rejected`.

## Common issues & how to solve them

- State mutations: RTK uses Immer so you can write "mutating" code safely in reducers. If you write custom reducers outside RTK, avoid mutating state.
- Too many re-renders: use `useSelector` with stable selectors and memoize expensive selectors with `reselect`.
- Complex async flows: prefer `createAsyncThunk` or RTK Query rather than raw thunks.
- Devtools not showing: ensure `NODE_ENV !== 'production'` and you're using the store from RTK (it enables devtools by default).

## Benefits of this approach

- Predictable single source of truth for global state.
- Less boilerplate with Redux Toolkit (`createSlice`, `configureStore`).
- Built-in support for immutability (Immer) and devtools.
- Easier testing of reducers and async thunks.
- Scales well: organize by feature (slices) and keep store configuration minimal.

## Tips and best practices

- Organize code by feature: `features/<featureName>/slice.js`, `features/<featureName>/components`.
- Keep UI state local (component state) when possible; use Redux for shared/global state.
- Use RTK Query for server state (caching, invalidation).
- Prefer typed selectors and actions if using TypeScript.

---

If you want, I can add a ready-to-copy `counter` feature files into the repo and wire them into `src/main.jsx`. Tell me if you prefer an example with `createAsyncThunk`, RTK Query, or TypeScript.
