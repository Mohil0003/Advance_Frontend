# 💎 React Hook: `useMemo`

### 📝 Definition
`useMemo` is a built-in React Hook that lets you **cache the result of a calculation** between re-renders. It remembers the **output** of a function so you don't have to perform expensive logic every time the component updates.

---

### ❓ The Problem
In React, when a component's state changes, the **entire function body re-executes**. 
* **Scenario:** Imagine you have a list of 5,000 users and a search filter. If you also have a "Dark Mode" toggle, clicking that toggle will cause the component to re-render.
* **The Issue:** Without `useMemo`, React will re-filter all 5,000 users every time you toggle the theme, even though the user list didn't change! This leads to "laggy" interfaces.

---

### 💡 The Solution
`useMemo` stores the **result** of the calculation. React will only re-run the code inside `useMemo` if one of the values in its **dependency array** has changed.

---

### 💻 Code Example: Expensive Calculation
In this example, we calculate the factorial of a number. We don't want this math to run if we are just toggling the UI theme.

```javascript
import React, { useState, useMemo } from 'react';

export const FactorialDemo = () => {
  const [number, setNumber] = useState(5);
  const [isDarkMode, setIsDarkMode] = useState(false);

  // 💎 useMemo caches the RESULT (the number)
  // Logic: "Only re-calculate if 'number' changes"
  const factorial = useMemo(() => {
    console.log("Running expensive math...");
    let result = 1;
    for (let i = 1; i <= number; i++) {
      result *= i;
    }
    return result;
  }, [number]); 

  return (
    <div style={{ 
      background: isDarkMode ? '#222' : '#fff', 
      color: isDarkMode ? '#fff' : '#000',
      padding: '20px' 
    }}>
      <h1>Factorial of {number} is: {factorial}</h1>
      
      <button onClick={() => setNumber(number + 1)}>
        Increase Number
      </button>

      <button onClick={() => setIsDarkMode(!isDarkMode)}>
        Toggle Dark Mode
      </button>

      <p>Check the console: "Running expensive math" won't appear when toggling Dark Mode!</p>
    </div>
  );
};
