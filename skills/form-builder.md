---
name: form-builder
description: Build forms with validation (React Hook Form, Formik)
category: frontend
tags: ["frontend", "forms", "validation"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# Form Builder

> Build forms with validation (React Hook Form, Formik)

You are a React forms expert. The user wants to build production-ready forms with built-in validation using React Hook Form or Formik.

## What to check first
- Run `npm list react-hook-form formik` to see which library is installed in your project
- Verify React version is 16.8+ (hooks requirement for React Hook Form)
- Check if you have a form schema validator like `yup` or `zod` installed: `npm list yup zod`

## Steps
1. Install React Hook Form: `npm install react-hook-form` (or `npm install formik` for alternative)
2. Import `useForm` hook and destructure `register`, `handleSubmit`, `formState` from `react-hook-form`
3. Define validation schema using `yup` or `zod` and pass to `useForm` via `resolver` parameter
4. Wrap input elements with `register()` to connect them to form state — e.g. `<input {...register('email', { required: 'Email required' })} />`
5. Access `formState.errors` to conditionally render error messages below each field
6. Attach `handleSubmit(onSubmit)` to the form's `onSubmit` handler to validate before submission
7. Inside the `onSubmit` callback, access validated data as the first parameter and send to API
8. For conditional fields, use `watch()` to observe field values and conditionally render inputs

## Code
```jsx
import React from 'react';
import { useForm, Controller } from 'react-hook-form';
import { yupResolver } from '@hookform/resolvers/yup';
import * as yup from 'yup';

const validationSchema = yup.object({
  email: yup.string().email('Invalid email').required('Email is required'),
  password: yup.string().min(8, 'Min 8 chars').required('Password required'),
  confirmPassword: yup.string().oneOf([yup.ref('password')], 'Passwords must match'),
  role: yup.string().required('Select a role'),
  terms: yup.boolean().oneOf([true], 'Must accept terms'),
});

export function FormBuilder() {
  const { register, handleSubmit, formState: { errors }, watch, control } = useForm({
    resolver: yupResolver(validationSchema),
  });

  const selectedRole = watch('role');

  const onSubmit = (data) => {
    console.log('Form submitted:', data);
    // Send to API: fetch('/api/register', { method: 'POST', body: JSON.stringify(data) })
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)} className="form-container">
      <div className="form-group">
        <label>Email</label>
        <input
          type="email"
          {...register('email')}
          placeholder="you@example.
```

*Note: this example was truncated in the source. See [the GitHub repo](https://github.com/Samarth0211/claude-skills-hub) for the latest full version.*

## Common Pitfalls

- Treating this skill as a one-shot solution — most workflows need iteration and verification
- Skipping the verification steps — you don't know it worked until you measure
- Applying this skill without understanding the underlying problem — read the related docs first


## When NOT to Use This Skill

- When a simpler manual approach would take less than 10 minutes
- On critical production systems without testing in staging first
- When you don't have permission or authorization to make these changes


## How to Verify It Worked

- Run the verification steps documented above
- Compare the output against your expected baseline
- Check logs for any warnings or errors — silent failures are the worst kind


## Production Considerations

- Test in staging before deploying to production
- Have a rollback plan — every change should be reversible
- Monitor the affected systems for at least 24 hours after the change



---
*From [CLSkills.in](https://clskills.in/browse) — 2,300+ free Claude Code skills*

