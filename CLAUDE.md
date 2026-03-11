# CLAUDE.md — AI Assistant Guide for vip

## Project Overview

This is a **client-side web application for a Hebrew-language audio recording studio** ("יקיר כהן הפקות" — Yakir Cohen Productions). It is a marketing and operations platform built entirely with vanilla HTML, CSS, and JavaScript — no framework, no build step, no backend.

The site serves three user types:
- **Customers** browsing packages and booking sessions
- **Gift givers** creating and viewing gift vouchers
- **Admins** creating voucher cards via a dedicated tool

---

## Repository Structure

```
/
├── index.html       # Marketing landing page (path selector, carousel, pricing)
├── portal.html      # Customer booking portal (multi-step wizard)
├── view.html        # Gift voucher opening experience (animations, confetti)
├── creator.html     # Admin voucher creation tool (v3.0, live preview)
└── _headers         # Netlify HTTP response headers configuration
```

No `node_modules`, `package.json`, `requirements.txt`, `Dockerfile`, or build artifacts exist. The project deploys as-is.

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Markup | HTML5 (semantic elements) |
| Styling | CSS3 (custom properties, Grid, Flexbox) |
| Scripting | Vanilla JavaScript (no libraries) |
| Fonts | Google Fonts (Heebo, Frank Ruhl Libre, Rajdhani) |
| Hosting | Netlify (static) |
| Locale | Hebrew (RTL — `dir="rtl"`, `lang="he"`) |

External calls are limited to:
- Google Fonts CDN
- Google Drive image thumbnails (carousel)
- WhatsApp Business API links

---

## Pages and Responsibilities

### `index.html` — Landing Page
- Path selector: singers/musicians, bar mitzvah, podcast
- Image carousel (auto-play + manual navigation)
- Dynamic package pricing per selected path
- "How it works" section, stats, WhatsApp CTA
- Light cream theme with red (`#c0392b`) accents

### `portal.html` — Booking Portal
- 4-step wizard: rules → agreement → upsells → confirmation
- Fixed top bar with user context
- Add-on upsell cards
- Payment integration readiness
- Same cream/red theme as landing

### `view.html` — Voucher Viewer
- Animated envelope with wax seal (open animation)
- Twinkling star background (canvas)
- Confetti celebration effect (canvas)
- 5 voucher card templates with distinct visual styles
- Dark purple theme with gold (`#d4a017`) accents

### `creator.html` — Admin Voucher Creator (v3.0)
- Split layout: form panel (left) + live preview panel (right)
- Step-by-step creation flow with progress indicator
- 5 template options, package selection, price visibility toggle
- Real-time preview mirroring `view.html` card output
- Dark theme (matches viewer for WYSIWYG accuracy)

---

## Code Conventions

### HTML / Structure
- Semantic tags: `<header>`, `<main>`, `<section>`, `<footer>`
- IDs use kebab-case: `#path-singers`, `#car-track`, `#detail`
- Classes use kebab-case: `.pkg-grid`, `.hero-h`, `.car-wrap`
- All inline `onclick` handlers reference top-level JS functions

### CSS
- Custom properties declared on `:root` for theming (`--bg`, `--red`, `--gray`, etc.)
- `box-sizing: border-box` applied globally
- RTL layout via `direction: rtl` on `<body>` or `:root`
- Responsive via `@media` queries (mobile-first approach)
- Vendor prefixes used where needed (`-webkit-backdrop-filter`)
- All styles are **inline within `<style>` tags** — no external `.css` files

### JavaScript
- **All JS is inline** within `<script>` tags at the bottom of each HTML file
- Functions: camelCase (`selectPath()`, `buildCarousel()`, `updateCar()`)
- Constants: UPPER_CASE object literals (`IMGS`, `PATHS`, `PKGS`)
- DOM access: `document.getElementById()` and `querySelector()`
- Event binding: `onclick` attributes in HTML, not `addEventListener`
- Animation: `setInterval()` for carousel; CSS transitions for UI
- No `async/await` — synchronous flow throughout
- No modules, no imports — global scope only

### Language and Locale
- All user-facing text is in **Hebrew**
- UI direction is RTL — maintain this in any new elements
- Do not introduce LTR-only patterns (e.g., `float: left` for primary layout)

---

## Security Headers (`_headers`)

```
/*
  X-Content-Type-Options: nosniff
  X-XSS-Protection: 1; mode=block
  Referrer-Policy: strict-origin-when-cross-origin

/view.html
  X-Frame-Options: ALLOWALL          # Allows embedding (gift links)
  Content-Security-Policy: frame-ancestors *

/creator.html
  X-Frame-Options: DENY              # Prevents embedding (admin tool)
```

When adding new pages, consider whether they should be embeddable and add appropriate headers.

---

## Local Development

No build or install step is required.

**Option 1 — Open directly:**
```bash
open index.html
```

**Option 2 — Serve locally (recommended for testing navigation):**
```bash
python -m http.server 8080
# or
npx http-server . -p 8080
```

Then visit `http://localhost:8080`.

---

## Deployment

The repository is connected to **Netlify**. Pushes to the tracked branch trigger automatic deployment. No additional configuration is needed.

---

## Testing

There is no automated test suite. Manual browser testing is expected:

1. Open each HTML file in a browser
2. Test responsive behavior with DevTools device emulation
3. Verify RTL layout integrity
4. Check animations in `view.html` (envelope open, confetti)
5. Verify live preview in `creator.html` matches `view.html` output

---

## Key Patterns for AI Assistants

### Adding a new section to a page
1. Write HTML in the appropriate semantic block
2. Add scoped CSS in the same file's `<style>` tag
3. If interactive, add a function to the `<script>` block and bind via `onclick`
4. Keep Hebrew text, RTL layout, and the page's color theme

### Modifying the carousel (`index.html`)
- Image URLs live in the `IMGS` object constant
- `buildCarousel()` renders slides; `updateCar()` updates active state
- Auto-play is managed by `setInterval`; pause on user interaction is expected

### Modifying voucher templates (`view.html` / `creator.html`)
- There are 5 template variants — changes must be reflected in **both** files
- Template selection in `creator.html` updates the preview to mirror `view.html`
- Template styles are self-contained CSS classes (`.t1` through `.t5` or similar)

### Adding a page
1. Create `newpage.html` following the same inline CSS/JS pattern
2. Add headers for the new page in `_headers`
3. Use the same Google Fonts link tag at the top
4. Set `dir="rtl"` and `lang="he"` on `<html>`

### Do not
- Add npm packages, build tools, or frameworks
- Split CSS or JS into external files (keep inline for simplicity)
- Use `float` for layout (use Flexbox or Grid)
- Add backend code, serverless functions, or APIs
- Introduce English UI text without Hebrew equivalent

---

## Git Workflow

- Branch names follow the pattern `claude/<task-id>`
- Commit messages are imperative and descriptive (`Update portal.html`, `Implement price visibility toggle feature`)
- The remote is at `http://local_proxy@127.0.0.1:37211/git/callwebboss-bit/vip`
- Push with: `git push -u origin <branch-name>`
