# 🧠 Master React Performance: `useCallback` vs `useMemo`

This guide is designed to help you understand the "Why", "How", and "When" of React's primary optimization hooks.

---

## 📑 Quick Reference Table

| Hook | What it Caches | Return Value | Main Purpose |
| :--- | :--- | :--- | :--- |
| **`useCallback`** | A **Function** | The actual function | Stable function references for Child Components. |
| **`useMemo`** | A **Value** | The result of a calculation | Preventing expensive logic from running every render. |

---

## 1. useCallback: Caching the "Action" ⚡

Every time a component re-renders, React re-creates all functions inside it. Even if the logic is the same, the "memory address" is new. `useCallback` stops this by "locking" the function in memory.



### ❓ Use Case
Used primarily when passing functions to child components that are wrapped in `React.memo` to prevent those children from re-rendering needlessly.

### 💻 Code Snippet
```javascript
const handleSave = useCallback(() => {
  console.log("Saving data...");
}, [dependency]); // Only changes if 'dependency' changes
