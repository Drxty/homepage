# Complete Customization Guide for the Home Page

This guide is a step-by-step tutorial to help you understand and customize **index.html**, covering styles, colors, features, adding songs, and much more.

---

## 1. Essential Tool: Using "Find" (CTRL+F)

- **Open** `index.html` in a code editor (VS Code, Sublime Text, Notepad++).
- Press **CTRL + F** (Windows/Linux) or **CMD + F** (Mac).
- Type the term you want to locate (e.g., `.btn`, `--bg-color`, `id="search-form"`).
- Use the arrows to navigate between occurrences.

---

## 2. Widget Toggle Example (Pomodoro)

To show or hide the Pomodoro widget:

```html
<!-- BEFORE -->
<section id="pomodoro-section" class="widget-section">
  <!-- content... -->
</section>

<!-- AFTER -->
<section id="pomodoro-section" class="widget-section hidden">
  <!-- content... -->
</section>
```

Simply add or remove the `hidden` class on the `<section>`.

---

## 3. Custom Colors in Settings Panel

Locate in the settings panel (`#settings-panel`):

```html
<section id="custom-colors-section" class="settings-section border-t">
  <h3 data-lang-key="customColors">Custom Colors</h3>
  <div id="color-inputs-container"></div>
  <button id="reset-colors-btn" class="btn btn-secondary">Reset Colors</button>
</section>
```

Each `<input type="color" data-css-var="--variable-name">` updates a CSS custom property in `:root` or `.dark:root`.

**To add**: duplicate a control `<div>`, update `id`, `for`, and `data-css-var`, and ensure the CSS variable is declared at the top of your CSS.

---

## 4. Text Selection Background (::selection)

The text selection style is defined globally:

```css
::selection {
  background-color: var(--selection-bg-color);
  color: var(--bg-color);
}
```

- **Modify the background**: change `--selection-bg-color` in `:root` or `.dark:root`.
- **Modify the text color**: adjust the `color` property or use a different CSS variable.

Example:

```css
:root {
  --selection-bg-color: rgba(200, 200, 0, 0.5);
}
.dark:root {
  --selection-bg-color: rgba(100, 100, 100, 0.5);
}
::selection {
  background-color: var(--selection-bg-color);
  color: var(--text-color);
}
```

---

## 5. Internationalization (i18n)

Translations are stored in the `translations` object at the end of the `<script>`:

```js
const translations = {
  en: { /* English strings */ },
  pt: { /* Portuguese strings */ }
};
```

- **Add a new language**: add a key (e.g., `es`) with its translations.
- **Update the language selector**: add `<option value="en">🇺🇸 EN</option>` and `<option value="pt">🇧🇷 PT</option>` in `<select id="language-selector">`.

---

## 6. Widget Visibility Toggles

In the settings panel, toggle individual widgets:

```html
<div class="flex-between">
  <label for="toggle-music-player">Enable Music Player</label>
  <input id="toggle-music-player" type="checkbox" class="form-checkbox">
</div>
```

Supported IDs: `toggle-music-player`, `toggle-widgets`, `toggle-pomodoro`. Checking/unchecking adds or removes `hidden` on the corresponding widget.

---

## 7. Background Image

To set a custom background image:

```html
<input id="background-image-url" type="text" placeholder="Image URL">
<button id="clear-background-btn" class="btn btn-secondary">Clear Background</button>
```

- The URL is saved to `localStorage.backgroundImageUrl` and applied via CSS:
  ```css
  :root { --bg-image: url('...'); }
  ```
- Use `localStorage.clear()` in the console to reset all settings.

---

## 8. Adding/Removing Default Songs

Inside the `<script>`, update the `musicPlayer.defaultSongs` array:

```js
musicPlayer.defaultSongs = [
  { name: "Chrono Cross", url: "...mp3" },
  { name: "My Song", url: "https://example.com/song.mp3" }
];
```

- Ensure each URL supports CORS.
- To remove a song, delete its object from the array.

---

## 9. Buttons & Icons in Detail

### 9.1 Base Styles

All buttons share the `.btn` class in the `<style>` block:

```css
.btn {
  border: 2px solid var(--hr-color);
  background: transparent;
  color: inherit;
  border-radius: 9999px;
  padding: 10px 16px;
  margin: 2px;
  font: inherit;
  font-size: 1.15em;
  font-weight: 600;
  white-space: nowrap;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: .5rem;
  transition: background-color .2s ease, transform .2s ease, color .2s;
  cursor: pointer;
  text-decoration: none;
}
```

- **Border** and **color** from `--hr-color`.
- **Border-radius** makes pills.
- **Padding** and **margin** control spacing.
- **Font-size** and **font-weight** control text appearance.
- **Gap** defines space between icon and text.

### 9.2 Icon Buttons

```css
.btn-icon {
  padding: .75rem;
  font-size: 1em;
  border-radius: 50%;
}
.btn svg {
  width: 1.25em;
  height: 1.25em;
}
```

Use `<button class="btn btn-primary btn-icon">…</button>` for circular icon buttons.

### 9.3 Interactive States (Hover, Focus, Active)

```css
.btn:hover,
.btn:focus-visible {
  outline: none;
  background-color: var(--btn-secondary-hover-bg-color);
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}
.btn:active {
  transform: scale(.92);
}
```

- **Hover/Focus**: change background and add shadow.
- **Active**: shrink button (`scale(.92)`).

### 9.4 Variants (.btn-primary, .btn-secondary, .btn-danger)

- `.btn-primary`: uses `--primary-color` and `--btn-primary-text-color` on hover.
- `.btn-secondary`: uses secondary variables.
- `.btn-danger`: red/danger variables.

Each variant overrides only color-related properties.

### 9.5 Search Button Details

Within the search form (`#search-form`):

```html
<form id="search-form" class="flex items-center gap-2">
  <input id="search-input" type="text" placeholder="Search..." class="flex-grow form-input">
  <button id="search-button" type="submit" class="btn btn-primary btn-search">
    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 24 24">…</svg>
  </button>
</form>
```

- **Padding**: override by adding `.btn-search { padding: 8px 12px; }`.
- **SVG size**: adjust `h-5 w-5` classes or set inline `width`/`height`.
- **Gap**: change form container `gap-2` to `gap-1` or `gap-3`.
- **Hover color**:
  ```css
  #search-button:hover { background-color: var(--primary-color-light); }
  ```
- **Focus shadow**:
  ```css
  #search-button:focus-visible { box-shadow: 0 0 0 3px var(--primary-color-transparent); }
  ```
- **Transition & Active**:
  ```css
  #search-button { transition: background-color .15s ease-in, transform .1s ease; }
  #search-button:active { transform: scale(.95); }
  ```

---

## 10. Responsive Layout

Built-in breakpoints:

```css
@media (max-width: 640px) { /* mobile */ }
@media (min-width: 640px) { /* tablet */ }
@media (min-width: 1024px) { /* desktop */ }
```

Add or modify as needed.

---

## 11. Testing Your Changes

1. Open `index.html` in your browser.
2. Use DevTools (F12) to inspect and test live styles.
3. Check the console for JavaScript errors.
4. Run `localStorage.clear()` in the console to reset all settings.

---

## Publishing on GitHub Pages

Make English the default and host a Portuguese version in `/pt/`:

1. **Repository structure**:
   ```
   / (repo root)
   ├─ index.html        ← English (default)
   ├─ pt/
   │    └─ index.html  ← Portuguese
   └─ assets/
        └─ ...
   ```
2. **Language switch links** (in header of each page):
   ```html
   <a href="/">EN</a> | <a href="/pt/">PT</a>
   ```
3. **Enable GitHub Pages**: in repo settings, point to `main` branch, root.
4. **Optional redirect** by browser language:
   ```html
   <script>
     const lang = navigator.language.startsWith('pt') ? '/pt/' : '/';
     if (location.pathname === '/') location.replace(lang);
   </script>
   ```

---

# End of Guide

Save this file as `README.md` and keep it updated alongside your `index.html`.

