# Red Five Trench Run

An on-rails polygon trench run in a single HTML file. It opens with a perspective crawl, jumps in, and drops you low across the battle station's surface. Hold the equatorial trench when it opens up beneath you, and put a proton torpedo down the thermal exhaust port at the end.

**▶ Play it: https://madd1in.github.io/trench-run/**

No build step, no dependencies, no asset files. One `index.html`:

- a small **flat-shaded 3D pipeline** written from scratch on Canvas 2D — model transforms, perspective projection, painter's-algorithm depth sorting, back-face culling, directional light with a bounce fill, a Blinn specular term and a camera-facing term, emissive faces, projected ground shadows, tumbling debris and distance fog
- a **procedural score** — an eight-bar chord cycle with timpani, snare, bass ostinato, pad, arpeggio and a brass lead through a dotted-eighth feedback delay, plus a separate progression for the run in
- **radio comms** through the Web Speech API, with subtitles when no voice is installed
- an **opening crawl** rendered in perspective on the canvas, then a hyperspace entry

Typography follows the films rather than the sci-fi shelf: the opening crawl of the original is set in News Gothic Bold, so the headings and the crawl here use **News Cycle**, its free equivalent, in the crawl's own yellow.

## Controls

| | |
|---|---|
| `←` `↑` `↓` `→` or `WASD` | fly |
| `SPACE` | lasers — hold to charge a homing shot; launches the torpedo on the run in |
| `Q` / `E`, or double-tap left/right | barrel roll |
| `SHIFT` / `CTRL` | boost / brake |
| `B` | torpedo salvo |
| `F` | targeting computer on / off |
| `P` | pause |
| `SHIFT` + `F` | expand to fullscreen (`ESC` to leave) |

Any key skips the crawl. It plays once per session; the **Crawl** toggle replays it on the next launch.

On a touchscreen: drag anywhere to fly, and holding also fires.

## The run

- **Surface, then trench, then the port.** The trench walls close in around you as you drop into it, and the gantries inside are solid — the gap is between the pillars.
- **Your squadron.** Red Two, Red Three and Red Six fly ahead of you and can be tailed by an interceptor. Shoot it off them before their shield runs out, or you finish the mission alone.
- **Shield boosters** patch a seventh of your deflector. Fly through the middle.
- **Barrel roll** deflects incoming fire. It answers a wall of green; it does not answer a gantry.
- **Three proton torpedoes, and they are the same three.** The salvo on `B` draws from the tubes you need at the end. Spend them all on TIEs and you will reach the port with nothing to shoot it with.
- **The targeting computer** builds a lock while you hold the port inside the brackets. Or press `F`, switch it off, and take the shot yourself — the tolerance is wider without it, and it is worth six thousand more.
- **Medals.** Bronze, Silver or Gold on the kill count. Gold on Ace is the one worth having.

Three flight statuses. **Recruit** is the default and is meant to be finished: a bigger deflector, slower and less frequent enemy fire, a wider lock box and a much more forgiving torpedo. **Pilot** is the old Standard. **Ace** is unkind.

## Fullscreen

Expanding goes edge to edge: no frame, no masthead, no page chrome. The controls fade back in when you move the pointer. The HUD scales with the viewport, and the field of view is fitted to whichever axis is tighter — so an ultrawide display shows more of the corridor instead of cropping the sky.

## Performance

The renderer targets 60 fps and reports both the frame rate and the actual per-frame work in the HUD. Quality adapts on **measured frame cost**, not on frame rate, so a throttled or backgrounded tab does not trigger a downgrade. Above 12.5 ms it drops the bloom pass, then ground detail; below 7.5 ms it puts them back.

Getting there took measurement rather than guesswork. An early build of this version cost 22 ms a frame. Profiling by removing one class of object at a time showed the ships dominated, and the single largest win was not fill rate at all — it was that every face built a fresh `rgb(...)` string for the browser to re-parse. Quantising colours to five bits per channel and caching the strings, along with back-face culling and off-screen rejection, brought a busy trench frame comfortably inside the 16.7 ms budget with room to spare.

## Browser notes

- The bloom pass needs `CanvasRenderingContext2D.filter`. Where that is missing the toggle disables itself and the game renders without it.
- Speech depends on the platform's installed voices; comms always appear as subtitles regardless.
- Audio only starts after the first click, as browsers require.
- Best scores are kept per difficulty in `localStorage`.

## Credits

Written with [Claude Code](https://claude.com/claude-code). Typefaces are News Cycle, Saira Condensed and Share Tech Mono from Google Fonts.

An unofficial fan tribute. Not affiliated with, authorised by, or endorsed by Lucasfilm Ltd. or The Walt Disney Company. Star Wars and all related marks are the property of their respective owners. Non-commercial; no assets from any film are used — every polygon and every sound here is generated by the code in this repository.
