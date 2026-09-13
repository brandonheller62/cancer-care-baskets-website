# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page static HTML website for **Cancer Care Baskets by Brandon & Mom**, a charity delivering care baskets to breast cancer patients at Mount Sinai Comprehensive Cancer Center in South Florida. Hosted on Netlify (push to `main` deploys).

## Running the site

No build system, no dependencies. Open `index.html` directly in a browser, or serve it with any static file server:

```bash
python3 -m http.server 8080
```

The embedded dashboard iframe will not render locally: the dashboard's CSP only allows cancercarebaskets.org, Netlify and Vercel domains to frame it. The page falls back to a link after 15s.

## Architecture

Everything lives in one file: **`index.html`**

- **CSS**: all styles are in a `<style>` block in `<head>`, grouped by section. Colors and the sans font stack are CSS custom properties in `:root`. Prefer classes over inline styles.
- **HTML**: `<header class="site-nav">` + mobile `<nav>`, then `<main id="main">` with anchor-linked sections: `#testimonial`, `#story`, `#baskets`, `#help`, `#press`, `#survey`, `#impact`, `#donate`, `#sponsors`, then `<footer>`.
- **JS**: one inline `<script>` at the bottom: mobile menu (aria-expanded, Escape to close), dashboard iframe resize + fallback (checks `e.origin`), and the PayPal copy-email button.

## Assets

| File | Purpose |
|------|---------|
| `dash.jpg` | Photo of Dash the dog in CCB backpack |
| `brandon-gail-2018.jpg`, `brandon-gail-2026.jpg` | Brandon with Gail Brown at Mount Sinai (keep photos ~1200px max) |
| `ccb-video-sm.mov` | Hero video (H.264, plays everywhere) |
| `video-poster.jpg` | Hero video poster; also the Open Graph share image |
| `_headers` | Netlify cache headers for media |
| `site.webmanifest` | PWA manifest |

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
- Nav collapses to the hamburger at 900px; grids collapse to one column at 640px.

## Key content areas to know

- **Sponsor tiers**: Collaborators → Diamond → Platinum → Gold → Silver → Sponsors. Each uses a `.pill` class with a tier modifier (`diamond`, `platinum`, `gold`, `silver`, `sponsor`).
- **Basket contents**: Chemotherapy Care Basket, Radiation Care Bundle, Mastectomy Recovery Bag, each in `.basket-card`.
- **Timeline**: `.tl-item` list entries, one per milestone.
- **Donation**: Venmo `Eugenia-Chu` (link), PayPal `chueugenia@yahoo.com` (copy button; no PayPal link exists). Appears in `#help` and `#donate`.
- **Contact**: `cancercarebaskets@gmail.com`.
