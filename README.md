# Cancer Care Baskets

The website for **Cancer Care Baskets by Brandon & Mom** — a charity that delivers care baskets to breast cancer patients at the Mount Sinai Comprehensive Cancer Center in South Florida.

🌐 **Live site:** [cancercarebaskets.org](https://cancercarebaskets.org)

---

## About

Cancer Care Baskets is a mother-and-son effort to bring comfort to people going through breast cancer treatment. Each basket is assembled around what patients actually tell us helps most — the contents evolve based on responses to our patient surveys.

The site tells the story, shows what goes into the baskets, invites people to help, and shares a **live analytics dashboard** of survey results.

## What's in this repo

A single-page static website. No build step, no dependencies.

| File | Purpose |
|------|---------|
| `index.html` | The entire site — HTML, CSS (in a `<style>` block), and minimal inline JS |
| `dash.jpg`, `brandon-gail-*.jpg` | Photos used throughout the page |
| `ccb-video-sm.mov` | Hero section video |
| `favicon.*`, `apple-touch-icon.png`, `web-app-manifest-*.png` | Icons and PWA assets |
| `site.webmanifest` | PWA manifest |

## Running locally

Open `index.html` directly in a browser, or serve it with any static file server:

```bash
python3 -m http.server 8080
# then visit http://localhost:8080
```

## Live survey dashboard

The **Data & Impact** section embeds a separately-hosted analytics dashboard via an `<iframe>` pointing at [ccb-survey-dashboard.vercel.app](https://ccb-survey-dashboard.vercel.app). It loads lazily and auto-resizes to its content height through a small `postMessage` handshake, so results stay live and in sync as new survey responses come in — no changes to this repo required.

## Deployment

Hosted on **Netlify** with continuous deployment. Every push to `main` triggers a new deploy.

- **Build command:** _(none — static site)_
- **Publish directory:** `.`

## Contact

- 📧 cancercarebaskets@gmail.com
- 💜 Venmo `@Eugenia-Chu` · PayPal `chueugenia@yahoo.com`
