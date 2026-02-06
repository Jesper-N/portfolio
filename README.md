# Portfolio

Personal portfolio built with SvelteKit 2 and Svelte 5, with a tactile UI system, animated interactions, and curated project sections.

## What is in this repo

This is a single-page portfolio with sections for a hero, about/info grid, education, projects, hobbies, and a floating bottom dock.

The style is intentionally dark and tactile, with subtle motion and custom UI components.

## Tech stack

- SvelteKit 2 + Svelte 5 (runes mode)
- TypeScript (strict mode)
- Tailwind CSS v4 + `tw-animate-css`
- `tailwind-variants`, `bits-ui`, `clsx`, `tailwind-merge`
- Lenis for smooth scrolling
- MapLibre GL for the map card
- pnpm

## Key files

- `src/routes/+layout.svelte`: global layout and Lenis setup
- `src/routes/+page.svelte`: page composition and content
- `src/routes/layout.css`: theme tokens and global styles
- `src/lib/components/ui/TactileCard.svelte`: base card primitive
- `src/lib/components/ui/ProjectGrid.svelte`: projects grid
- `src/lib/components/ui/map/Map.svelte`: map wrapper
- `src/lib/components/ui/map/MapMarker.svelte`: map marker primitive

## Project structure

```text
src/
  routes/
    +layout.svelte
    +page.svelte
    layout.css
  lib/
    components/
      ui/
        map/
```

## Development

Prerequisites:
- Node.js 20+
- pnpm

Commands:

```bash
pnpm install
pnpm dev
pnpm build
pnpm preview
pnpm check
pnpm check:watch
```

Default local URL: `http://localhost:5173`.

## Build adapter

`svelte.config.js` currently uses `@sveltejs/adapter-auto`.
