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
  - Card titles: `24px / 700`
  - Card desc: `14px / 400`
  - Card READ MORE: `11px / 600`, uppercase, `letter-spacing: 0.06em`
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

- Full-viewport background: `assets/images/landing-rotated.webp` fixed, `object-fit: cover`
- Dark painterly landscape (mountains, forest, lake, rowers)
- Content order: `ascnd.` logo (mobile only) → headline → video card → buttons → trust logos
- No subtitle text (`.hero-sub` unused on home)

**Hero inner wrapper (`.hero-inner`):**
- `display:flex; flex-direction:column; align-items:center; justify-content:flex-start`
- Desktop: `padding: 160px 24px 120px`
- Mobile: `padding-top: 80px` override

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

- Moves to **bottom of screen**, centered, `16px` from bottom
- Logo and right column hidden — only the 5 tabs in the pill
- Pill: `background: rgba(18,18,18,0.82)`, `backdrop-filter: blur(24px)`, `border-radius: 999px`, `padding: 3px`
- Tab sizes: `height: 32px`, `padding: 0 11px`, `font-size: 12px`
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

Mobile: `flex-direction: column`, each button `width: 100%`, `max-width: 320px` group.

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

Section title: `white-space: nowrap` so "Three ways we deploy AI into your business" stays one line on desktop.

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
- Nav pill indicator: `left` + `width` CSS transition, `0.32s cubic-bezier(0.4,0,0.2,1)`
- Nav scroll fade: `background + backdrop-filter 0.3s ease`
- Buttons: `background-color 0.2s, color 0.2s`
- Cards: `transform 0.2s, background 0.2s`
- Dropdowns: `opacity + transform(scale + translateY)` fade-in
- No scroll animations or JS entrance effects
