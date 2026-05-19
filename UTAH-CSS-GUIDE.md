# Utah.css — User Guide & Technical Reference

**Version:** Utah-Omega-23 Protocol v4.1 (as declared in `utah.css`)  
**Philosophy:** Full-stack–style UI patterns using **HTML + CSS only**—no JavaScript framework required for navigation, modals, dismissible alerts, theme toggle, routing, or basic form feedback.

See also the project **[README.md](./README.md)** for the enterprise overview and quick-start modal.

This document is split so **non-technical readers** can copy working patterns, and **developers** can understand behavior, limitations, and customization.

---

## The Child Protocol — Why We Avoid Client-Side JavaScript

### The analogy (for everyone)

Imagine you want a secure vault for your toys.

- **Lego bricks (CSS):** Solid plastic. Pieces stay exactly where you place them. There are no gears or wires for a “spy robot” to hijack.
- **Clockwork + wires (JavaScript):** Can look impressive, but a bad actor can slip in a bad gear. When the machine runs, that gear can unlock the door from the inside.

UtahCSS is the Lego approach: **clicking a control checks a box**; **CSS changes shape** to show a menu or modal. No engine runs in the browser.

### The professional pivot (for developers)

Client-side JavaScript introduces an active runtime in the user’s browser—a large attack surface and the primary vector for XSS, arbitrary execution, and supply-chain risk (e.g. npm). UtahCSS uses a **declarative state machine**: hidden checkboxes/radios plus `:checked`, `:has()`, `:target`, and sibling combinators. The UI layer is **air-gapped from executable code** for everything the framework covers.

**Caveat:** Server logic, analytics you add yourself, or third-party embeds are separate concerns. UtahCSS eliminates JS for *UI state* shipped with the framework—not for your entire product unless you choose that architecture end-to-end.

---

## Part 1 — For everyone (non-technical overview)

### What is Utah.css?

Utah.css is a **stylesheet** (a design recipe) that turns plain web pages into a cohesive “terminal / sci-fi console” look: dark background, monospace type, red accents, and reusable building blocks (navigation bar, buttons, forms, alerts, and more).

You do **not** need to install special software to try it: if you have a web page that links to `utah.css`, opening that page in a browser applies the design.

### What you can build without coding “logic”

| Capability | In plain terms |
|------------|----------------|
| **Styled pages** | Headings, paragraphs, and layout helpers look consistent. |
| **Top navigation** | A fixed bar with links stays at the top while you scroll. |
| **“Multiple pages” in one file** | Different sections can show or hide based on the **link** someone clicks (uses the `#` part of the address). |
| **Light / dark** | A control can flip the site between a dark theme and a light theme (implemented with a hidden checkbox and CSS). |
| **Dropdown menu** | A menu appears when you hover or use keyboard focus—no scripts. |
| **Dismissible alert** | A notice with an **[X]** can disappear when clicked. |
| **Accordion** | Expand/collapse panels using built-in browser controls. |
| **Image-free “slider”** | Horizontal panels you can snap to using numbered links. |
| **Form hints** | Underlines can turn green/red to reflect valid or invalid input (browser validation + CSS). |
| **Secure modal** | A popup “vault” opens and closes via hidden checkboxes—no script. |

### What Utah.css does *not* do

- It does **not** send data to a server, save passwords, or run business rules—that still belongs to your server, your CMS, or (if you add it) your own JavaScript.
- It does **not** guarantee identical behavior on **very old** browsers; some features need modern CSS (see [Browser support](#browser-support)).

### How to “use” it if you are not a developer

1. Ask whoever maintains your site to **add one line** in the page `<head>`:  
   `<link rel="stylesheet" href="utah.css">`  
   (The path must point to where `utah.css` lives.)
2. Use the **copy-paste patterns** in [Part 3 — Tutorials](#part-3--tutorials-step-by-step) so class names match what the stylesheet expects (`btn`, `utah-nav`, etc.).

If class names or HTML structure do not match the examples, the design may not appear or interactive pieces may not work.

---

## Part 2 — Quick start (technical)

### Installation

There is no npm package in this repo for the CSS itself. Include the file directly:

```html
<link rel="stylesheet" href="utah.css">
```

Optional: serve `utah.css` from a CDN or bundle it into your asset pipeline; behavior is the same as long as the rules load.

### Minimal page skeleton

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>My Page</title>
  <link rel="stylesheet" href="utah.css">
</head>
<body>
  <main class="container">
    <h1>Hello</h1>
    <a class="btn" href="#">Primary action</a>
  </main>
</body>
</html>
```

`body` already receives top padding equal to `--nav-height` (60px) so content is not hidden under a fixed nav **if** you use `.utah-nav`. If you skip the nav, you may want to override padding in your own CSS.

---

## Part 3 — Tutorials (step by step)

### Tutorial A — Fixed navigation bar

**Goal:** Brand on the left, links on the right, bar stays on screen while scrolling.

1. Place this near the top of `<body>` (order matters for focus and layout):

```html
<nav class="utah-nav">
  <a href="#home" class="utah-brand">YOUR BRAND</a>
  <div class="utah-nav-links">
    <a href="#home" class="utah-nav-link">HOME</a>
    <a href="#about" class="utah-nav-link">ABOUT</a>
  </div>
</nav>
```

2. Wrap main content in `<main class="container">` (or another wrapper) for a centered max width of **1100px**.

**Customize:** Change link text and `href` values. Brand color uses `--plasma-red`.

---

### Tutorial: SPA Routing Without JavaScript {#tutorial-spa-routing-without-javascript}

**Goal:** Build a Single Page Application with native back-button support—no React Router, no JavaScript.

#### The analogy (for everyone)

Imagine a hallway with many doors, all in the dark. You have a flashlight that shines on **one** door at a time. When the light hits a door, that door opens; the rest stay hidden.

On a website, the URL hash (`#dashboard`) is the flashlight. If the URL is `#settings`, the browser “lights up” the section whose `id` is `settings`.

#### The step-by-step

1. Normally, “About Us” triggers JavaScript to hide the home page and render About.
2. With UtahCSS, **all pages live in one HTML file**, hidden by default.
3. Give each page an `id`: `<section id="dashboard" class="utah-page">`.
4. Nav links use hashes: `<a href="#dashboard">`.
5. CSS rule `:target` means *“If my name is in the URL bar, show me.”*

#### The professional pivot

SPA routing is usually handled by `react-router` (history API + DOM manipulation). UtahCSS uses **`:target`**: the browser updates history natively; CSS sets `display: block` on the matching section and `display: none` on others. **Zero bytes of executable routing code.**

#### Implementation

```html
<nav class="utah-nav">
  <a href="#dashboard" class="utah-btn">Dashboard</a>
  <a href="#settings" class="utah-btn">Settings</a>
</nav>

<main class="utah-router-view container">
  <section id="dashboard" class="utah-page">
    <h2>Secure Dashboard</h2>
    <p>Welcome to the main terminal.</p>
  </section>

  <section id="settings" class="utah-page">
    <h2>User Settings</h2>
    <p>Configure your secure preferences here.</p>
  </section>
</main>
```

**Core CSS (in `utah.css`):**

```css
.utah-page { display: none; }
.utah-page:target {
  display: block;
  animation: utahFadeIn 0.3s ease-in-out;
}
```

#### Default “home” route

With only the rules above, a section with **no** hash stays hidden. For a landing page, use the **home fallback** from `index.html`:

```html
<section id="home" class="utah-page" style="display:block;">
  <style>
    body:has(.utah-page:target) #home { display: none; }
    #home:target { display: block !important; }
  </style>
</section>
```

**Technical note:** `:has()` affects browser support; see [Browser support](#browser-support).

---

### Tutorial C — Modal (secure vault, no JavaScript)

**Goal:** Open/close an overlay using checkbox state.

**Single modal (matches README):**

```html
<input type="checkbox" id="modal-toggle" class="utah-hidden-state">
<label for="modal-toggle" class="utah-btn">Open Secure Vault</label>

<div class="utah-modal">
  <div class="utah-modal-content">
    <h2>Secure Data</h2>
    <p>Rendered via CSS only.</p>
    <label for="modal-toggle" class="utah-close-btn">Close</label>
  </div>
</div>
```

**Multiple modals:** wrap each set in `.utah-modal-root` with its own checkbox id; CSS uses `.utah-modal-root:has(.utah-hidden-state:checked) .utah-modal`.

**Do not** reuse `id="modal-toggle"` for more than one control on the same page.

---

### Tutorial D — Light / dark theme toggle (no JavaScript)

**Goal:** Clicking a label flips global colors.

1. Add a **hidden** checkbox **once** in the document (typically top of `body`):

```html
<input type="checkbox" id="theme-toggle" class="utah-hidden-state">
```

2. Use a `<label for="theme-toggle">` anywhere (e.g. in the nav) so clicking it toggles the checkbox.

3. Utah.css defines:

```css
html:has(#theme-toggle:checked) {
  --void-black: #e0e0e0;
  --void-deep: #ffffff;
  --neon-salt: #111111;
  --glass: rgba(0,0,0,0.05);
```

So checking the box **redefines** only those variables; everything using `var(--void-black)` etc. updates.

**Caveat:** `--plasma-red`, `--cyber-blue`, `--bio-green`, `--warn-gold` are **not** remapped in light mode—they stay as defined in `:root`. Tune those in your own override layer if contrast is insufficient.

---

### Tutorial E — Dropdown menu

```html
<div class="utah-dropdown">
  <button class="btn" type="button">Menu ▼</button>
  <div class="utah-dropdown-menu">
    <a href="#" class="utah-menu-item">Item A</a>
    <a href="#" class="utah-menu-item">Item B</a>
  </div>
</div>
```

**Behavior:** `.utah-dropdown-menu` becomes visible on **hover** or when focus is **inside** the dropdown (`:focus-within`). Keyboard users should tab into the button then into links.

---

### Tutorial F — Dismissible alert

```html
<div class="utah-alert alert-danger">
  <input type="checkbox" id="alert-1" class="utah-close-trigger">
  <strong>Title:</strong> Message text.
  <label for="alert-1" class="utah-close-btn">[X]</label>
</div>
```

**Behavior:** Checking the hidden checkbox makes the whole `.utah-alert` `display: none` via `:has(.utah-close-trigger:checked)`.

**Important:** Each alert needs a **unique** `id` on the checkbox and matching `for` on the label.

**Variants:** Add `alert-success` instead of (or in addition to) `alert-danger` for green accent.

---

### Tutorial G — Accordion

```html
<details class="utah-accordion">
  <summary class="utah-summary">Panel title</summary>
  <div class="utah-accordion-content">
    Panel body text.
  </div>
</details>
```

Uses native `<details>` / `<summary>`; the `+` / `-` indicator is pure CSS on `summary.utah-summary::after`.

---

### Tutorial H — Horizontal “carousel” with anchor jumps

```html
<div style="text-align:center;">
  <a href="#slide1" class="btn">1</a>
  <a href="#slide2" class="btn">2</a>
</div>
<div class="utah-carousel">
  <div id="slide1" class="utah-slide">A</div>
  <div id="slide2" class="utah-slide">B</div>
</div>
```

**Behavior:** `.utah-carousel` is a horizontal scroll container with `scroll-snap-type: x mandatory`. Links with `href="#slideX"` scroll the matching slide into view (`scroll-behavior: smooth` on the container).

**Tip:** Each `.utah-slide` is `flex: 0 0 100%` and **300px** tall—adjust in custom CSS if needed.

---

### Tutorial: Zero-Script Form Validation {#tutorial-zero-script-form-validation}

**Goal:** Real-time visual validation with HTML5 `pattern` and CSS—no validation library.

#### The analogy (for everyone)

A **JavaScript** bouncer reads your ID, radios central office, waits, then opens the door—someone could intercept the radio.

**UtahCSS** is a door with a key-shaped slot. If your ID matches the shape (`pattern`), the door opens physically. No bouncer.

#### The step-by-step

1. JavaScript often runs on every keystroke to check password rules.
2. UtahCSS uses HTML `pattern` so the **browser** checks the shape.
3. CSS says: if `:valid`, green border; if `:invalid` and the user has typed (`:not(:placeholder-shown)`), red border and show `.utah-error-text`.

#### The professional pivot

Client-side JS validation adds overhead and can be abused (e.g. ReDoS). UtahCSS uses the **HTML5 Constraint Validation API** plus `:valid` / `:invalid` / `:placeholder-shown`—validation runs in the browser’s engine, not your script bundle.

#### Implementation

See **[forms.html](./forms.html)** or copy:

```html
<form class="utah-form">
  <div class="utah-input-group">
    <input
      type="password"
      id="secure-pass"
      class="utah-input"
      placeholder=" "
      pattern="^(?=.*[A-Z])(?=.*\d).{8,}$"
      required
    >
    <label for="secure-pass" class="utah-label">Enter Passcode</label>
    <span class="utah-error-text">Must be 8+ chars with a number and uppercase letter.</span>
  </div>
  <button type="submit" class="utah-submit-btn">Initialize</button>
</form>
```

**Order matters:** `input` → `label` → `.utah-error-text` (siblings; CSS uses `~`).

**Why `placeholder=" "`:** Hides red state until the user starts typing.

#### Legacy underline fields

`.utah-form-group` still uses bottom-border-only styling for older snippets; `.utah-input-group` uses full border + error text.

---

### Tutorial J — Loading spinner

```html
<div class="utah-spinner" role="status" aria-label="Loading"></div>
```

Pure CSS rotation; add `aria` attributes for screen readers in real projects.

---

### Tutorial: Zero-Script Calculator & Cart {#tutorial-zero-script-calculator}

**Goal:** Sum line-item prices and show checkout only when items are selected—using CSS counters, not JavaScript.

Full walkthrough, monetization notes, and extension patterns: **[ADVANCED_CALCULATION.md](./ADVANCED_CALCULATION.md)**.

**Live demo:** [`cart.html`](./cart.html)

**Core idea:**

1. `counter-reset: cart-total 0` on `.utah-cart-system`
2. Each `.utah-item.tier-name:checked { counter-increment: cart-total <price>; }`
3. `.utah-total-display::after { content: "$" counter(cart-total); }`
4. `.utah-cart-system:has(.utah-item:checked) .utah-checkout-btn { display: block; }`

---

## Part 4 — Technical reference

### Design tokens (`:root` custom properties)

| Token | Role |
|--------|------|
| `--void-black` | Default page background (OKLCH dark). |
| `--void-deep` | Deep background (dropdown menus, etc.). |
| `--neon-salt` | Primary text color. |
| `--plasma-red` | Accent / brand / danger line. |
| `--cyber-blue` | Secondary accent (hover, floated label). |
| `--bio-green` | Success accent. |
| `--warn-gold` | Warning accent (defined; use in your extensions). |
| `--flux-pad` | Responsive section padding `clamp(1rem, 3vw, 2rem)`. |
| `--anim-snap` | Transition timing `0.3s cubic-bezier(0.2, 0.8, 0.2, 1)`. |
| `--glass` | Translucent border/fill `rgba(255,255,255,0.05)` (light mode overridden). |
| `--nav-height` | Fixed nav height (60px); also `body` top padding. |

**OKLCH:** Modern perceptual color space; very old browsers may ignore OKLCH declarations—provide fallbacks if you must support them.

### Global reset

```css
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
```

All components assume border-box sizing and zeroed default margins.

### Component class map

| Class | Purpose |
|--------|---------|
| `.utah-page` | Section hidden unless `:target` (hash routing). |
| `.utah-nav` | Fixed top navigation shell. |
| `.utah-brand` | Nav brand link styling. |
| `.utah-nav-links` | Flex row for links. |
| `.utah-nav-link` | Individual nav link. |
| `.utah-carousel` | Horizontal scroll snap track. |
| `.utah-slide` | Full-width snap slide. |
| `.utah-alert` | Alert container. |
| `.alert-danger` | Red left border. |
| `.alert-success` | Green left border. |
| `.utah-hidden-state` | Visually hidden checkbox/radio (state memory). |
| `.utah-close-trigger` | Same hiding technique; used inside alerts. |
| `.utah-modal` | Full-screen overlay (shown when toggle checked). |
| `.utah-modal-content` | Modal panel. |
| `.utah-modal-root` | Wrapper for scoped multi-modal pages. |
| `.utah-btn` | Primary button (alias of `.btn`). |
| `.utah-close-btn` | Label acting as close control. |
| `.utah-form` | Form container (max-width). |
| `.utah-form-group` | Floating-label group (underline style). |
| `.utah-input-group` | Full-border input + error text. |
| `.utah-input` | Textual input. |
| `.utah-error-text` | Shown when sibling input is invalid. |
| `.utah-submit-btn` | Primary submit control. |
| `.utah-router-view` | SPA page container. |
| `.utah-menu-root` / `.utah-menu` | Checkbox-toggled menu (migration pattern). |
| `.utah-tabs` | Radio-driven tab panels. |
| `.utah-cart-system` | Cart form with `counter-reset` total. |
| `.utah-product-row` | Single cart line (checkbox + label). |
| `.utah-item` | Cart checkbox; tier class sets increment. |
| `.utah-total-display` | Renders `$` + `counter(cart-total)` via `::after`. |
| `.utah-checkout-btn` | Shown when cart `:has(.utah-item:checked)`. |
| `.utah-summary-panel` | Total row layout. |
| `.utah-label` | Floating label. |
| `.utah-dropdown` | Dropdown positioning context. |
| `.utah-dropdown-menu` | Popover panel. |
| `.utah-menu-item` | Dropdown row link. |
| `.utah-spinner` | Loading animation. |
| `details.utah-accordion` | Accordion shell. |
| `summary.utah-summary` | Accordion header. |
| `.utah-accordion-content` | Accordion body. |

### Utilities

| Class | CSS |
|--------|-----|
| `.container` | `max-width: 1100px; margin: 0 auto;` |
| `.btn` | Primary button / link styled as button. |
| `.text-center` | `text-align: center` |
| `.grid-2` | Two-column grid, `gap: 2rem` |

### Keyframes

| Name | Used by |
|------|---------|
| `page-materialize` | `.utah-page` entrance |
| `spin` | `.utah-spinner` |

### Selectors that imply modern browsers

- `:has()` — theme toggle on `html`, dismissible alerts, home/hash fallback in the demo.
- `:focus-within` — dropdown visibility.
- `oklch()` — core palette in `:root`.

### Browser support

**Recommended baseline:** Evergreen desktop and mobile browsers from the last ~2 years.

**Risk areas:**

- **Firefox / Safari:** `:has()` is now widely available but was historically a gap—verify if you target older versions.
- **OKLCH:** Unsupported browsers may skip those declarations; add hex fallbacks if needed.

### Accessibility checklist (when shipping real products)

- Ensure **visible focus** on `a`, `button`, `input` matches your brand (Utah.css does not add a strong custom focus ring everywhere).
- For **dropdowns**, verify keyboard traversal and consider `aria-expanded` if you add JavaScript later.
- **Hash-only “pages”** are still one document: set `aria-live` or manage title updates if you need screen-reader announcements on route change (would typically require JS).
- Dismissible alerts: the demo hides content with `display: none`; pair with **server-side** or **client** logic if the same alert must not reappear.

### Extending the framework (recommended practice)

1. **Do not edit `utah.css` in vendor copies**—load a second stylesheet `utah-overrides.css` after it.
2. Override tokens, e.g.:

```css
:root {
  --plasma-red: oklch(60% 0.2 25);
}
```

3. Add new utilities as single-purpose classes to keep parity with existing style.

---

## Part 5 — Demo file walkthrough (`index.html`)

The bundled demo wires every major feature together:

1. **Theme toggle** — `#theme-toggle` with `.utah-hidden-state` + nav label.
2. **Modal** — `#modal-toggle` opens `.utah-modal` from home; close label inside panel.
3. **Nav** — `.utah-nav` links to `#home`, `#components`, `#forms`.
4. **Home** — default visible section with spinner, CTA, and vault trigger.
5. **Components** — dropdown + alert; accordions; carousel with three slides.
6. **Forms** — three floating-label fields and success alert note.

Use `index.html` as a **canonical example** when unsure of markup order (especially forms and alerts).

---

## Part 6 — Troubleshooting

| Symptom | Likely cause |
|---------|----------------|
| Section never appears | Missing `id` on section or `href` hash mismatch; missing `utah-page` class. |
| Home hidden on first load | Removed `style="display:block;"` or home fallback `:has()` CSS. |
| Floating label wrong | Label not a **sibling after** input; missing `placeholder=" "`. |
| Dropdown never opens | Typo in class names; menu not nested inside `.utah-dropdown`. |
| Alert [X] does nothing | `for` / `id` mismatch; missing `utah-close-trigger` on checkbox. |
| Theme toggle no effect | Checkbox `id` is not exactly `theme-toggle` (selector is fixed in `utah.css`). |
| Modal won’t open | Missing `id="modal-toggle"` on input, or typo in `for` on labels. |
| Wrong modal opens | Duplicate `modal-toggle` ids; use `.utah-modal-root` per modal. |
| Carousel doesn’t scroll to slide | Slide missing `id` matching `href`; parent not `.utah-carousel`. |

---

## Part 7 — File map

| File | Role |
|------|------|
| `utah.css` | Framework source. |
| `index.html` | Living examples / showroom. |
| `README.md` | Project overview and quick start. |
| `UTAH-CSS-GUIDE.md` | This document. |
| `LICENSE` | MIT license. |
| `SECURITY_MANIFEST.md` | Enterprise / government security case. |
| `MIGRATION_GUIDE.md` | React / Bootstrap migration. |
| `forms.html` | Pattern-matching form demo. |
| `cart.html` | CSS counter shopping cart demo. |
| `ADVANCED_CALCULATION.md` | Calculator/cart tutorial + roadmap. |

---

## Summary

- **Non-technical:** Link `utah.css`, copy the HTML patterns, keep class names identical.  
- **Technical:** Leverage CSS variables, `:target` sections, `:has()` for state, native form validation, and scroll-snap for carousels—**zero JS** for the patterns above, with clear modern-browser constraints.

For questions or contributions, treat `index.html` as the contract for supported markup.
