<!-- From: /Users/albinsalihu/Downloads/Montagebau Fabian/mfwebsite/AGENTS.md -->
# AGENTS.md – Montagebau Fabian Website

> AI coding agent reference for the `mfwebsite` project.
> This is a static, German-language business website for **Montagebau Fabian**, a construction and installation company based in Schwerte, Germany.

---

## Project Overview

- **Name**: `mfwebsite`
- **Type**: Static marketing / brochure website (no dynamic backend, no CMS)
- **Language**: German (all content, UI text, and SEO metadata are in German)
- **Domain**: [https://montagebau-fabian.de](https://montagebau-fabian.de)
- **Owner**: Manuela Fabian — Montagebau Fabian (founded 2008)
- **Purpose**: Present services (doors, gates, windows, terrace roofing, fences), company info, contact details, and a contact form. Emphasizes emergency repair service.

---

## Technology Stack

| Layer | Technology | Version / Notes |
|-------|-----------|-----------------|
| Framework | [Astro](https://astro.build) | `^6.1.9` — static site generator, zero client-side JS by default |
| Styling | [Tailwind CSS](https://tailwindcss.com) | `^4.2.4` via `@tailwindcss/vite` plugin |
| Build Tool | Vite (bundled with Astro) | Configured in `astro.config.mjs` |
| Language | TypeScript | Strict config extending `astro/tsconfigs/strict` |
| Runtime | Node.js | `>=22.12.0` (specified in `package.json` engines) |
| Forms | Web3Forms | Contact form POSTs to `https://api.web3forms.com/submit` |
| Hosting | Netlify | Deployed from `dist/` directory |

**Notable absences**:
- No React, Vue, Svelte, or other UI frameworks
- No CSS preprocessor (Sass/Less) — plain CSS in `global.css` with Tailwind directives
- No testing framework (no Jest, Vitest, Playwright, etc.)
- No linting tools configured (no ESLint, Prettier, or Biome)
- No external analytics or tracking scripts

---

## Project Structure

```
├── public/                  # Static assets copied as-is to dist/
│   ├── fonts/               # (currently empty — fonts are loaded via Google Fonts or system fonts)
│   ├── *.jpeg / *.jpg       # Service and hero images
│   ├── favicon.ico / .svg   # Favicon variants
│   └── logo.jpg             # Company logo
├── src/
│   ├── components/          # Reusable Astro components
│   │   ├── CookieBanner.astro
│   │   ├── Footer.astro
│   │   ├── Header.astro
│   │   └── SEOHead.astro
│   ├── layouts/
│   │   └── Layout.astro     # Root HTML wrapper used by every page
│   ├── pages/               # File-based routing (Astro convention)
│   │   ├── index.astro      # Startseite (Home)
│   │   ├── leistungen.astro # Services overview
│   │   ├── ueber-uns.astro  # About us
│   │   ├── kontakt.astro    # Contact page + form
│   │   ├── impressum.astro  # Legal imprint (noindex)
│   │   ├── datenschutz.astro# Privacy policy (noindex)
│   │   └── 404.astro        # Custom 404 page
│   └── styles/
│       └── global.css       # Tailwind entry point + CSS custom properties
├── astro.config.mjs
├── tailwind.config.mjs      # Tailwind theme extension (colors mapped to CSS vars)
├── tsconfig.json
├── netlify.toml             # Build command, publish dir, security & cache headers
└── .env                     # Environment variables (see below)
```

---

## Build and Development Commands

All commands run from the project root.

| Command | Action |
|---------|--------|
| `npm install` | Install dependencies |
| `npm run dev` | Start local dev server at `localhost:4321` |
| `npm run build` | Production static build → `./dist/` |
| `npm run preview` | Preview the production build locally |
| `npm run astro -- --help` | Astro CLI help |

**Build output**: The `dist/` folder is the deployable artifact. It contains fully static HTML, CSS, JS, and images.

---

## Environment Variables

Defined in `.env` (not committed — listed in `.gitignore`):

| Variable | Prefix | Purpose |
|----------|--------|---------|
| `PUBLIC_WEB3FORMS_KEY` | `PUBLIC_` | Access key for Web3Forms contact form submissions |
| `PUBLIC_SITE_URL` | `PUBLIC_` | Canonical site URL (used in SEO metadata) |
| `PUBLIC_SITE_NAME` | `PUBLIC_` | Site name (used in SEO metadata) |

> Astro exposes only `PUBLIC_*` env vars to the client. There are no server-side secrets in this project.

---

## Code Style and Conventions

### File Naming
- Components, layouts, and pages use **PascalCase** filenames (e.g., `CookieBanner.astro`, `SEOHead.astro`).
- Config files use lowercase with dots (e.g., `astro.config.mjs`, `tailwind.config.mjs`).

### Component Structure (Astro)
- Frontmatter (`---`) at the top for TypeScript logic, props interfaces, and imports.
- Template below the frontmatter — plain HTML with Astro expressions `{ }`.
- `<script>` tags at the bottom of components for isolated, vanilla JavaScript (no framework).

### Styling Conventions
- **Tailwind CSS utility classes** are used for 99% of styling.
- Brand colors are defined as CSS custom properties in `src/styles/global.css` and mapped in `tailwind.config.mjs`:
  - `primary`: `#25383c` (dark teal)
  - `accent`: `#8a6a42` (gold/bronze)
  - `bg`: `#ffffff`
  - `text`: `#1a1a1a`
  - `text-light`: `#4a4a4a`
  - `text-muted`: `#737373`
  - `surface`: `#f8f9fa`
  - `surface-alt`: `#eef0f1`
  - `border`: `#e2e4e7`
- Font family: `Inter` (system fallback `ui-sans-serif`).
- Tailwind v4 uses `@import "tailwindcss"` and `@theme` blocks in `global.css`.

### Accessibility Patterns
- All pages use `lang="de"` and `dir="ltr"`.
- Skip link (`Zum Hauptinhalt springen`) is present in `Layout.astro`.
- Semantic HTML: `<header>`, `<nav>`, `<main>`, `<footer>`, `<section>`, `<article>`.
- `aria-label`, `aria-labelledby`, `aria-expanded`, `aria-current="page"` used consistently.
- Focus-visible styles: `outline: 2px solid var(--color-accent)`.
- Images use `loading="eager"` for above-the-fold content and `loading="lazy"` for below-the-fold.
- `prefers-reduced-motion` media query respects user motion preferences.

### Language
- **All user-facing text is German.** When adding new UI text, labels, or aria attributes, use German.
- TODO comments in the codebase are also written in German (e.g., `<!-- TODO: DMW Schwarze Logo einfügen -->`).

---

## Page Inventory

| Route | File | Purpose | SEO Index |
|-------|------|---------|-----------|
| `/` | `index.astro` | Home — hero, services teaser, about teaser, emergency CTA | ✅ index |
| `/leistungen` | `leistungen.astro` | Detailed services: doors, gates, windows, terrace roofing, fences | ✅ index |
| `/ueber-uns` | `ueber-uns.astro` | Company story, partner manufacturers, values | ✅ index |
| `/kontakt` | `kontakt.astro` | Contact info + Web3Forms contact form | ✅ index |
| `/impressum` | `impressum.astro` | German legal imprint | ❌ noindex |
| `/datenschutz` | `datenschutz.astro` | GDPR privacy policy | ❌ noindex |
| `/*` | `404.astro` | Custom 404 page | ❌ noindex |

---

## Key Components

### `Layout.astro`
- Wraps every page.
- Props: `title`, `description`, `canonical`, `ogImage`, `noindex`.
- Imports `SEOHead`, `Header`, `Footer`, `CookieBanner`, and `global.css`.
- Provides skip link and semantic page structure.

### `SEOHead.astro`
- Generates `<meta>`, Open Graph, Twitter Card, canonical, favicon, and theme-color tags.
- Injects **Schema.org `LocalBusiness` JSON-LD** structured data (hardcoded business info).
- Default title suffix: `| Montagebau Fabian`.
- Uses `Astro.site?.origin` for canonical URL generation.

### `Header.astro`
- Sticky header with logo, desktop nav, mobile hamburger menu, and emergency call CTA.
- Mobile menu toggle implemented with vanilla JS inside a `<script>` tag.
- Uses `astro:assets` `Image` component for optimized logo rendering.
- Includes `<noscript>` fallback to ensure mobile menu is accessible without JS.

### `Footer.astro`
- 4-column footer: logo/about, navigation, contact info, legal links.
- Displays dynamic year (`new Date().getFullYear()`).
- Shows tax number and VAT ID.

### `CookieBanner.astro`
- Fixed bottom banner for cookie consent.
- Stores consent in `localStorage` under key `cookie-consent-mf`.
- Only **necessary cookies** are used — no analytics or marketing cookies.
- Dispatches `cookieConsentUpdated` CustomEvent on decision.

---

## Contact Form (`kontakt.astro`)

- Submits via `fetch()` to `https://api.web3forms.com/submit`.
- Client-side validation (name ≥2 chars, valid email, message ≥10 chars, privacy checkbox).
- Honeypot field (`botcheck`) for spam protection.
- Success / error states toggled via DOM classes (`hidden`).
- Loading state disables submit button and shows spinner.
- Real-time validation clears errors on input.

---

## Deployment

- **Platform**: Netlify
- **Build command**: `npm run build`
- **Publish directory**: `dist`
- **Security headers** (configured in `netlify.toml`):
  - `X-Frame-Options: DENY`
  - `X-Content-Type-Options: nosniff`
  - `X-XSS-Protection: 1; mode=block`
  - `Referrer-Policy: strict-origin-when-cross-origin`
  - `Permissions-Policy` restricts sensors/camera/microphone/etc.
  - `Strict-Transport-Security: max-age=31536000; includeSubDomains; preload`
  - `Content-Security-Policy` restricts sources and allows `connect-src` to `https://api.web3forms.com`
- **Cache headers**: Static assets (`.woff2`, `.css`, `.js`, `.webp`, `.avif`) cached for 1 year.

---

## Testing

- **No automated tests are currently configured.**
- Manual testing checklist for changes:
  1. Run `npm run dev` and verify pages at `localhost:4321`.
  2. Test mobile menu toggle on narrow viewports.
  3. Test contact form validation and submission flow.
  4. Run `npm run build` and `npm run preview` to verify production build.
  5. Check Lighthouse scores (performance, accessibility, SEO, best practices).

---

## Security Considerations

1. **No user authentication or sessions** — purely static.
2. **No external tracking** — no Google Analytics, no social media pixels.
3. **Contact form** uses Web3Forms (third-party API) — no backend code in this repo.
4. **Honeypot** spam protection on contact form.
5. **Security headers** enforced at Netlify edge (CSP, HSTS, frame options, etc.).
6. **Environment variables** with `PUBLIC_` prefix are embedded in client bundles — do not place secrets here.

---

## Common Pitfalls / Agent Notes

- **Do not add external tracking or analytics scripts** without explicit consent mechanism updates. The privacy policy and cookie banner explicitly state that no tracking occurs.
- **Keep all UI text in German.** Even internal labels, alt text, and aria labels should remain German.
- **Images**: Place new static images in `public/` and reference with root-relative paths (e.g., `/logo.jpg`). For optimized images, place in `src/assets/` and use `astro:assets` `Image` component.
- **TODOs in codebase**: There are placeholder images and incomplete legal details (e.g., insurance info in `impressum.astro`) marked with `<!-- TODO: ... -->` comments.
- **No React/Vue/Svelte**: If you need interactivity, use vanilla `<script>` tags inside Astro components or Astro's `client:*` directives (though none are currently used).
- **Tailwind v4**: Uses `@import "tailwindcss"` and `@theme` blocks in CSS. Do not mix with Tailwind v3 config patterns.
- **Astro Image component**: When using `<Image>` from `astro:assets`, always provide `width`, `alt`, and consider `densities={[1, 2]}` and `decoding="async"` for performance.
