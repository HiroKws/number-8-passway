# Repository Guidelines

## Project Structure & Module Organization
- Current layout: `index.html` (single-page entry point).
- When adding files, use clear folders:
  - `css/` for styles (e.g., `css/main.css`)
  - `js/` for scripts (e.g., `js/app.js`)
  - `assets/` for images, audio, fonts (e.g., `assets/img/logo.png`)
- Keep third‑party libraries in `vendor/` if needed; avoid committing large binaries.

## Build, Test, and Development Commands
- Serve locally (no build step required):
  - Python: `python3 -m http.server 8000`
  - Node: `npx serve .`
- Format check (optional, recommended):
  - Check: `npx prettier --check .`
  - Fix: `npx prettier --write .`
- HTML validation: `npx html-validate index.html` or use the W3C validator.

## Coding Style & Naming Conventions
- Indentation: 2 spaces; encoding: UTF‑8; EOL: LF.
- HTML: semantic elements; lowercase attributes; keep markup small and accessible.
- CSS: use BEM (`block__element--modifier`) for class names; group related rules.
- JS: ES modules when applicable; `camelCase` for variables/functions, `PascalCase` for classes.
- Filenames: kebab‑case (e.g., `about-page.html`, `main.css`, `app.js`).

## Testing Guidelines
- Cross‑browser check in current Chrome/Firefox/Edge.
- Accessibility: run Lighthouse; fix contrast, landmarks, and tab order issues.
- If JS grows, add unit tests under `tests/` (e.g., Vitest/Jest):
  - Run: `npx vitest`
  - Test files: `*.test.js` near source or under `tests/`.
- Target ≥ 80% coverage for core JS logic.

## Commit & Pull Request Guidelines
- Conventional Commits: `feat:`, `fix:`, `docs:`, `style:`, `refactor:`, `test:`, `chore:`
  - Example: `feat(home): add loop signage animation`
- PRs include: concise description, motivation, screenshots/GIFs for UI, linked issues, and local test steps.
- Keep PRs focused (≤ ~300 LOC). Split large changes.

## Security & Configuration Tips
- Prefer external JS/CSS over inline; avoid `eval` and inline event handlers.
- Do not commit secrets or large media. Configure `.gitignore` accordingly.
- Set basic CSP and cache headers when deploying (e.g., disable sniffing, enable gzip/brotli).

## Agent-Specific Instructions
- Preserve current structure; do not introduce frameworks or build tools without request.
- When adding tools, prefer zero‑config and document new commands here.
