# Handoff: Rave to Save Website

## Overview
Marketing/donation website for "Rave to Save", a Würzburg-based charity party collective. Communicates the mission (fundraising through club events), introduces the DJ/organizer team, shows past and upcoming events with donation totals, and offers a contact form + Instagram link. Mobile-first, single scrolling page plus two legal pages (Impressum, Datenschutz).

## About the Design Files
The files in this bundle (`Rave to Save.dc.html`, `Impressum.dc.html`, `Datenschutz.dc.html`) are **design references built in HTML** — high-fidelity prototypes of look, content, and interaction, not production code to copy as-is. The task is to **recreate these designs in the target codebase's environment** (React, Vue, plain static site, whatever the team standardizes on) using that environment's conventions. If no environment exists yet, plain HTML/CSS/JS (or a static site generator) is a reasonable default given the site's simplicity — no backend is required except for the contact form (see below).

## Fidelity
**High-fidelity.** Colors, typography, spacing, copy, and layout are final/near-final. Recreate pixel-close. The `.dc.html` files are plain HTML with inline styles — legible directly by reading them; there is no hidden JS framework magic beyond a small amount of React-like state (mobile menu toggle, animated donation counter, image carousel, contact form fields).

## Pages
1. **Rave to Save.dc.html** — the main page, sections in order:
   - **Header/Nav** — fixed, logo left, links (Unsere Mission, Wer wir sind, Events) right, hamburger menu on mobile (full-screen overlay menu).
   - **Hero** — full-bleed background image (`images/hero-bg.png`), large serif headline, subtext, scroll cue.
   - **Mission** (`#mission`) — centered heading + paragraph on light background (#f5f2ed area, check inline styles).
   - **Spendenziele** (`#spendenziele`) — dark gradient section (`#252f37 → #2d3b45 → #3d4f5c`), animated donation counter (counts up to 23.490 € on mount via `requestAnimationFrame`), plus organization logos grid (Tierheim, Wildwasser, Streetwork, Regenbogen).
   - **Wer wir sind** (`#wer-wir-sind`) — 4-person team grid (2 cols mobile, 4 cols desktop), each card: photo (3:4, object-fit cover), name, role, two lines of meta text.
   - **Events** (`#events`) — upcoming event card (image + name + date), and a horizontal carousel of past events (image, date, name, donation amount raised) with prev/next arrows.
   - **Unterstütze uns** — three support cards: buy a ticket, share on Instagram, join the team (each with an emoji icon, heading, short copy).
   - **Kontakt** (`#kontakt`) — contact form (name, email, message) that submits via `mailto:` (opens the user's email client — no backend), plus a direct Instagram link.
   - **Footer** — brand blurb, Navigation links (Mission, Wer wir sind, Events, Kontakt), "Folge uns" (currently Instagram only — Soundcloud/others planned), Kontakt email, legal links (Impressum, Datenschutz), copyright.
2. **Impressum.dc.html** / **Datenschutz.dc.html** — legal pages, same visual system (typography, colors, header/footer), static content only.

## Interactions & Behavior
- **Mobile menu**: hamburger icon toggles a full-screen overlay nav (`mobileMenuOpen` state); burger lines animate into an X.
- **Donation counter**: animates from 0 to 23.490 € over 2s with an ease-out-quart curve on page load (`componentDidMount`), formatted with German locale (`de-DE`, e.g. "23.490 €").
- **Past-events carousel**: shows 2 cards on desktop / 1 on mobile, `translateX` slide transform, prev/next buttons disable (opacity 0.25) at the ends.
- **Contact form**: controlled inputs (name/email/message) in local state; submit builds a `mailto:hallo@ravetosave.de` link with URL-encoded subject/body and navigates to it — **this is a placeholder, not real form delivery** (no message is stored or sent via a backend).
- **Responsive breakpoint**: single breakpoint at `window.innerWidth <= 767` toggles between mobile/desktop layout values (grid columns, paddings, font sizes) — recreate as a CSS media query at `max-width: 767px` rather than JS-driven layout switching if using a standard web stack.

## State Management
- `isMobile` (bool, derived from viewport width, updates on resize)
- `mobileMenuOpen` (bool)
- `donationValue` (number, animates 0 → 23490 on mount)
- `carouselIndex` (number, current scroll position of past-events carousel)
- `contactName`, `contactEmail`, `contactMessage` (strings, contact form fields)
- No data fetching — all content (team, past events, orgs) is hardcoded in arrays in the component.

## Design Tokens
**Colors**
- Dark navy gradient (footer, spendenziele bg): `#252f37 → #2d3b45 → #3d4f5c` (135deg)
- Heading text: `#4a5b69`
- Body text: `#5a6b78`
- Muted label text: `#7a8c99`
- Light section bg: `#f5f2ed`, `#f0ede8`
- Card bg / borders: `#fff8f5` bg, `#d4c9bd` border
- Accent (buttons, role labels): `#b87c6e → #c49a8a` gradient
- Footer link text: `#a8b8c2`

**Typography**
- Display/headings: `'DM Serif Display', serif`
- Body/UI: `'Inter', sans-serif`
- Section eyebrow labels: 13px, 500 weight, 2.5px letter-spacing, uppercase, `#7a8c99`
- H2: `clamp(32px, 4vw, 52px)`, line-height 1.08
- Body copy: 17px / 1.7 line-height

**Spacing**
- Section padding: `96px 24px` desktop / `72px 20px` mobile
- Team grid gap: `28px` desktop / `16px` mobile

**Radius**: cards/images generally `20px–24px` border-radius.

## Assets
All images live in `images/` (bundled in this handoff):
- `hero-bg.png` — hero background photo
- `logo.png` — site logo
- `event-graphic.png` — **upcoming event graphic; swapped out manually before every party** (see note below)
- `team-c2e.webp`, `team-ectivate.jpg`, `team-ranger.jpg`, `team-design.png` — team member photos
- `past-1.png`…`past-4.png` — past event photos for the carousel
- `org-tierheim.png`, `org-wildwasser.jpg`, `org-streetwork.png`, `org-regenbogen.png` — partner org logos

**Important for the dev handoff — recurring content that changes often:**
- `images/event-graphic.png` and its accompanying copy (event name/date) in the Events section are replaced before every single party. Build this as an easily-swappable config value (e.g. a JSON/data file or CMS field), not a hardcoded image import buried in markup.
- The `teamData` array (4 members) and `pastEventsData` array (past events + donation totals) are also edited periodically as the roster/event history grows — keep these as simple, editable data structures (JSON file or CMS collection), not inline in component code.
- Google Fonts (`DM Serif Display`, `Inter`) are currently loaded from Google's CDN — **should be self-hosted** in the production build for reliability/perf.
- The contact form currently just opens `mailto:` — needs a real form backend (e.g. serverless function + email service, or a form provider) if actual delivery/tracking is wanted.

## Files
- `Rave to Save.dc.html` — main page (all sections described above)
- `Impressum.dc.html` — legal notice page
- `Datenschutz.dc.html` — privacy policy page
- `images/` — all referenced image assets
