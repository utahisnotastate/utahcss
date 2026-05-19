# Module 2: Doors in the Dark (Single Page Routing)

Welcome to Module 2. In modern web development, **Single Page Applications** (SPAs) feel fast because they do not reload the entire browser window when you click a link. Traditionally, developers write thousands of lines of JavaScript to fake this behavior. Today, you will learn how to achieve it using pure geometry.

**Hands-on example:** [examples/module-02/](./examples/module-02/)  
**UtahCSS production classes:** `.utah-page`, `.utah-router-view`, `.utah-nav` — see [UTAH-CSS-GUIDE](../UTAH-CSS-GUIDE.md#tutorial-spa-routing-without-javascript).

---

## 1. The Analogy: The Theater Stage

Imagine a grand theater stage.

If you use **JavaScript**, a crew smashes the old set, reads a blueprint, and hammers a new set together before the curtain opens. It takes energy, and sometimes the set collapses (a crash).

If you use **our CSS method**, every backdrop is already hanging in the dark. You hold a **spotlight**. When the script says “Scene 2,” you point the spotlight at Backdrop 2. Scene 1 stays in shadow. No building, no breaking—just light.

In web design, the **URL bar** is the spotlight.

---

## 2. The Step-by-Step Tutorial

We will build a site with three “pages” that swap instantly without reloading.

### Step 1: Create the navigation (the spotlights)

Open `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <nav class="main-nav">
        <a href="#home" class="nav-button">Home</a>
        <a href="#dashboard" class="nav-button">Dashboard</a>
        <a href="#settings" class="nav-button">Settings</a>
    </nav>

    <main class="content-stage">

        <section id="home" class="page-set">
            <h2>Welcome Home</h2>
            <p>This is the main landing area.</p>
        </section>

        <section id="dashboard" class="page-set">
            <h2>Secure Dashboard</h2>
            <p>Your data is displayed here.</p>
        </section>

        <section id="settings" class="page-set">
            <h2>User Settings</h2>
            <p>Adjust your preferences securely.</p>
        </section>

    </main>

</body>
</html>
```

**Why are we doing this?**

- `<a href="#dashboard">` does not load a new file—it adds `#dashboard` to the URL.
- Each `<section>` `id` must **exactly match** the `href` hash.

### Step 2: Create the CSS rules (darkness and light)

Open `style.css`:

```css
/* 1. Basic styling */
body {
    font-family: sans-serif;
    background-color: #1a1a1a;
    color: #ffffff;
    padding: 20px;
}

.nav-button {
    display: inline-block;
    padding: 10px 15px;
    background-color: #333;
    color: #00ff66;
    text-decoration: none;
    margin-right: 10px;
    border-radius: 4px;
}

/* 2. THE DARKNESS: hide all pages by default */
.page-set {
    display: none;
    margin-top: 30px;
    padding: 20px;
    border: 1px solid #444;
    background-color: #222;
}

/* 3. THE SPOTLIGHT: show the page whose id is in the URL */
.page-set:target {
    display: block;
}
```

**Why are we doing this?**

The `:target` pseudo-class means: *if this element’s `id` matches the hashtag in the URL, apply these styles.* The browser handles the URL and **Back/Forward** for free—no script.

### Step 3: Default “home” when there is no hash

With only the rules above, **no section shows** until someone clicks a link. For a landing page, add to the home section:

```html
<section id="home" class="page-set" style="display:block;">
  <style>
    body:has(.page-set:target) #home { display: none; }
    #home:target { display: block !important; }
  </style>
  ...
</section>
```

This pattern is built into the full [index.html](../index.html) demo.

---

## 3. The professional implementation

SPA routing is often implemented with JavaScript libraries that intercept the History API and run expensive virtual DOM reconciliation.

This module introduces **declarative DOM routing**: anchor tags map to section IDs; CSS `:target` delegates routing to the native engine. Page transitions are O(1) from the UI’s perspective, mount/unmount leak risks drop, and the routing bundle size is **zero bytes** of executable code—with native history support.

---

## 4. Graduate to UtahCSS

| Tutorial concept | UtahCSS class |
|------------------|---------------|
| `.page-set` | `.utah-page` |
| `.content-stage` | `.utah-router-view` |
| `.nav-button` | `.utah-nav-link` or `.utah-btn` |
| `:target` animation | `utahFadeIn` in `utah.css` |

Previous: **[Module 1 — The Invisible Switch](./MODULE_01_INVISIBLE_SWITCH.md)**  
Next (Module 3): [forms.html](../forms.html) and [Form Validation tutorial](../UTAH-CSS-GUIDE.md#tutorial-zero-script-form-validation)
