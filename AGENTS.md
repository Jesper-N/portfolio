# AGENTS.md - Coding Guidelines for AI Agents

## Build/Development Commands

```bash
# Development server
pnpm dev

# Build for production
pnpm build

# Type checking (REQUIRED before committing)
pnpm check

# Watch mode for type checking
pnpm check:watch

# Preview production build
pnpm preview
```

**Note:** No test framework is configured. No linting (ESLint/Prettier) is configured. Always run `pnpm check` before committing.

## Project Overview

- **Framework:** SvelteKit 2 with Svelte 5 (runes mode)
- **Language:** TypeScript (strict mode enabled)
- **Styling:** Tailwind CSS v4 + tw-animate-css
- **Package Manager:** pnpm
- **UI Components:** shadcn-svelte pattern with bits-ui primitives
- **Font:** Geist Sans via @fontsource

## Code Style Guidelines

### TypeScript/Svelte

1. **Always use TypeScript** - All `.svelte` files must have `lang="ts"`
2. **Strict mode** - No `any` types unless absolutely necessary (use `@typescript-eslint/no-explicit-any` comment if needed)
3. **Svelte 5 Runes** - Use runes syntax exclusively:
   - `$props()` for component props
   - `$bindable()` for two-way binding
   - `$derived()` for computed values
   - `{@render children?.()}` for slots
4. **Comments must be intentional** - Only add comments when code is non-obvious and the comment explains intent or a real constraint; do not add routine, restating, or decorative comments.

### Component Structure

Components follow the shadcn-svelte pattern:

```svelte
<script lang="ts" module>
  // 1. Variant definitions using tailwind-variants
  import { tv, type VariantProps } from "tailwind-variants";
  
  export const componentVariants = tv({
    base: "...",
    variants: { ... },
    defaultVariants: { ... }
  });
  
  export type ComponentVariant = VariantProps<typeof componentVariants>["variant"];
  export type ComponentSize = VariantProps<typeof componentVariants>["size"];
</script>

<script lang="ts">
  // 2. Component logic
  import { cn, type WithElementRef } from "$lib/utils.js";
  import type { HTMLAttributes } from "svelte/elements";
  
  let {
    ref = $bindable(null),
    class: className,
    variant = "default",
    children,
    ...restProps
  }: WithElementRef<HTMLAttributes<HTMLDivElement>> & {
    variant?: ComponentVariant;
  } = $props();
</script>

<!-- 3. Template with data-slot attributes -->
<div
  bind:this={ref}
  data-slot="component-name"
  class={cn(componentVariants({ variant }), className)}
  {...restProps}
>
  {@render children?.()}
</div>
```

### Component Organization

Components live in `src/lib/components/ui/<component-name>/`:

```
button/
  button.svelte      # Main component with variants
  index.ts           # Exports (see pattern below)
```

Index.ts pattern:
```typescript
import Root, { type ButtonProps, buttonVariants } from "./button.svelte";

export {
  Root,
  type ButtonProps as Props,
  Root as Button,      // PascalCase alias
  buttonVariants,
  type ButtonProps,
};
```

### Imports & Path Aliases

Use these path aliases:
- `$lib` → `src/lib`
- `$lib/components` → `src/lib/components`
- `$lib/components/ui` → `src/lib/components/ui`
- `$lib/utils` → `src/lib/utils`
- `$lib/hooks` → `src/lib/hooks`

**Import order:**
1. External libraries
2. Svelte types (`svelte/elements`, `svelte`)
3. Internal utilities (`$lib/utils.js`)
4. Internal components

Always include `.js` extension for internal imports: `from "$lib/utils.js"`

### Styling Conventions

1. **Use `cn()` utility** for class merging: `class={cn(baseVariants({ variant }), className)}`
2. **tailwind-variants** for component variants - never inline conditional classes
3. **CSS variables** defined in `src/routes/layout.css` - use theme tokens like `bg-background`, `text-foreground`
4. **data-slot attributes** on root elements: `data-slot="component-name"`
5. **Group selectors** for nested hover: `[a]:hover:bg-accent/50`
6. **Accessibility:** Include `focus-visible:ring-ring/50`, `aria-invalid` states, `disabled:` states

### Naming Conventions

- **Components:** PascalCase (e.g., `Button`, `CardHeader`)
- **Files:** kebab-case (e.g., `card-header.svelte`)
- **Variant types:** Suffix with type name (e.g., `ButtonVariant`, `ButtonSize`)
- **Props interface:** Suffix with `Props` (e.g., `ButtonProps`)
- **Ref props:** Always named `ref`, use `$bindable(null)`

### Error Handling

- No try/catch in UI components - let errors bubble
- Use TypeScript strictness to prevent errors at compile time
- Validate props via TypeScript types, not runtime checks

### Accessibility Requirements

- All interactive elements must be keyboard accessible
- Include `focus-visible:` ring styles
- Support `aria-invalid` and `aria-disabled` states
- Use semantic HTML elements

### CSS Theme Tokens

Available CSS variables (use Tailwind classes):
- Colors: `bg-background`, `text-foreground`, `bg-primary`, `text-primary-foreground`
- Semantic: `bg-card`, `bg-muted`, `bg-accent`, `bg-destructive`
- Borders: `border-border`, `border-input`
- Focus: `ring-ring`, `border-ring`
- Radius: `rounded-md`, `rounded-lg` (via --radius)
