# Tratan Motors website

Static HTML/CSS/JS sales site for Tratan Motors, a DAF and FAW truck and trailer dealer in Ghana. There is no build step and no framework: every page is a standalone `.html` file.

Rebranded from "ML Ghana Limited" in October 2026. Contact details still belong to the old business until the owner confirms replacements (see "Open owner questions").

## File structure

```
index.html              Home: hero slider, fleet cards, why-us, gallery, CTA band, contact strip
about.html              About us
products.html           Our Fleet overview (DAF, FAW, trailers)
daf-trucks.html         DAF range
faw-trucks.html         FAW range
trailers.html           Trailer range
services.html           Services, process steps, industries
contact.html            Contact details, mailto form, map, FAQ
assets/
  brand.css             Design tokens + shared chrome (header, nav, menus, footer, buttons, cards, icons)
  tratan-logo.png       The logo for pages: transparent, trimmed, 752x240
  tratan-logo-mark.png  Road mark only, transparent, 512x512
tratan-logo.jpg         Master logo file (cream background). Source only, not used by pages
favicon.ico             16/32/48, PNG-compressed
favicon-16.png, favicon-32.png, favicon-48.png, apple-touch-icon.png (180)
android-chrome-192x192.png, android-chrome-512x512.png, site.webmanifest
ml.png, favicon/, favicon_io (2)/   Old ML Ghana assets. Not referenced anywhere; delete once the owner signs off
```

## How the CSS is organised

- `assets/brand.css` is linked on every page immediately before the page's own `<style>` block, so page rules can override it.
- `brand.css` owns the tokens, the reset, header, desktop nav, dropdown, mobile menu, footer, button skins, card skin, icon circles, step numbers, the 4px accent strip and the scrollbar.
- Each page's `<style>` owns only that page's sections (heroes, grids, section spacing). It must use tokens, never hex values.
- Buttons and cards are painted by selector lists in `brand.css` (the `:is(...)` rules). A new primary button, ghost button or card goes into the matching list; page CSS sets only its layout (padding, width, display).
- Header, mobile menu and footer markup is identical on every page. The only per-page differences are the "you are here" classes: `active` on the desktop nav link, `is-current` in the Products dropdown, `current` in the mobile menu. Copy index.html's markup when adding anything to the chrome.

## Colour tokens

Defined once in `assets/brand.css` under `:root`.

| Token | Value | Use |
|---|---|---|
| `--brand-orange` | `#F47920` | Icons, bullets, borders, underlines, accent bars, button fills |
| `--brand-orange-light` | `#FBA43C` | Eyebrows and link hovers on navy; gradient start |
| `--brand-orange-dark` | `#D95F0E` | Primary button hover; gradient end |
| `--brand-navy` | `#16365C` | Navy sections, hamburger bars |
| `--brand-navy-light` | `#23507F` | Links in body copy; gradient start |
| `--brand-navy-dark` | `#0E2340` | Darkest sections, footer bar, numerals on orange |
| `--cream` | `#FAF7F2` | Logo plate on dark backgrounds |
| `--surface` | `#FFFFFF` | Page, header, cards, inputs |
| `--surface-alt` | `#F3F6FA` | Alternate light sections, dropdown hover |
| `--surface-dark` | `var(--brand-navy)` | Alias for dark surfaces |
| `--text-strong` | `#16365C` | Headings, nav links, labels |
| `--text-body` | `#44536B` | Body copy |
| `--text-muted` | `#7B8794` | Large text only (3.66:1 on white) |
| `--text-on-dark` | `#EEF3F9` | All text on navy |
| `--text-on-orange` | `#FFFFFF` | Labels on primary buttons, icons in orange circles |
| `--border` | `#E3E9F0` | Card, input, divider lines |
| `--border-strong` | `#C9D4E0` | Resting underline of body links |
| `--shadow-sm` | `0 1px 3px rgba(22,54,92,.08)` | Header at rest, cards at rest |
| `--shadow-md` | `0 6px 20px rgba(22,54,92,.10)` | Scrolled header, button hover, small images |
| `--shadow-lg` | `0 18px 48px rgba(22,54,92,.14)` | Card hover, dropdown, feature images, form card |
| `--gradient-brand` | `135deg, #FBA43C 0% → #F47920 55% → #D95F0E 100%` | Primary buttons, accent strips, icon tiles |
| `--gradient-navy` | `160deg, #23507F 0% → #16365C 60% → #0E2340 100%` | Footer, CTA bands, page heroes without photos |
| `--gradient-overlay` | `90deg, navy-dark .88 → .55 (60%) → .15` | Photo overlay behind left-aligned hero text (index slider) |
| `--radius-sm` / `--radius-md` / `--radius-lg` | `6px` / `12px` / `20px` | Buttons and inputs / cards and images / spare |
| `--transition` | `180ms cubic-bezier(.4,0,.2,1)` | All hover transitions |

Allowed non-token colours: `rgba(255,255,255,…)` tints on navy, `rgba(0,0,0,…)` text shadows on photos, the flat navy-dark photo overlay `rgba(14,35,64,.78)` on centred page heroes, the orange focus ring `rgba(244,121,32,.25)`, and the WhatsApp button's own green.

## Component rules

- **Navbar:** white, `--shadow-sm`, sticky; `--shadow-md` once scrolled (`.is-scrolled`, set by each page's script). Links `--text-strong`; hover and active show a 2px `--brand-orange` underline while the label stays navy.
- **Primary button:** `--gradient-brand`, `--text-on-orange`, `--radius-sm`, no border. Hover: `--brand-orange-dark`, lift 2px, `--shadow-md`.
- **Ghost button (on navy only):** transparent, 2px `--text-on-dark` border and text. Hover: white 12% tint, lift 2px.
- **Secondary button (on light):** transparent, 2px `--brand-navy` border, `--brand-navy` text. Hover: fill navy, text `--text-on-dark`. None exist yet; add the class to `brand.css` when one is needed.
- **Headings (h1–h4):** `--text-strong` on light sections, inherited `--text-on-dark` on navy; line-height 1.2; bold.
- **Eyebrow / kicker:** uppercase, letter-spaced. On light backgrounds: `--text-strong` text with a 24×3px `--brand-orange` bar before it. On navy: `--brand-orange-light` text.
- **Hero:** photo with `--gradient-overlay` when text is left-aligned; the flat navy-dark overlay when text is centred, and on screens up to 768px wide (the text spans the full width there). White text, one primary and one ghost button. Slider arrows sit beside the dots on small screens so they never cover text.
- **Cards:** `--surface`, 1px `--border`, `--radius-md`, `--shadow-sm`. Hover: `--shadow-lg`, lift 4px, a 3px `--brand-orange` top bar that scales in.
- **CTA bands:** `--gradient-navy` with a 4px `--gradient-brand` strip on top, white text, primary + ghost buttons.
- **Footer:** `--gradient-navy`, 4px `--gradient-brand` strip on top, `--text-on-dark` text, headings at full strength, links hover to `--brand-orange-light`.
- **Forms:** `--surface` inputs, 1px `--border`, `--radius-sm`. Focus: `--brand-orange` border plus `0 0 0 3px rgba(244,121,32,.25)`.
- **Icons and bullets:** `--brand-orange`. White icons inside orange circles. Step numerals are `--brand-navy-dark` on orange.
- **Body links:** `--brand-navy-light`, underlined in `--border-strong`; the underline turns `--brand-orange` on hover.
- **Scrollbar:** navy track, orange thumb, desktop pointers only.
- **Typography:** Poppins 400/600/700/800 from Google Fonts. Do not load further weights.

## Responsive checks

Check every change at 1440, 1024, 768 and 390px wide. Known traps:

- A photo with `min-height` and width/height attributes gets a minimum *width* from its aspect ratio (280px x 1.54 = 431px), which overflows phones. Every such rule carries `min-width: 0`; keep it.
- Footer columns stack and centre under 900px; flex rows inside them need `justify-content: center`.
- Most product photos are hot-linked from third-party sites and some refuse to load. Layouts must survive a broken image.

## Contrast (WCAG 2.x, measured)

- `--brand-orange` text on white is **2.76:1** and fails even for large text. Never use orange for text on light backgrounds; use it for icons, bars, borders and fills.
- White on `--gradient-brand` measures 2.0–3.75:1. The primary button keeps white labels by explicit owner decision; this is a known exception, not a pattern to copy.
- `--text-muted` is 3.66:1 on white: large text only.
- Safe pairs: `--text-strong` on white 12.2:1, `--text-body` on white 7.8:1, `--text-on-dark` on any navy 7.4–14.1:1, `--brand-orange-light` on `--brand-navy` 6.1:1, `--brand-navy-dark` on `--brand-orange` 5.7:1.

## Logo usage

- Use `assets/tratan-logo.png` on pages. Never the JPG (it carries a cream box and a large empty margin).
- Size by height only, with `width: auto`. Header 44px, 34px under 768px; footer 44px. Minimum height 34px; below that use `assets/tratan-logo-mark.png`.
- Clear space: at least a quarter of the logo height on every side (11px at 44px). Nothing else inside that space.
- Never stretch or squash: never set both width and height, never change `aspect-ratio` away from `auto 752 / 240`.
- Never recolour: no filters, no white or inverted version. On dark backgrounds, place it on a `--cream` plate with `--radius-sm`, as the footer does.
- Logo `<img>` tags: `alt="Tratan Motors"`, `loading="eager"` in the header, `loading="lazy"` below the fold, no width/height attributes.

## Do not

- Do not invent contact details: no new phone numbers, emails, addresses, social handles, domains or testimonials.
- Do not add new pages without asking.
- Do not change the colour tokens without asking.
- Do not put hex colours in page CSS; use a token.
- Do not use orange, or `--text-muted`, for normal-size text on light backgrounds.
- Do not reference `ml.png`, `favicon/` or `favicon_io (2)/`.
- Do not restructure page sections or rewrite copy as part of styling work.

## Open owner questions

- The logo wordmark reads "TRATAN TRANSPORT"; the brand name used on the site is "Tratan Motors".
- Whether the registered name includes "Limited" (the old site said "ML Ghana Limited"; the site now says "Tratan Motors").
- Phone `0537685549`, WhatsApp `233537685549`, email `mlghana@hotmail.com`, address "Fishing Harbour Road, Adjacent Grimaldi, Tema", mailing address "P.O. Box CO 1458, Accra", and the canonical domain `mlghana.com` are all carried over from ML Ghana.
- Facebook and LinkedIn links are `href="#"` placeholders.
- WhatsApp button colours (WhatsApp green with white text, 1.98:1) were left unchanged.
