# Module 3: The Bouncer at the Door (Form Validation)

Welcome to Module 3. In the archaic web, developers write scripts that fire every time a user presses a key on their keyboard to check if an email looks correct or if a password is long enough. Today, you will learn how to make the browser do this natively, purely through physical shape-matching.

**Hands-on example:** [examples/module-03/](./examples/module-03/)  
**UtahCSS:** `.utah-input`, `.utah-input-group`, `.utah-error-text` — see [forms.html](../forms.html).

---

## 1. The Analogy: The Lock and the Key

Imagine a bouncer at a highly secure vault.

If you use **JavaScript**, the bouncer takes your ID, calls central office on a radio, waits for an answer, then opens the door. A hacker could intercept that radio call.

If you use **our CSS method**, there is no bouncer. The lock has a specific shape. Your password is the key. If it matches, the pins align and the door clicks open. Pure physics.

---

## 2. The Step-by-Step Tutorial

We will build a password field that turns green when strong and red when weak—no script.

### Step 1: Create the HTML structure (the lock)

```html
<form class="secure-form">
    <h3>Enter Secure Code</h3>

    <div class="input-container">
        <input
            type="password"
            id="vault-pass"
            class="smart-input"
            placeholder=" "
            pattern="^(?=.*[A-Z])(?=.*\d).{8,}$"
            required
        >
        <label for="vault-pass">Password</label>

        <div class="error-alert">Must be 8+ chars, 1 uppercase, 1 number.</div>
    </div>

    <button type="submit" class="submit-btn">Unlock</button>
</form>
```

**Why are we doing this?**

- `pattern` is a regular expression—the geometric shape the text must match.
- `placeholder=" "` (one space) is a trick for CSS (see Step 2).
- **Sibling order:** `input` → `label` → `.error-alert` so `~` selectors work.

### Step 2: Create the CSS rules (visual feedback)

```css
.error-alert {
    display: none;
    color: red;
    font-size: 12px;
}

.smart-input:not(:placeholder-shown):invalid {
    border: 2px solid red;
    background-color: #ffe6e6;
}

.smart-input:not(:placeholder-shown):invalid ~ .error-alert {
    display: block;
}

.smart-input:not(:placeholder-shown):valid {
    border: 2px solid #00ff66;
    background-color: #e6ffe6;
}
```

**Why are we doing this?**

- `:valid` / `:invalid` are native browser states against `pattern`.
- `:not(:placeholder-shown)` avoids red styling before the user types.

---

## 3. The professional implementation (Module 3)

Client-side validation via JavaScript adds overhead and ReDoS risk. This module is a **constraint validation matrix**: regex in HTML5 + `:valid` / `:invalid` in CSS, with string checks in the browser’s engine—not your script bundle.

---

# Module 4: The Mechanical Abacus (Math and Counting)

Welcome to Module 4. Shopping carts usually use JavaScript to sum prices. You will use a **CSS counter**—a mechanical dial wired to checkboxes.

**Hands-on example:** [examples/module-04/](./examples/module-04/)  
**UtahCSS:** `.utah-cart-system`, `.utah-total-display` — see [cart.html](../cart.html) and [ADVANCED_CALCULATION.md](../ADVANCED_CALCULATION.md).

---

## 1. The Analogy: The Grocery Scale

If you use **JavaScript**, a worker writes each weight, calculates, and updates a whiteboard.

If you use **our CSS method**, the scale is wired to a dial. Drop an apple; the dial moves by that weight. No worker.

---

## 2. The Step-by-Step Tutorial

### Step 1: The HTML checkboxes

```html
<div class="cart-system">
    <input type="checkbox" id="item1" class="cart-item price-50">
    <label for="item1">Quantum Processor ($50)</label><br>

    <input type="checkbox" id="item2" class="cart-item price-100">
    <label for="item2">Neural Link ($100)</label><br>

    <div class="total-display">Total: $</div>
</div>
```

### Step 2: The CSS abacus

```css
.cart-system {
    counter-reset: cart-total 0;
}

.cart-item.price-50:checked {
    counter-increment: cart-total 50;
}

.cart-item.price-100:checked {
    counter-increment: cart-total 100;
}

.total-display::after {
    content: counter(cart-total);
    font-weight: bold;
    color: #00ff66;
}
```

The number appears in `::after` right after the text `Total: $`.

---

## 3. The professional implementation (Module 4)

Dynamic totals usually need JS state. With `counter-reset` / `counter-increment` on `:checked` inputs, the DOM becomes a **physical state machine**; the layout engine sums in real time with zero script.

**Limits:** integers only in basic counters; real checkout still needs server-side pricing.

---

## Next steps

- Previous: [Module 2 — Doors in the Dark](./MODULE_02_DOORS_IN_THE_DARK.md)
- Next: [Module 5 — The Reactive Grid](./MODULE_05_REACTIVE_GRID.md)
- Full framework: [curriculum/README.md](./README.md)
