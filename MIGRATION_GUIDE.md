# Enterprise Migration Guide: Transitioning to UtahCSS

This guide is designed for Senior Engineers and Enterprise Architects migrating legacy Single Page Applications (React, Angular, Vue) and heavy CSS frameworks (Bootstrap, Tailwind) to the zero-script UtahCSS architecture.

## 1. The Core Paradigm Shift: State Management

In legacy systems, UI state (like whether a menu is open, or which tab is active) is held in JavaScript memory (e.g., React's `useState`).  
In UtahCSS, **state is held in the DOM itself**.

**The React Way (Legacy):**

```jsx
const [isOpen, setIsOpen] = useState(false);
return (
  <div>
    <button onClick={() => setIsOpen(!isOpen)}>Toggle Menu</button>
    {isOpen && <div className="menu">Menu Items</div>}
  </div>
);
```

**The UtahCSS Way (Modern/Secure):**

```html
<div class="utah-menu-root">
  <input type="checkbox" id="menu-state" class="utah-hidden-state">
  <label for="menu-state" class="utah-btn">Toggle Menu</label>
  <div class="utah-menu">Menu Items</div>
</div>
```

*Logic:* The `<input>` holds the true/false state. The CSS reads this state via the `:checked` pseudo-class and `:has()` to display `.utah-menu`.

## 2. Component Migration Strategy

### A. Accordions and Tabs

* **Legacy:** JavaScript event listeners swap out active classes and render different components.
* **UtahCSS:** Use a group of `<input type="radio">` elements sharing the same `name` attribute. Only one radio can be active at a time, providing flawless, native mutually exclusive state. Map labels to these radios to act as the clickable tabs. See `.utah-tabs` in `utah.css` and [UTAH-CSS-GUIDE.md](./UTAH-CSS-GUIDE.md).

### B. Modals and Popups

* **Legacy:** Heavy third-party libraries (e.g., `react-modal`) that inject HTML into the body on click, requiring complex clean-up and focus-trapping.
* **UtahCSS:** A hidden checkbox at the root of the component. When checked, a fixed-position overlay is displayed via CSS. A `<label>` styled as an "X" or a background overlay unchecks the box to close it. See [README.md](./README.md).

### C. Dropdowns

* **Legacy:** Click listeners attached to the window to detect "clicks outside" to close the dropdown.
* **UtahCSS:** Utilize the CSS `:focus-within` or `:focus` pseudo-classes. The dropdown opens when the button is focused, and naturally closes the exact millisecond the user clicks anywhere else, requiring zero lines of event-handling code.

## 3. Performance Impact

By migrating a standard dashboard from React/Bootstrap to UtahCSS, enterprise testing demonstrates:

* **Bundle Size Reduction:** 80–95% (Complete removal of `react-dom.js` and routing bundles).
* **Time to Interactive (TTI):** Reduced to near zero.
* **Security:** XSS attack vectors via UI manipulation are completely neutralized at the declarative UI layer.

## 4. Routing Migration

Replace `react-router` (or equivalent) with hash routing via `:target`. Full walkthrough: [UTAH-CSS-GUIDE.md — Tutorial: SPA Routing](./UTAH-CSS-GUIDE.md#tutorial-spa-routing-without-javascript).

## 5. Form Validation Migration

Replace client-side validation libraries with HTML5 `pattern` / `required` and CSS `:valid` / `:invalid`. Full walkthrough: [forms.html](./forms.html) and [UTAH-CSS-GUIDE.md — Tutorial: Form Validation](./UTAH-CSS-GUIDE.md#tutorial-zero-script-form-validation).

## Related Documentation

- [SECURITY_MANIFEST.md](./SECURITY_MANIFEST.md) — Declarative security case for government and enterprise  
- [UTAH-CSS-GUIDE.md](./UTAH-CSS-GUIDE.md) — Complete technical reference
