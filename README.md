# ROND

A landing page for **ROND**, a fictional ceramics studio and kiln in Woodstock, Cape Town. I built it as a motion study: every animation recreates one from [neutomni.com](https://neutomni.com), a Webflow studio site whose idea and motion I like. The brand, copy and artwork are new.

The idea I borrowed is Neutomni's "the road can get bumpy, we keep you rolling": a polygon rolls along a bumpy road and turns into a circle as you scroll. For a pottery studio that is the whole job. Clay starts as a lump, and the wheel keeps turning until it comes out round.

## How I studied the reference

I used [REA](https://github.com/morluto/rea) against a headless Chromium session on neutomni.com:

- `analyze_web_bundle` listed all 27 scripts on the page and their libraries: GSAP 3.15 (ScrollTrigger, SplitText, MorphSVG, Observer, ScrollTo), Lenis 1.3, SplitType, CircleType, and Webflow's IX runtime.
- The motion turned out to be hand-written inline scripts and two Slater modules, not Webflow interactions. I read those to get the exact timings, eases and snapping rules.
- Scrolled screenshots of the reference are in `docs/reference/`.

## Animation map

| Neutomni | ROND | Notes |
|---|---|---|
| Logo paths drop from `y:-300`, 2 s, stagger 0.05 from centre | Wordmark pieces (R stem, bowl, leg…) drop the same way | Same values |
| 0 → 100 odometer built from digit strips | Odometer in the corner during load | Same strip maths (hundreds, tens, ones in steps of 5) |
| Hero lines blur in (`blur(10px)`, `y:15`, stagger 0.1) after the loader | Same, then the pottery strip scales in from 0 | |
| MorphSVG loop in the logo's "ō", rotating 60° per step | The "O" in ROND cycles circle → square → triangle → hexagon → pentagon → octagon | Shapes generated in code |
| `[data-reveal="blur"]` line reveals on scroll | Same attribute and timings | |
| Canvas wheel on a catenary road; square → 12-gon → circle | Same algorithm, drawn over the four process cards | Cards level out from a staircase as the wheel rounds |
| CircleType "Our Process" button | SVG `textPath` badge that spins with scroll | No library needed |
| Pinned portfolio: corner clip-path wipes, image zoom 1.1 → 1 → 1.2, dimmed list, Lenis snapping (forward at 10%, back at 90%) | Collections section, same timeline and snapping rules | Desktop only; on phones it becomes a swipe row |
| Hover track: shape follows the cursor, morphs at checkpoints, leaves fading trail outlines | "From rough to round" track, shows % round | Drag on touch, arrow keys on keyboard |
| Blurred gradient "We keep our focus on important things" + pricing tickets with barcodes | "…on what's on the wheel" + class tickets | Barcodes generated from each class name |
| Desktop finale with draggable windows that open from icons | Studio desk with FAQ, kiln log, a student note and directions | Pointer events, clamped to the desk |

## Stack

One `index.html`. GSAP 3.13 (ScrollTrigger, SplitText, MorphSVG, all free since 3.13) from cdnjs, and Lenis from jsDelivr. No build step and no images: every pot is an SVG drawn from a radius profile, with an unglazed foot like real stoneware.

Run it with any static server:

```sh
python3 -m http.server 8765
```

## Details

- Clock in Cape Town time (SAST).
- `prefers-reduced-motion` skips the intro, the logo loop, the blur reveals and smooth scrolling. Scroll-linked pieces still follow the scrollbar directly.
- The rough-to-round track is a keyboard slider.

ROND, its address and its people are made up. Animation credit goes to Neutomni.
