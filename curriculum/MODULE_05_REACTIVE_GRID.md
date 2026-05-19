# Module 5: The Reactive Grid (Spatial Awareness & Filtering)

Welcome to the final module. In the legacy web, filtering 100 gallery items means JavaScript loops an array, removes nodes, and redraws the grid. Today you will make the grid reorganize itself using **relationship rules**—no script.

**Hands-on example:** [examples/module-05/](./examples/module-05/)  
**UtahCSS:** `:has()` throughout `utah.css` (cart checkout, modals, theme, archive patterns).

---

## 1. The Analogy: The Self-Organizing Library

If you use **JavaScript**, a librarian runs around moving books when you ask for “Science” only.

If you use **our CSS method**, shelves sit on mechanical tracks. Place a brass weight on the “Science” pedestal—Fiction drops through trapdoors, shelves slide shut. One physical motion.

In web design, **`:has()`** is that track system: a parent can “look inside” itself and change layout.

---

## 2. The Step-by-Step Tutorial

We will build a **Data Archive** that filters Secure vs. Public files instantly.

### Step 1: Create the HTML structure (the books and the weights)

```html
<div class="utah-archive-system">

    <input type="radio" name="filter" id="show-all" class="hidden-state" checked>
    <input type="radio" name="filter" id="show-secure" class="hidden-state">
    <input type="radio" name="filter" id="show-public" class="hidden-state">

    <div class="filter-controls">
        <label for="show-all" class="filter-btn">All Data</label>
        <label for="show-secure" class="filter-btn">Secure Only</label>
        <label for="show-public" class="filter-btn">Public Only</label>
    </div>

    <div class="data-grid">
        <div class="data-card type-secure">Classified Map</div>
        <div class="data-card type-public">Press Release</div>
        <div class="data-card type-secure">Encryption Key</div>
        <div class="data-card type-public">Menu PDF</div>
    </div>

</div>
```

**Why are we doing this?**

- Radios share `name="filter"`—only one filter active at a time.
- `.utah-archive-system` wraps **both** controls and grid so `:has()` can see checked radios.

### Step 2: Create the CSS rules (the trapdoors)

```css
.data-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
    gap: 15px;
    margin-top: 20px;
}

.data-card {
    background: #333;
    color: #fff;
    padding: 20px;
    border-radius: 5px;
    transition: all 0.3s ease;
}

.utah-archive-system:has(#show-secure:checked) .type-public {
    display: none;
}

.utah-archive-system:has(#show-public:checked) .type-secure {
    display: none;
}
```

**Why are we doing this?**

- `.utah-archive-system:has(#show-secure:checked)` — “Does this wrapper contain a checked `#show-secure`?”
- If yes, hide `.type-public`.
- **Show All** needs no extra rule—default `display` stays visible.
- **CSS Grid** reflows remaining cards into the gaps automatically.

Hide radios visually:

```css
.hidden-state {
    position: absolute;
    width: 1px;
    height: 1px;
    overflow: hidden;
    clip: rect(0, 0, 0, 0);
}
```

---

## 3. The professional implementation

Filtering is often `Array.prototype.filter()` plus virtual DOM diffing—full dataset in JS memory, loop on every click.

This module is **declarative DOM reflow**: `:has()` gives the parent localized awareness; the CSSOM applies `display: none` from radio state. No JavaScript runtime; layout runs on the browser’s optimized engine—**O(1)** work for the author, instant UX for the user.

---

## Series complete — mastery log

You now have the five pillars of zero-script UI:

| # | Pillar | Mechanism |
|---|--------|-----------|
| 1 | State | Checkboxes + `:checked` |
| 2 | Routing | `:target` |
| 3 | Validation | `pattern` + `:valid` / `:invalid` |
| 4 | Computation | `counter-increment` |
| 5 | Logic / filtering | `:has()` |

- [Module 1](./MODULE_01_INVISIBLE_SWITCH.md) · [Module 2](./MODULE_02_DOORS_IN_THE_DARK.md) · [Module 3 & 4](./MODULE_03_AND_04.md)
- [Curriculum home](./README.md) · [Full framework](../README.md)
