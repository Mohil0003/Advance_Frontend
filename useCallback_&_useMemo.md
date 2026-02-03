# 🧠 Master React Performance: `useCallback` vs `useMemo`

This guide is designed to help you understand the "Why", "How", and "When" of React's primary optimization hooks.

---

## 📑 Quick Reference Table

| Hook | What it Caches (Remembers) | Return Value | Main Purpose |
| :--- | :--- | :--- | :--- |
| **`useCallback`** | A **Function** | The actual function | Stable function references for Child Components. |
| **`useMemo`** | A **Value** | The result of a calculation | Preventing expensive logic from running every render. |

---

## 1. useCallback: Caching the "Action" ⚡

Every time a component re-renders, React re-creates all functions inside it. Even if the logic is the same, the "memory address" is new. `useCallback` stops this.

### ❓ Use Case
When you pass a function to a child component that is wrapped in `React.memo`.

### 💻 Code Snippet
```javascript
const handleSave = useCallback(() => {
  console.log("Saving data...");
}, [dependency]); // Only changes if 'dependency' changes


Here is the complete content of the guide formatted as a .md file. You can copy the code block below and save it as README.md.

Markdown
# 🧠 Master React Performance: `useCallback` vs `useMemo`

This guide is designed to help you understand the "Why", "How", and "When" of React's primary optimization hooks.

---

## 📑 Quick Reference Table

| Hook | What it Caches (Remembers) | Return Value | Main Purpose |
| :--- | :--- | :--- | :--- |
| **`useCallback`** | A **Function** | The actual function | Stable function references for Child Components. |
| **`useMemo`** | A **Value** | The result of a calculation | Preventing expensive logic from running every render. |

---

## 1. useCallback: Caching the "Action" ⚡

Every time a component re-renders, React re-creates all functions inside it. Even if the logic is the same, the "memory address" is new. `useCallback` stops this.

### ❓ Use Case
When you pass a function to a child component that is wrapped in `React.memo`.

### 💻 Code Snippet
```javascript
const handleSave = useCallback(() => {
  console.log("Saving data...");
}, [dependency]); // Only changes if 'dependency' changes





2. useMemo: Caching the "Result" 💎
useMemo runs a function and stores the output. It will only run that function again if the dependencies change.

❓ Use Case
When you have a heavy calculation (like sorting 10,000 items or calculating factorials) that you don't want to repeat every time you toggle a UI switch.



const totalResult = useMemo(() => {
  return someHeavyMath(number);
}, [number]); // Only re-runs math if 'number' changes
