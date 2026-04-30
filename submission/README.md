# Infinite Luck Casino — Kitboga Code Jam 2026 Submission

## What is this?

**Infinite Luck Casino** is a fake casino ad overlay that plays on top of a YouTube-style video ad. The player is lured into a "free spin" minigame that keeps resetting the skip timer, adds prizes to a growing list, and ultimately holds the skip button hostage for as long as possible.

The whole joke is that the casino is rigged in the player's favour — you keep winning things — but you can never actually leave because you always have tickets left to spend.

---

## How to run

Start a local web server from the **repo root** (not the `submission/` folder):

```
python -m http.server 8000
```

Then open:

```
http://localhost:8000/
```

The shell picks a random ad from `ads/` and loads `submission/submission.html` inside an iframe. Click **Play** to start the ad — the game begins immediately.

> **Important:** The file must be served over HTTP. Opening `index.html` directly as `file://` will break the maze minigame due to browser canvas security restrictions (`getImageData` is blocked on cross-origin images loaded from disk).

---

## URL parameters

Append these to the submission URL (`submission/submission.html?...`) when testing:

| Parameter | Effect |
|-----------|--------|
| `?game=wheel` | Force wheel-of-fortune game mode |
| `?game=slot` | Force slot machine game mode |
| `?game=scratch` | Force scratch card game mode |
| `?game=random` | Pick a random mode (default when loaded by the shell) |
| `?debug` | Show debug buttons (test dog, maze, sniper) and the safety-net countdown timer |

Example: `http://localhost:8000/submission/submission.html?game=slot&debug`

---

## Game overview

### Core loop

1. Ad starts → skip button shows a countdown timer (6 minutes)
2. Player spins/scratches → wins prizes (tickets, real-world items, etc.)
3. Ticket prizes refill the supply; the skip timer can be shortened by special prizes
4. When the skip timer reaches 0, the skip button turns gold — but if the player has tickets left, clicking it warns them they'll lose everything
5. Player enters **drain mode**: ticket prizes are disabled, the supply counts down to zero
6. At zero tickets → **Out of Tickets popup** offers "3 More Tickets" (bonus round) or "Skip Ad" (end)
7. If "3 More Tickets" is chosen, the bonus round begins: tickets refill indefinitely and the skip button requires a confirmation

### Tier system

Three upgrade tiers, each with a different visual theme and prize set. Every 6 spins (or 6 wins for slots) a tier-upgrade prize is guaranteed. Upgrades reveal a celebration popup before continuing.

### Minigames

Certain jackpot prizes trigger an interactive minigame instead of an instant win:

- **Dog popup** (`prize: dog`) — Pet the dog in the right spot within 2 minutes. Clicking wrong makes the dog cry and penalises you. If the timer runs out, the prize is cancelled.
- **Maze** (`prize: BTC`) — Drag Krakus the kraken through a maze to the exit within 30 seconds. Collision detection uses pixel data from `assets/maze.png`.
- **Sniper** (`prize: AWP Dragon Lore`) — Shoot 5 pop-up targets within a zoomed sniper scope. Miss more than 1 and the prize is cancelled.

### Skip button state machine

| State | Visual | Skip click behaviour |
|-------|--------|----------------------|
| Counting down | Grey, shows `Skip in M:SS` | Flash "not yet" denied sound |
| Unlocked, tickets > 0 | Gold | Show "ABANDON PRIZES?" confirm; if confirmed → drain mode |
| Drain mode, tickets > 0 | Grey, shows ticket count | Show "LAST WARNING" warn popup; if confirmed → drain mode continues |
| Drain mode, tickets = 0 | — | OOT popup shown automatically |
| Bonus round | Gold | Show "ABANDON PRIZES?" confirm → skip ad |
| Safety net (10 min) | Gold | Show "ABANDON PRIZES?" confirm regardless of other state |

### 10-minute safety net

If the player is still in the game after 10 minutes the skip is unconditionally unblocked regardless of ticket count or drain state. This ensures the game can never become truly impossible to escape. With `?debug`, a countdown timer for this is visible top-right.

---

## File structure

```
submission/
├── submission.html       — entire game (single file)
└── assets/
    ├── ILC-AD.mp4        — Infinite Luck Casino promo video (plays before the game)
    ├── maze.png          — maze image used for pixel-based collision detection
    ├── dusty.png         — building background for the sniper minigame
    ├── krakus.png        — Krakus sprite (maze minigame player)
    ├── target.png        — target sprite (sniper minigame)
    ├── Doot.png          — dog sprite (dog popup)
    ├── DootCry.png       — dog cry sprite
    ├── DootLove.png      — dog love sprite
    ├── awp-dl.png        — AWP Dragon Lore image
    ├── CrowPro.png       — fake antivirus logo (Crow Pro)
    ├── SeraphSecure.png  — fake antivirus logo (Seraph Secure)
    ├── WindowsRG.png     — fake OS logo
    ├── WWWW.png          — fake browser logo
    ├── alert.mp3         — (unused, kept for reference)
    ├── awp_shot.mp3      — sniper shot sound
    ├── doot_bark.mp3     — dog bark sound
    ├── doot_kiss.mp3     — dog kiss sound
    ├── waka.mp3          — looping background music for the dog popup
    └── fonts/
        ├── Bungee-Regular.ttf
        └── BungeeInline-Regular.ttf
```

All assets are self-contained. No external network requests are made.

---

## Code structure (submission.html)

The entire game is a single HTML file with one `<script>` block wrapped in an IIFE. Sections are clearly marked with `// ═══` banners. Reading order from top to bottom:

| Section | What it does |
|---------|-------------|
| **CSS** | All styles inline in `<style>`. Debug elements have `display: none` by default. |
| **HTML** | Static markup for all popups, game containers, and debug buttons. |
| **ROUTING** | Reads `?game=` param, picks random game if not specified, sets `ACTIVE_GAME` and `IS_DEBUG`. |
| **TIERS** | Prize definitions and weights for each tier × game mode combination. |
| **STATE VARIABLES** | All mutable game state declared at the top of the IIFE. |
| **AUDIO** | `AudioContext` + `_assetAudio` for MP3s. All synth sounds (`playKaChing`, `playJackpot`, etc.) use Web Audio oscillators — no external files needed for game sounds. |
| **PRIZE LOGIC** | `_pickPrizeWheel`, `_pickPrizeSlot`, `pickWinningPrize` — weighted random selection with drain-mode and skip-reduction prize gating. |
| **SKIP SYSTEM** | `startSkipCooldown`, `unlockSkip`, `adjustSkipCooldown`, skip button click handler, `showLosePrizesConfirm`. |
| **WIN / COLLECT** | `showWinPopup`, `_renderWinPopup`, `collectBtn` click handler. |
| **GAME MODES** | Wheel spin, slot spin (`_doSpinSlot`, `onAllReelsDone`), scratch card (`dealNewCard`). Each has its own section. |
| **MINIGAMES** | Dog popup, maze (canvas + pixel collision), sniper (canvas + scope rendering). Each is self-contained. |
| **UPGRADE POPUP** | Tier upgrade celebration with animated background tiles. |
| **SHELL API** | `adStarted` / `adFinished` handlers + built-in ILC-AD.mp4 video overlay. |

---

## Shell API usage

| Sent | When |
|------|------|
| `pause` | Any popup opens or minigame starts |
| `play` | Popup dismissed, spin result shown, no pending prize |
| `setVolume: 0` | On `adStarted` — silences shell background video permanently |
| `success` | Player confirms skip |
| `getVideoInfo` | On `adStarted` — used to detect if running inside the real shell |

The submission **does not call `fail`**. The game is intended to never end by timeout — the player must choose to skip.

---

## Known limitations

- **Maze on `file://`**: Chrome blocks `canvas.getImageData()` when the image is loaded from disk due to canvas taint security rules. This is not an issue when served over HTTP (the normal judging environment).
- The maze pixel-collision map assumes the canvas renders `maze.png` at its natural 1254×1254 resolution into the offscreen canvas. On very unusual DPR configurations this could theoretically drift, but has been tested on standard desktop Chrome/Edge.
