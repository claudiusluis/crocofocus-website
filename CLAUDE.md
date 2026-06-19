# CrocoFocus — Marketing Website

## Project Brief

Marketing website for the iOS app **CrocoFocus**. Hosted on GitHub Pages initially, later
migrated to a custom domain (e.g. crocofocus.app) — no code changes needed for migration,
just a CNAME file.

**App summary:** CrocoFocus blocks selected apps each evening using Apple's Screen Time API
(FamilyControls / ManagedSettings). Features: bedtime schedule, streak counter, temporary
bypass ("open anyway" for N minutes), premium tier via in-app purchase. App is not yet live
— waiting for Apple Developer Account verification.

---

## Hosting & URLs

| Environment | URL |
|---|---|
| GitHub Pages (now) | https://claudiusluis.github.io/crocofocus-website/ |
| Privacy page | https://claudiusluis.github.io/crocofocus-website/privacy/ |
| Support page | https://claudiusluis.github.io/crocofocus-website/support/ |
| Future domain | https://crocofocus.app (replace above, paths stay identical) |

**GitHub repo:** `claudiusluis/crocofocus-website`

**Privacy URL in Swift app:**
`CrocoFocus/CrocoFocus/CrocoFocus/Features/Info/InfoView.swift`
Currently set to: `https://claudiusluis.github.io/crocofocus-website/privacy/`

---

## Pages

| File | URL path | Purpose |
|---|---|---|
| `index.html` | `/` | Landing page — main marketing page |
| `privacy/index.html` | `/privacy/` | Privacy Policy (required for App Store) |
| `support/index.html` | `/support/` | FAQ + contact email |

Using subfolders (`privacy/index.html`) instead of flat files (`privacy.html`) gives
clean URLs without `.html` extensions — important for App Store policy link stability.

---

## Tech Stack

- **HTML5** — no build step, no framework, no Node.js needed
- **Tailwind CSS** — via CDN (Play CDN), utility-first styling
- **AOS (Animate On Scroll)** — via CDN, Apple-like scroll fade-in animations
- **Google Fonts: Nunito** — rounded, playful, free

CDN links to use in every HTML file:
```html
<!-- Google Fonts: Nunito -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Nunito:wght@400;600;700;800&display=swap" rel="stylesheet">

<!-- Tailwind CSS Play CDN -->
<script src="https://cdn.tailwindcss.com"></script>

<!-- AOS -->
<link href="https://unpkg.com/aos@2.3.4/dist/aos.css" rel="stylesheet">
<script src="https://unpkg.com/aos@2.3.4/dist/aos.js"></script>
<script>AOS.init({ duration: 700, easing: 'ease-out-cubic', once: true });</script>
```

---

## Design Principles

Inspired by apple.com: minimal text, generous whitespace, smooth scroll animations.
Character (Croco) is childlike and playful — the design should reflect that while
staying calm and premium (sleep-app audience).

- **Lots of whitespace** — sections breathe, nothing feels cramped
- **One focus per section** — no information overload
- **Rounded corners** — `rounded-3xl` (24px) as default for cards/images
- **AOS animations** — `data-aos="fade-up"` on headings and cards, staggered with `data-aos-delay`
- **Light mode first** — dark mode can be added later
- **Croco appears early** — in the hero section, sets the playful tone immediately

---

## Color Palette

Extracted directly from Croco's SVG files in the iOS app.

| Role | Hex | Usage |
|---|---|---|
| **Croco Primary** | `#58CC02` | Buttons, pill badges, key accents |
| **Croco Light** | `#8EE820` | Gradient tops, hover states |
| **Croco Belly** | `#C8F06A` | Light background chips, tags |
| **Croco Dark** | `#3A8E00` | Button hover, outlines |
| **Croco Outline** | `#236200` | Decorative borders if needed |
| **Background** | `#FAFAF8` | Page background (warm white, not pure white) |
| **Section Alt** | `#F0FDF4` | Alternating section backgrounds |
| **Text Primary** | `#1C1C1E` | Headings and body text |
| **Text Secondary** | `#6B7280` | Subtext, captions, labels |
| **Sleep Accent** | `#9B8EC4` | Moon/night themed decorative elements |

Tailwind config to inject at top of each page:
```html
<script>
  tailwind.config = {
    theme: {
      extend: {
        colors: {
          croco: {
            DEFAULT: '#58CC02',
            light:   '#8EE820',
            belly:   '#C8F06A',
            dark:    '#3A8E00',
            outline: '#236200',
          },
          sleep: '#9B8EC4',
        },
        fontFamily: {
          sans: ['Nunito', 'sans-serif'],
        },
        borderRadius: {
          '4xl': '2rem',
        },
      }
    }
  }
</script>
```

---

## Typography

- **Font:** Nunito (Google Fonts)
- **Weights:** 400 body, 600 semi-bold, 700 bold, 800 extra-bold for big headlines
- **Scale (Tailwind):**
  - Hero headline: `text-5xl md:text-7xl font-extrabold`
  - Section headline: `text-3xl md:text-4xl font-bold`
  - Body: `text-lg font-normal`
  - Caption / label: `text-sm text-gray-500`

---

## Croco Assets

Copied from iOS app into `assets/images/`. All SVGs are current:

| File | Usage |
|---|---|
| `croco_splash_logo.svg` | Hero section — full body, large display |
| `croco_head_left.svg` | Feature sections, facing left |
| `croco_head_right.svg` | Feature sections, facing right |
| `croco_head_center.svg` | Small decorations, centered |
| `croco_slider_bg.svg` | Decorative background element |

**Do not use:** `CrocodileHead.png` / `CrocodileHead.svg` in the iOS assets — outdated.

App icon (`AppIcon.png`) not yet finalized — placeholder needed until icon is ready.
App Store screenshots not yet available — use iPhone frame mockup placeholder.

---

## Landing Page Structure

Planned sections in order:

1. **Navbar** — Logo (Croco + "CrocoFocus"), nav links (Privacy, Support), App Store button
2. **Hero** — Big headline, subline, App Store badge (placeholder), Croco illustration
3. **Problem** — "You scroll through your phone every night..." emotional hook
4. **Features** — 3–4 feature cards: Bedtime Block, Streak, Bypass, Premium
5. **How It Works** — 3 simple steps with icons
6. **CTA** — Download button repeated, Croco waving goodbye
7. **Footer** — Links to Privacy / Support, copyright

---

## Privacy Policy Notes

Based on code review of `ScreenTimeShieldService.swift` and related files:

- Screen Time data (which apps are blocked): processed **on-device only**, never leaves device
- App settings (bedtime schedule, app selection): stored via `@AppStorage` / UserDefaults (on-device)
- Streak / usage data: on-device only
- No network requests to any server found in code
- No third-party analytics or tracking SDKs found
- In-app purchases: handled entirely by Apple — developer never sees payment info
- Local notifications only (UNUserNotificationCenter) — never sent to a server

Result: Privacy Policy can state that **no personal data is collected or transmitted**.
This is strong from a DSGVO/GDPR perspective and a selling point.

---

## Support Page Notes

Placeholder content until real content is ready:
- 4–5 example FAQ questions and answers
- Placeholder email: `support@crocofocus.app` (replace with real address later)

---

## Build Steps — Status

- [x] Schritt 1: Folder structure, GitHub repo, CLAUDE.md, copy assets, update Swift URL
- [ ] Schritt 2: Base HTML skeleton for all 3 pages (shared nav + footer pattern)
- [ ] Schritt 3: Tailwind + AOS + Nunito wired up, Tailwind config, CSS variables
- [ ] Schritt 4: Hero section (headline, subline, CTA button, Croco, iPhone placeholder)
- [ ] Schritt 5: Feature sections with AOS scroll animations
- [ ] Schritt 6: "How It Works" + second CTA + Footer
- [ ] Schritt 7: Privacy Policy page — full DSGVO-compliant text
- [ ] Schritt 8: Support page — FAQ placeholder + contact email
- [ ] Schritt 9: GitHub Pages deploy + CNAME prep for custom domain
