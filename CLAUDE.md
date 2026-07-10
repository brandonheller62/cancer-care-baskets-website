# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page static HTML website for **Cancer Care Baskets by Brandon & Mom** — a charity delivering care baskets to breast cancer patients at Mount Sinai Comprehensive Cancer Center in South Florida.

## Running the site

No build system, no dependencies. Open `index.html` directly in a browser, or serve it with any static file server:

```bash
python3 -m http.server 8080
```

## Architecture

Everything lives in one file: **`index.html`**

- **CSS** — all styles are in a `<style>` block in `<head>`. Color palette is defined via CSS custom properties in `:root` (rose, gold, cream, muted, border).
- **HTML** — single-page layout with anchor-linked sections: `#story`, `#baskets`, `#help`, `#press`, `#survey`, `#impact`, `#donate`, `#sponsors`.
- **JS** — minimal inline `<script>` at the bottom handles only the mobile hamburger menu toggle.

## Assets

| File | Purpose |
|------|---------|
| `dash.jpg` | Photo of Dash the dog in CCB backpack |
| `brandon-gail.jpg` | Brandon with Gail Brown at Mount Sinai (2018) |
| `chart1.png`, `chart2.png` | Patient survey result charts |
| `ccb-video-sm.mov` | Hero section video |
| `site.webmanifest` | PWA manifest (note: `name`/`short_name` still say "MyWebSite" — should be updated to match the site) |

## Design system (CSS variables)

```css
--rose: #b5476a        /* primary accent, CTAs */
--rose-light: #fbeaf0  /* backgrounds, tags */
--rose-mid: #e8a0b4    /* footer links */
--gold: #c9953a        /* divider bars, gold tier */
--gold-light: #fdf6ec  /* gold sponsor background */
--cream: #fdf9f6       /* alternate section background */
--text: #2c1a1a        /* body text */
--muted: #7a5c5c       /* secondary text */
--border: #f0dde4      /* card borders */
```

## Key content areas to know

- **Sponsor tiers** (lines 378–405): Diamond → Platinum → Gold → Silver → Sponsors. Each uses a `.pill` class with a tier modifier (`diamond`, `platinum`, `gold`, `silver`, `sponsor`).
- **Basket contents** (lines 230–253): Chemotherapy Care Basket and Radiation Care Bundle, each in `.basket-card`.
- **Timeline** (lines 202–208): `.tl-item` entries, one per milestone year.
- **Donation** — Venmo: `Eugenia-Chu`, PayPal: `chueugenia@yahoo.com` (appears in `#help`, `#donate`).
- **Contact** — `cancercarebaskets@gmail.com`.

## Responsive behavior

- Below 640px: `.story-grid` and `.basket-grid` collapse to single column; `.nav-links` hides and hamburger appears; `.press-grid` collapses (via `!important` override in the media query).
