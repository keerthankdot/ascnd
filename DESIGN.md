# Ascnd — DESIGN.md

## Brand identity

**Name:** Ascnd (stylised lowercase: `ascnd.`)
**Positioning:** AI venture studio — builds custom automations, AI products, and internal systems for businesses.
**Aesthetic:** Blueprint precision meets premium dark glass. Technical but approachable.

---

## Typography

- **Stack:** `'Helvetica Neue', Helvetica, Arial, sans-serif` — no external font loaded
- **Weights used:** 400 (body), 500 (nav, buttons), 600 (card READ MORE), 700 (headings, card titles)
- **Sizes:**
  - Hero headline: `clamp(42px, 6vw, 76px)` / 800 desktop; `36px` mobile
  - Section titles: `~2rem`
  - Body: `16px`
  - Nav tabs desktop: `16px / 500`
  - Nav tabs mobile: `12px / 500`
  - Hero buttons: `15px / 500`
  - Card titles: `24px / 700` desktop; `20px` mobile
  - Card desc: `14px / 400`
  - Card READ MORE: `11px / 600`, uppercase, `letter-spacing: 0.06em`; mobile `9px`, `height:28px` (sits beside the title instead of below the description)
  - Labels/tags: `11–12px / 600`, uppercase, `letter-spacing: 0.08–0.1em`

---

## Colour system

All colours live as CSS custom properties on `:root`.

| Token | Value | Use |
|---|---|---|
| `--bp-bg` | `#FFFFFF` | Page background |
| `--bp-text-primary` | `#222222` | Headings, body |
| `--bp-text-secondary` | `#717171` | Subtext, labels |
| `--bp-text-tertiary` | `#AAAAAA` | Faint UI |
| `--bp-text-faint` | `#DDDDDD` | Decorative |
| `--bp-accent-cyan` | `#1E4D8C` | Brand blue accent |
| `--bp-accent-green` | `#16A34A` | Status/success |
| `--bp-card-border` | `#EBEBEB` | Light-page card borders |
| `--bp-cta-bg` | `#222222` | Dark CTA buttons |
| `--bp-cta-text` | `#FFFFFF` | CTA button text |

**Glass palette (dark hero/services sections):**
- Surface: `rgba(255,255,255,0.08)` — card bg
- Surface light: `rgba(255,255,255,0.04)` — automation card bg
- Border: `rgba(255,255,255,0.15–0.25)` — card/button borders
- Text primary: `#ffffff`
- Text secondary: `rgba(255,255,255,0.65)`
- Text faint: `rgba(255,255,255,0.5–0.55)`
- Divider: `rgba(255,255,255,0.12)`

---

## Hero

- Full-viewport background: `assets/images/landing-bg.webp` (`id="hero-bg-image"`) fixed, `object-fit: cover`
- Dark painterly sunset landscape (snow-capped mountain, forest silhouette)
- Content order: `ascnd.` logo (mobile only) → headline → video card → buttons → trust logos
- No subtitle text (`.hero-sub` unused on home)
- Mobile-only: `object-position: 66% center !important` — the mountain peak sits ~68% across the source image, so the default centered crop clipped it off-screen on narrow/tall viewports; shifting the focal point right re-centers the subject

**Hero inner wrapper (`.hero-inner`):**
- `display:flex; flex-direction:column; align-items:center; justify-content:flex-start`
- Desktop: `padding: 160px 24px 40px`
- Mobile: `padding-top: 32px` override (tightened from 80px to pull hero content up so the third button clears the bottom nav)

**Hero video card (`.hero-video-card` / `.hero-video-inner`):**
```css
.hero-video-card {
  max-width: 620px;
  margin: 16px auto 0;
  border-radius: 16px;
  background: rgba(255,255,255,0.06);
  border: 1px solid rgba(255,255,255,0.15);
  backdrop-filter: saturate(180%) brightness(1.05) blur(12px);
}
.hero-video-inner { aspect-ratio: 16 / 9; }
```
Placeholder only — no video source wired up yet. Mobile: `max-width: 100%`.

---

## Trust logos (`.hero-trust`)

Four logos — Talented Grid, Wakefit, prompterdoer, TNT — all linking out (except TNT, no URL supplied). `filter: brightness(0) invert(1)` flattens every logo to a plain white silhouette so mixed brand marks (colored, chrome/glossy, wordmark) read consistently in the glass hero.

**Desktop:** normal centered, wrapping flex row (`.trust-carousel-track` mirrors `.hero-trust-logos`: `display:flex; gap:40px; flex-wrap:wrap; justify-content:center`).

**Mobile — auto-rotating carousel**, 3 logos visible at a time:
- `#hero-trust-logos` is the `overflow:hidden` viewport, `max-width:300px`
- `#trust-carousel-track` is the flex row of logo `<a>` items; on init, JS clones the first 3 items and appends them to the end (for a seamless wrap) and sets each item's pixel `width` to `viewport.width / 3`
- `setInterval(2600ms)` increments an index and sets `track.style.transform = translateX(-index * itemWidth)` with `transition: transform 0.6s ease` — shifts the row left by one slot, new logo slides in from the right
- On reaching the cloned tail (`index === originalItems.length`), a `transitionend` listener snaps back to `translateX(0)` with `transition:none` and resets the index — since the clones are exact copies of the originals, the jump is visually seamless
- **Gotcha hit during build:** the *desktop* base rule for `.trust-carousel-track` sets `justify-content:center; width:100%` — on mobile this shrank the track to the container width and centered the (much wider) overflowing content, so the visible window showed the *middle* of the sequence instead of the start. Mobile override needs explicit `justify-content:flex-start !important; width:max-content !important` to size the track to its true content width and anchor it left.

---

## Glass design language

The site's interactive layer uses one consistent glass recipe across nav, buttons, and cards:

```
background: rgba(255,255,255,0.08–0.10)
border: 1px solid rgba(255,255,255,0.15–0.20)
backdrop-filter: saturate(180%) brightness(1.05) blur(2px)   /* buttons/nav */
backdrop-filter: saturate(180%) brightness(1.05) blur(12px)  /* cards */
```

Hover pattern for buttons: inverts to solid white bg + `#222` text.

**Rule:** never use `overflow: hidden` on elements with this glass treatment — it breaks SVG `backdrop-filter` displacement.

---

## Nav pill — desktop (`#header-nav-pill` / `#header-pill-indicator`)

- Fixed header, 3-column grid `1fr auto 1fr` — logo left, pill center, right empty
- Only the **active tab** gets the glass pill — inactive tabs are plain white text (`rgba(255,255,255,0.65)`)
- Pill is an absolutely-positioned sliding element that animates via `left` + `width` CSS transition
- Tab sizes: `height: 48px`, `padding: 0 26px`, `font-size: 16px / 500`
- **Scroll behaviour**: transparent at `scrollY ≤ 10`; `.scrolled` adds `background: rgba(10,10,10,0.55)` + `backdrop-filter: blur(16px)`, `transition: 0.3s`

```css
#header-pill-indicator {
  background: rgba(255,255,255,0.10);
  border: 1px solid rgba(255,255,255,0.20);
  backdrop-filter: saturate(180%) brightness(1.05) blur(2px);
  transition: left 0.32s cubic-bezier(0.4,0,0.2,1), width 0.32s cubic-bezier(0.4,0,0.2,1);
}
```

## Nav pill — mobile (`≤768px`)

- Full-width floating pill: `position:fixed; bottom:20px; left:12px; right:12px` (no longer content-hugging/centered — spans the viewport minus insets)
- Logo and right column hidden — only the 5 tabs in the pill, each `flex:1` and evenly split (except the active tab, see below)

```css
#header-nav-pill {
  width: 100%;
  background: rgba(90, 90, 90, 0.32);
  backdrop-filter: saturate(180%) brightness(1.15) blur(20px);
  border: 1px solid rgba(255,255,255,0.22);
  border-radius: 999px;
  padding: 6px;
  box-shadow: 0 8px 32px rgba(0,0,0,0.30), inset 0 1px 0 rgba(255,255,255,0.14);
}
```

- Tab: `height:40px`, `font-size:12px`. Inactive text `rgba(255,255,255,0.8)`; active `#fff` / `font-weight:600`
- **Active tab → icon, others → text.** Each tab renders a hidden `.tab-icon` (20px inline SVG) next to a `.tab-label` (text). `.nav-tab.active .tab-icon{display:inline-flex}` + `.nav-tab.active .tab-label{display:none}` flips it — the active tab shows only its icon, everyone else shows only their label. Icons (simple filled SVG paths, `fill="currentColor"`): home, briefcase (Work), package (Products), two overlapping people (Community), single person (About).
- **Active tab becomes a circle.** `.nav-tab.active{flex:0 0 40px; padding:0}` — a fixed 40×40 square instead of an equal `flex:1` share. The other (text) tabs pick up the freed space via `.nav-tab:not(.active){flex:1 1 0}`. Because the active tab is exactly as wide as it is tall, the sliding indicator (which JS sizes to match `activeTab.getBoundingClientRect()`) renders as a perfect circle rather than a pill there.
- Indicator: `top:6px; height:40px` (must match the pill's own `padding:6px` — using `top:0;height:100%` makes it touch the outer edge since percentage-height on an absolutely-positioned child resolves against the *padding box*, not the content box). Transition uses a springy `cubic-bezier(0.34, 1.56, 0.64, 1)` (slight overshoot) instead of the desktop linear ease, `0.5s`.
- `.mobile-nav-scrim` — separate fixed full-width gradient (`linear-gradient(to top, rgba(8,8,8,0.85), transparent)`, `height:130px`, `z-index:99` — one below the pill's `z-index:100`) so scrolling content fades out before it reaches the nav instead of showing crisply/messily through the glass.
- Does NOT inherit `.scrolled` glass — always has own dark glass bg

---

## Hero buttons (`.hero-btn-primary`, `.hero-btn-secondary`)

Both variants identical — outline glass pill.

```css
height: 52px; padding: 0 28px; border-radius: 9999px;
font-size: 15px; font-weight: 500; letter-spacing: 0.025em;
color: #fff;
background: rgba(255,255,255,0.10);
border: 1px solid rgba(255,255,255,0.20);
backdrop-filter: saturate(180%) brightness(1.05) blur(2px);
transition: background-color 0.2s, color 0.2s;
margin-top: 24px; /* gap from video card */
```

Hover: `background: #fff; color: #222222`

Mobile: `flex-direction: row; flex-wrap: wrap`, `max-width: 320px` group. Primary ("Book a call") is `flex:0 0 100%` — its own full-width row. The two secondary buttons ("See our work" / "Our services") are `flex:1 1 calc(50% - 5px)` and sit side-by-side on the row beneath it.

"Our services" links to `#services-home` (a plain in-page anchor, no `data-page`) rather than switching to the separate Services page — it smooth-scrolls to the same-page services section on both desktop and mobile (`scroll-behavior:smooth` is set globally).

---

## Service cards (`.service-card`)

Dark glass cards in the `#services-home` section.

```css
border-radius: 20px;
padding: 14px;
background: rgba(255,255,255,0.08);
border: 1px solid rgba(255,255,255,0.15);
backdrop-filter: saturate(180%) brightness(1.05) blur(12px);
transition: transform 0.2s, background 0.2s;
```

Hover: `translateY(-4px)`, background lightens to `rgba(255,255,255,0.12)`.

**All 3 cards** now have a `.card-video` 16:9 placeholder at the top, then `.card-content` below:
```css
.card-video {
  width: 100%;
  aspect-ratio: 16 / 9;
  border-radius: 10px;
  background: rgba(255,255,255,0.05);
  border: 1px solid rgba(255,255,255,0.10);
}
```
No gradient header blocks — replaced entirely by video placeholders.

**READ MORE button** — glass pill matching hero buttons but smaller:
- `height: 36px`, `padding: 0 18px`, `font-size: 11px / 600`, uppercase
- Same `bg-white/10`, `border-white/20`, `backdrop-filter` recipe
- Hover: solid white + `#222` text

**No timeline lines** — duration text removed from all 3 cards.

Section title: `white-space: nowrap` so "Three ways we deploy AI into your business" stays one line on desktop. On mobile, the tail is wrapped in `<span class="title-break">` and `.title-break{display:block}` forces an exact 2-line break: "Three ways we deploy" / "AI into your business" — a deliberate split, not natural reflow (a `max-width`-based reflow was tried first but doesn't guarantee the same break across font-metric edge cases).

**Mobile card-content layout** (`≤768px`): `.card-content` switches to CSS grid —
```css
.card-content {
  display: grid;
  grid-template-columns: 1fr auto;
  grid-template-areas: "title btn" "desc desc";
  column-gap: 10px;
  align-items: start;
  align-content: start;   /* prevents grid from stretching to fill leftover flex height */
}
.card-title { grid-area: title; font-size: 20px; }
.card-read-more { grid-area: btn; height: 28px; padding: 0 12px; font-size: 9px; }
.card-desc { grid-area: desc; margin-top: 10px; }
```
Title stays left-aligned; READ MORE sits directly beside it (smaller pill) instead of below the description. Getting rid of the leftover dead space at the bottom of each card required also zeroing out `.card-desc`'s base (desktop) `flex:1` and `margin-bottom:20px` — those don't apply to a grid item's sizing directly, but the *unconditional* base rule was still winning on mobile (see cascade gotcha below) and stretching the grid container taller than its content.

---

## Liquid glass (`assets/js/liquid-glass.js`)

MIT — deepika-builds/liquid-glass. Applied only to `#card-automation-home`.

**How it works:**
1. Canvas draws a displacement map: red channel = X axis, blue = Y axis, blurred grey inset = neutral interior (no displacement inside, refraction only at edges)
2. SVG `<feDisplacementMap>` filter displaces backdrop pixels per channel
3. R/G/B channels displaced at staggered scales → chromatic aberration (prism fringe)
4. `backdrop-filter: url(#filter-id) blur(2px) saturate(2.0)` applies it

**Current params:**
```js
liquidGlass(card, {
  scale: -120,    // displacement strength
  chroma: 8,      // prism fringe per channel
  blur: 2,        // backdrop blur
  saturate: 2.0,  // colour pop
  border: 0.05,   // neutral inset fraction
  mapBlur: 18,    // edge band softness (larger = softer)
  radius: 20,     // must match card border-radius exactly
  fallbackBlur: 12
})
```

**Browser support:** Chromium (Chrome, Edge, Arc, Brave) only. Safari/Firefox get `blur(12px) saturate(2.0)` frosted fallback.

**Critical rule:** element must NOT have `overflow: hidden` — it silently kills the SVG displacement.

**Visibility note:** refraction requires high-contrast content behind the glass. Dark forest background = subtle effect.

---

## Layout patterns

### Header
`position: fixed`, 3-column CSS grid: `1fr auto 1fr`
- Left: logo (`ascnd.`) — hidden on mobile
- Center: pill nav — moves to bottom on mobile
- Right: empty

### Blueprint grid
`.blueprint-page::before` — 60px major grid (transparent by default)
`.blueprint-page::after` — 12px minor grid
Corner registration marks (`.corner-mark`) at all four viewport corners.

### Section anatomy
```
.section-label   // "// process" — small uppercase tag
.section-title   // main heading
.section-body    // prose
```

---

## Motion

- All transitions: `0.18–0.2s` with `ease` or `cubic-bezier(0.4, 0, 0.2, 1)`
- Nav pill indicator (desktop): `left` + `width` CSS transition, `0.32s cubic-bezier(0.4,0,0.2,1)`
- Nav pill indicator (mobile): same `left`/`width` transition but a springy `0.5s cubic-bezier(0.34, 1.56, 0.64, 1)` (slight overshoot) to sell the icon/circle morph
- Nav scroll fade: `background + backdrop-filter 0.3s ease`
- Trust logo carousel (mobile only): `setInterval` every 2600ms, `transform 0.6s ease` per shift, instant (`transition:none`) snap-back on loop reset
- Buttons: `background-color 0.2s, color 0.2s`
- Cards: `transform 0.2s, background 0.2s`
- Dropdowns: `opacity + transform(scale + translateY)` fade-in
- No scroll animations or JS entrance effects beyond the trust carousel

---

## ⚠️ Recurring gotcha: CSS cascade order in the single-file stylesheet

`index.html` has one big `<style>` block. Its `@media (max-width:768px)` rules live physically **early** in the file, but a lot of unconditional "base" rules for the same selectors (`.nav-tab`, `.card-content`, `.card-desc`, `.hero-trust-logos`, `.trust-carousel-track`...) are defined **later**, with no media guard. When specificity ties, later source wins — so those later unconditional rules silently override the earlier mobile-only rule, even on a mobile viewport. This caused real bugs during the mobile nav/carousel/card work: labels truncating via an inherited `text-overflow:ellipsis`, a `flex-basis:33%` from an unrelated legacy nav system, the trust carousel centering on the wrong slice, and dead space in the service cards.

**When writing a mobile override for a class that also has a base (non-media) rule further down the file:** add `!important` to the properties you're overriding, or raise selector specificity (e.g. `#id .class`) — don't rely on source order alone. If a mobile style "looks right in the CSS but doesn't apply," grep for the same selector elsewhere in the file before assuming the change didn't save.
