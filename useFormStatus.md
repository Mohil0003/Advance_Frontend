# useFormStatus (custom hook guide)

## What is it?
`useFormStatus` is a pattern for a custom hook that manages common form UI states: `touched`, `dirty`, `isSubmitting`, `errors`, and `isValid`. It's not a built-in React hook — this doc shows a simple API and examples.

## Why you need it
- Centralize form status logic and reduce boilerplate across forms.  
- Provide a consistent API for validation state and submission flow.

## Suggested API (example)
```js
const { status, setTouched, setErrors, validate, submit } = useFormStatus({ initialValues, validateFn });
```
`status` could include: `{ touched, dirty, isSubmitting, errors, isValid }`.

## Basic implementation sketch
```js
function useFormStatus({ initialValues = {}, validateFn } = {}) {
  const [errors, setErrors] = useState({});
  const [touched, setTouched] = useState({});
  const [isSubmitting, setIsSubmitting] = useState(false);

  const validate = useCallback((values) => {
    const e = validateFn ? validateFn(values) : {};
    setErrors(e);
    return e;
  }, [validateFn]);

  const submit = useCallback(async (values, onSubmit) => {
    setIsSubmitting(true);
    const e = validate(values);
    if (Object.keys(e).length === 0) await onSubmit(values);
    setIsSubmitting(false);
  }, [validate]);

  return { status: { errors, touched, isSubmitting, isValid: Object.keys(errors).length === 0 }, setTouched, setErrors, validate, submit };
}
```

## Examples of usage
```jsx
const { status, setTouched, validate, submit } = useFormStatus({ initialValues, validateFn });

<input name="email" onBlur={() => setTouched(t => ({ ...t, email: true }))} />
<button onClick={() => submit(values, handleSubmit)} disabled={status.isSubmitting}>Send</button>
```

## Rules & best practices
- Keep validation logic separate (validators or library like Yup).  
- Keep `useFormStatus` focused on UI status — don't mix large domain logic into it.  
- Debounce expensive validations.  
- Prefer controlled inputs or use libraries (Formik, React Hook Form) for advanced needs.

## When to implement vs use a library
- Implement a small `useFormStatus` for simple forms and consistent UI states.  
- Use React Hook Form or Formik for complex forms, performance optimizations, and edge cases.

---

**Tip:** expose minimal API (status + small setters) so components can stay declarative and testable.