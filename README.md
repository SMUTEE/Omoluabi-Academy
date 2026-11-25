# Ọmọlúàbí Academy — Landing Page (Image Version)

Build brief and asset pack for Claude Code. Single-page marketing site for a premium online Yoruba class brand aimed at Nigerian diaspora parents. The deliverable here is a working static page plus everything needed to rebuild or extend it.

## Stack
Static HTML, inline CSS, vanilla JavaScript. No framework, no build step. Opens directly in a browser. Google Fonts loaded via `<link>`.

## Files
- `index.html` — the full page (CSS and JS are inline).
- `logo_cream.svg` — reversed brand logo (cream flower + gold centre) for dark backgrounds.
- `pattern.svg` — adire pattern, used as the faint hero background texture and the footer band.
- `img/hero.jpg`, `parent.jpg`, `songs.jpg`, `teen.jpg`, `still.jpg`, `group.jpg` — section photos. These are AI-generated placeholders; replace with real class/student photos before launch.
- `img/qr.png` — WhatsApp scan-to-chat QR. Encodes `wa.me/447361539140` with a prefilled booking message.

## Brand tokens (Warm Heritage)
Colours (CSS variables in `index.html`):
- Background navy `--bg #051A40`, `--bg-soft #081F47`
- Surfaces `--surface #0B2550`, `--surface-2 #10305F`
- Text cream `--text #F8F1E5`, muted `--muted #B7C2DA`
- Gold `--accent #E6A62C`, `--accent-soft #F3C867`
- WhatsApp green `#25D366`

Type (Google Fonts):
- Headings/display: **Bricolage Grotesque**
- Body: **Inter**
- (Fraunces is also loaded for an unused "editorial" theme block and can be removed.)

Layout: max width 1140px, radii 18 to 26px.

## IMPORTANT: theme attribute
The page sets `data-theme="heritage"` on `<html>` both statically and via a small inline `<script>` in the `<head>`. Some sandboxed iframe runtimes drop the static attribute, so the script re-applies it on load. Keep that script. If you port to a framework, set the theme attribute on the root element in your layout/root component.

## Page structure (top to bottom)
1. Sticky nav: logo, section links, "Book a spot" button.
2. Hero: eyebrow pill, headline, subhead, two CTAs, trust row, hero image on the right. Faint adire pattern behind the whole section.
3. How it works: a photo plus four points, each with a Feather SVG icon (video, clock, music, users).
4. Classes: section header plus a teen photo, then four age-tier cards (Little 3-5, Bright 6-9, Super 10-12, Teen 13+).
5. Group band: full-bleed group photo with an overlay heading.
6. Why it matters: parent-and-child photo with copy and a CTA.
7. Culture quote band: still-life photo with a short quote.
8. Pricing: two plan cards (group vs private), one marked "Most popular".
9. Book: enquiry form, direct WhatsApp buttons, scan QR.
10. Footer: logo, numbers, fine print, pattern band.

## Icons
Feather icons (https://feathericons.com), inlined as SVG with `stroke="currentColor"`. Used in "How it works". To add or change icons, copy the SVG markup from Feather, set width/height to 22, and let the colour come from the parent (`--accent-soft`).

## Enquiry form behaviour
On submit, `sendEnquiry()` builds a prefilled WhatsApp message from the fields and opens `wa.me`. It also sends the lead to a capture endpoint if one is configured.

To capture leads server-side, set the `FORM_ENDPOINT` constant near the top of the inline `<script>` to your endpoint URL:
- **Formspree:** create a form and paste its URL, e.g. `var FORM_ENDPOINT="https://formspree.io/f/xxxxxxx";`
- **Your own API:** paste any URL that accepts a JSON `POST`.

Leave `FORM_ENDPOINT=""` (the default) to only open WhatsApp — no POST is made. The POST is fire-and-forget: a failed or unreachable endpoint never blocks the WhatsApp hand-off. The JSON body is:

```json
{ "parent": "", "phone": "+44 7700 900123", "country": "United Kingdom", "dialCode": "+44", "email": "", "ageGroup": "", "classType": "", "note": "", "source": "omoluabi-academy-site", "submittedAt": "<ISO 8601>" }
```

Phone (with a country selector) is the required contact field; email is optional. `phone` is the dial code plus the number the parent typed, `country` / `dialCode` come from the selector and give a read on where each enquiry originates. The country list lives as `<option>` tags in the enquiry form — add or reorder countries there (each needs `data-dial` and `data-name`).

## WhatsApp
- UK: +44 7361 539140 → `https://wa.me/447361539140`
- NG: +234 803 331 2114 → `https://wa.me/2348033312114`
- Prefilled message via `?text=` (URL-encoded). The numbers live as `UK`/`NG` constants in the inline script. If you change the number or the message, regenerate `img/qr.png` to match.

## Pricing
- Little & Bright Stars (ages 3-9): Group £50/mo, Private £200/mo
- Super & Teen Stars (ages 10-13+): Group £70/mo, Private £250/mo

## Replacing images
Swap files in `img/` keeping the same names, or update the `src` paths. Suggested widths: hero ~1600px, section images ~1100 to 1400px, group ~1600px. Keep hero subjects weighted to the right so the overlay text stays clear.

## Before launch (TODO)
- Replace AI images with licensed or real photos.
- Write real, descriptive `alt` text per image.
- Respect `prefers-reduced-motion` (the scroll reveal uses IntersectionObserver).
- Add a favicon from the logo.

## If moving off static HTML
- Next.js or Astro: split the sections into components, move tokens to CSS variables or a Tailwind theme, put images under `/public` (use `next/image` or Astro assets).
- Deploy as a static site on Vercel, Netlify or Cloudflare Pages.
