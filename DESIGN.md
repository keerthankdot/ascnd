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
  - Hero headline: `clamp(42px, 6vw, 76px)` / 800
  - Section titles: `~2rem`
  - Body: `16px`
  - Nav tabs: `16px / 500`
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

- Full-viewport background: `assets/images/landing-rotated.png` fixed, `object-fit: cover`
- Dark painterly landscape (mountains, forest, lake, rowers)
- Hero text and buttons sit over the image with no overlay
- Layout switches to two-column at wider breakpoints

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

## Nav pill (`#header-nav-pill` / `#header-pill-indicator`)

- Only the **active tab** gets the glass pill — inactive tabs are plain white text (`rgba(255,255,255,0.65)`)
- Pill is an absolutely-positioned sliding element (`#header-pill-indicator`) that animates via `left` + `width` CSS transition
- Tab sizes: `height: 48px`, `padding: 0 26px`, `font-size: 16px / 500`

```css
#header-pill-indicator {
  background: rgba(255,255,255,0.10);
  border: 1px solid rgba(255,255,255,0.20);
  box-shadow: 0 4px 6px -1px rgba(0,0,0,0.10), 0 2px 4px -1px rgba(0,0,0,0.06);
  backdrop-filter: saturate(180%) brightness(1.05) blur(2px);
  transition: left 0.32s cubic-bezier(0.4,0,0.2,1), width 0.32s cubic-bezier(0.4,0,0.2,1);
}
```

---

## Hero buttons (`.hero-btn-primary`, `.hero-btn-secondary`)

Both variants identical — outline glass pill.

```css
height: 52px; padding: 0 28px; border-radius: 9999px;
font-size: 15px; font-weight: 500; letter-spacing: 0.025em;
color: #fff;
background: rgba(255,255,255,0.10);
border: 1px solid rgba(255,255,255,0.20);
box-shadow: 0 4px 6px -1px rgba(0,0,0,0.10), 0 2px 4px -1px rgba(0,0,0,0.06);
backdrop-filter: saturate(180%) brightness(1.05) blur(2px);
transition: background-color 0.2s, color 0.2s;
```

Hover: `background: #fff; color: #222222`

Pure CSS — no JS, no SVG filters. Works in all browsers.

---

## Service cards (`.service-card`)

Dark glass cards sitting over the hero background image.

```css
border-radius: 20px;
padding: 14px;
background: rgba(255,255,255,0.08);
border: 1px solid rgba(255,255,255,0.15);
backdrop-filter: saturate(180%) brightness(1.05) blur(12px);
transition: transform 0.2s, background 0.2s;
```

Hover: `translateY(-4px)`, background lightens to `rgba(255,255,255,0.12)`.

**Header blocks** — CSS gradients, no images:
- Automations: `linear-gradient(135deg, #0f2a4a, #1E4D8C)` — deep blue
- AI Products: `linear-gradient(135deg, #16213e, #0d3b6e)` — navy
- Systems: `linear-gradient(135deg, #1a0a2e, #2d1b69)` — dark purple

**AI Automations card (`#card-automation-home`)** — different treatment:
- No header block — fully transparent glass (`bg-white/4`)
- Liquid glass JS applied (see below)
- Tagline shown as small uppercase label above the title

**READ MORE button** — glass pill matching hero buttons but smaller:
- `height: 36px`, `padding: 0 18px`, `font-size: 11px / 600`, uppercase
- Same `bg-white/10`, `border-white/20`, `backdrop-filter` recipe
- Hover: solid white + `#222` text

**No timeline lines** — duration text removed from all 3 cards.

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

**Visibility note:** the refraction effect requires high-contrast content behind the glass to be visible. On the dark forest background the effect is subtle. Adding a bright/colourful background behind the card section would make it dramatic.

---

## Layout patterns

### Header
`position: fixed`, 3-column CSS grid: `1fr auto 1fr`
- Left: logo (`ascnd.`)
- Center: pill nav
- Right: CTA button

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
- Buttons: `background-color 0.2s, color 0.2s`
- Cards: `transform 0.2s, background 0.2s`
- Dropdowns: `opacity + transform(scale + translateY)` fade-in
- No scroll animations or JS entrance effects
