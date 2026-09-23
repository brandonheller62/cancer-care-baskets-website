# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page static HTML website for **Cancer Care Baskets by Brandon & Mom**, a charity delivering care baskets to breast cancer patients at Mount Sinai Comprehensive Cancer Center in South Florida. Hosted on Netlify (push to `main` deploys).

## Running the site

No build step. Serve the folder with any static file server (opening the file directly works too, but a server matches production):

```bash
python3 -m http.server 8080   # then visit http://localhost:8080
```

Third-party scripts come from cdnjs with pinned versions and SRI hashes: GSAP 3.15.0 + ScrollTrigger (animation), Chart.js 4.5.1 (impact charts, lazy-loaded). Do not add other libraries.

To replay the intro, clear sessionStorage (or open a new tab). To test reduced motion, turn it on in the OS or emulate it in DevTools (Rendering > prefers-reduced-motion).

The embedded dashboard iframe will not render locally: the dashboard's CSP only allows cancercarebaskets.org, Netlify and Vercel domains to frame it. The page falls back to a link after 15s.

## Architecture

Everything lives in one file: **`index.html`**

- **CSS**: all styles are in a `<style>` block in `<head>`, grouped by section. Colors and the sans font stack are CSS custom properties in `:root`. Prefer classes over inline styles.
- **HTML**: `#intro` overlay, `<header class="site-nav">` + mobile `<nav>` (a fixed overlay under the header), then `<main id="main">`: the `#hero` (canvas scrub), then anchor-linked sections `#watch` (video), `#testimonial`, `#story`, `#baskets`, `#help`, `#press`, `#survey`, `#impact`, `#donate`, `#sponsors`, then `<footer>`.
- **Head script**: adds `html.motion` when the visitor does not prefer reduced motion, and `html.intro-play` on the first visit of a browser session (`sessionStorage.ccbIntroSeen`). CSS uses `.motion` to pre-hide things that animate in, so nothing flashes.
- **JS** (inline `<script>` at the bottom, after the GSAP scripts). If GSAP failed to load, it removes `.motion` so the page falls back to its static, fully visible state. Functions:
  - `initIntro`: the ribbon intro itself is pure CSS (2.4s); JS skips it on scroll/click/touch/key and removes it.
  - `initCharts`: Chart.js is injected when `#impact` is ~800px away; each `canvas[data-chart]` is built when 35% visible. Data lives in the `specs` object. No animation for reduced motion.
  - `initMarquee`: wraps the Sponsors tier `[data-marquee]` pills in a seamless looping marquee (clone is `aria-hidden`). Skipped for reduced motion.
  - Motion only (`MOTION`): `initHero`, `initReveals` (headings fade up, cards batch-stagger in), `initTimeline` (rose `.tl-line` scrubs down the track, dots appear), `initParallax` (Dash + Brandon and Gail photos), plus a re-scroll to `location.hash` because the pinned hero adds scroll length after the browser's first jump.
  - Mobile menu, dashboard iframe resize + fallback (checks `e.origin`), PayPal copy-email button.

## Hero scroll scrub

- `frames/frame_001.webp` to `frame_124.webp` (1600x900) are drawn to `.hero-canvas`, cover-fit, DPR capped at 2, crop biased to 60% horizontal under 768px wide.
- Frame 1 is `<link rel="preload">`ed and is also the CSS background of `.hero` so there is never a blank hero. The rest load 4 at a time after frame 1, coarse to fine (every 16th, 8th, ...), and `render()` draws the nearest loaded frame, so the scrub never waits on the network.
- ScrollTrigger pins `.hero` from `top <nav height>` for `+=150%` with `scrub: 0.5`. One timeline drives the frame index, the overlay strength, and the `.hero-reveal` elements (tag, h1, text, buttons, stats) which rise in and are fully visible by the last frame. Stat counters (`data-count`, `data-suffix`) count up at 62% of the scrub.
- Reduced motion: no intro, no pin, no canvas; `.hero` shows `hero-end.jpg` as a static background with everything visible.
- Motion should stay calm (audience includes cancer patients): small distances (about 20 to 30px), slow ease-outs, no bounce, no fast loops.

## Assets

| File | Purpose |
|------|---------|
| `logo.png` | Header logo (104px, shown at 40px; resized from `web-app-manifest-512x512.png`) |
| `frames/frame_001.webp` ... `frame_124.webp` | Hero scroll-scrub image sequence (~3.5 MB total) |
| `hero-end.jpg` | Static hero for reduced motion and the no-GSAP fallback |
| `dash.jpg` | Photo of Dash the dog in CCB backpack |
| `brandon-gail-2018.jpg`, `brandon-gail-2026.jpg` | Brandon with Gail Brown at Mount Sinai (keep photos ~1200px max) |
| `ccb-video.mp4` | "Watch our story" video (H.264/AAC MP4, fast start; remuxed from `ccb-video-sm.mov`, which is kept but unreferenced) |
| `video-poster.jpg` | Video poster; also the Open Graph share image |
| `_headers` | Netlify cache headers for media |
| `site.webmanifest` | PWA manifest (name "Cancer Care Baskets", short_name "CCB", theme `#b5476a`) |

`favicon.svg` is 1.6 MB (embedded PNG) and intentionally not linked; the `.ico` and 96px PNG cover it.

## Design system (CSS variables)

```css
--rose: #aa4063        /* primary accent, CTAs (darkened for WCAG AA on rose-light) */
--rose-dark: #8e2f4f   /* hover */
--rose-light: #fbeaf0  /* backgrounds, tags */
--rose-mid: #e8a0b4    /* footer links, card hover borders */
--gold: #c9953a        /* divider bars only (fails contrast as text) */
--gold-dark: #8a6020   /* gold text: tier labels, liaison callout, gold pills */
--gold-light: #fdf6ec  /* gold sponsor background */
--cream: #fdf9f6       /* alternate section background */
--text: #2c1a1a        /* body text, footer background */
--muted: #7a5c5c       /* secondary text */
--border: #f0dde4      /* card borders */
```

## Conventions

- **No em dashes** in site copy. Use commas, colons, or periods.
- Interactive elements are at least 44px tall on touch (`.btn-small`, mobile menu links, footer links, linked pills via `::after` hit area).
- Decorative emoji/glyphs get `aria-hidden="true"`. `target="_blank"` links get `rel="noopener"`.
- Nav collapses to the hamburger at 900px; grids (including `.chart-grid`) collapse to one column at 640px.
- Mobile first: check layouts at 375px wide and on iPhone Safari. The hero uses `100svh` so the iOS URL bar does not resize it.
- Sections have `scroll-margin-top: 64px` so anchor jumps clear the sticky header.
- New animated content must stay visible when `.motion` is absent (reduced motion, no JS, GSAP blocked).

## Key content areas to know

- **Sponsor tiers**: Collaborators → Diamond → Platinum → Gold → Silver → Sponsors. Each uses a `.pill` class with a tier modifier (`diamond`, `platinum`, `gold`, `silver`, `sponsor`). Only the final Sponsors tier is a marquee (`data-marquee`); edit its pills in the HTML as usual.
- **Impact**: four Chart.js charts (data in `initCharts` > `specs`; keep each canvas `aria-label` in sync), then the live survey dashboard iframe below them. Chart colors: `#b5476a`, `#e8a0b4`, `#c9953a`.
- **Basket contents**: Chemotherapy Care Basket, Radiation Care Bundle, Mastectomy Recovery Bag, each in `.basket-card`.
- **Timeline**: `.tl-item` list entries, one per milestone.
- **Donation**: Venmo `Eugenia-Chu` (link), PayPal `chueugenia@yahoo.com` (copy button; no PayPal link exists). Appears in `#help` and `#donate`.
- **Contact**: `cancercarebaskets@gmail.com`.
