# UtahCSS: The Zero-Script Declarative Framework

UtahCSS is a high-performance, purely declarative CSS framework designed to eliminate the need for client-side JavaScript in modern web applications.

By utilizing advanced CSS selectors (`:has()`, `:checked`, `:target`, `~`, `+`) and native HTML state elements, UtahCSS allows developers to build complex, interactive interfaces—including routing, state management, and conditional rendering—without exposing users to the massive attack surface of executable client-side scripts.

## Core Philosophy

The modern web has become dangerously reliant on heavy JavaScript bundles for basic user interface logic. This reliance introduces severe security vulnerabilities (XSS, supply chain attacks), degrades battery life, and slows down content delivery.

UtahCSS returns the web to a secure, declarative paradigm. **If it doesn't need a runtime environment, it shouldn't have one.**

### Key Advantages

* **Zero Attack Surface:** Without JavaScript, Cross-Site Scripting (XSS) and client-side code injection vulnerabilities are mathematically impossible at the UI layer.
* **Instant Time-to-Interactive (TTI):** No scripts to download, parse, or execute. The application is interactive the exact millisecond the CSS is painted to the screen.
* **Native State Management:** Utilizes HTML forms (hidden checkboxes and radio buttons) as a highly stable, browser-native memory state.
* **Absolute Privacy:** No background tracking scripts, no telemetrics, no external dependencies.

## Installation

UtahCSS is designed for immediate integration. There are no package managers, build steps, or compilers required.

1. Download `utah.css` from this repository.
2. Link it in the `<head>` of your HTML document:

```html
<link rel="stylesheet" href="utah.css">
```

## Quick Start: The "No-JS" State Toggle

How to build a functional, interactive modal without a single line of JavaScript.

```html
<input type="checkbox" id="modal-toggle" class="utah-hidden-state">

<label for="modal-toggle" class="utah-btn">Open Secure Vault</label>

<div class="utah-modal">
  <div class="utah-modal-content">
    <h2>Secure Data</h2>
    <p>This modal is rendered entirely via CSS state changes.</p>
    <label for="modal-toggle" class="utah-close-btn">Close</label>
  </div>
</div>
```

*How it works:* The `<label>` triggers the hidden `<input>`. The `utah.css` file listens for the `:checked` pseudo-class and dynamically updates the `display` properties of the `.utah-modal`. Simple, secure, and unbreakable.

For **multiple modals** on one page, wrap each control in `.utah-modal-root` (see [UTAH-CSS-GUIDE.md](./UTAH-CSS-GUIDE.md)).

## Live Demo

**Online (GitHub Pages):** [https://utahisnotastate.github.io/utahcss/](https://utahisnotastate.github.io/utahcss/)  
— served from `main` after the Pages workflow runs (repo **Settings → Pages → Build: GitHub Actions**).

**Local:** open [`index.html`](./index.html), [`forms.html`](./forms.html), or [`cart.html`](./cart.html) in a browser.

**Repository:** [github.com/utahisnotastate/utahcss](https://github.com/utahisnotastate/utahcss)

## Documentation

| Document | Audience |
|----------|----------|
| **[UTAH-CSS-GUIDE.md](./UTAH-CSS-GUIDE.md)** | Developers — tutorials, components, reference |
| **[SECURITY_MANIFEST.md](./SECURITY_MANIFEST.md)** | Enterprise / government — declarative security case |
| **[MIGRATION_GUIDE.md](./MIGRATION_GUIDE.md)** | Architects — React, Bootstrap → UtahCSS |
| **[forms.html](./forms.html)** | Live pattern-matching form validation demo |
| **[cart.html](./cart.html)** | Live CSS-counter cart / calculator demo |
| **[ADVANCED_CALCULATION.md](./ADVANCED_CALCULATION.md)** | Tutorial 3 — counters, `:has()`, compliance roadmap |

For SPA routing, carousel logic, pattern-matching forms, and the Child Protocol security model, start with **UTAH-CSS-GUIDE.md**.

## License

MIT License. Free for enterprise, academic, and sovereign use. See [LICENSE](./LICENSE).
