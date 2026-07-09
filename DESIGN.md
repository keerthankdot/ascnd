# Ascnd — DESIGN.md

## Brand identity

**Name:** Ascnd (stylised lowercase: `ascnd.`)
**Positioning:** AI venture studio — builds custom automations, AI products, and internal systems for businesses.
**Aesthetic:** Blueprint precision meets premium dark glass. Technical but approachable.

---

## Typography

- **Stack:** `'Helvetica Neue', Helvetica, Arial, sans-serif`
- No custom font loaded — intentional. Helvetica Neue = clean, authoritative, no load cost.
- **Weights used:** 400 (body), 600 (nav, secondary buttons), 700 (headings, primary buttons)
- **Sizes:**
  - Hero headline: `clamp(2.5rem, 5vw, 4rem)` on detail pages; large on home
  - Section titles: `~2rem`
  - Body: `16px`
  - Nav tabs: `15px / 600`
  - Button labels: `15px / 700` (primary), `15px / 600` (secondary)
  - Labels/tags: `12px / 600`, uppercase, `letter-spacing: 0.05em`

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
| `--bp-card-bg` | `#FFFFFF` | Card backgrounds |
| `--bp-card-border` | `#EBEBEB` | Card borders |
| `--bp-card-hover-border` | `#BBBBBB` | Hovered card borders |
| `--bp-cta-bg` | `#222222` | Dark CTA buttons |
| `--bp-cta-text` | `#FFFFFF` | CTA button text |
| `--bp-dashed` | `#EBEBEB` | Dashed dividers |
| `--bp-divider` | `#EBEBEB` | Section dividers |

---

## Hero

- Full-viewport background: `assets/images/landing-rotated.png` fixed, `object-fit: cover`
- Dark painterly landscape (mountains, forest, lake, rowers) — provides rich texture for glass effects to refract
- Hero text and buttons sit over the image with no overlay — image is dark enough for white text contrast
- Layout switches to two-column (text + video col) at wider breakpoints

---

## Glass effects

### Nav pill indicator (`#header-pill-indicator`)
Pure CSS glass. Tracks the active tab with a smooth `left` / `width` transition.

```css
backdrop-filter: blur(14px) saturate(220%) brightness(1.12);
background: linear-gradient(145deg, rgba(255,255,255,0.32), rgba(255,255,255,0.10), rgba(255,255,255,0.18));
border: 1px solid rgba(255,255,255,0.55);
box-shadow:
  inset 0 1.5px 0 rgba(255,255,255,0.85),
  inset 0 -1px 0 rgba(255,255,255,0.12),
  inset 1px 0 0 rgba(255,255,255,0.25),
  0 4px 20px rgba(0,0,0,0.07);
```

### Hero buttons — liquid glass (`.hero-btn-primary`, `.hero-btn-secondary`)
Uses `assets/js/liquid-glass.js` (MIT — deepika-builds/liquid-glass).

**How it works:**
1. Canvas draws a displacement map: red = X axis, blue = Y axis, blurred grey inset = neutral interior
2. SVG `<feDisplacementMap>` filter uses that map to physically bend backdrop pixels at the edges
3. R/G/B channels displaced at staggered scales → chromatic aberration (prism fringe)
4. `backdrop-filter: url(#filter-id) blur(1px) saturate(2.5)` wires it all together

**Current params:**
```js
liquidGlass(btn, {
  scale: -160,   // displacement strength
  chroma: 10,    // prism fringe per channel
  blur: 1,       // backdrop blur (low = sharper refraction)
  saturate: 2.5, // colour pop behind glass
  border: 0.04,  // neutral inset fraction
  mapBlur: 6,    // edge band softness
  fallbackBlur: 14
})
```

**Critical rule:** buttons must NOT have `overflow: hidden` — it blocks SVG `backdrop-filter`. Shape is maintained by `border-radius` alone.

**Browser support:** Chromium only (Chrome, Edge, Arc, Brave). Safari/Firefox get `blur(14px) saturate(2.5)` frosted fallback automatically.

**CSS dressing (kept in stylesheet, not owned by the library):**
```css
background: linear-gradient(145deg, rgba(255,255,255,0.32), rgba(255,255,255,0.10), rgba(255,255,255,0.18));
border: 1px solid rgba(255,255,255,0.55);
box-shadow:
  inset 0 1.5px 0 rgba(255,255,255,0.85),
  inset 0 -1px 0 rgba(255,255,255,0.12),
  inset 1px 0 0 rgba(255,255,255,0.25),
  0 4px 20px rgba(0,0,0,0.07);
```
Plus a `::before` glare: `linear-gradient(160deg, rgba(255,255,255,0.18) 0%, transparent 50%)`

---

## Layout patterns

### Header
`position: fixed`, 3-column CSS grid: `1fr auto 1fr`
- Left: empty (logo sits outside header on home)
- Center: pill nav
- Right: CTA button

### Blueprint grid
`.blueprint-page::before` — 60px major grid (transparent by default, can be activated)
`.blueprint-page::after` — 12px minor grid (same)
Corner registration marks (`.corner-mark`) at all four viewport corners.

### Cards
- `border-radius: 12–16px`
- `border: 1px solid var(--bp-card-border)`
- Hover: border shifts to `--bp-card-hover-border`, bg to `--bp-card-hover-bg`
- No heavy shadows — flat premium feel

### Section anatomy
```
.section-label   // "// process" — small uppercase tag
.section-title   // main heading
.section-body    // prose
```

---

## Motion

- All transitions: `0.18–0.2s` with `ease` or `cubic-bezier(0.4, 0, 0.2, 1)`
- Nav pill indicator: CSS transition on `left` + `width`
- Buttons: `opacity 0.18s, transform 0.18s` — lifts `1px` on hover
- Dropdowns: `opacity + transform(scale + translateY)` fade-in
- No scroll animations or JS-driven entrance effects — keeps it fast and clean
