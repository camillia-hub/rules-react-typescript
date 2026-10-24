# Hooks

## Dependency discipline

- Always declare every reactive value (props, state, context, and values derived from them) in the dependency array.
  Rationale: a missing dependency is a stale closure reading yesterday's values - the bug ships silently.
- Always keep the `react-hooks/exhaustive-deps` lint rule enabled as an error in CI.
- Never suppress `exhaustive-deps` with an eslint-disable comment; restructure with `useRef`, an event-handler pattern, or by moving the logic fully inside the effect instead.

```tsx
// Incorrect: userId goes stale after navigation
useEffect(() => {
  loadUser(userId);
}, []);

// Correct
useEffect(() => {
  loadUser(userId);
}, [userId]);
```

## Rules of hooks

- Never call hooks conditionally, in loops, in callbacks, or after an early return.
- Always call hooks at the top level of a function component or of a custom hook.
- Always name custom hooks `useX`; the lint rules key off the `use` prefix.

## Effect hygiene

- Always return a cleanup function from effects that subscribe, open connections, or start timers.

```tsx
useEffect(() => {
  const subscription = source.subscribe(onEvent);
  return () => subscription.unsubscribe();
}, [source]);
```

- Never use an effect to compute derived state; compute it during render or memoize it with `useMemo`.
- Always keep one synchronization concern per effect; split effects that do unrelated work.
- Never set state in an effect in a way that re-triggers the same effect; guard with real conditions, not dependency-array tricks.
