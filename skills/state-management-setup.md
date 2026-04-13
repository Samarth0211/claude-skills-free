---
name: state-management-setup
description: Set up state management (Zustand, Redux, Jotai)
category: frontend
tags: ["frontend", "state", "management"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# State Management Setup

> Set up state management (Zustand, Redux, Jotai)

You are a frontend developer setting up state management for a React application. The user wants to choose and configure a state management library (Zustand, Redux, or Jotai) with proper store structure, actions, and middleware.

## What to check first
- Verify Node.js and npm are installed: `node --version && npm --version`
- Check your React version: `npm list react` — state management works best with React 16.8+ for hooks
- Review your project structure: `ls -la src/` to see where you'll add store files

## Steps
1. Install your chosen state management library: `npm install zustand` (simplest), `npm install redux @reduxjs/toolkit react-redux` (most powerful), or `npm install jotai` (atomic approach)
2. Create a store directory: `mkdir -p src/store` to organize state management files separately
3. Define your store file with initial state, actions, and selectors using your chosen library's API
4. Wrap your app with Provider or DevTools depending on library (Redux Toolkit, Jotai use Provider; Zustand doesn't require it)
5. Create custom hooks that return store state and actions for use in components
6. Import and use hooks in components with `const { state, action } = useStore()`
7. Add Redux DevTools middleware if using Redux for time-travel debugging
8. Test store updates by dispatching actions and verifying state changes in React DevTools

## Code
```javascript
// ===== ZUSTAND SETUP (Simplest) =====
// src/store/useStore.js
import create from 'zustand';
import { devtools, persist } from 'zustand/middleware';

const useStore = create(
  devtools(
    persist(
      (set, get) => ({
        // State
        count: 0,
        user: null,
        todos: [],

        // Actions
        increment: () => set((state) => ({ count: state.count + 1 })),
        decrement: () => set((state) => ({ count: state.count - 1 })),
        setUser: (user) => set({ user }),
        addTodo: (todo) => set((state) => ({ todos: [...state.todos, todo] })),
        removeTodo: (id) =>
          set((state) => ({
            todos: state.todos.filter((t) => t.id !== id),
          })),

        // Selectors (optional, can be inline)
        getTodoCount: () => get().todos.length,
      }),
      { name: 'app-store' } // Persists to localStorage
    )
  )
);

export default useStore;

// ===== USAGE IN COMPONENT =====
// src/components/Counter.jsx
import useStore from '../store/useStore';

export function Counter() {
  const count = useStore((state) => state.count);
  const increment = useStore((state) => state.increment);
  const decrement
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

