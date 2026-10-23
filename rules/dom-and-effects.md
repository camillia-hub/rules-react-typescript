# DOM and Side Effects

## Direct DOM manipulation

- Never manipulate the DOM directly: no `document.querySelector`, no `element.style` mutation, no `appendChild`, no `innerHTML`.
  Rationale: React owns those nodes; outside mutation desynchronizes the virtual DOM and corrupts rendering in ways that reproduce only in production.
- Always express the UI as a function of state and props; change data, not nodes.

```tsx
// Incorrect
document.getElementById('price')!.textContent = String(price);

// Correct
<span id="price">{price}</span>
```

- Always use `useRef` for the rare legitimate imperative need (focus, scroll position, measuring); never reach for `document.getElementById` inside a component.

```tsx
const inputRef = useRef<HTMLInputElement>(null);
// later, in an event handler or effect:
inputRef.current?.focus();
```

- Never store DOM nodes in React state; nodes belong in refs - state changes re-render, ref changes do not.
- Always use `createPortal` for overlays that must escape a stacking context; never append nodes to `document.body` by hand.
- Always toggle visual variation through CSS classes driven by state, never through inline style mutation.

## Browser APIs

- Always guard browser-only globals (`window`, `document`, `localStorage`, `matchMedia`) behind an effect or an environment check so server-side rendering and test renderers never touch them during render.
- Never read `window.location` or `navigator` during render; read them inside effects or event handlers.
- Always remove listeners attached to `window` or `document` in the effect cleanup function.
- Always prefer React event props (`onScroll`, `onResize`) over manual `addEventListener` when the target is a React-rendered element; manual listeners are for window-level or non-bubbling events.
- Always cancel `requestAnimationFrame` handles and disconnect `MutationObserver` / `ResizeObserver` instances in the effect cleanup.
- Never measure layout during render; measure in `useLayoutEffect` and store the result in state when it affects the output.

## External widgets

- Always wrap non-React widgets (charts, maps, rich-text editors) in a dedicated component that owns mount, update, and unmount via refs and effects.
- Always destroy the third-party instance in the effect cleanup; leaked widget instances double-listen and double-render.
- Never mix React rendering and third-party-library rendering into the same DOM node; the wrapper owns the node, React owns everything around it.
