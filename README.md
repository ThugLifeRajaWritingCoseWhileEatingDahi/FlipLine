# Flipline

A fast, one-button gravity-flip runner that runs in the browser. You auto-run along a rail, and every tap flips gravity between the floor and the ceiling. Dodge six kinds of obstacles, grab gold orbs, and beat your best score.

Everything lives in a single file, `flipline.html`. There are no dependencies, no build step and no audio or image assets.

## How to play

Flip gravity to dodge hazards. The longer you survive, the faster the run gets and the more obstacle types appear. Gold orbs give bonus points and a short speed jump.

### Controls

| Action | Keyboard | Mouse | Touch |
|---|---|---|---|
| Flip gravity | Space, Up, Down | Click the game | Tap anywhere |
| Pause or resume | Esc or P | Pause icon, top centre | Pause icon, top centre |
| Menu (from game over) | Esc | Menu button | Menu button |
| Obstacle guide | I (on the menu) | Obstacles button | Obstacles button |
| Mute sound and music | M | Sound button | Sound button |

The game pauses by itself when the tab loses focus or you switch apps.

## Obstacles

| Obstacle | Behaviour | How to beat it |
|---|---|---|
| Spikes | Fixed on the floor or ceiling, wider as you progress | Flip over them |
| Saws | Bob up and down across the full height | Time your flip around the motion |
| Lasers | Cover half the screen height and blink, with a red warning phase | Wait for the beam to switch off, or flip to the safe half |
| Meteors | A warning marker appears at the top, then the meteor drops fast | Be on the opposite rail when it lands |
| Shifter walls | A wall whose gap slides up and down | Flip through when the gap lines up |
| Ghost blocks | Fade in and out and are only solid while bright | Pass through while they are faded |

Obstacles unlock gradually: spikes, saws and lasers first, then meteors, then shifter walls and ghost blocks. Spacing between obstacles scales with your speed, so later stages stay hard but fair.

## Features

- Six unique obstacle types with warnings and timing patterns
- Progressive difficulty and speed ramp
- Gold orbs for bonus points and a speed boost
- Best score saved in the browser
- Synthesized sound effects and a very quiet ambient background loop, all generated with the Web Audio API
- Main menu, in-game obstacle guide, pause screen and game-over screen
- Keyboard, mouse and touch support
- Responsive layout for phones, with fullscreen and landscape lock where the browser allows it
- Respects the system light and dark theme and the reduced-motion setting

## Running it

Open `flipline.html` in any modern browser. It works from a local file or any static host (GitHub Pages, Netlify, an S3 bucket, and so on). Nothing needs to be installed.

Sound starts after your first tap or keypress, because browsers block audio until you interact with the page.

## Project structure

```
flipline.html   The whole game: markup, styles and script
README.md       This file
```

Inside `flipline.html` the script is organised roughly in this order: setup and state, audio engine, sound effects, background music, input handling, menu logic, obstacle spawning and collision, the update loop, rendering, and the main loop.

## Customising

The game is tuned with plain constants, so tweaking it is easy. Search the file for these:

| What | Where to look | Notes |
|---|---|---|
| Canvas size | `const W=800,H=400` | The layout and menu coordinates assume 800 by 400 |
| Run speed | `speed=300+Math.min(dist/40,260)` | Base speed and the maximum speed boost |
| Gravity strength | `P.vy+=P.dir*2100*dt` | Higher feels snappier |
| Obstacle unlock points | `dist>900`, `dist>1800`, `dist>3500` in `spawn()` | Distance needed before each type appears |
| Obstacle spacing | `spawnIn=speed*rnd(1.0,1.5)*...` | Lower the range for a harder game |
| Music volume | `BGM_VOL` | Set to `0` to disable the background loop |
| Colours | Hex values in the draw functions | The game scene is always dark |

To add a new obstacle, add its type name to the pool in `spawn()`, set its properties there, then add its movement and collision in `update()` and its look in `draw()`.

## Saved data

Two values are stored in the browser with `localStorage`:

- `flipline-best`: your best score
- `flipline-mute`: whether sound is muted

Both are wrapped in `try/catch`, so the game still works when storage is blocked, for example in private browsing.

## Browser support

Works in current versions of Chrome, Edge, Firefox and Safari, on desktop and mobile. It uses Canvas 2D, Web Audio and pointer events. Fullscreen and orientation lock are only offered where the browser supports them (iPhones do not support fullscreen for web pages).

## Known limitations

- The game is drawn on a fixed 800 by 400 canvas, so in portrait on a small phone the text is small. Turning the phone sideways gives a much better view.
- Collision boxes are slightly forgiving by design, so very close calls may count as a pass.
- There is no online leaderboard. Scores are local to each browser.

## Ideas for later

- A portrait layout with larger text
- Extra obstacle types and a difficulty selector
- A shared leaderboard
- Skins for the player cube

Add a licence of your choice before sharing the code publicly.
