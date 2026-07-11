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

### Nav — desktop
- Fixed header, 3-column grid: left (logo), center (pill nav), right (empty)
- Tabs: Home, Work, Products, Community, About
- Active tab only gets the glass pill indicator — inactive tabs are plain white `rgba(255,255,255,0.65)`
- Indicator slides with smooth CSS transition (`left` + `width`)
- Sizes: `height: 48px`, `padding: 0 26px`, `font-size: 16px`
- **Scroll behaviour**: transparent at top (`scrollY ≤ 10`); on scroll, `.scrolled` class adds `background: rgba(10,10,10,0.55)` + `backdrop-filter: blur(16px)` with `0.3s` transition

### Nav — mobile (`≤768px`)
- Moves to a **bottom pill** fixed `16px` from bottom, horizontally centered
- Logo and right column hidden; only the 5 nav tabs shown inside the pill
- Pill: `background: rgba(18,18,18,0.82)`, `backdrop-filter: blur(24px)`, `border-radius: 999px`, `padding: 3px`
- Tab sizes: `height: 32px`, `padding: 0 11px`, `font-size: 12px`
- Scroll-frosted class does NOT apply to the bottom nav (always has its own glass bg)

### Hero layout (`page-home`)
Order on all viewports: `ascnd.` logo (mobile only) → headline → video card → buttons → trust logos

**Hero inner wrapper** (`.hero-inner`):
- `display:flex; flex-direction:column; align-items:center; justify-content:flex-start`
- Desktop: `padding: 160px 24px 120px`
- Mobile: `padding-top: 80px` (overridden via `.hero-inner` media query)

**Headline** (`.hero-headline`):
- `font-size: clamp(42px, 6vw, 76px)` / weight 800 / desktop
- Mobile: `36px` — wraps naturally to 2 lines, no forced `<br>`

**Hero video card** (`.hero-video-card`, `.hero-video-inner`):
- Glass card: `max-width: 620px`, `margin: 16px auto 0`, `border-radius: 16px`
- `.hero-video-inner`: `aspect-ratio: 16/9` — empty placeholder, no `<video>` src yet
- Mobile: `max-width: 100%`

**Hero buttons** (`.hero-btn-primary`, `.hero-btn-secondary`):
- Same style — outline glass pill, `height: 52px`, `padding: 0 28px`, `font-size: 15px`
- `margin-top: 24px` from video card
- Desktop: row layout; Mobile: stacked column, `max-width: 320px`, full-width each

**Trust section** (`.hero-trust`):
- `margin-top: 40px` below buttons
- Labels: Talented Grid, Wakefit, Prompterdoer

**Mobile logo** (`.hero-mobile-logo`):
- `display:none` on desktop; shown on mobile above headline
- `font-size: 28px`, `font-weight: 700`, centered

### Service cards (`#services-home`)
- Section title: "Three ways we deploy AI into your business" — single line, `white-space: nowrap`
- Section padding: `56px 0 80px`
- All 3 cards now have a `.card-video` (16:9 placeholder) at top, then `.card-content` below
- Dark glass cards: `bg-white/8`, `border-white/15`, `backdrop-filter blur(12px)`, `border-radius: 20px`
- No timeline/duration lines — removed
- No gradient header blocks — replaced with `.card-video` placeholders
- READ MORE button: glass pill, `height: 36px`, hover inverts to white

### Liquid glass (`assets/js/liquid-glass.js`)
Applied only to `#card-automation-home` via `liquidGlass()` in a `requestAnimationFrame` at end of `<body>`.
- `scale: -120`, `chroma: 8`, `blur: 2`, `saturate: 2.0`, `border: 0.05`, `mapBlur: 18`, `radius: 20`
- Chromium only (Chrome, Edge, Arc, Brave) — frosted blur fallback on Safari/Firefox
- Element must NOT have `overflow: hidden` — it blocks SVG `backdrop-filter` displacement
- See `DESIGN.md` for full technique notes

### Search strip
- Airbnb-style search bar: "Problem" field with suggestion dropdown, "Industry" dropdown, "Timeline" dropdown
- JS matches input to services and surfaces a result card

### Assets
- `assets/images/talented-logo.webp` — Talented Grid logo
- `assets/images/wakefit-logo.webp` — Wakefit logo
- `assets/images/landing-rotated.webp` — hero background image (preloaded, `fetchpriority="high"`, LCP element)
- `assets/js/liquid-glass.js` — liquid glass refraction library (MIT, deepika-builds/liquid-glass)

### Performance
- Hero bg and trust logos are WebP (`landing-rotated.webp` ~131KB, `wakefit-logo.webp` ~8.4KB)
- Hero image is the LCP element: `<link rel="preload" as="image">` in `<head>` + `fetchpriority="high"` on the `<img>`
- `index.html` itself is ~200KB single file (inline `<style>` dominates) — not yet minified

## Deployment
- GitHub Pages
- `CNAME`: `ascend.build`
