# TypeScript Strictness

## The `any` ban

- Never use `any` - not in annotations, not in assertions (`as any`), not as a generic default.
  Rationale: `any` disables the type checker transitively; one `any` poisons every value it touches.
- Always use `unknown` for values entering from outside the program (fetch responses, `JSON.parse`, form data) and narrow before use.

```ts
// Incorrect
const data: any = await res.json();
return data.user.name;

// Correct
const data: unknown = await res.json();
const parsed = UserSchema.parse(data); // zod schema or a hand-written type guard
return parsed.user.name;
```

- Never use `@ts-ignore` or `@ts-expect-error` without a comment stating why and a linked issue.

## Null handling

- Always compile with `"strict": true` (which includes `strictNullChecks`) in every tsconfig; never relax strict flags per package.
- Always handle `null` and `undefined` at the boundary where they can occur; never let `| undefined` leak deeper into the program than necessary.
- Never use the non-null assertion (`x!`); narrow with a guard or throw instead.

```ts
// Incorrect
const root = document.getElementById('app')!;

// Correct
const root = document.getElementById('app');
if (!root) throw new Error('missing #app root element');
```

- Always model absence explicitly: pick one convention per codebase (for example `T | null` for "loaded but empty" and `T | undefined` for "not loaded yet") and document it.

## Types at boundaries

- Never use an `as` cast on data you do not control; validate with a schema or a type predicate instead.
- Always annotate return types on exported functions; inference is for locals.
- Always prefer `readonly` arrays and `readonly` properties for data crossing component or module boundaries.
