# Advanced Calculation: The Zero-Script Calculator & Cart

**Utah-Omega-23 Protocol v4.1**  
Dynamic totals without JavaScript — using CSS counters and `:has()`.

---

## Tutorial 3 — The Zero-Script Calculator

### The analogy (for everyone)

Imagine an old-fashioned scale at a grocery store.

- **JavaScript:** A worker stands beside the scale. Every time you add an apple, they write down the weight, pull out a calculator, and update a whiteboard. The worker can make mistakes—or be bribed to write the wrong total.
- **UtahCSS:** No worker. The scale is wired to a mechanical dial. Drop the apple; the dial moves by exactly that weight. Instant cause and effect.

### The step-by-step

1. Normally, a shopping cart script watches clicks, updates a total in memory, and repaints the screen.
2. UtahCSS uses the browser’s built-in **CSS counter** (“abacus”).
3. Set the counter to `0` at the top of the cart (`counter-reset`).
4. Each checked item runs `counter-increment` by that item’s price.
5. The total display uses `content: counter(cart-total)` to show the current abacus value.
6. `:has(.utah-item:checked)` reveals the checkout button only when at least one item is selected.

### The professional pivot

Dynamic aggregation (cart totals) usually needs JavaScript state-binding. UtahCSS uses **`counter-reset`** and **`counter-increment`** on the DOM tree, chained to checkbox `:checked` state. The rendering engine computes the sum at paint time. Combined with **`:has()`**, checkout UI appears only when positive selection exists—zero-latency, no client-side memory for totals, and no script surface for tampering with display logic.

**Limits (honest engineering):** CSS counters handle integers only (no decimals/currency formatting without tricks). Server-side pricing and authorization remain mandatory for real commerce. This pattern suits **estimates, requisition UIs, and compliance demos**—not a replacement for audited payment systems.

---

## Implementation: The Air-Gapped Shopping Cart

### HTML (`cart.html`)

```html
<form class="utah-cart-system">
  <h2>Secure Hardware Requisition</h2>

  <div class="utah-product-row">
    <input type="checkbox" id="item-server" class="utah-item server-tier">
    <label for="item-server" class="utah-label">Quantum Server Rack ($50,000)</label>
  </div>

  <div class="utah-product-row">
    <input type="checkbox" id="item-firewall" class="utah-item firewall-tier">
    <label for="item-firewall" class="utah-label">Air-Gap Firewall ($10,000)</label>
  </div>

  <div class="utah-summary-panel">
    <span>Total Authorized: </span>
    <span class="utah-total-display"></span>
  </div>

  <button type="submit" class="utah-checkout-btn">Authorize Requisition</button>
</form>
```

**Live demo:** Open [`cart.html`](./cart.html) in a browser.

### CSS (in `utah.css`)

```css
/* 1. Initialize internal counter state */
.utah-cart-system {
  counter-reset: cart-total 0;
}

/* 2. Bind increments to checked items (tier classes = price in dollars) */
.utah-item.server-tier:checked {
  counter-increment: cart-total 50000;
}
.utah-item.firewall-tier:checked {
  counter-increment: cart-total 10000;
}

/* 3. Render counter to screen */
.utah-total-display::after {
  content: "$" counter(cart-total);
  font-weight: bold;
  color: var(--bio-green);
}

/* 4. Show checkout only when something is selected */
.utah-checkout-btn {
  display: none;
}
.utah-cart-system:has(.utah-item:checked) .utah-checkout-btn {
  display: block;
}
```

### Adding a new line item

1. Add a row with a unique `id` on the checkbox.
2. Add a **tier class** (e.g. `widget-tier`) on the input.
3. In your CSS (or `utah-overrides.css`):

```css
.utah-item.widget-tier:checked {
  counter-increment: cart-total 1500;
}
```

Keep increments in **whole dollars** (or whole cents if you treat 1 unit = 1 cent).

---

## Strategic Monetization Roadmap (Utah-Omega-23)

**Niche:** Medical (HIPAA) and defense (SOC 2) data intake.

**Market gap:** Hospitals and contractors face liability when third-party pixels (Meta, Google Analytics) scrape forms. Buyers want UIs that never execute script in the browser.

**Product execution:**

1. Package routing, validation, and cart patterns as a premium single-file build (e.g. **AegisCSS** / **Utah.Secure**).
2. Position as **“Air-gapped, zero-script UI for high-compliance environments.”**
3. Landing page for compliance officers: *“Eliminate client-side script injection and third-party pixel leaks. Build state machines with zero JavaScript.”*
4. Enterprise license (templates for intake, checkout, internal dashboards): **$499–$1,500** per seat (illustrative; adjust to market).

UtahCSS open source remains the reference implementation; commercial packaging is optional.

---

## Related documentation

| Document | Topic |
|----------|--------|
| [UTAH-CSS-GUIDE.md](./UTAH-CSS-GUIDE.md) | All tutorials and API |
| [SECURITY_MANIFEST.md](./SECURITY_MANIFEST.md) | Declarative security case |
| [MIGRATION_GUIDE.md](./MIGRATION_GUIDE.md) | Legacy stack migration |
| [forms.html](./forms.html) | Pattern validation demo |
| [cart.html](./cart.html) | Counter cart demo |

---

**Mastery log:** The UtahCSS repository now covers routing, validation, modals, and declarative calculation—geometry over execution.
