# Repository Guidelines

## Project Structure & Module Organization
This repository is a flat collection of standalone browser sketches. Each root-level `.html` file is a complete experiment with inline HTML, CSS, and JavaScript. Examples include `zalgo_vortex.html`, `physarum_zalgo.html`, and `fractal.html`.

There is no `src/`, `tests/`, or asset pipeline. Shared architectural notes live in `CLAUDE.md`, which documents the common canvas loop, grid sizing, typed-array usage, and interaction model used across the sketches.

## Build, Test, and Development Commands
There is no build step or package manager.

- `xdg-open zalgo_vortex.html`
  Opens a sketch directly in the browser.
- `python3 -m http.server 8000`
  Starts a simple local server if the browser blocks local file access or you want faster reload testing.
- `python3 -m http.server 8000 --directory /home/glitches/Projects/characterglitch`
  Serves the repository root explicitly.

When editing, verify changes in at least one desktop browser and resize the viewport to confirm the canvas reinitializes correctly.

## Coding Style & Naming Conventions
Keep each experiment self-contained in a single HTML file unless a refactor clearly benefits multiple sketches. Match the existing style:

- Use 2-space indentation inside functions and blocks.
- Use `camelCase` for variables and functions such as `resize`, `frame`, and `spawnSpore`.
- Use `UPPER_SNAKE_CASE` for simulation constants and glyph sets such as `CELL`, `DECAY`, and `ABOVE`.
- Prefer typed arrays (`Float32Array`, `Int8Array`) for grid and agent state.
- Preserve the existing `requestAnimationFrame` loop and `resize()` reallocation pattern.

## Testing Guidelines
This repository has no automated test suite yet. Testing is manual and visual.

- Open the modified sketch and confirm it renders without console errors.
- Exercise mouse, touch, click, and wheel handlers if the file defines them.
- Check performance at fullscreen size and confirm arrays reset cleanly after resize.
- If you add a new sketch, keep the filename lowercase with underscores, for example `new_pattern.html`.

## Commit & Pull Request Guidelines
Git history is not available in this workspace, so no established commit convention can be inferred. Use short, imperative commit messages such as `Add bloom pulse to mycelium sketch` or `Tighten vortex cursor smoothing`.

Pull requests should include a brief visual summary, list the edited sketch files, and attach screenshots or a short screen recording for behavior changes.
