# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Context

This is the starter project for Mosh's Claude Code course. Per the README, it **intentionally** ships with a bug, poor UI, and messy code that get fixed throughout the course. Do not assume existing patterns are intentional — they are starting points to be improved.

## Commands

```bash
npm install       # install dependencies
npm run dev       # start Vite dev server at http://localhost:5173
npm run build     # production build into dist/
npm run preview   # preview the production build
npm run lint      # run ESLint over **/*.{js,jsx}
```

There is no test runner configured.

## Architecture

Single-page React 19 + Vite 7 app. The entire application lives in **`src/App.jsx`** — there are no separate components, hooks, contexts, routes, or utility modules. State (transactions, form inputs, filters) is managed with `useState` inside `App`, with seed data hardcoded in the initial state.

Entry chain: `index.html` → `src/main.jsx` (mounts `<App />` in `<StrictMode>`) → `src/App.jsx`. Styles are plain CSS in `src/App.css` and `src/index.css`.

### Known data-shape gotcha

Transaction `amount` values are stored as **strings** (both in seed data and when read from the `<input type="number">` via `e.target.value`). The income/expense totals use `reduce((sum, t) => sum + t.amount, 0)`, which string-concatenates instead of summing. When touching totals or adding numeric logic, convert to `Number` at the boundary.

## Lint rule worth knowing

`no-unused-vars` is configured with `varsIgnorePattern: '^[A-Z_]'` — unused **variables** whose names are uppercase- or underscore-prefixed won't trigger the rule (function arguments are not exempted).
