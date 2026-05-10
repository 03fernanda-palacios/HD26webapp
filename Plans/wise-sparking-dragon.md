# Plan: fullPage.js 4-Section Scroll Experience

## Context

User wants a single `index.html` file (no framework, no bundler) that delivers a fullPage.js scroll experience with four full-page sections. All user-editable values must live in a top-level `CONFIG` object at the top of the embedded `<script>` block, each property marked with `/* EDITABLE */`, so swapping content never requires hunting through code. This is a net-new file — no existing code to modify.

---

## Output

**File to create:** `/Users/fernanda/Desktop/projects/HD26webapp/index.html`

No other files. Images are drop-ins placed by the user into `/images/`.

---

## CDN Versions

- fullPage.js **v3.1.2** (free, no license required)
  - CSS: `https://cdnjs.cloudflare.com/ajax/libs/fullPage.js/3.1.2/fullpage.min.css`
  - JS: `https://cdnjs.cloudflare.com/ajax/libs/fullPage.js/3.1.2/fullpage.min.js`

---

## CONFIG Object Structure

```js
const CONFIG = {
  /* EDITABLE */ date: "November 8, 2018",
  /* EDITABLE */ gridBg: "#000000",
  images: {
    /* EDITABLE */ nasa:  "images/nasa.jpg",
    /* EDITABLE */ epa:   "images/epa.jpg",
    /* EDITABLE */ usgs:  "images/usgs.jpg",
    /* EDITABLE */ usda:  "images/usda.jpg",
  },
  /* EDITABLE */ logo:        "images/logo.png",
  /* EDITABLE */ productName: "Mooch",
  /* EDITABLE */ tagline:     "Track every dollar, effortlessly.",
  /* EDITABLE */ description: "Mooch is the modern way to manage shared expenses. Simple, fast, and fair.",
};
```

---

## Section Implementations

### Section 1 — Date Screen
- `background: #000`
- Flex center (both axes)
- `<span>` populated via `textContent = CONFIG.date`
- Font: `font-size: clamp(2.5rem, 8vw, 10rem)`, `font-weight: 100`, `letter-spacing: 0.2em`, white, uppercase — stark/editorial feel

### Section 2 — Agency Grid
- Section element is a flex container (fullPage.js-safe)
- Inner `.agency-grid` wrapper: `display: grid; grid-template-columns: 1fr 1fr; grid-template-rows: 1fr 1fr; width: 100%; height: 100%`
- Each `.grid-cell`: `display: flex; flex-direction: column` — img fills remaining space, label is fixed height at bottom
- Images: `width: 100%; flex: 1; object-fit: cover; min-height: 0; background: #222` (CSS fallback for missing assets)
- Labels: white text, small uppercase, `background: rgba(0,0,0,0.75)`, centered
- Background set via `document.getElementById('section-grid').style.backgroundColor = CONFIG.gridBg`

### Section 3 — Product Intro
- Section: `display: flex; align-items: center; justify-content: center; background: #111`
- Inner `.product-layout`: `display: flex; flex-direction: row; align-items: center; gap: 6vw; padding: 4rem; max-width: 1200px`
- Logo: left flex child, `max-width: 260px`, `img { width: 100%; height: auto; background: #333 }` (fallback for missing logo)
- Text: right flex child, headline from CONFIG.productName, tagline from CONFIG.tagline, description from CONFIG.description

### Section 4 — Stats Placeholder
- `background: #1a1a1a`
- HTML comment `<!-- REPLACE: stats content goes here -->` immediately before `.stats-container`
- 3 `.stat-card` elements: each with `.stat-title` ("Stat Title") and `.stat-value` ("000")
- Cards: dark surface (`#252525`), border, padding, centered text

---

## DOM Binding Pattern (after CONFIG, before fullpage init)

```js
document.getElementById('date-text').textContent     = CONFIG.date;
document.getElementById('section-grid').style.backgroundColor = CONFIG.gridBg;
document.getElementById('img-nasa').src              = CONFIG.images.nasa;
document.getElementById('img-epa').src               = CONFIG.images.epa;
document.getElementById('img-usgs').src              = CONFIG.images.usgs;
document.getElementById('img-usda').src              = CONFIG.images.usda;
document.getElementById('product-logo-img').src      = CONFIG.logo;
document.getElementById('product-name').textContent  = CONFIG.productName;
document.getElementById('product-tagline').textContent = CONFIG.tagline;
document.getElementById('product-desc').textContent  = CONFIG.description;
```

---

## /images Comment Block

```html
<!--
  IMAGES FOLDER STRUCTURE
  Drop your assets here and update the CONFIG paths above.

  /images/
    ├── nasa.jpg    → CONFIG.images.nasa
    ├── epa.jpg     → CONFIG.images.epa
    ├── usgs.jpg    → CONFIG.images.usgs
    ├── usda.jpg    → CONFIG.images.usda
    └── logo.png    → CONFIG.logo
-->
```

---

## ISC Checklist (38 criteria, Extended tier)

CONFIG: ISC-1 through ISC-11 (object declaration + 10 property slots, each with EDITABLE comment)
fullPage.js: ISC-12 CSS CDN, ISC-13 JS CDN, ISC-14 scrollOverflow: false, ISC-15 autoScrolling: true, ISC-16 fitToSection: true
Section 1: ISC-17 black bg, ISC-18 white text, ISC-19 centered, ISC-20 bound to CONFIG.date, ISC-21 editorial font-size
Section 2: ISC-22 2×2 grid wrapper, ISC-23 bg from CONFIG.gridBg, ISC-24 four img srcs bound, ISC-25 object-fit: cover, ISC-26 labels below each image
Section 3: ISC-27 flexbox row layout, ISC-28 logo from CONFIG.logo, ISC-29–31 three text bindings, ISC-32 vertically centered
Section 4: ISC-33 dark bg, ISC-34 three stat cards, ISC-35 REPLACE comment
Structure: ISC-36 EDITABLE on every CONFIG property, ISC-37 /images comment block, ISC-38 single file

---

## Verification

1. Open `index.html` directly in browser (file:// is fine — no server needed for static assets check)
2. Confirm fullPage.js loads (no console errors about missing library)
3. Scroll through all 4 sections — each should snap to full height
4. Section 1: date text visible centered in white on black
5. Section 2: 2×2 grid cells fill the screen; labels at bottom of each cell
6. Section 3: two-column layout with logo placeholder left, text right
7. Section 4: three stat cards visible; inspect HTML for REPLACE comment
8. Verify CONFIG is at top of `<script>` and every property has `/* EDITABLE */`
