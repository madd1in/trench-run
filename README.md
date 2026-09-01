# Nova Squadron

An on-rails polygon shooter in a single HTML file. Fly the corridor at wave-top height, keep the interceptors off your squadron, and punch through to the carrier holding station at the end of the run.

**▶ Play it: https://madd1in.github.io/trench-run/**

No build step, no dependencies, no asset files. One `index.html`:

- a small **flat-shaded 3D pipeline** written from scratch on Canvas 2D — model transforms, perspective projection, painter's-algorithm depth sorting, two-sided directional lighting with a bounce fill, emissive faces, and distance fog
- a **procedural score** and every sound effect synthesised live with the Web Audio API
- **radio comms** through the Web Speech API, with subtitles when no voice is installed

> The repository is still named `trench-run` from the project this grew out of, so the published URL stays where it was.

## Controls

| | |
|---|---|
| `←` `↑` `↓` `→` or `WASD` | fly |
| `SPACE` | lasers — hold to charge a homing shot |
| `Q` / `E`, or double-tap left/right | barrel roll |
| `SHIFT` / `CTRL` | boost / brake |
| `B` | smart bomb |
| `P` | pause |
| `SHIFT` + `F` | expand to fullscreen |

On a touchscreen: drag anywhere to fly, and holding also fires.

## The run

- **Surface, then canyon, then the carrier.** Arches in the canyon are solid — the gap is between the pillars.
- **Your squadron.** Kestrel, Pike and Marlow fly ahead of you and can be tailed by an interceptor. Shoot it off them before their shield runs out, or you finish the mission alone. Their bars are on the left of the HUD.
- **Gold rings** patch twelve points of shield. Fly through the middle.
- **Barrel roll** deflects incoming fire. It is the answer to a wall of bolts, not the answer to a gantry.
- **The dreadnought** is armoured everywhere except its core, and the core only opens while the main gun fires. Which is also the moment it is shooting at you.

## Performance

The renderer targets 60 fps and reports both the frame rate and the actual per-frame work in the HUD. Quality adapts on **measured frame cost**, not on frame rate — a throttled or backgrounded tab does not trigger a downgrade. Above 12.5 ms it drops the bloom pass, then ground detail; below 7.5 ms it puts them back. A full scene — eight interceptors, nine structures, rings, and forty bolts in flight, bloom on — measures around 5 ms median and 10 ms at the 95th percentile against the 16.7 ms budget.

## Browser notes

- The bloom pass needs `CanvasRenderingContext2D.filter`. Where that is missing the toggle disables itself and the game renders without it.
- Speech depends on the platform's installed voices; comms always appear as subtitles regardless.
- Audio only starts after the first click, as browsers require.
- Best scores are kept per difficulty in `localStorage`.

## Credits

Written with [Claude Code](https://claude.com/claude-code). Typefaces are Bruno Ace SC, Saira Condensed and Share Tech Mono from Google Fonts.

An original homage to the on-rails polygon shooters of the 16-bit era. Not affiliated with, authorised by, or endorsed by Nintendo or any other rights holder; the ships, callsigns, music and artwork here are original and generated entirely by the code in this repository.
