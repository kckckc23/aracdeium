# ARCADEIUM

A collection of six classic arcade games, presented as a retro **CRT arcade desktop**. Pure HTML/CSS/JavaScript — no build step, no frameworks, no dependencies. Open `index.html` and play.

The whole suite shares one visual identity — **"AV-1: The Tube"**: amber phosphor on a dark CRT tube, scanlines, a power-on flicker, and a backlit marquee over SMPTE color bars.

---

- [Part 1 — The Arcade](#part-1--the-arcade)
- [Part 2 — Development Guide](#part-2--development-guide)

---

# Part 1 — The Arcade

## Running it

No install, no server needed.

```bash
git clone [repository-url]
cd aracdeium-main
```

Then open `index.html` in any modern browser (or open an individual game's `.html` directly).

> Audio only starts after your first key press or click — that's a browser autoplay rule, not a bug.

## The games

| Channel | Game | File | Controls |
|--------|------|------|----------|
| CH1 | **Snake** | `snakes.html` | Arrow keys move · Space pauses |
| CH2 | **Pong** | `pong.html` | Mouse, or ↑/↓ / W/S · P pauses |
| CH3 | **Tetris** | `tetris.html` | ←/→ move · ↑ rotate · ↓ soft drop · Space hard drop · P pause |
| CH4 | **Space Invaders** | `spaceinvaders.html` | ←/→ move · Space fire |
| CH5 | **Pac-Man** | `pacman.html` | Arrow keys or WASD |
| CH6 | **Tic-Tac-Toe** | `ttt.html` | Click a square · toggle 1P (vs CPU) / 2P |

**Launcher** (`index.html`): double-click a channel tile to launch, or select one and press Enter. The **Start** menu also lists every program as a one-click launcher. The top OSD bar and Start button open in-page CRT "OS windows" (About, Theme Manager, Retro Chat).

### What each game does

- **Snake** — grid movement with an input queue (you can't accidentally reverse into yourself), speed ramps as you grow, rounded glowing body, pause overlay.
- **Pong** — first to 10. Delta-time ball physics with edge-based bounce angles that speed up each rally; a fair, beatable CPU that only chases incoming shots; serve countdown; mouse **or** keyboard.
- **Tetris** — 7 distinctly colored tetrominoes, a ghost-drop preview, a **NEXT** piece panel, a 7-bag randomizer, wall-kick rotation, and standard 100/300/500/800×level line scoring.
- **Space Invaders** — pixel-art aliens that march and animate, four destructible bunkers, a bonus UFO, the classic one-shot rule, accelerating waves, and a clean wave→wave state machine.
- **Pac-Man** — smooth pixel-step movement, authentic scatter↔chase ghost AI (Blinky/Pinky/Inky/Clyde), frightened mode with eyes that return to the pen, tunnel wrap, and endless levels.
- **Tic-Tac-Toe** — play a friend (2P) or an **unbeatable minimax CPU** (1P); winning line highlights; a running scoreboard.

## Audio

Sound files live in `music/`, named `gamename_soundtype.ext`. The set today is a mix of real effects and synthesized placeholders:

| File | Status |
|------|--------|
| `pong_ballhit.mp3` | real |
| `snakes_food.mp3`, `snakes_gameover.mp3`, `snakes_movement.mp3` | real |
| `spaceinvaders_shoot.wav`, `spaceinvaders_invaderkilled.wav`, `spaceinvaders_playerdeath.wav` | real |
| `tetris_backgroundmusic.mp3`, `tetris_lineclear.mp3` | real |
| `pacman_chomp.wav`, `pacman_powerpellet.wav`, `pacman_eatghost.wav`, `pacman_death.wav` | **placeholder** (synthesized beeps) |
| `ttt_place.wav`, `ttt_win.wav`, `ttt_draw.wav` | **placeholder** (synthesized beeps) |

To swap a sound, drop a file with the **same name** into `music/` — no code changes needed.

## Project structure

```
aracdeium-main/
├── index.html          # Launcher / CRT desktop (self-contained styles + OS windows)
├── retro-desktop.css   # Shared theme + layout for ALL game pages
├── snakes.html         # Each game = one self-contained HTML file with an inline <script>
├── pong.html
├── tetris.html
├── spaceinvaders.html
├── pacman.html
├── ttt.html
├── music/              # Audio assets (gamename_soundtype.ext)
└── README.md
```

## Browser support

Tested on Chromium/Edge; works in current Firefox and Safari. Requires modern features (`requestAnimationFrame`, `dvh` units, `ctx.roundRect`, CSS `:has()`), so a current browser is expected.

---

# Part 2 — Development Guide

Conventions for extending Arcadeium. Follow these and a new game will look, feel, and behave like the rest of the suite.

## Principles

1. **No build, no dependencies.** Vanilla HTML/CSS/JS only. Fonts come from a Google Fonts `@import`; everything else is local.
2. **One game = one self-contained `.html` file** with its logic in an inline `<script>`. Game pages share styling through `retro-desktop.css`; they do not share JS.
3. **The look is fixed** — "AV-1: The Tube." Don't reintroduce neon cyan/magenta, Orbitron, or starfields (the old style). Derive every color and font from the tokens below.
4. **Frame-rate independent.** All motion is delta-time based, never per-frame constants.

## Design system — "AV-1: The Tube"

### Color tokens

Defined in `:root` in `retro-desktop.css` (and a subset is duplicated inline in `index.html`, which doesn't link the shared sheet). Always use the variables.

| Token | Value | Use |
|-------|-------|-----|
| `--tube` | `#07090d` | unlit tube black (page background) |
| `--tube-2` | `#0c1016` | screen glass |
| `--bezel` / `--bezel-hi` | `#15110b` / `#2a2318` | monitor plastic + bevel |
| `--amber` | `#ffb000` | **primary phosphor** — text, accents, default buttons |
| `--amber-dim` | `#b87800` | borders, dim accents |
| `--ink` / `--ink-dim` | `#ffe7b0` / `#c9a86a` | phosphor-white / muted body text |
| `--green` | `#4dff88` | "GO"/active/primary-action only |
| `--red` | `#ff4533` | danger / close / alert only |
| `--line` | `rgba(255,176,0,.22)` | hairline borders |
| `--bars` | SMPTE gradient | the marquee underline only |
| `--glow-amber` | text-shadow | amber phosphor glow |

In canvas games, use literal hex that matches this palette (e.g. amber `#ffb000`, green `#4dff88`, red `#ff4533`, tube black `#04040a`). Pieces/sprites may use their own iconic colors (Tetris 7-color set, Pac-Man ghosts) — those are intentional exceptions.

### Typography (3 roles)

Loaded once via `@import`. Use them by role, not interchangeably:

- **Press Start 2P** — display: marquee, game-over headings, big banners. Use sparingly.
- **Silkscreen** — chunky UI: buttons, labels, channel names.
- **VT323** — the "OSD / teletext" voice: HUD readouts, clock, status lines, dialog body text.

### CRT effects

The global glass — scanlines + curvature vignette + one-shot power-on flicker — lives on `body::before` / `body::after` (see `retro-desktop.css` and `index.html`). They're `pointer-events: none` and sit above content so clicks pass through. Don't duplicate them per game; the shared sheet already applies them. All of it is gated by `@media (prefers-reduced-motion: reduce)`.

## Shared CSS contract

`retro-desktop.css` styles game pages entirely through a fixed class vocabulary. A new game page **must** use these names (no new structural classes needed):

| Class | Role |
|-------|------|
| `.game-menu-bar` + `.game-menu-button` | fixed top OSD bar (Back / Help / Pause) |
| `.game-window` | the monitor bezel |
| `.window-title` + `.window-controls` + `.window-button` | OSD title strip + min/close |
| `.game-content` | the screen; **becomes the bento grid ≥769px** |
| `.hud-panel` / `.hud-item` / `.hud-label` / `.hud-value` | scoreboard readouts |
| `.screen-cell` | wraps the `<canvas>` (or board) — the left bento cell |
| `.side-cell` | wraps instructions + buttons — the right bento cell |
| `.control-panel` / `.control-section` / `.control-title` / `.control-text` | instruction placard |
| `.button-container` + `.retro-button` (`.primary`) | arcade pushbuttons |
| `.game-over` (`h2`, `p`) | centered end-of-game dialog |

### Bento layout rule

On desktop (`min-width: 769px`) `.game-content` is a grid: **HUD spans the top, the `.screen-cell` sits left, the `.side-cell` stacks on the right.** Below that it's a vertical stack. So the body order inside `.game-content` is:

```html
<div class="game-content">
  <div class="hud-panel"> …readouts… </div>
  <div class="screen-cell"><canvas id="…"></canvas></div>
  <div class="side-cell">
    <div class="control-panel"> …instructions… </div>
    <div class="button-container"> …buttons… </div>
  </div>
</div>
```

The canvas is capped to the viewport with `max-height: calc(100dvh - 250px)` so it never scrolls off the bottom, regardless of aspect ratio.

## Game page boilerplate

```html
<head>
  <title>Name.exe - Arcadeium</title>
  <link rel="stylesheet" href="retro-desktop.css">
</head>
<body>
  <div class="game-menu-bar">
    <a href="index.html" class="game-menu-button">← Desktop</a>
    <div class="game-menu-button" onclick="showHelp()">Help</div>
    <div class="game-menu-button" onclick="pauseGame()">Pause</div>
  </div>
  <div class="game-window">
    <div class="window-title">
      <span>🎮 Name.exe - Arcadeium Games</span>
      <div class="window-controls">
        <div class="window-button" onclick="minimizeWindow()">_</div>
        <div class="window-button" onclick="window.location.href='index.html'">×</div>
      </div>
    </div>
    <div class="game-content"> … (see bento rule) … </div>
  </div>
  <div class="game-over" id="gameOver" style="display:none;"> … </div>
  <script> /* game logic */ </script>
</body>
```

## Game-loop conventions

- **Single, guarded `requestAnimationFrame` loop.** Track `loopRunning` and only kick off a new loop if one isn't already running — never start a second chain on pause/restart.
- **Delta time, clamped:** `const dt = Math.min((ts - lastTs) / 1000, 0.05);`. Use `dt` (seconds) for all motion. Use a time **accumulator** for fixed-step games (Snake, Tetris gravity).
- **State machine,** not scattered booleans. Use one `state` string per game — e.g. `play | paused | over`, plus transient states like `serve`, `wavebanner`, `respawn`, `ready`. The loop only updates in `play`; it still renders other states (overlays/banners).
- **Pause** is a flag + an on-canvas overlay, never `alert()`.
- Stop the loop on game over (`state === 'over'`), show the `#gameOver` dialog.

## Rendering conventions

- Canvas is pixel art: `image-rendering: pixelated` (set in the shared sheet) and integer pixel sizes. Derive tile size from the canvas (`const TILE = canvas.width / COLS`) so the grid always fits.
- Draw sprites from bitmap strings (`'01101'`) via a `drawSprite(bitmap, x, y, scale, color)` helper — see `spaceinvaders.html` / `pacman.html`.
- Banners/overlays use Press Start 2P; readouts use VT323.

## Input conventions

- `keydown` / `keyup`; `e.preventDefault()` on arrows and Space to stop page scroll.
- For grid games, **queue direction changes** and apply one per step, rejecting reversals — this is what stops Snake from folding into itself. Don't read raw key state at move time.
- For mouse-on-canvas, scale pointer coords into canvas space: `const scaleY = canvas.height / rect.height;` (the canvas is CSS-scaled on small screens).

## Audio conventions

- Files in `music/`, named `gamename_soundtype.ext`. WAV and MP3 both work.
- **Gate on first user gesture** (autoplay policy): an `enableAudio()` that runs once on the first `keydown`/`click`, then `playSound(s)` does `s.currentTime = 0; s.play().catch(()=>{})`. Keep volumes modest (0.3–0.6).
- **Generating placeholder sounds:** WAV can be written by hand (no encoder needed) — a 44-byte RIFF/WAVE header + 16-bit PCM samples (square wave + fade in/out). See the generator approach used for the `pacman_*` / `ttt_*` placeholders. MP3/OGG **cannot** be hand-written (they need an encoder like ffmpeg); use WAV for synthesized placeholders.

## Required hooks & element IDs

The shared markup's `onclick`s and the launcher expect these to exist:

- **Functions:** `startNewGame()` and/or `restartGame()`, `pauseGame()`, `showHelp()`, `minimizeWindow()`. Launcher uses `playGame(file)`.
- **HUD IDs:** `score` (+ game-specific like `lives`, `level`, `lines`). 
- **Dialog:** `#gameOver` (toggled via `style.display`), `#finalScore`, `#gameOverTitle` where used.

## The launcher (`index.html`)

`index.html` is **self-contained** (its own inline `<style>`, a subset of the tokens — it does not link `retro-desktop.css`). It hosts:

- The **marquee** + **channel grid** (`.tile-grid`, one `.desktop-icon` per game).
- The fixed OSD **menu-bar** and **taskbar** (Start + clock).
- The reusable **CRT OS window** modal (`.os-modal` / `.os-window`) that replaces native `alert()`s — opened via `openWindow(icon, title, bodyHTML)`, closed by ×, OK, backdrop click, or Esc.

To add a game to the launcher:
1. Add a `.desktop-icon <game>-icon` tile with `ondblclick="playGame('<file>.html')"` and a `::before` channel label (`CHn`).
2. Add an entry to the `PROGRAMS` array so it appears in the Start menu launcher.

## Responsiveness rules

- Use `100dvh` (with `100vh` fallback) and `clamp()` for fluid spacing.
- Breakpoint ladder: **≥769px** bento / **≤768** stacked tablet / **≤600** 2-col launcher / **≤480** small phone / **≤360** narrow phone / landscape-short. Game pages mainly need the bento + stacked split; the canvas height cap does the rest.
- The page body must never scroll horizontally; wide content fits or its container scrolls.

## Accessibility floor

Every screen should meet this without fanfare: visible `:focus-visible` outlines, `prefers-reduced-motion` disables the power-on/flicker/marquee animations, full keyboard play, and modals use `role="dialog"` / `aria-modal` with focus moved in on open and Esc to close.

## Validating changes

Two complementary checks — do both before calling a change done.

**1. Logic (Node).** Game logic is decoupled from rendering, so you can run it headless. Stub `document`/`canvas`/`Audio` (a `getContext` that returns a no-op Proxy, an element stub with `style`/`textContent`, an `Audio()` shim, a no-op `requestAnimationFrame`), append a small epilogue that exposes internals on `globalThis.__api`, drive `update(dt)` in a loop, and assert mechanics (eating/scoring, collisions, win/level transitions, no thrown errors over thousands of frames). This is how Pac-Man's AI, Tetris's 7-bag, and TTT's unbeatable minimax were verified.

**2. Visual (headless screenshot).** Render with headless Edge/Chrome (`--headless=new --screenshot`). **Caveats:** Chromium clamps the headless window to a ~470px minimum (sub-470 captures are cropped, not truly laid out — verify small layouts by math/real devices), and virtual-time mode freezes `requestAnimationFrame`/CSS animations (so the power-on overlay may appear stuck and gameplay frozen — that's the harness, not the game).

## Adding a new game — checklist

- [ ] New `<file>.html` from the boilerplate, linking `retro-desktop.css`.
- [ ] Markup uses the shared class vocabulary; canvas in `.screen-cell`, instructions+buttons in `.side-cell`.
- [ ] Single guarded rAF loop, delta-time, a `state` machine, pause overlay (no `alert()`).
- [ ] Implements `startNewGame`/`restartGame`, `pauseGame`, `showHelp`, `minimizeWindow`; wires HUD IDs and `#gameOver`.
- [ ] Audio (if any) gated on first gesture; files in `music/` with the naming convention.
- [ ] Colors/fonts from the AV-1 tokens; `prefers-reduced-motion` respected; visible focus.
- [ ] Launcher tile + `PROGRAMS` entry added in `index.html`.
- [ ] Validated: Node logic harness passes, and it renders correctly on desktop + mobile widths.

---

**Built for retro gaming, one phosphor dot at a time. 🕹️**
