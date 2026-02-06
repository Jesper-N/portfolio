# Portfolio

Personal portfolio built with SvelteKit 2 and Svelte 5, with a tactile UI system, animated interactions, and curated project sections.

## Overview

This repository contains a single-page portfolio site with distinct sections for:

- Hero introduction
- About (location and skills)
- Education
- Selected projects
- Hobbies

The visual style uses dark tactile surfaces, subtle grain overlays, and Geist Sans/Mono typography.

## Tech stack

- SvelteKit 2 + Svelte 5 (runes mode)
- TypeScript (strict mode)
- Tailwind CSS v4 + `tw-animate-css`
- `tailwind-variants` + `bits-ui`
- Lenis (smooth scrolling)
- Map component built with MapLibre GL + D3/TopoJSON utilities
- pnpm

## Features

- Smooth scrolling initialized in `portfolio/src/routes/+layout.svelte` via Lenis
- Floating bottom dock navigation in `portfolio/src/lib/components/ui/NavDock.svelte`
- Reusable tactile card primitives in `portfolio/src/lib/components/ui/TactileCard.svelte`
- Responsive project grid in `portfolio/src/lib/components/ui/ProjectGrid.svelte`
- Interactive location card with map marker in `portfolio/src/lib/components/ui/InfoGrid.svelte`
- Modular UI component organization under `portfolio/src/lib/components/ui/`

## Project structure

```text
src/
  routes/
    +layout.svelte         # Global layout, Lenis setup, app wrapper
    +page.svelte           # Main portfolio page composition
    layout.css             # Theme tokens, utilities, global styles
  lib/
    components/
      ui/                  # Reusable UI primitives and feature components
```

## Getting started

### Prerequisites

- Node.js 20+ (current LTS recommended)
- pnpm

### Setup

```bash
pnpm install
pnpm dev
```

Then open `http://localhost:5173` (Vite default).

## Available scripts

- `pnpm dev` - Start the development server.
- `pnpm build` - Build production assets.
- `pnpm preview` - Preview the production build locally.
- `pnpm check` - Run Svelte sync + TypeScript checks (required quality gate).
- `pnpm check:watch` - Run checks in watch mode.

## Build and deployment

This project currently uses `@sveltejs/adapter-auto` in `portfolio/svelte.config.js`.

For fixed deployment targets (for example Vercel, Netlify, or a Node adapter), replace adapter-auto with the platform-specific SvelteKit adapter.

## Content customization

- Main page content: `portfolio/src/routes/+page.svelte`
- Hero copy: `portfolio/src/lib/components/ui/Hero.svelte`
- Theme tokens and global styling: `portfolio/src/routes/layout.css`

## Notes

- No license is declared in this repository at the time of writing.
- This README intentionally omits contribution workflow and social link sections.
