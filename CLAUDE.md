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
- Full-width **floating pill**: `position:fixed; bottom:20px; left:12px; right:12px` (not centered/auto-width anymore — it spans the viewport minus 12px insets)
- Logo and right column hidden; only the 5 nav tabs shown inside the pill, each `flex:1` (equal width, splits evenly) except the active tab
- Pill: `background: rgba(90,90,90,0.32)`, `backdrop-filter: saturate(180%) brightness(1.15) blur(20px)`, `border: 1px solid rgba(255,255,255,0.22)`, `border-radius:999px`, `padding:6px`, layered shadow (`0 8px 32px rgba(0,0,0,0.30)` + `inset 0 1px 0 rgba(255,255,255,0.14)`)
- Tab sizes: `height:40px`, `font-size:12px`, inactive text `rgba(255,255,255,0.8)`, active `#fff` / `font-weight:600`
- **Active-tab icon swap**: each tab has a hidden `.tab-icon` (inline SVG, 20px) and a `.tab-label` (text). When `.nav-tab.active`, the icon shows and the label hides — so the active tab becomes an icon-only circle (`flex:0 0 40px`, `padding:0`) while the other tabs (still text) redistribute the freed space via `flex:1 1 0`. Icons: home / briefcase / package / two-people / person for Home / Work / Products / Community / About.
- Indicator (`#header-pill-indicator`) is `top:6px; height:40px` (inset to match the pill's `padding:6px`, not `top:0;height:100%` — that touches the outer edge). Since the active tab is a 40×40 square, the indicator naturally renders as a **circle** there. Transition uses a springy `cubic-bezier(0.34, 1.56, 0.64, 1)` (slight overshoot) instead of the desktop's linear ease.
- `.mobile-nav-scrim` — a fixed full-width gradient (`linear-gradient(to top, rgba(8,8,8,0.85)…transparent)`, `height:130px`, `z-index:99`, below the pill's `z-index:100`) fades scrolling page content before it reaches the nav, so text/logos don't bleed messily through the glass pill.
- Scroll-frosted class does NOT apply to the bottom nav (always has its own glass bg)

### Hero layout (`page-home`)
Order on all viewports: `ascnd.` logo (mobile only) → headline → video card → buttons → trust logos

**Hero inner wrapper** (`.hero-inner`):
- `display:flex; flex-direction:column; align-items:center; justify-content:flex-start`
- Desktop: `padding: 160px 24px 40px`
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
- Desktop: row layout, all three inline
- Mobile: `flex-direction:row; flex-wrap:wrap`. Primary ("Book a call") is `flex:0 0 100%` (its own full-width row); the two secondary buttons ("See our work" / "Our services") are `flex:1 1 calc(50% - 5px)` and sit side-by-side on the row below
- "Our services" is a plain anchor to `#services-home` (no `data-page`) so it smooth-scrolls to the services section on the same page instead of switching to the separate Services page — works on both desktop and mobile since `scroll-behavior:smooth` is global

**Trust section** (`.hero-trust`):
- `margin-top: 40px` below buttons
- Logos: Talented Grid, Wakefit, prompterdoer, TNT — all link out except TNT (no URL given)
- **Mobile carousel**: on `≤768px` the 4 logos auto-rotate — `#hero-trust-logos` is an `overflow:hidden` viewport (`max-width:300px`), `#trust-carousel-track` is the flex row of `<a class="hero-trust-logo">` items (JS clones the first 3 and appends them for a seamless loop). Shows 3 logos at a time; a `setInterval` (2600ms) shifts `translateX` by one item-width (`transform 0.6s ease`), and on reaching the cloned tail it snaps back to `translateX(0)` with `transition:none` for the illusion of an infinite loop. Desktop is unaffected — the same markup lays out as a normal centered wrapping flex row there (`.trust-carousel-track` base rule mirrors the old `.hero-trust-logos` flex/gap/wrap).

**Mobile logo** (`.hero-mobile-logo`):
- `display:none` on desktop; shown on mobile above headline
- `font-size: 28px`, `font-weight: 700`, centered

### Service cards (`#services-home`)
- Section title: "Three ways we deploy AI into your business" — single line, `white-space: nowrap` on desktop
  - Mobile: the heading text is split with a `<span class="title-break"> AI into your business</span>` — on `≤768px` `.title-break{display:block}` forces the exact break "Three ways we deploy" / "AI into your business" (not a natural reflow — deliberate 2-line split)
- Section padding: `20px 0 80px`
- All 3 cards now have a `.card-video` (16:9 placeholder) at top, then `.card-content` below
- Dark glass cards: `bg-white/8`, `border-white/15`, `backdrop-filter blur(12px)`, `border-radius: 20px`
- No timeline/duration lines — removed
- No gradient header blocks — replaced with `.card-video` placeholders
- READ MORE button: glass pill, `height: 36px`, hover inverts to white
- **Mobile card-content layout**: on `≤768px`, `.card-content` becomes a CSS grid (`grid-template-areas: "title btn" / "desc desc"`) so the title and READ MORE button sit on the same row (button shrinks to `height:28px; font-size:9px`) and the description wraps full-width below. `align-content:start` + zeroing `.card-desc`'s base `flex:1`/`margin-bottom:20px` prevents the dead vertical space that grid's default row-stretching + the old flex-fill would otherwise leave at the bottom of the card.

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
- `assets/images/prompterdoer-logo.webp` — prompterdoer logo (bg removed, trimmed from `prompterdoer.png`)
- `assets/images/tnt-logo.webp` — TNT wordmark logo (from `tnt website/public/tnt-logo-mark.png`)
- `assets/images/landing-bg.webp` — hero background image, `id="hero-bg-image"` (preloaded, `fetchpriority="high"`, LCP element). Mobile-only `object-position:66% center !important` (vs. default `center`) — the mountain peak sits at ~68% across the source image, so on narrow/tall mobile viewports the default centered crop pushed it off the right edge; shifting the focal point right re-centers the subject.
- `assets/js/liquid-glass.js` — liquid glass refraction library (MIT, deepika-builds/liquid-glass)

### Performance
- Hero bg and trust logos are WebP (`landing-bg.webp` ~157KB, `wakefit-logo.webp` ~8.4KB)
- Hero image is the LCP element: `<link rel="preload" as="image">` in `<head>` + `fetchpriority="high"` on the `<img>`
- `index.html` itself is ~200KB single file (inline `<style>` dominates) — not yet minified

### ⚠️ CSS cascade gotcha — mobile media query placement
The single `<style>` block has its `@media (max-width:768px)` rules physically **early** in the file (~line 200-340), but many unconditional "base" rules for the same classes (`.nav-tab`, `.card-content`, `.card-desc`, `.hero-trust-logos`, `.trust-carousel-track`, etc.) are defined **later**, unconditionally. Since source order breaks ties when specificity is equal, those later base rules silently win on mobile too, clobbering the earlier mobile override. This has bitten the nav pill (`flex-basis`, text-overflow ellipsis), the trust carousel (`justify-content`, `width`), and the service cards (`display`, `margin-bottom`) more than once.
**Fix pattern:** when adding/editing a mobile-only override for a class that also has an unconditional base rule later in the file, add `!important` to the overridden properties (or raise selector specificity) rather than assuming source order will save you. When something "doesn't apply" on mobile despite looking correct, check for a same-specificity rule further down the file first.

## Deployment
- GitHub Pages
- `CNAME`: `ascend.build`
