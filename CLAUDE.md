# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

A two-page static site. There is no build system, package manager, linter, or test suite.

- `index.html` — home / intro
- `photography.html` — photo gallery
- Assets live in `photos/` and `music/`

## Running locally

Open `index.html` directly, or serve the directory so relative audio/image paths resolve the same way they do when hosted:

```bash
python3 -m http.server 8000
```

## Home page

- Full-viewport grey placeholder (`.hero-bg`) until a looping video is added.
- To add video: place a file at `video/background.mp4` and uncomment the `<source>` in `index.html`. The script shows the video only when a `src` is present.
- Water ripples: a WebGL `.ripple-canvas` (inserted by script right after the video) redraws the video and bends it around the cursor — a gentle trail every ~40px of movement, a bigger splash on click/tap. Skipped with `prefers-reduced-motion`, and removed if WebGL or the video texture is unavailable (e.g. opened via `file://`), so serve the directory to see it.
- Water sound: the video's own audio track, routed through a Web Audio gain node. Silent until the first click/tap/key press (browser autoplay rules); after that, moving or tapping swells it to ~15% volume and it fades out ~2s after the visitor stops. No mute toggle by design.
- Left-aligned intro block, vertically centered at every screen size: name (EB Garamond), bio (Source Sans 3) with "adventure" linking to `photography.html`, then "say hello on linkedin or email" (email is a `mailto:` link).
- LinkedIn: `https://www.linkedin.com/in/charlotteyao27`

## Photography page

- Each photo is a `.photo-item` div in `<main id="gallery">` with an `<img>`, a `.photo-meta` overlay (location + date), and a `data-song` attribute pointing at an audio file in `music/`.
- Clicking an item adds `.active` (dims/zooms the image, reveals the overlay) and plays its `data-song` through the shared `<audio id="bg-music">`. The track only restarts if the clicked item's song differs from the one loaded.
- The first grid item shows a pulsing white dot (`.photo-item:first-child::before`) as a click hint; it is hidden once that item becomes active. Because it's keyed to `:first-child`, whatever photo is listed first gets the hint.
- Header name links back to `index.html`.
- Grid is 3 columns, dropping to 2 at ≤900px and 1 at ≤560px.

## Adding a photo

- Photos are ordered newest first in `photography.html`; insert new entries at the correct chronological position (usually the top).
- Filename convention: `<place><M><D><YY>.<ext>` with no separators, e.g. `zion5226.jpeg` → 5.2.26. Extensions vary in case (`.jpeg`, `.jpg`, `.JPG`) — match the actual file exactly, since the hosted site is case-sensitive.
- Displayed date format is `M.D.YY`; location text is lowercase and matches the `alt` text.
- Every item currently uses `./music/Morning in the Aves.m4a`.

## Design

- Design: "dewy misty naturalism"
- Palette: mist white, pale sage, moss, warm cream; faint prism accent
- Type: light serif headings (EB Garamond), clean sans body (Source Sans 3)
- Glass panels over blurred nature photos, keep text panels fairly opaque
- Motion: slow shimmer, drifting mist, soft fades
- Avoid: gingham/florals, neon, dark night moods
