# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the Games

Open any HTML file directly in a browser — no build step, no server required:

```bash
open shooter.html
open shooter2.html
open tictactoe.html
```

## Repository Overview

This is a collection of browser games built as single self-contained HTML files. Each file embeds all CSS and JavaScript inline — no external dependencies, no frameworks, no bundler.

## Architecture

### `shooter.html` — Pixel Assault (top-down shooter)

Game loop driven by `requestAnimationFrame`. All sprites are drawn programmatically on an HTML5 Canvas using the 2D API — no image assets.

**State machine:** A `screen` variable (`SCREEN.MENU | PLAY | LEVEL_DONE | GAME_OVER | PAUSED`) controls which update/draw path runs each frame.

**Key globals:**
- `player` — position, angle (tracks mouse), HP, shoot cooldown
- `enemies[]` — active enemies; `spawnQueue[]` — enemies waiting to spawn this wave
- `bullets[]`, `particles[]`, `flashes[]` — pooled effect objects

**Rendering pipeline per frame:** `drawBg()` → `drawParticles()` → `drawBullets()` → enemies → player → `drawFlashes()` → `drawHUD()`

**Level progression:** `waveConfig(level)` returns enemy counts per type. `spawnQueue` is shuffled and enemies are released one-by-one on a timer (`spawnInterval`). Level advances when both `spawnQueue` and `enemies[]` are empty.

**Enemy types:** `basic` (straight charge), `fast` (zigzag, unlocks level 3), `tank` (3 HP hexagon, unlocks level 5). Each enemy is a plain object with `type`, `hp`, `speed`, `radius`, `pts`.

**Score:** Multiplied by current level (`e.pts * level`). High score persisted via `localStorage`.

### `shooter2.html` — Pixel Assault II (sequel)

Same architecture as `shooter.html` with two additions:

**Cyberpunk color palette:** Deep space purple background with scrolling stars, cyan grid, cyan bullets, pink/magenta basic enemies, neon green fast enemies, orange tank enemies. Neon glow via `ctx.shadowBlur`.

**Web Audio API sound:** All sounds are procedurally generated — no audio files. Key functions: `sndShoot()`, `sndHit()`, `sndExplode()`, `sndPlayerHit()`, `sndLevelUp()`, `sndGameOver()`. Uses `playTone()` (oscillator) and `playNoise()` (buffer noise + bandpass filter). `audioCtx.resume()` is called on first click to satisfy browser autoplay policy.

**High score key:** `pxA2Hi` (separate from `shooter.html`'s `pxAHi`).

### `tictactoe.html` — Tic Tac Toe

Simple 2-player game. 3×3 grid of `.cell` divs. Win state checked against all 8 winning combinations after each move. Scores persist in JS variables for the session.

## Git / GitHub Workflow

**Commit and push frequently** — after every meaningful unit of work (feature added, bug fixed, file created). Never batch multiple unrelated changes into one commit. This ensures work is never lost and the history is easy to revert.

```bash
git add <specific files>
git commit -m "concise description of what and why"
git push
```

Commit message format: short imperative summary (e.g. `Add enemy health bars`, `Fix bullet collision with tank`). No need for a body unless the change is complex.

Remote: `https://github.com/kelleyrw-fb/claude-code-full-tutorial-for-beginners`
