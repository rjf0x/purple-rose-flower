# Purple Rose

An animated purple rose built with plain HTML, CSS and JavaScript. A stem rises from the bottom of the screen, leaves unfold along it, and a bud on top opens petal by petal. Afterward the rose sways, sparkles, and sheds petals that drift down.

No frameworks, no build step, no dependencies. Everything lives in one file.

## Run it

1. Save `index.html` anywhere on your computer.
2. Open it in a modern browser (Chrome, Edge, Firefox, Safari).

That is all. The only network request is the optional DM Mono font from Google Fonts for the button labels. If you are offline, the buttons fall back to your system monospace font and everything else is unchanged.

## What you see

- **Stem and leaves.** The stem grows in about 1.6 seconds. Three rounded leaves unfold as the stem passes them.
- **Bloom.** The flower opens over about 7 seconds. It has 34 petals placed 137.5° apart (the golden angle), and the outer petals open first.
- **Ambient motion.** The stem sways, the flower head turns slowly, and small sparkles twinkle around it.
- **Falling petals.** A petal drops every few seconds once the rose is mostly open.
- **Light.** A soft highlight on the petals follows your mouse or finger, and the flower head leans slightly toward it.

## Controls

| Control | What it does |
| --- | --- |
| Bloom again | Grows a new rose with a different petal shape and replays the whole animation. |
| Shed petals | Releases seven petals at once. |

## Reduced motion

If your system is set to reduce motion, the rose appears fully bloomed and still. "Bloom again" becomes "New rose" and generates a new shape without animating, and "Shed petals" is hidden.

## How it works

- **Canvas drawing.** The rose, stem, leaves, sparkles and falling petals are drawn on a single `<canvas>`.
- **Pre-drawn shapes.** Each petal, leaf, sepal and sparkle is painted once into a small off-screen canvas, with its soft shadow included. Each frame only copies and transforms those images, which is far cheaper than redrawing curves and shadows 60 times a second.
- **Separate flower head.** The 34 petals are composed in their own small canvas, so the light and shading only touch the flower and not the stem.
- **CSS glow.** The violet glow behind the rose is a CSS animation, so it costs the script nothing.
- **Frame pacing.** The loop holds to roughly 60 frames per second. If frames run long on a slow device, it lowers its drawing resolution in steps (a pixel ratio of up to 1.5, then 1, then 0.7) until it keeps up.
- **Seeded randomness.** Each rose is generated from a random seed, so petal sizes, tones and leaf placement differ every time.

## Customize

Colors live in CSS variables at the top of the `<style>` block:

| Variable | Default | Effect |
| --- | --- | --- |
| `--hue` | `276` | Petal hue, read by the script on load. Try `340` for pink or `0` for red. |
| `--leaf-hue` | `158` | Hue of the leaves and stem. |
| `--bg` | `#150a22` | Page background. |
| `--glow-a`, `--glow-b` | violet `rgba` values | Colors of the halo behind the rose. Change these along with `--hue`. |
| `--accent` | `#cfa8ff` | Button highlight color. |

Timing and structure are constants at the top of the script:

| Constant | Default | Effect |
| --- | --- | --- |
| `N` | `34` | Number of petals. |
| `STEM` | `1.6` | Seconds for the stem to rise. |
| `BLOOM` | `7` | Seconds for the flower to open. |

Petal proportions, leaf positions and the number of sparkles are set inside the `build()` function.

## Files

- `purple-rose-standalone.html` is the complete page: markup, styles and script.
- `README.md` is this file.

## Browser support

Any current browser with HTML canvas support. It uses `ResizeObserver` when available and falls back to the window resize event otherwise.
