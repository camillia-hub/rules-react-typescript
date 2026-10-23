# Components

## Component shape

- Always write function components with hooks; never write new class components.
  Rationale: hooks are the supported modern model; mixing component styles doubles the review and testing surface.
- Always type props explicitly with `interface Props` or `type Props`; never rely on `React.FC` (it hides generics and historically injected an implicit `children` prop).
- Always name the file after the component: `UserCard.tsx` exports `UserCard`.
- Always destructure props in the signature and give optional values sensible defaults where a default exists.

## Exports

- Always use named exports for components, hooks, and utilities.
  Rationale: named exports keep renames consistent across the codebase and make usages greppable.
- Never use default exports - with one exception: a page/route module may default-export its page component when the router framework requires it (for example file-based route files).
- Always export the props type next to the component when consumers compose or extend it.

## Rendering rules

- Always keep components pure: same props and state in, same JSX out; never mutate module-level or external variables during render.
- Never perform network requests or storage writes during render; they belong in event handlers or effects.
- Always key list items with stable ids from the data; never use array indexes for lists that can reorder, filter, or paginate.

```tsx
// Incorrect
{items.map((item, index) => <Row key={index} item={item} />)}

// Correct
{items.map((item) => <Row key={item.id} item={item} />)}
```

- Always handle loading, error, and empty states explicitly; never let a component silently render nothing when data is missing.
- Never store in state what can be computed from props during render; duplicated state drifts out of sync.

## Size and responsibility

- Always extract a custom hook when a component accumulates more than one piece of non-trivial stateful logic.
- Always extract complex conditional JSX into named variables or small components above the `return`; JSX is not a ternary obstacle course.
- Never define a component inside another component; every render creates a new component type and remounts the subtree, destroying state and focus.
- Always lift shared state to the closest common owner; never keep two copies of the same state in sibling components.
