# Utah.css — User Guide & Technical Reference

**Version:** Utah-Omega-23 Protocol v4.0 (as declared in `utah.css`)  
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

### Tutorial B — Hash “routing” (show one section at a time)

**Goal:** Clicking `href="#components"` shows the section whose `id` is `components`, and hides other `.utah-page` blocks.

**Rules from the framework:**

- Each “page” is a `<section class="utah-page" id="unique-id">`.
- By default, `.utah-page { display: none }`.
- When the URL hash matches, `.utah-page:target { display: block }`.

**Problem:** With only that, the **home** section has no hash, so it would stay hidden unless you add a default.

**Pattern from `index.html` (home fallback):**

1. Give the home section `id="home"` and **inline** `style="display:block;"` so it shows when there is no hash.
2. Add a small `<style>` block (can live inside the home section or in the head) that:
   - Hides `#home` when **any** `.utah-page` is targeted.
   - Forces `#home` visible when `#home` is explicitly targeted.

Example (same idea as the demo):

```html
<section id="home" class="utah-page" style="display:block;">
  <style>
    body:has(.utah-page:target) #home { display: none; }
    #home:target { display: block !important; }
  </style>
  <!-- home content -->
</section>

<section id="components" class="utah-page">
  <!-- shown when URL ends with #components -->
</section>
```

**Technical note:** `:has()` is powerful but affects which browsers qualify; see [Browser support](#browser-support).

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

### Tutorial I — Floating-label inputs with validation styling

**Required structure:** `input` **first**, then `label` **after** (sibling), because CSS uses `~` combinator.

```html
<div class="utah-form-group">
  <input
    class="utah-input"
    id="codename"
    placeholder=" "
    required
    pattern="[A-Za-z0-9]{3,}"
  >
  <label for="codename" class="utah-label">CODENAME (3+ chars)</label>
</div>
```

**Why `placeholder=" "`:** The floating label uses `:not(:placeholder-shown)` so the label stays floated when there is content. A single space counts as a non-empty placeholder for that selector while still looking empty.

**Validation colors:**

- `:valid` → green bottom border (`--bio-green`).
- `:invalid:not(:placeholder-shown)` → red bottom border (`--plasma-red`) once the user has typed something.

Combine with standard HTML5 attributes: `required`, `type="email"`, `pattern`, `minlength`, etc.

---

### Tutorial J — Loading spinner

```html
<div class="utah-spinner" role="status" aria-label="Loading"></div>
```

Pure CSS rotation; add `aria` attributes for screen readers in real projects.

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
| `.utah-form-group` | Wrapper for floating label layout. |
| `.utah-input` | Textual input underline style. |
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

---

## Summary

- **Non-technical:** Link `utah.css`, copy the HTML patterns, keep class names identical.  
- **Technical:** Leverage CSS variables, `:target` sections, `:has()` for state, native form validation, and scroll-snap for carousels—**zero JS** for the patterns above, with clear modern-browser constraints.

For questions or contributions, treat `index.html` as the contract for supported markup.
