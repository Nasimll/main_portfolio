# Handoff: Nasimjon Mullojonov — Portfolio

## Overview
A single-page personal portfolio for Nasimjon Mullojonov (software engineer / AI & product builder, Warsaw). Concept: "Man from the Mountains" — a Tajik mountain night/day scene in the hero, then a calm navy content site: intro (BUILDER), About, Work, Stack, Path (journey map Dushanbe → Warsaw) and Contact.

## About the Design Files
The files here are **design references built in HTML** — they show intended look and behavior, not production code to ship. Recreate them in the target stack. If there is no codebase yet, recommended: **Next.js (or Vite + React) + TypeScript + Tailwind or CSS Modules**, with GSAP/ScrollTrigger or plain `requestAnimationFrame` for the parallax, and `d3-geo` + `topojson-client` for the map.

`Portfolio.dc.html` opens directly in a browser (it needs `support.js` beside it). All styles are inline; all behavior is in the `<script data-dc-script>` class at the bottom of the file (`componentDidMount` holds the animation loop). `Path Map.html` is the journey map, embedded via `<iframe>` — in production make it a normal component.

## Fidelity
**High-fidelity.** Colors, type, spacing, copy, and motion are final. Recreate pixel-accurately.

## Page structure (top → bottom)

### 0. Fixed background scene (hero only)
A `position:fixed` full-viewport stack behind the hero:
- `#sky-night` / `#sky-day` — `assets/sky-night-m.png`, `assets/sky-day-m.png`, `background-size:cover`.
- `#stars` — `<canvas>`: twinkling stars (also low near the mountains), shooting stars in night mode, cursor star-trail.
- `#mtn-night` / `#mtn-day` — `assets/mtn-night.png`, `assets/mtn-day.png` (the man standing on the ridge, centered). `bottom:-8vh; height:112%; background-size:112% auto`.
- `#ground` gradient + `#veil` gradient for text legibility.
- **Day/night toggle**: a diagonal wipe (mask-image linear-gradient at 100°) sweeps across the screen; day layers are revealed left → right. Duration ≈ 4.8 s (`__pfWipe` runs from −4800 → 0 ms). Theme switch affects the hero only — every section below stays in the night palette.
- **Scroll parallax**: mountains rise faster than the sky as the user scrolls (sky ~0.2×, mountains ~0.6–0.7× scroll); scene is hidden (`visibility:hidden`) once the intro section fully covers it, so its back edge is never visible on fast scroll.

### 1. Header (fixed)
- Left: logo **"NM"** — Nevera, 34px, uppercase, letter-spacing .06em.
- Center/right: nav links (About, Work, Stack, Contact), day/night toggle pill (☾ / ☀ knob), "Download CV" pill.
- Transparent over the hero; after scrolling past it: background `color-mix(var(--bg) 84%, transparent)` + `backdrop-filter: blur`, 1px bottom border `rgba(220,234,247,.14)`.

### 2. Hero
Centered column, vertically centered over the man.
- Eyebrow: "Tajikistan — the heart of the mountains" (small uppercase, dot before).
- H1 (Nevera): "Man from the **MOUNTAINS**" — "MOUNTAINS" uppercase, color `#dceaf7`.
- Paragraph (Manrope 18px/1.7, white 90%, soft text-shadow).
- Buttons: primary "Download CV" (filled `#4a74a7`, white), secondary "See selected work" (glass: white 10% bg, 1px white 34% border, backdrop blur). Pills, padding 17×29px.
- Light mode: a soft, blurred, wide dark plate behind the headline for contrast.
- Elements fade/rise in on load (staggered `animation-delay` .08s steps).

### 3. Intro — BUILDER
- Giant word **BUILDER** in Nevera, `font-size: clamp(64px,15.5vw,236px)`, `transform: scaleY(1.3)` (taller, not wider), very subtle muted text-shadow for 3D.
- Portrait `assets/portrait-v4.png` (transparent PNG) centered in front of the word, bottom-aligned. Behind the portrait: soft radial blue glow `rgba(74,116,167,.42)` → transparent, `filter: blur(30px)` — no box shadow.
- Name below in large type; small line "Engineering · Product · AI" style caption.
- On scroll-in: name, caption and portrait animate in (rise + fade, staggered). On hover: the word/photo plays a subtle motion.
- Section background fades from transparent to `--bg` so it covers the fixed scene.

### 4. About
Two equal columns (`grid-template-columns: repeat(auto-fit, minmax(min(100%,300px),1fr))`, column-gap `clamp(40px,6vw,88px)`, stacks on narrow screens).
- Left: `assets/workspace.jpg`, `object-fit:cover; object-position:28% 50%`, radius 20px, shadow `0 30px 70px rgba(0,0,0,.45)`; height matches the text column (min `clamp(320px,38vw,520px)`).
- Right (no box, vertically centered, gap 28px): H2 "I turn ideas into working products." (Nevera), body paragraph, stat row (~150 leads generated · +30% site traffic · 2 major companies served · 5 languages).
- Skills (bars, 7px tall, radius 7px, track `rgba(220,234,247,.14)`, fill gradient `#4a74a7 → #dceaf7` + glow `0 0 14px rgba(157,187,224,.45)`; label 16px/600, % in accent `#9dbbe0`). Bars animate width 0 → value over 1.6s on scroll-in:
  - Frontend 90% · Product / roadmap / analysis (incl. marketing) 70% · AI & automation 50% · Backend 45%
- Languages (5 solid cards, `#142a44`): English — Professional · B2 (upper-intermediate); Russian — Fluent; Persian — Fluent; Turkish — A1; Polish — A1.
- Full-bleed banner: `assets/dushanbe-night.jpg` with slow parallax, gradient fade to `--bg` top & bottom, centered white copy + "Get in touch" button.

### 5. Work — "Things I shipped"
- **Flagship card — Sadykov Group** (big project, 2025—26). Green brand gradient `linear-gradient(150deg,#14251a 0%,#0c1610 60%,#070a14 100%)`, radius 18px. Default state: logo mark (`assets/sg-mark.png`) + "SADYKOV / GROUP" in Nevera, letters rise in one by one (45ms stagger) on scroll-in; a large faint mark watermark at 9% opacity on the right.
  - **Hover / focus / tap** expands details (grid-template-rows 0fr → 1fr, .6s): marquee strip of what was done (Designed the site · Built the frontend · Wired the backend · Automated recruiting · Ran the marketing funnel · Owned the roadmap — 28s loop), description, tags (React, Express, SQLite, Apps Script, i18n), stats (~150 leads, +30% traffic), CTA "Visit sadykovgroup.pl ↗" → https://www.sadykovgroup.pl/
- **Mini projects** grid (4 cards, `#142a44`): type label + name by default, "+" rotates to "×" and details expand on hover/tap:
  - Telegram auto-reply (Python, Telethon) → github.com/Nasimll/auto_reply
  - Library system (C#, WinForms, SQL) → github.com/Nasimll/WF-LibrarySystem
  - University admissions (C#, SQL) → github.com/Nasimll/WF-University-Application
  - Interface studies (HTML, CSS, JavaScript) → github.com/Nasimll/Projects-HTML-CSS-JS
- Header link "All repos on GitHub →" → https://github.com/Nasimll

### 6. Stack
Grid of solid `#142a44` cards grouping tools (frontend, backend, AI/automation, product). Below: centered note "Explore the full stack, projects and experience in my CV." + "Download CV ↓" pill.

### 7. Path — "How I got here" (`Path Map.html`)
Left: map (d3-geo Mercator, world-atlas 110m countries, framed lon 8–80 / lat 34–60). Land `#142a44`; Tajikistan (id 762) & Poland (616) `#24456e` with `rgba(157,187,224,.6)` stroke; borders `rgba(220,234,247,.12)`. Right: 4 step cards.
Auto-plays when scrolled into view:
1. Dushanbe pin appears (pulsing ring) — "Where it started".
2. Plane flies along the great-circle arc to Warsaw (2.6s, cubic in-out), drawing a dashed `#9dbbe0` route.
3. Warsaw pin + "Sadykov Group" tag.
4. "Vistula University" tag (BSc Computer Engineering).
Active step: bg `#1c3a5e`, border `rgba(157,187,224,.55)`, `translateX(4px)`. Cards are clickable to jump; "↺ Replay the journey" button.

### 8. Contact
Two equal columns, aligned center.
- Left (no box): H2 "Let's start a conversation" (Nevera, `clamp(28px,3.9vw,56px)`), paragraph, **Stay connected** cards (LinkedIn → linkedin.com/in/nasimmulladzhanov, GitHub → github.com/Nasimll, Email → nasimjonmullojonov@gmail.com) with `#4a74a7` icon tiles; "Download CV" + phone "+48 575 416 431" pills.
- Right: form card (`#142a44`, radius 20px): Your full name, Work email address, Message details, "Send message" (pill, `#4a74a7`, hover `#5a86bb`). Inputs: bg `#0c1c33`, 1px `rgba(220,234,247,.22)`, radius 10px, left icon; focus border `#4a74a7` + ring `0 0 0 3px rgba(74,116,167,.35)`.
- Validation: all required, email regex; error text "Please add your name, a valid email and a message." in `#f2b8b5`. Current behavior opens a `mailto:` draft — **replace with a real endpoint** (Formspree / Resend / API route).
- Footer: "Nasimjon Mullojonov — Warsaw, Poland" · "LinkedIn ↗".

## Global background
Below the hero a fixed `<canvas id="field">`: 56px grid of faint lines (`rgba(220,234,247,.045)`), random cells softly glowing `rgba(74,116,167,…)` with blur 26, cells also light under the cursor; grid drifts at 0.25× scroll. Fades in as the intro section ends.

## Interactions summary
- Scroll reveal: `[data-rise]` elements fade + translateY in via IntersectionObserver.
- Card details reveal on hover (mouse), focus (keyboard), click/tap (touch).
- All motion should respect `prefers-reduced-motion` in production (disable parallax, wipe → crossfade, no marquee).

## Design tokens
Colors
- `--bg` #081426 · `--bg2` #0c1c33 · card #142a44 · active card #1c3a5e · highlighted land #24456e
- primary #4a74a7 (hover #5a86bb) · accent #9dbbe0 · light #dceaf7 · ink #eef4fb
- text dim rgba(220,234,247,.76) · muted rgba(220,234,247,.62) · line rgba(220,234,247,.14)
- Sadykov brand: #14251a / #0c1610, cream #f3ecd2

Type
- Display: **Nevera** (headlines, BUILDER, NM logo, SADYKOV GROUP). Loaded from fonts.cdnfonts.com — free for personal use only; **buy the commercial license (Almeera Studio) before publishing** and self-host.
- Text/UI: **Manrope** 400–800 (Google Fonts).
- Body 15–18px / 1.6–1.7; small labels ~12.6px uppercase, letter-spacing .14–.2em.

Radius: pills 100px · cards 14px · big cards/photos 18–20px · inputs 10px.
Spacing: section padding ~110–130px vertical, `clamp(20px,5vw,56px)` horizontal; content max-width 1120px; card gaps 14–20px.

## Assets (`assets/`)
- sky-night-m.png, sky-day-m.png — hero skies
- mtn-night.png, mtn-day.png — mountains with the standing man
- portrait-v4.png — transparent portrait for BUILDER
- workspace.jpg — About photo
- dushanbe-night.jpg — About banner
- sg-mark.png, sg-logo.png — Sadykov Group logo (extracted, transparent)
- Nasimjon-Mullojonov-CV.docx — CV (consider a PDF)

## Files
- `Portfolio.dc.html` — full page (markup + logic class at the bottom)
- `Path Map.html` — journey map component
- `support.js` — runtime needed only to open the reference HTML locally
