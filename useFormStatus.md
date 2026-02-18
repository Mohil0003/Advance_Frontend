# useFormStatus (react-dom hook guide)

## What is it?
`useFormStatus` is a built-in React hook from `react-dom` (React 19+) that provides form submission state. It returns information about the last form submission, including `pending` (submission in progress) and `action` (which form action is being processed). This hook is useful for disabling submit buttons and showing loading states during form submission.

## Why you need it
- Centralize form status logic and reduce boilerplate across forms.  
- Provide a consistent API for validation state and submission flow.

## Suggested API (example)
```js
import { useFormStatus } from 'react-dom';

const { pending, action } = useFormStatus();
```
`pending` (boolean): Indicates if the form action is currently submitting.  
`action` (FormAction): The form action being processed.

## Basic usage with Server Actions
```js
import { useFormStatus } from 'react-dom';

async function submitForm(formData) {
  // Server action - validates and processes data
  const email = formData.get('email');
  const password = formData.get('password');
  // Send to server...
  console.log('Submitted:', email, password);
}

export function LoginForm() {
  const { pending } = useFormStatus();

  return (
    <form action={submitForm}>
      <input type="email" name="email" required />
      <input type="password" name="password" required />
      
      <button type="submit" disabled={pending}>
        {pending ? 'Submitting…' : 'Login'}
      </button>
    </form>
  );
}
```

**Note:** `useFormStatus` tracks the parent `<form>` element's submission state. It only works with form actions (Server Actions in Next.js or similar frameworks).

## Examples of usage
```jsx
import { useFormStatus } from 'react-dom';

// Server Action (typically in a server-side file)
async function handleLogin(formData) {
  const email = formData.get('email');
  const password = formData.get('password');
  
  try {
    const response = await fetch('/api/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ email, password })
    });
    const data = await response.json();
    console.log('Success:', data);
  } catch (error) {
    console.error('Error:', error);
  }
}

// Client Component
export function LoginForm() {
  const { pending } = useFormStatus();

  return (
    <form action={handleLogin}>
      <div>
        <label htmlFor="email">Email:</label>
        <input 
          type="email"
          id="email"
          name="email" 
          required
        />
      </div>

      <div>
        <label htmlFor="password">Password:</label>
        <input 
          type="password"
          id="password"
          name="password" 
          required
        />
      </div>

      <button 
        type="submit" 
        disabled={pending}
      >
        {pending ? 'Submitting…' : 'Login'}
      </button>
    </form>
  );
}
```

## Rules & best practices
- Use `useFormStatus` only inside a `<form>` element with an `action` prop.
- Works best with React 19+ and frameworks that support Server Actions (Next.js, etc.).
- The `pending` state will automatically update when form submission starts/completes.
- Combine with client-side validation libraries (Zod, Yup) for better UX.
- For complex forms, pair with `useActionState` hook for handling action results and errors.

## Requirements & browser support
- **React 19+** with `react-dom`
- Requires a parent `<form>` element with an `action` attribute
- Works with Server Actions or custom form handlers
- Modern browsers with ES6+ support

---

**Tip:** expose minimal API (status + small setters) so components can stay declarative and testable.
