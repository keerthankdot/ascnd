# Ascnd — CLAUDE.md

## What we're building

**ascend.build** — marketing site for Ascnd, an AI venture studio that builds custom tech (automations, AI products, internal systems) for businesses. Deployed via GitHub Pages with a CNAME pointing to `ascend.build`.

## Current site: `index.html` (single file SPA)

Everything lives in one `index.html`. Client-side JS swaps `.page-view` divs to simulate pages — no router, no framework.

### Pages
- `page-home` — hero + trust logos + `#services-home` section (3 service cards: Automations, AI Products, Systems). Services grid is scoped to this page only.
- `page-work` — work/case study grid with filters and search (no services grid)
- `page-services` / `page-services-process` — services overview
- `page-process` — 5-step circular process diagram
- `page-ai-products` — AI Products detail + FAQ + examples
- `page-systems` — Systems detail + FAQ + examples
- `page-automations` — Automations detail + FAQ + examples
- `page-products` — Products page (coming soon stub)
- `page-about` — founder profile
- `page-community` — freelancer network (stub)

### Known structural note
`page-home` contains two child wrappers: a `z-index:2` hero div (closed before the services grid) and the `#services-home` section. Both are inside `page-home` — do not add extra closing `</div>` between them or the services grid escapes into the global DOM and renders on every tab.

### Design system
- Font: Helvetica Neue / Helvetica / Arial
- Colors:
  - `--bp-bg: #FFFFFF`
  - `--bp-text-primary: #222222`
  - `--bp-text-secondary: #717171`
  - `--bp-accent-cyan: #1E4D8C` (dark blue brand accent)
  - `--bp-accent-green: #16A34A`
  - `--bp-card-border: #EBEBEB`
  - CTA bg: `#222222`
- Blueprint grid aesthetic — subtle grid via CSS `::before` / `::after` pseudo-elements on `.blueprint-page`
- Glass-morphism nav pill with sliding indicator (`#header-pill-indicator`)
- Corner registration marks (`.corner-mark`)

### Nav
- Fixed header, 3-column grid: left (empty), center (pill nav), right (CTA)
- Tabs: Home, Work, Products, About
- Sliding pill indicator tracks active tab with smooth CSS transition

### Search strip
- Airbnb-style search bar: "Problem" field with suggestion dropdown, "Industry" dropdown, "Timeline" dropdown
- JS matches input to services and surfaces a result card

### Trust logos (hero)
- Talented Grid
- Wakefit

### Assets
- `assets/images/talented-logo.webp` — Talented Grid logo
- `assets/images/wakefit-logo.png` — Wakefit logo
- `assets/images/landing-rotated.png` — hero background image

## Deployment
- GitHub Pages
- `CNAME`: `ascend.build`
