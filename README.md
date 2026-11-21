# Enhancing Web Page Performance and Accessibility (Week 4 Practical)

A small, responsive, accessible website built with plain HTML5, CSS3 and JavaScript. It demonstrates faster loading, semantic structure, and WCAG-aware design.

## Folder structure

```
week4-performance-accessibility/
├── index.html        Page structure (semantic HTML, ARIA, skip link)
├── style.css         Readable CSS (edit this one)
├── style.min.css     Minified CSS used by index.html (production)
├── script.js         Readable JavaScript (edit this one)
├── script.min.js     Minified JavaScript used by index.html (production)
├── README.md         This file
├── report.md         Project report with blank test templates
└── assets/images/    Compressed WebP + JPG images (480w and 1000w)
```

## How to run in VS Code

1. Install [VS Code](https://code.visualstudio.com/) and open the folder: **File > Open Folder > week4-performance-accessibility**.
2. Install the **Live Server** extension (Ritwick Dey) from the Extensions panel.
3. Right-click `index.html` and choose **Open with Live Server**. The page opens in your browser and reloads when you save.
4. No server? Double-click `index.html`. It also works from the file system.

### Editing workflow
`index.html` loads `style.min.css` and `script.min.js`. After you change `style.css` or `script.js`, either:
- temporarily point `index.html` at `style.css` / `script.js`, **or**
- regenerate the min files with Node.js: `npx clean-css-cli -o style.min.css style.css` and `npx terser script.js -c -m -o script.min.js`.

## How to test performance and accessibility

Always test on a **served page** (Live Server or your deployed site). Use an incognito window so extensions do not change results. Record the real numbers you get. Do not guess them.

1. **Lighthouse**: open Chrome, press F12, choose the **Lighthouse** tab, tick Performance and Accessibility, select Mobile, click **Analyze page load**.
2. **PageSpeed Insights**: needs a public URL (for example from GitHub Pages or Netlify). Paste it at pagespeed.web.dev.
3. **WAVE**: install the WAVE browser extension, open the page, click the WAVE icon, and review errors, contrast errors and alerts.
4. **axe DevTools**: install the extension, open DevTools, go to the **axe DevTools** tab, click **Scan all of my page**.
5. **Keyboard test** (no mouse): press Tab through the whole page. Check the skip link appears first, focus is always visible, the menu button works with Enter/Space, Esc closes the menu, and the form can be completed and submitted.
6. **Screen reader** (optional): NVDA (Windows, free), VoiceOver (macOS/iOS), TalkBack (Android).
7. **Responsive test**: DevTools device toolbar (Ctrl+Shift+M) at about 375, 768 and 1280 px.

Automated tools find only part of the accessibility problems, so manual testing is required too.

## Performance techniques used

| Technique | Where |
|---|---|
| WebP with JPG fallback via `<picture>` | `index.html`, `assets/images` |
| Responsive `srcset` / `sizes` | all three images |
| Lazy loading below the fold | `loading="lazy"` on images 2 and 3 |
| Hero not lazy, preloaded, `fetchpriority="high"` | hero image and `<head>` |
| `width` and `height` on images (stops layout shift) | all images |
| Minified CSS and JS, one file each | `style.min.css`, `script.min.js` |
| `defer` script (does not block rendering) | `<head>` |
| System font stack (no font downloads) | `--font` in CSS |
| Inline SVG favicon (no extra request) | `<head>` |
| Simple class selectors, CSS variables, shared `.btn` and `.card` rules | `style.css` |

### Optimized font loading
This project uses system fonts, which is the fastest option. If you add a custom font, self-host a `.woff2` file, preload it, and use `font-display: swap`:

```html
<link rel="preload" href="assets/fonts/myfont.woff2" as="font" type="font/woff2" crossorigin>
```
```css
@font-face { font-family: "MyFont"; src: url("assets/fonts/myfont.woff2") format("woff2"); font-display: swap; }
```

## Browser caching guidance

Caching is set on the **server**, so it cannot be seen when opening files locally.

- **Apache (`.htaccess`)**
  ```apache
  <IfModule mod_expires.c>
    ExpiresActive On
    ExpiresByType image/webp "access plus 1 year"
    ExpiresByType image/jpeg "access plus 1 year"
    ExpiresByType text/css   "access plus 1 month"
    ExpiresByType application/javascript "access plus 1 month"
  </IfModule>
  ```
- **Nginx**: `location ~* \.(webp|jpg|css|js)$ { expires 30d; add_header Cache-Control "public"; }`
- **Static hosts** (Netlify, GitHub Pages, Vercel) already send sensible cache headers. Check the `Cache-Control` header in DevTools > Network.
- When you update a cached file, rename it or add a version (`style.min.css?v=2`) so visitors get the new copy.
- Also enable **gzip or Brotli** compression on the server. Most hosts do this automatically.

## Accessibility features

Skip link, landmarks (`header`, `nav`, `main`, `section`, `article`, `footer`), one `h1` and a logical h2/h3 order, descriptive `alt` text, labelled form fields with `aria-describedby`, `role="alert"` error messages, `aria-expanded` on the menu button, visible focus outlines, touch targets of at least 44 px, `prefers-reduced-motion` support, and `lang="en"`.

## Requirements checklist

Tick each item after you have verified it yourself.

**Design and content**
- [ ] Home section with modern navigation bar
- [ ] About, Features, Performance, Accessibility, Contact sections present
- [ ] Footer with navigation links and copyright
- [ ] Works on mobile, tablet and desktop widths

**HTML**
- [ ] `header`, `nav`, `main`, `section`, `article`, `footer` used
- [ ] One `h1`, headings in order
- [ ] Every image has descriptive `alt` text
- [ ] ARIA used where needed (menu button, form errors, status)
- [ ] Form inputs all have `<label>`
- [ ] Skip-to-content link works with Tab then Enter

**CSS**
- [ ] CSS variables for colors
- [ ] Flexbox and Grid layouts
- [ ] No duplicated rules (shared `.btn`, `.card`)
- [ ] Contrast checked (WAVE or axe reports no contrast errors)
- [ ] Reduced-motion media query present

**JavaScript**
- [ ] Mobile menu opens/closes by click, Enter, Space and Esc
- [ ] Form validation shows messages and moves focus to first error
- [ ] Smooth scrolling to sections
- [ ] No libraries or unnecessary code

**Performance**
- [ ] WebP + JPG fallback
- [ ] Lazy loading on below-the-fold images
- [ ] Minified CSS and JS in use
- [ ] Image width/height set
- [ ] Caching guidance read and understood
- [ ] Font loading approach explained

**Testing and documentation**
- [ ] Lighthouse run and real scores recorded in `report.md`
- [ ] PageSpeed Insights run (public URL)
- [ ] WAVE and axe DevTools run and issues recorded
- [ ] Keyboard-only test completed
- [ ] `report.md` completed with your own results
