# Repository Guidelines

## Project Structure & Module Organization
- `chess.html` is the single-page entry point; it inlines React components, Chess.js game logic, Stockfish worker wiring, puzzle data, and UI helpers.
- `assets/pieces/` stores SVG piece sprites for offline use; the app currently defaults to the Lichess Alpha CDN set via `getPieceImg`.
- Keep new helpers and components co-located in `chess.html` near their usage; prefer small, focused functions over sprawling globals.

## Build, Test, and Development Commands
- Run locally with a static server to avoid worker/CORS surprises: `python -m http.server 8000` then open `http://localhost:8000/chess.html`.
- No build step is required; edits to `chess.html` take effect on refresh. Favor incremental browser testing after each change.

## Coding Style & Naming Conventions
- Use 2-space indentation and modern ES2015+ syntax; keep logic in functional React components with hooks.
- Prefer camelCase for functions/variables and SCREAMING_SNAKE_CASE for configuration constants (`ENGINE_LEVELS`, default options).
- Styling uses Tailwind utility classes; group related utilities logically and avoid inline style attributes unless unavoidable.
- Keep comments concise and only for non-obvious logic (engine orchestration, puzzle validation, async worker handling).

## Testing Guidelines
- No automated test harness exists; perform manual passes covering: page load without console errors, move legality, engine reply at each skill level, hint/analysis output, and puzzle flow (correct vs. incorrect attempts).
- After changes to asset loading or worker code, verify in both local-server mode and file:// if supported by the target browser.
- If you introduce tests, prefer lightweight browser-based checks or add a minimal Playwright script that exercises move input and puzzle success paths.

## Commit & Pull Request Guidelines
- Follow a conventional, imperative subject: `feat: ...`, `fix: ...`, `chore: ...` (repo history uses `feat: at least work`).
- In PRs, include: scope/intent summary, affected UI areas, manual test notes with commands or steps, and screenshots/GIFs for UI changes.
- Link related issues/tasks and call out any external dependency changes (CDN bumps, new assets) for reviewer awareness.

## Security & Configuration Tips
- Third-party CDNs are used for React, Tailwind, Chess.js, and Stockfish; pin versions and note changes when updating. Keep fallbacks in `assets/` when feasible for offline or restricted environments.
- Stockfish is fetched inside a Web Worker via Blob; ensure network availability or ship a vendored copy if deploying to locked-down hosts. Clean up workers on teardown to avoid stray threads in future enhancements.
