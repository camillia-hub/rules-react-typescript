# React + TypeScript Rules

A Camillia rules pack that enforces strict React and TypeScript practice as hard constraints: no `any`, disciplined hooks, predictable component structure.

## Rules at a glance

- **TypeScript strictness** (`rules/cam-ext-react-typescript-typescript-strictness.md`) - no `any`, `unknown` plus narrowing at boundaries, strict null handling, no non-null assertions, no unsound `as` casts.
- **Hooks** (`rules/cam-ext-react-typescript-hooks.md`) - exhaustive dependency arrays enforced as errors, rules-of-hooks compliance, mandatory effect cleanup, no effects for derived state.
- **Components** (`rules/cam-ext-react-typescript-components.md`) - function components only, named exports (default exports only where a router requires them for pages), typed props, stable list keys.
- **Project structure** (`rules/cam-ext-react-typescript-project-structure.md`) - colocated tests, one-way import layering, no barrel-file cycles, public-API-only imports.
- **DOM and side effects** (`rules/cam-ext-react-typescript-dom-and-effects.md`) - no direct DOM manipulation, refs for imperative needs, browser APIs guarded for SSR and tests.

## Install

Install from the Camillia Community Hub. On install, the rule files are placed under `.workspace/rules/` in your workspace and apply to every agent session there.

## License

MIT - see [LICENSE](LICENSE). Copyright 2026 Camillia contributors.
