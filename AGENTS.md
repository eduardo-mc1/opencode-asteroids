# AGENTS.md

## Repo shape

- Vanilla HTML5 Canvas remake of the classic arcade Asteroids. No frameworks, bundler, dependencies, or `package.json`.
- `index.html` — DOM + fixed 800×600 `<canvas>`; `game.js` — all game logic in global-scope ES6 classes (no modules).

## Running and verification

- No build, test, lint, or typecheck steps exist. Don't invent or require them.
- Run: open `index.html` directly, or `npx serve .` → `http://localhost:3000`.
- No automated tests: verify changes by loading the page and playing by hand.

## Conventions to preserve

- Comments, HUD/UI strings, and README are in **Spanish**; new text should be Spanish. Code identifiers are English (`class Ship`, etc.).
- `game.js` loads via a plain `<script>` tag — keep everything on the global scope, no `import`/`export` (ES modules break direct `file://` opening).
- Canvas is always 800×600: `W`/`H` in `game.js` mirror the `width`/`height` attributes on `#canvas` in `index.html`. Keep both in sync.
- Moving entities must wrap their `x`/`y` within `W`/`H` via `wrap()` (toroidal space) in every `update`.

## Game mechanics gotchas

- Main loop: `requestAnimationFrame` + `dt` in seconds, capped at `0.05` in `loop()`. Time-based updates only.
- Input is two-channel: `keys` (held) vs `justPressed`/`pressed(code)` (one-shot edge). `pressed()` consumes the flag on call — invoke once per frame per code. `Space` both fires (state `'playing'`) and restarts (state `'gameover'`, only on a fresh keypress); arrows/space are `preventDefault`'d to stop page scroll.
- Game phase is a global `state` variable: `'playing' | 'dead' | 'gameover'`; `update()` branches on it.
- Asteroid sizes 1–3; `RADII`/`SPEEDS`/`POINTS` lookup arrays are indexed by size, and destroying one spawns two of `size - 1` via `split()`.