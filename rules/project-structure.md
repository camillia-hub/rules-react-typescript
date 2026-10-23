# Project Structure

## Colocation

- Always colocate tests with the code they cover: `Button.tsx` is tested by `Button.test.tsx` in the same folder.
  Rationale: colocated tests move and rename with the code, and untested modules are obvious in review.
- Always name test files `<unit>.test.ts` / `<unit>.test.tsx` so the runner and reviewers pair them with the source at a glance.
- Always colocate component-scoped styles, fixtures, and stories next to the component.
- Never build a detached `__tests__` mirror tree; it drifts out of sync and hides coverage gaps.
- Always promote a utility to a shared folder only when at least two features use it; premature sharing creates coupling.
- Always keep `shared/` free of feature knowledge; if a `shared` module names a feature, the code belongs to that feature.
- Always keep route and page files thin - composition and data loading only; business logic lives in feature modules.
- Always keep generated code out of hand-edited folders, clearly marked as generated, and never hand-edit generated output.

## Imports and barrels

- Always import from a feature's public barrel (`index.ts`), never from its internal file paths.

```ts
// Incorrect
import { saveOrder } from '../orders/internal/saveOrder';

// Correct
import { saveOrder } from '../orders';
```

- Never import across a sibling feature's internals even within the same layer; go through the owning feature's public barrel.
- Never let barrel files form cycles: feature A's barrel must not re-export from feature B's barrel when B imports from A.
  Rationale: barrel cycles break tree-shaking and can yield `undefined` imports at runtime during circular module initialization.
- Never re-export the world from a barrel; export only the intended public API of the feature.
- Never put executable side effects in a barrel file; barrels re-export, nothing more.
- Never give a feature more than one public entry point; mid-hierarchy barrels inside a feature create side doors around the boundary.
- Always keep the import direction one-way: `shared` is imported by `features`, which is imported by `app` - never the reverse.
- Always order imports external packages first, then absolute aliases, then relative paths - enforced by lint, never sorted by hand.

## Naming and files

- Always use `PascalCase.tsx` for components, `useX.ts` for hooks, and `camelCase.ts` for other utilities.
- Never leave dead exports behind; delete code that has no importer instead of deprecating it in place.
- Always name a utility file for what it does (`formatCurrency.ts`), not for what it is (`helpers.ts`, `misc.ts`).
- Always delete a barrel export when its last consumer disappears; empty public APIs mislead the next reader.
- Never create a new top-level folder without checking it against the documented layering; new layers are an architecture decision, not a drive-by.
