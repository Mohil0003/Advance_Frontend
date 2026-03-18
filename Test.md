# Comprehensive Guide to Automated Testing

Automated testing is a crucial practice in modern software development. It ensures that our application behaves as expected, prevents regressions when new features are added, and provides documentation for how components and functions should behave. By implementing a robust testing strategy, we can confidently refactor code and maintain a high-quality codebase.

```
### 1. Configuration

### `babel.config.cjs` Setup
Jest requires Babel to transpile modern JavaScript (ESNext) and React (JSX) code into a format that Node.js can execute.

```javascript
module.exports = {
  presets: [
    ['@babel/preset-env', { targets: { node: 'current' } }],
    ['@babel/preset-react', { runtime: 'automatic' }] // Enables JSX without importing React
  ],
};
```
This configuration tells Babel to use the current Node.js version for environment targeting and the automatic React runtime, which removes the need to manually import React in every test file.

### 2. `package.json` Configuration
To properly execute React tests that require a DOM environment, you must specify `jsdom` as the test environment within your **package.json** configuration.

```json
{
  "name": "materialui",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "lint": "eslint .",
    "preview": "vite preview",
    "test": "jest"
  },
  "jest": {
    "testEnvironment": "jsdom"
  },
  "dependencies": {
    "@emotion/react": "^11.14.0",
    "@emotion/styled": "^11.14.1",
    "@fontsource/roboto": "^5.2.9",
    "@mui/icons-material": "^7.3.6",
    "@mui/material": "^7.3.6",
    "@reduxjs/toolkit": "^2.11.2",
    "react": "^19.2.0",
    "react-dom": "^19.2.0",
    "react-redux": "^9.2.0"
  },
  "devDependencies": {
    "@babel/preset-env": "^7.29.0",
    "@babel/preset-react": "^7.28.5",
    "@eslint/js": "^9.39.1",
    "@testing-library/jest-dom": "^6.9.1",
    "@testing-library/react": "^16.3.2",
    "@types/react": "^19.2.5",
    "@types/react-dom": "^19.2.3",
    "@vitejs/plugin-react": "^5.1.1",
    "babel-jest": "^30.3.0",
    "eslint": "^9.39.1",
    "eslint-plugin-react-hooks": "^7.0.1",
    "eslint-plugin-react-refresh": "^0.4.24",
    "globals": "^16.5.0",
    "jest": "^30.3.0",
    "jest-environment-jsdom": "^30.3.0",
    "vite": "^7.2.4"
  }
}

```
This ensures Jest simulates a browser environment by dynamically providing global objects like `window` and `document` that are required by React Testing Library.

## 3. Test Categories

### Unit Testing

**Unit tests** focus on verifying the core logic of individual functions isolated from other components.

#### `add` Function Logic
Here is an implementation testing a simple math utility function, cleanly covering **positive**, **negative**, and **zero** cases.

```javascript
// utils/math.js
export const add = (a, b) => a + b;

// utils/math.test.js
import { add } from './math';

describe('add function logic', () => {
  it('should correctly handle positive cases', () => {
    expect(add(2, 3)).toBe(5);
  });

  it('should correctly handle negative cases', () => {
    expect(add(5, -3)).toBe(2);
    expect(add(-5, -5)).toBe(-10);
  });

  it('should handle zero cases correctly', () => {
    expect(add(0, 5)).toBe(5);
    expect(add(0, 0)).toBe(0);
  });
});
```
**Explanation:** The tests utilize `describe` to group related test cases and `it` to specify expected behaviors. We validate the mathematical logic across varying edge cases using the assertion matcher `expect(...).toBe(...)`.

### Component Testing

**Component tests** ensure that React UI components render their template correctly and suitably respond to specific user interactions.

#### `Counter` Component Logic
This test suite validates that the Counter component structurally renders its initial text state and handles click events properly.

```jsx
// components/Counter.jsx
import { useState } from 'react';

export const Counter = () => {
  const [count, setCount] = useState(0);
  return (
    <div>
      <h1>Current Count: {count}</h1>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
};

// components/Counter.test.jsx

import { render, screen, fireEvent } from '@testing-library/react';
import '@testing-library/jest-dom';
import { Counter } from './Counter';

test('renders the correct initial text', () => {
  render(<Counter />);
  expect(screen.getByText(/Current Count: 0/i)).toBeInTheDocument();
});

test('button click increments the value', () => {
  render(<Counter />);
  const button = screen.getByText(/Increment/i);
  fireEvent.click(button);
  expect(screen.getByText(/Current Count: 1/i)).toBeInTheDocument();
});
```
**Explanation:** We effectively use `render` to mount our encapsulated component and `screen` queries to locate text nodes / interactive elements. User interaction handling is simulated securely using `fireEvent.click()`, verifying the resulting dynamic UI updates.

### Hook Testing

**Hook tests** effectively validate pure custom React hooks in isolation to guarantee robust state management boundaries.

#### `useToggle` Custom Hook
Testing hooks explicitly requires utilizing `renderHook` and defensively wrapping all state updates within the React `act` utility function.

```javascript
// hooks/useToggle.js
import { useState } from 'react';

export const useToggle = (initialValue = false) => {
  const [value, setValue] = useState(initialValue);
  const toggle = () => setValue((prev) => !prev);
  return [value, toggle];
};

// hooks/useToggle.test.js
import { renderHook, act } from '@testing-library/react';
import { useToggle } from './useToggle';

describe('useToggle hook', () => {
  test('should use initial value', () => {
    const { result } = renderHook(() => useToggle(true));
    expect(result.current[0]).toBe(true);
  });

  test('should toggle state', () => {
    const { result } = renderHook(() => useToggle(false));
    
    act(() => {
      result.current[1](); // Call the toggle function
    });

    expect(result.current[0]).toBe(true);
  });
});
```
**Explanation:** `renderHook` allows testing logic workflows internally without constructing complex wrapper DOM components. Critical actions mutating standard React lifecycles (like polling our `toggle` function) must reside wrapped inside `act(...)` so the simulation properly processes asynchronous queueing updates accurately.

## 4. Execution

To systematically execute your compiled logical test suite, simply use the designated test runner pipeline tool. Open your specific terminal working directory and run:

```bash
npm test
```

This foundational command automatically instructs Jest runtime workers and discovery components to find and securely run all structural files ending with generic `.test.js` / `.test.jsx` suffixes immediately.
