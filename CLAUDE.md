# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the Games

Open any HTML file directly in a browser — no build step, no server required:

```bash
open shooter.html
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

### `tictactoe.html` — Tic Tac Toe

Simple 2-player game. 3×3 grid of `.cell` divs. Win state checked against all 8 winning combinations after each move. Scores persist in JS variables for the session.

## Git / GitHub Workflow

Always commit after meaningful changes and push to `origin/main`:

```bash
git add <specific files>
git commit -m "concise description of what and why"
git push
```

Remote: `https://github.com/kelleyrw-fb/claude-code-full-tutorial-for-beginners`
