# Nexyra Consulting — Digital Business Card

## Files

- `index.html` — **the deployable file.** Fully self-contained: logos, fonts, QR library and runtime are all inlined. No build step, no dependencies, works offline.
- `../Nexyra Digital Card.dc.html` — editable source (Design Component).
- `../nexyra-digital-card.src.html` — build input for the bundler (do not edit by hand; it is generated from the source above).
- `favicon.svg` — the violet Nexyra app icon. Already inlined inside `index.html` as the favicon and Apple touch icon; the loose copy is here only for hosts that also want a `/favicon.svg` at the domain root.
- `../assets/logos/*.svg` — the nine Nexyra logo variations.
- `../assets/app-icon-violet.svg` — app icon source.

## Two card styles

The header switcher offers **Ink** (dark, default) and **White**. The choice — like the field edits — is remembered per visitor. To ship one style as the published default, set the `cardTheme` prop (`ink` | `white`) in the source component and re-export. Logo variants swap automatically on the white card so white-on-white never occurs.

## Deploy

Any static host works. Upload `index.html` as-is:

- **Netlify / Vercel** — drag the file into the dashboard, or `netlify deploy --prod --dir .`
- **Cloudflare Pages / GitHub Pages** — commit `index.html` on the published branch
- **Own server** — drop it in the web root; nothing else is required

Recommended URL: `card.nexyraconsulting.co.uk` or `nexyraconsulting.co.uk/card`.

## After deploying — point the QR code and share links at the live URL

The card reads the page's own URL for sharing and for the QR code, so once it is hosted the QR and every share target resolve automatically. To force a specific URL (e.g. a short link), set the `shareUrl` prop on the source component and re-export.

## Editing details

Details entered in the "Edit details" drawer are saved in the visitor's own browser (localStorage), so they do not change what other people see. To change the **published** defaults, edit `DEFAULTS` in the source component and re-export:

1. Update `DEFAULTS` (name, job title, phone, email, website, address, LinkedIn, WhatsApp, tagline).
2. Re-run the standalone export to regenerate `index.html`.

Leaving a field empty removes it from the card entirely — no gaps, no placeholders.

## Brand notes

Colours, type and logo treatment are taken from the supplied Nexyra logo artwork:

- Gradient: `#A78BFA` → `#7C3AED` (39%) → `#3B82F6`
- Accents: violet `#9333EA`, blue `#2563EB`
- Ground: `#07070C` page, `#0B0B12` card
- Typeface: Hanken Grotesk (500/600 display, 300/400 text)
- Zero corner radius throughout; flush-left hierarchy

Default logo: **lockup — on dark**. Light-ground logo variants are given a white plate automatically so clear-space rules hold.
