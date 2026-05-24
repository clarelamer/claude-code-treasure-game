# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm install       # install dependencies
npm run dev       # start dev server at http://localhost:3000 (auto-opens browser)
npm run build     # build to ./build directory (not ./dist)
```

No test runner is configured.

## Architecture

This is a single-page React + TypeScript app built with Vite. All game logic lives in `src/App.tsx` — there are no routes, no state management library, and no backend.

**Entry point**: `src/main.tsx` mounts `<App />` to `#root` in `index.html` and imports `src/index.css`.

**Game mechanics** (`src/App.tsx`): Three treasure chests, one randomly assigned a treasure on mount via `initializeGame()`. Clicking opens a chest: treasure gives +$100, skeleton gives -$50. Game ends when treasure is found or all boxes are opened. Score and box state tracked in component state via `useState`.

**UI layer** (`src/components/ui/`): Full shadcn/ui component library (Radix UI primitives + Tailwind). These are pre-built and rarely need modification. Import via `import { Button } from './components/ui/button'`.

Available components: `accordion`, `alert`, `alert-dialog`, `aspect-ratio`, `avatar`, `badge`, `breadcrumb`, `button`, `calendar`, `card`, `carousel`, `chart`, `checkbox`, `collapsible`, `command`, `context-menu`, `dialog`, `drawer`, `dropdown-menu`, `form`, `hover-card`, `input`, `input-otp`, `label`, `menubar`, `navigation-menu`, `pagination`, `popover`, `progress`, `radio-group`, `resizable`, `scroll-area`, `select`, `separator`, `sheet`, `sidebar`, `skeleton`, `slider`, `sonner`, `switch`, `table`, `tabs`, `textarea`, `toggle`, `toggle-group`, `tooltip`.

`src/components/figma/ImageWithFallback.tsx`: Drop-in `<img>` replacement that shows a placeholder SVG on load error. Use instead of `<img>` when the image source may be unreliable.

**Path alias**: `@` resolves to `./src` (configured in `vite.config.ts`).

**Assets**:
- Images: `src/assets/` — `treasure_closed.png`, `treasure_opened.png`, `treasure_opened_skeleton.png`, `key.png`
- Audio: `src/audios/` — `chest_open.mp3` (normal open), `chest_open_with_evil_laugh.mp3` (skeleton open)
- Reference screenshot: `src/results/key_hover.png` — shows the intended key cursor hover effect on closed chests

## Styling

**Tailwind v4** (not v3) — uses `@custom-variant` and `@theme inline` syntax.

- `src/styles/globals.css` — **edit this file** for theme changes. Defines all CSS design tokens (`--background`, `--primary`, `--radius`, etc.) for both light and dark modes, and the `dark` custom variant.
- `src/index.css` — compiled Tailwind v4 output. **Do not edit manually**; it is auto-generated from the component classes used across the project.

Dark mode is toggled by adding the `.dark` class to a parent element (not via `prefers-color-scheme`).

Animations use the `motion` package imported as `motion/react`.

## Guidelines

`src/guidelines/Guidelines.md` is a placeholder for project-specific design rules — add constraints there when the design system needs them.
