# React: useTransition Hook

### Overview
`useTransition` is a React Hook that lets you update state without blocking the UI. It marks specific state updates as "Transitions" (low priority).

### The "Mental Model"
It provides a **Priority Pass**. Urgent updates (typing, clicking) happen immediately. Transition updates (filtering, tab switching) happen in the background. It also provides a built-in "isPending" state.

---

### 💻 Code Implementation

```javascript
import { useState, useTransition } from 'react';

function TabSwitcher() {
  const [tab, setTab] = useState('home');
  
  // 1. Initialize the transition hook
  const [isPending, startTransition] = useTransition();

  function handleTabChange(nextTab) {
    // 2. Mark the switch as a transition
    startTransition(() => {
      // If 'heavy-table' is slow, the UI won't freeze!
      setTab(nextTab);
    });
  }

  return (
    <div>
      <nav>
        <button onClick={() => handleTabChange('home')}>Home</button>
        <button onClick={() => handleTabChange('heavy-table')}>Heavy Table</button>
      </nav>

      {/* 3. Immediate feedback using isPending */}
      {isPending && <p style={{ color: 'blue' }}>Loading new view...</p>}

      <div style={{ opacity: isPending ? 0.6 : 1 }}>
        {tab === 'home' ? <HomeView /> : <SlowTableView />}
      </div>
    </div>
  );
}
