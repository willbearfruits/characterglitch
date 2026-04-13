# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A collection of standalone single-file HTML canvas art experiments that render real-time animations using Unicode/ASCII characters instead of pixels. Each file is self-contained with zero dependencies and opens directly in a browser.

## Running

Open any `.html` file directly in a browser — no server, build step, or tooling required.

## Architecture patterns shared across all files

Every experiment follows the same structure:

**Rendering pipeline:**
- A `<canvas id="c">` fills the viewport; the cursor is hidden and replaced by a custom Unicode glyph drawn on canvas
- A `CELL` constant (8–13px) drives the monospace grid: `cols = floor(W / (CELL * ~0.58))`, `rows = floor(H / CELL)`
- Mouse position is normalized to `[0,1]` as `mx`/`my`; a smoothed copy (`pmx`/`pmy`) follows with lerp ~0.06–0.08
- `resize()` reallocates all `Float32Array` buffers and resets `ctx.font`
- `frame()` runs via `requestAnimationFrame`; increments a global `t` timer each tick
- The grid is cleared each frame with a solid background fill, then characters are drawn cell-by-cell

**Character selection:**
- Characters are selected from curated Unicode sets by a normalized `[0,1]` intensity/density value
- Zalgo-style stacking uses Unicode combining marks: ABOVE (`\u0300`–`\u0311`), BELOW (`\u0323`–`\u0348`), and OVERLAY/STRIKE (`\u0334`–`\u0336`) appended directly to base characters — the number of marks added scales with intensity
- Character sets are layered by density tier (e.g. DUST → THREAD → WEAVE → NODES at increasing density)

**Color computation:**
- Colors are computed per-cell as inline `rgb(r,g,b)` strings — no pre-built palettes except `fractal.html` which uses hard-stepped neon `PAL[]` arrays
- `zalgo_vortex.html` does full inline HSL→RGB conversion using the CSS algorithm (the `ff`/`kk`/`aa` pattern)
- Mouse proximity typically adds a brightness boost within a radius of ~0.06 normalized units

**Simulation algorithms per file:**
| File | Algorithm |
|---|---|
| `zalgo_vortex.html` / `zalgo_vortex (1).html` | Energy field from multiple orbiting vortices + sinusoidal noise; click spawns new vortices (max 5) |
| `fractal.html` | Burning Ship fractal with gravitational lens distortion near mouse, glitch tear lines, click shockwave |
| `dejong_organism.html` | De Jong strange attractor — runs 18k+4k iterations/frame, accumulates to `grid[]` + `colorGrid[]`, both decay each frame |
| `mycelium_network.html` | Agent-based hyphal growth: stigmergy (trail following), branching, fusion at dense nodes, chemotaxis toward mouse |
| `physarum_zalgo.html` | Physarum polycephalum (slime mold): 4000 agents sense pheromone left/center/right, deposit + diffuse + decay each frame |

**Interaction model (consistent across all files):**
- `mousemove` / `touchmove` — updates `mx`/`my`
- `click` — simulation-specific injection (new vortex / pheromone burst / attractor noise burst / spore drop)
- Some files also handle `wheel` for additional effects

## Key implementation details

- All grid data uses `Float32Array` for performance (energy, pheromone, trail, age, color channels, agent positions/headings)
- Physarum agents use three separate typed arrays `ax`, `ay`, `ah` (SoA layout) for N=4000 agents
- `dejong_organism.html` runs a second attractor with slightly perturbed params each frame for texture variety
- `mycelium_network.html` caps total hyphae at 2000 and respawns near mouse at low probability each frame; hyphae carry `gen` depth (max 8) to limit branch explosion
- `fractal.html` uses a `tears[]` array for glitch horizontal displacement lines; tears spawn randomly each frame and decay over ~8–15 ticks
