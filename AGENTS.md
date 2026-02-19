# AGENTS.md — AI Coding Agent Guide for btc-dream

## Project Overview

Bitcoin Profit Target Calculator — a single-file web app (`index.html`) with inline CSS and JS.
Zero dependencies, zero build tools, no package.json, no bundler, no linter, no test framework.
Neo-brutalist design aesthetic. Fully client-side, no backend, no APIs (except Google Fonts).
Repository: https://github.com/kawaiier/bbttcc.git

## File Structure

```
btc-dream/
├── index.html      # Entire application (HTML + CSS + JS)
├── README.md       # Documentation
├── AGENTS.md       # This file
├── .gitignore      # Ignores: telemetry-id
└── telemetry-id    # Local AI tool session ID (gitignored)
```

## Build / Run / Test

There are NO build, lint, or test commands. No package.json, no npm scripts, no Makefile.

- **Run:** open `index.html` in a browser, or `python3 -m http.server 8000` / `npx http-server`
- **Validate:** use W3C HTML validator or browser dev tools
- **Test:** manual only — enter values in browser and click Calculate

## Critical Constraints

- **Everything stays in a single `index.html`** — never split into separate CSS/JS files
- **No external dependencies** (no npm, no CDN libs) except Google Fonts @import
- **No build step** — file must work when opened directly in a browser
- **All calculations client-side** — no server, no API calls, no data collection
- **Maintain full accessibility** — ARIA attributes, focus management, screen reader support

## Code Style — HTML

- 2-space indentation
- Self-closing void elements: `<meta ... />`, `<input ... />`
- Semantic elements: `<main>`, `<footer>`, `<h1>`, `<label>`, `<small>`
- All content inside `<main><div class="container">...</div></main>`
- Each input field inside `<div class="input-group">`
- ARIA attributes required on all interactive elements:
  `aria-describedby`, `aria-required`, `aria-invalid`, `aria-live`, `aria-label`, `aria-hidden`
- `role="alert"` on error output div
- `lang="en"` on `<html>`

## Code Style — CSS

- All CSS in a single `<style>` block in `<head>`
- Plain CSS only — no preprocessors, no CSS variables/custom properties
- Class names: lowercase kebab-case (`.input-group`, `.help-text`, `.or-divider`)
- Global reset: `* { box-sizing: border-box; margin: 0; padding: 0; }`
- All `border-radius: 0` — NEVER use rounded corners (neo-brutalist)
- Borders: pure black `#000`, varying widths (4px container/button/result, 3px inputs/divider, 2px help-text)
- Box-shadows: hard offset, no blur (`Xpx Xpx 0 #000`)
- Font: Google Fonts Inter (400, 600, 700, 800, 900) via `@import`
- Always include `:focus-visible` outlines — never remove outlines without replacement
- Use `-webkit-appearance: none` on inputs, `-webkit-tap-highlight-color: transparent` on interactive

### Color Palette (fixed — do not deviate)

| Usage               | Color     |
|----------------------|-----------|
| Body background      | `#f0e6d3` |
| Container            | `#ffffff` |
| Button / help-text   | `#fef08a` |
| Result               | `#d1fae5` |
| Highlight / error    | `#fca5a5` |
| OR divider           | `#a5f3fc` |
| All text             | `#000`    |
| Placeholder / footer | `#666`    |
| Focus ring           | `#3b82f6` |

### Responsive Breakpoints (mobile-first cascade)

- Base styles (all screens)
- `@media (max-width: 640px)` — tablet/large phone
- `@media (max-width: 480px)` — phone
- `@media (max-width: 360px)` — small phone

## Code Style — JavaScript

- All JS in a single `<script>` block at bottom of `<body>`
- **Use `var` declarations** — not `const` or `let` (established convention)
- Function declarations only — no arrow functions, no classes, no modules
- DOM access: `document.getElementById()` exclusively
- Show/hide: `element.style.display = "none"` / `"block"`
- Event handlers: inline HTML attributes (`onclick`, `onkeydown`, `oninput`)
- Input parsing: `parseFloat()` for all numeric inputs
- Validation: `isNaN()` checks
- Number formatting (USD): `value.toLocaleString("en-US", { minimumFractionDigits: 2, maximumFractionDigits: 2 })`
- Percentage formatting: `value.toFixed(2)`
- Emoji: use Unicode escape sequences (`"\u26a0\ufe0f"` for warning, `"\u2705"` for checkmark)
- HTML output: string concatenation (not template literals)

### Key Functions

| Function         | Purpose                                          |
|------------------|--------------------------------------------------|
| `formatUSD()`    | Format number as USD string                      |
| `clearField()`   | Clear a specific input field                     |
| `handleKeydown()`| Handle Enter key to trigger calculation          |
| `showError()`    | Hide `#output`, show `#errorOutput` with message |
| `showResult()`   | Hide `#errorOutput`, show `#output` with HTML    |
| `calculate()`    | Main calculation logic                           |

## Naming Conventions

- **HTML ids:** camelCase — `btcOwned`, `avgPrice`, `profitUSD`, `profitPercent`, `calculateBtn`, `btnText`, `btnSpinner`, `errorOutput`
- **HTML help text ids:** camelCase with "Help" suffix — `btcOwnedHelp`, `avgPriceHelp`, `profitUSDHelp`, `profitPercentHelp`
- **CSS classes:** kebab-case — `.input-group`, `.help-text`, `.or-divider`, `.error-output`
- **JS functions:** camelCase — `formatUSD`, `clearField`, `handleKeydown`, `showError`, `showResult`, `calculate`

## Error Handling

- Validate all required inputs before calculation
- Check for `NaN` and `<= 0` on numeric inputs
- Display errors in `#errorOutput` div (red background, `role="alert"`)
- Hide `#output` when showing errors, and vice versa
- Prefix error messages with `"\u26a0\ufe0f"` (warning emoji)
- Prefix success highlights with `"\u2705"` (checkmark emoji)

## Design Rules (Neo-Brutalist)

- NEVER use rounded corners — all `border-radius: 0`
- ALWAYS use hard offset box-shadows (no blur, no spread beyond offset)
- ALWAYS use solid black borders
- Uppercase text for headings, labels, and buttons (`text-transform: uppercase`)
- Interactive elements must have `transform` + shadow shift on hover/active
- Use the established color palette — do not introduce new colors without strong reason

## Git Conventions

- Branch: `main`
- Commit messages: short, informal, imperative or descriptive
  - Examples: "Update README.md", "Add dark mode toggle"
  - No conventional commit prefixes enforced (no `feat:`, `fix:`, etc.)

## Adding New Features

1. All changes go in `index.html` — HTML structure, `<style>` block, or `<script>` block
2. Follow existing patterns: input groups, ARIA attributes, color palette, `var` declarations
3. Test manually in browser — there is no automated test suite
4. Validate accessibility: check ARIA, focus order, screen reader behavior
5. Test all four responsive breakpoints (base, 640px, 480px, 360px)
6. Verify neo-brutalist style: no rounded corners, hard shadows, solid borders
