# Design Guidelines

Brand reference: <https://sii.pl/>, extracted on 2026-10-17 with the Playwright CLI (headed Chrome, desktop 1440px plus a 929px pass). Machine-readable tokens live in [`assets/design-tokens.json`](../assets/design-tokens.json); this document explains how to use them.

> License note: the Lato font files and the Sii logo were downloaded from sii.pl. Verify usage rights before using or redistributing them outside this course.

## Assets

| File | Purpose |
|---|---|
| [`assets/homepage.png`](../assets/homepage.png) | Full-page desktop screenshot of sii.pl, the visual reference for Manual QA |
| [`assets/logo.svg`](../assets/logo.svg) | Sii logo (vector, 471.8 x 306.1 viewBox) |
| [`assets/favicon.png`](../assets/favicon.png) | Favicon, 32x32 PNG |
| [`assets/design-tokens.json`](../assets/design-tokens.json) | Colors, typography, spacing, radii, component tokens |
| [`assets/fonts/lato/`](../assets/fonts/lato/) | Self-hosted Lato subset, 7 weights (woff2) |

## Colors

### Brand

| Token | Hex | Usage |
|---|---|---|
| `brand.primary` | `#0E6EE0` | Main blue: links, secondary-button text, active states, solid CTA fallback |
| `brand.primaryAlt` | `#2079E2` | Outline ("ghost blue") buttons on light backgrounds |
| `brand.violet` | `#4C38C6` | Gradient start |
| `brand.cyan` | `#47DDE2` | Gradient end, accent highlights (for example the "AI" word in the hero) |
| `brand.navy` | `#021C69` | Modal backdrop base (used at 40% opacity) |
| `brand.midnight` | `#020024` | Hero image overlay, bottom of the dark gradient |
| `brand.gradient` | `linear-gradient(95deg, #4C38C6, #0E6EE0 50%, #47DDE2)` | Primary CTA buttons ("Dołącz do nas"), active chips |

The violet-blue-cyan gradient is the signature of the brand. Use it for the single most important action on a screen, not for large surfaces.

### Logo palette

| Token | Hex |
|---|---|
| `logo.dark` | `#005AA2` |
| `logo.mid` | `#88B4DC` |
| `logo.light` | `#BCD5EC` |

These are for the logo only; do not use them as UI colors.

### Backgrounds

| Token | Value | Usage |
|---|---|---|
| `background.default` | `#FFFFFF` | Page background |
| `background.light` | `#F5F9FE` | Pale-blue panels, secondary button fill, alternating sections |
| `background.dark` | `#2B2B2B` | Dark buttons (newsletter "Subskrybuj") |
| `background.overlay` | `rgba(2, 28, 105, 0.4)` | Modal backdrop |
| `background.heroOverlay` | `linear-gradient(0deg, #020024, rgba(255,255,255,0.396))` | Darkening layer over hero photos |

### Text

| Token | Hex | Usage |
|---|---|---|
| `text.primary` | `#2B2B2B` | Body copy and headings on light backgrounds (never pure black) |
| `text.secondary` | `#7F7F7F` | Meta text, dates, placeholders |
| `text.muted` | `#B7B7B7` | Disabled and helper text |
| `text.link` | `#0E6EE0` | Inline links |
| `text.onDark` | `#FFFFFF` | Text on hero, gradients and dark sections |
| `text.iconOnDark` | `#E1E1E1` | Social icons in the footer |

Error and success colors are not used on the homepage and were not extracted. If the app needs them, agree on values and add them to the tokens.

## Typography

**Font family:** `Lato, Arial, sans-serif` is the only family used on the site.

Self-hosted files (from the site's own `@font-face` rules, `font-display: swap`):

| Weight | File |
|---|---|
| 300 Light | `assets/fonts/lato/subset-Lato-Light.woff2` |
| 300 Light Italic | `assets/fonts/lato/subset-Lato-LightItalic.woff2` |
| 400 Regular | `assets/fonts/lato/subset-Lato-Regular.woff2` |
| 500 Medium | `assets/fonts/lato/subset-Lato-Medium.woff2` |
| 600 Semibold | `assets/fonts/lato/subset-Lato-Semibold.woff2` |
| 700 Bold | `assets/fonts/lato/subset-Lato-Bold.woff2` |
| 900 Heavy | `assets/fonts/lato/subset-Lato-Heavy.woff2` |

These files are a subset and their license must be verified before use. Lato itself is open source (SIL OFL), so loading it from Google Fonts is a safe alternative.

Example `@font-face`:

```css
@font-face {
  font-family: Lato;
  src: url("/fonts/lato/subset-Lato-Regular.woff2") format("woff2");
  font-weight: 400;
  font-style: normal;
  font-display: swap;
}
```

### Scale

| Role | Size / line height | Weight | Notes |
|---|---|---|---|
| Hero display | 145px / 145px | 900 | Uppercase, white, desktop only |
| Hero (mobile) | 35px / 39px | 900 | White |
| Section title | 45px / 55px | 400 | Uppercase, a key word may be colored blue |
| Stat number | 55px / 66px | 400 | "7 500+" style figures |
| Section subtitle | 28px / 42px | 400 | |
| Modal title | 24px / 34px | 700 | |
| Card title | 22px / 28px | 400 | |
| Lead / card teaser | 18px / 28px | 400 | |
| Navigation | 16px / 19px | 600 | |
| Body | 14px / 20px | 400 | Default copy size on the site |
| Small / meta | 12px / 14px | 400 | Dates, form inputs |

Headings rely on size and case rather than weight: section titles are regular (400), only the hero uses Heavy (900).

## Spacing

Base unit is 4px; common steps are 4, 8, 12, 16, 20, 24, 28, 32, 40, 48px.

- Content container: max-width `1170px`, side padding `15px` (Bootstrap-style grid).
- Header: `39px` top, `20px` bottom padding, about `154px` tall, overlaid on the hero.
- Buttons: `5px 20px` (small), `7px 20px` (standard outline), `8px 32px 10px` (large).
- Card text block: `35px` vertical padding.

## Border radius

| Token | Value | Usage |
|---|---|---|
| `none` | `0px` | Sections, images, floating sticky buttons |
| `sm` | `4px` | Small UI details |
| `md` | `8px` | Panels |
| `cardBottom` | `0 0 8px 8px` | News/event cards (image on top, rounded bottom) |
| `pill` | `20px` | All standard buttons (fully rounded at 30-32px height) |
| `lg` | `30px` | Large CTA buttons |
| `chip` | `42px` | Filter chips/tabs |
| `inputLeft` | `35px 0 0 35px` | Input joined to a pill button on its right |
| `circle` | `50%` | Avatars, icon buttons, carousel dots |

## Components

### Header and navigation
- Transparent, absolutely positioned over the hero image; logo on the left, menu on the right.
- Menu items: white `#FFFFFF`, 16px, weight 600, padding `10px 12px`. Items with submenus open a dropdown.
- Right side holds two small pill buttons: "Dołącz do nas" (gradient, primary) and "Kontakt" (secondary: `#F5F9FE` fill, `#0E6EE0` text).
- Mobile (below about 1000px): hamburger opening a full-screen white menu with `#2B2B2B` 14px items.

### Buttons
| Variant | Look |
|---|---|
| Primary | Brand gradient, white text, 20px radius, no border |
| Secondary | `#F5F9FE` fill, `#0E6EE0` text, 20px radius |
| Ghost light | Transparent, 1px white border, white text, on hero/dark backgrounds ("Więcej", "Obejrzyj video") |
| Ghost blue | Transparent, 1px `#2079E2` border and text, on white backgrounds |
| Dark | `#2B2B2B` fill, white text (newsletter submit) |
| Chip (active) | Gradient, 42px radius, `12px 26px` padding, uppercase 14px |

All buttons use regular (400) weight and sentence case, except chips.

### Hero / slider
Full-width photo with a dark gradient overlay (`#020024` to transparent), huge white uppercase headline, short lead text and a ghost-light "Więcej" button.

### Cards
News and event cards: image on top, text below on a transparent background, `0 0 8px 8px` radius, no shadow, `35px` vertical padding on the text block, 22px title.

### Forms
Newsletter: white input with left-rounded corners (`35px 0 0 35px`), 12px placeholder in `#7F7F7F`, joined to a dark pill button "Subskrybuj".

### Sections
White sections alternate with pale-blue `#F5F9FE` panels. Section headings are large, uppercase and regular weight, often with one blue word (for example "POZNAJ **OFERTĘ**").

## Logo usage

- Use [`assets/logo.svg`](../assets/logo.svg): two overlapping tilted ellipses in `#005AA2` / `#88B4DC` / `#BCD5EC` with the lowercase "sii" wordmark knocked out in white.
- Place it top-left in the header, about 44px wide in the nav bar.
- The site shows the same full-color logo over the dark hero; no separate inverted variant was found. On dark backgrounds keep the full-color version with enough clear space around it.
- Do not recolor, stretch or add effects to the logo.

## Visual style summary

Sii presents itself as a large, confident technology and engineering partner ("Power People"). The look is corporate but energetic: lots of white space, photo-driven heroes with dark overlays and massive white uppercase Heavy headlines, and a signature violet-blue-cyan gradient reserved for key actions. Typography is a single family (Lato) used mostly at regular weight, with size and uppercase providing hierarchy. Shapes are soft: every button is a pill, while sections and images stay square. The palette is restrained to blues on white, with dark gray `#2B2B2B` instead of black for text.

## Notes from extraction

- The cookie consent banner ("Pliki cookies na sii.pl") was accepted before extraction and the screenshot.
- In `homepage.png` the "Nasze case studies" background video and two news thumbnails did not render (lazy media); the layout is otherwise complete.
- The site is built on WordPress; the `--wp-*` and `--cmplz-*` CSS variables belong to WordPress and the cookie plugin, not to the brand, and were ignored.
