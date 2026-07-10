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
- Font: Helvetica Neue / Helvetica / Arial (no external font loaded)
- Colors: see `DESIGN.md` for full token table
- Dark glass aesthetic — all interactive elements use `bg-white/10`, `border-white/20`, `backdrop-filter: saturate(180%) brightness(1.05) blur(2px)`
- Blueprint grid aesthetic — subtle grid via CSS `::before` / `::after` pseudo-elements on `.blueprint-page`
- Glass pill nav with sliding indicator (`#header-pill-indicator`) — only active tab gets the pill
- Corner registration marks (`.corner-mark`)

### Nav
- Fixed header, 3-column grid: left (logo), center (pill nav), right (CTA)
- Tabs: Home, Work, Products, Community, About
- Active tab only gets the glass pill indicator — inactive tabs are plain white text
- Indicator slides with smooth CSS transition (`left` + `width`)
- Sizes: `height: 48px`, `padding: 0 26px`, `font-size: 16px`

### Hero buttons (`.hero-btn-primary`, `.hero-btn-secondary`)
- Same style — outline glass pill
- `height: 52px`, `padding: 0 28px`, `font-size: 15px`
- `bg-white/10`, `border-white/20`, `backdrop-filter: saturate(180%) brightness(1.05) blur(2px)`
- Hover: solid white bg, `#222` text
- No JS, no SVG filters — pure CSS, works everywhere

### Service cards (`#services-home`)
- Dark glass cards: `bg-white/8`, `border-white/15`, `backdrop-filter blur(12px)`
- No timeline/duration lines — removed
- Header blocks use CSS gradients (no Unsplash images):
  - Card 1 (Automations): deep blue `#0f2a4a → #1E4D8C`
  - Card 2 (AI Products): navy `#16213e → #0d3b6e`
  - Card 3 (Systems): dark purple `#1a0a2e → #2d1b69`
- `#card-automation-home` is stripped — no header block, near-transparent `bg-white/4`, liquid glass JS applied
- READ MORE button: glass pill matching hero buttons, hover inverts to white
- Font: Helvetica Neue throughout (Gilroy removed)

### Liquid glass (`assets/js/liquid-glass.js`)
Applied only to `#card-automation-home` via `liquidGlass()` in a `requestAnimationFrame` at end of `<body>`.
- `scale: -120`, `chroma: 8`, `blur: 2`, `saturate: 2.0`, `border: 0.05`, `mapBlur: 18`, `radius: 20`
- Chromium only (Chrome, Edge, Arc, Brave) — frosted blur fallback on Safari/Firefox
- Element must NOT have `overflow: hidden` — it blocks SVG `backdrop-filter` displacement
- See `DESIGN.md` for full technique notes

### Search strip
- Airbnb-style search bar: "Problem" field with suggestion dropdown, "Industry" dropdown, "Timeline" dropdown
- JS matches input to services and surfaces a result card

### Trust logos (hero)
- Talented Grid
- Wakefit
- Prompterdoer

### Assets
- `assets/images/talented-logo.webp` — Talented Grid logo
- `assets/images/wakefit-logo.png` — Wakefit logo
- `assets/images/landing-rotated.png` — hero background image
- `assets/js/liquid-glass.js` — liquid glass refraction library (MIT, deepika-builds/liquid-glass)

## Deployment
- GitHub Pages
- `CNAME`: `ascend.build`
