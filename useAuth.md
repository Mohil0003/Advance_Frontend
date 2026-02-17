# useAuth — Authentication & auth-status (login / logout)

## What is it?
`useAuth` is a common custom hook + context pattern that centralizes authentication state and operations (login, logout, restore session, auth status) for your React app.

## Why you need it
- Provide a single source of truth for `user`, `isAuthenticated`, and `isLoading`.  
- Expose `login` / `logout` APIs to components without prop drilling.  
- Easily protect routes and handle token persistence/refresh logic in one place.

## Suggested API (example)
```js
const { user, isAuthenticated, isLoading, login, logout } = useAuth();
```
- `user` — authenticated user object or null
- `isAuthenticated` — boolean
- `isLoading` — boolean while restoring session / logging in
- `login(credentials)` — returns a Promise
- `logout()` — clears session

## Minimal implementation (Context + hook)
```js
import { createContext, useContext, useEffect, useState } from 'react';

const AuthContext = createContext();

export function AuthProvider({ children }) {
  const [user, setUser] = useState(null);
  const [token, setToken] = useState(() => localStorage.getItem('token'));
  const [isLoading, setIsLoading] = useState(Boolean(token));

  useEffect(() => {
    if (!token) return setIsLoading(false);
    // restore session (example: fetch /me)
    fetch('/api/me', { headers: { Authorization: `Bearer ${token}` } })
      .then(r => r.json())
      .then(data => setUser(data.user))
      .catch(() => { setToken(null); localStorage.removeItem('token'); })
      .finally(() => setIsLoading(false));
  }, [token]);

  async function login(credentials) {
    setIsLoading(true);
    const res = await fetch('/api/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(credentials)
    });
    if (!res.ok) throw new Error('Login failed');
    const { token, user } = await res.json();
    localStorage.setItem('token', token);
    setToken(token);
    setUser(user);
    setIsLoading(false);
  }

  function logout() {
    setUser(null);
    setToken(null);
    localStorage.removeItem('token');
    // optionally inform server: fetch('/api/logout', { method: 'POST' })
  }

  return (
    <AuthContext.Provider value={{ user, token, isAuthenticated: !!user, isLoading, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
}

export function useAuth() {
  const ctx = useContext(AuthContext);
  if (!ctx) throw new Error('useAuth must be used inside <AuthProvider>');
  return ctx;
}
```

## Protecting routes (React Router v6 example)
```jsx
import { Navigate } from 'react-router-dom';
function PrivateRoute({ children }) {
  const { isAuthenticated, isLoading } = useAuth();
  if (isLoading) return <div>Loading…</div>;
  return isAuthenticated ? children : <Navigate to="/login" replace />;
}
```

## Persistent auth & refresh tokens
- Prefer httpOnly, Secure cookies for refresh tokens (mitigates XSS).  
- Use short-lived access tokens + server-side refresh endpoints.  
- On 401 responses, attempt a refresh; if refresh fails, call `logout()`.

## Security best practices 🔒
- Do NOT store long-lived tokens in localStorage if you can use httpOnly cookies.  
- Always use HTTPS.  
- Use SameSite and Secure cookie flags for cookies.  
- Rotate and revoke refresh tokens on logout or suspicious activity.  
- Keep the client-side `user` object minimal (avoid sensitive data).

## Rules & best practices ✅
- Expose simple boolean states: `isAuthenticated`, `isLoading`, `isSubmitting`.  
- Centralize token handling (interceptors or fetch wrapper).  
- Keep login/logout side effects (redirects, analytics) in the provider or in components that call `login`/`logout`.
- Test flows: successful login, failed login, token expiry, refresh failure, logout.

## Common gotchas ⚠️
- Forgetting to clear tokens on logout → stale session.  
- Race conditions when multiple components try to refresh simultaneously — use a refresh lock.  
- Relying on localStorage-only auth for highly sensitive apps.

## Quick usage examples
- Show login button when not authenticated; show `Logout` and user name when authenticated.
- Use `useEffect` to restore session once on mount.

## When to use a library
- Use React Hook Form + existing auth libraries or a server-driven session when you need production-ready robustness.  
- For complex token rotation and concurrency, consider libraries or backend-managed sessions.

---

**Tip:** prefer a server-managed session (httpOnly cookie + refresh endpoint) for best security; use client tokens only when you control refresh/rotation securely.
