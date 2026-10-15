# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

A two-page static site. There is no build system, package manager, linter, or test suite.

- `index.html` — home / intro
- `photography.html` — interactive photo deck
- Assets live in `photos/` (originals), `photos/web/` (smaller copies the site loads), `video/`, and `music/` (not used right now)

## Running locally

Open `index.html` directly, or serve the directory so relative audio/image paths resolve the same way they do when hosted:

```bash
python3 -m http.server 8000
```

## Home page

- Full-viewport grey placeholder (`.hero-bg`) until a looping video is added.
- To add video: place a file at `video/background.mp4` and uncomment the `<source>` in `index.html`. The script shows the video only when a `src` is present.
- Water ripples: a WebGL `.ripple-canvas` (inserted by script right after the video) redraws the video and bends it around the cursor — a gentle trail every ~40px of movement, a bigger splash on click/tap. Ripples are foreshortened to match the video's perspective: higher on screen they get smaller (down to half size) and flatter (oval). Skipped with `prefers-reduced-motion`, and removed if WebGL or the video texture is unavailable (e.g. opened via `file://`), so serve the directory to see it.
- Water sound: `video/water.m4a` (the video's audio, extracted with `afconvert -f m4af -d aac -b 128000 video/background.mp4 video/water.m4a`), looped in Web Audio with a 0.6s equal-power crossfade between passes so the seam doesn't gap. Re-extract it if the video changes. The video itself stays muted. Silent until the first click/tap/key press (browser autoplay rules); after that, moving or tapping swells it to ~15% volume and it fades out ~2s after the visitor stops. No mute toggle by design.
- Left-aligned intro block, vertically centered at every screen size: name (EB Garamond), bio (Source Sans 3) with "adventure" linking to `photography.html`, then "say hello on linkedin or email" (email is a `mailto:` link).
- LinkedIn: `https://www.linkedin.com/in/charlotteyao27`

## Photography page

- A single deck of glossy prints (`.print` figures in `#deck`): no border, each photo keeps its own shape inside a max box (`--print-w` / `--print-h`). Only the top 6 are drawn; each print has a fixed pseudo-random tilt so the pile looks hand-stacked.
- Forward (right arrow, or clicking the top print): the top print slides out right, then tucks back under the pile. Back (left arrow): the bottom print slides out left from underneath, then lands on top. Animations use the Web Animations API, with no libraries. With `prefers-reduced-motion` the pile just reorders.
- The caption under the deck shows the top print's `data-location` and `data-date`.
- Background: `video/background.mp4`, blurred, under a light mist overlay (`.haze`). No music on this page.
- At ≤700px the arrows move to their own row under the deck.
- "charlotte yao" at top left links back to `index.html`.

## Adding a photo

- Put the original in `photos/`, then make a web copy with the same filename in `photos/web/` (the page only loads web copies): `sips -s format jpeg -s formatOptions 70 -Z 1600 photos/NAME --out photos/web/NAME`. If the original is already smaller, copy it as-is.
- Add a `<figure class="print" data-location="..." data-date="..."><img data-src="./photos/web/NAME" alt="..." /></figure>` to `#deck`. Images use `data-src`; the script loads them as they near the top of the pile.
- Photos are ordered newest first (the first figure is the top of the pile); insert new entries at the correct chronological position (usually the top).
- Filename convention: `<place><M><D><YY>.<ext>` with no separators, e.g. `zion5226.jpeg` → 5.2.26. Extensions vary in case (`.jpeg`, `.jpg`, `.JPG`). Match the actual file exactly, since the hosted site is case-sensitive.
- Displayed date format is `M.D.YY`; location text is lowercase and matches the `alt` text.

## Design

- Design: "dewy misty naturalism"
- Palette: mist white, pale sage, moss, warm cream; faint prism accent
- Type: light serif headings (EB Garamond), clean sans body (Source Sans 3)
- Glass panels over blurred nature photos, keep text panels fairly opaque
- Motion: slow shimmer, drifting mist, soft fades
- Avoid: gingham/florals, neon, dark night moods
