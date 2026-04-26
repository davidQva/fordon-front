# Fordonskollen Design System

This is the design system for **Fordonskollen** — a Swedish vehicle-management
SaaS aimed at fleet owners, individual car owners, drivers, and transport
companies in the Nordics.

> **Fordonskollen — Enklare, Smartare, Grönare.** ("Simpler, Smarter, Greener.")
>
> "Samla dina fordon med ett verktyg." — *Pool your vehicles into one tool.*

---

## What the product does

Fordonskollen is a web app that pulls vehicle data from the Swedish
Transportstyrelsen (Transport Agency) and presents it in one overview. Fleet
managers and private owners use it to:

- Watch inspection (*besiktning*) deadlines with a red/yellow/green
  "trafikljusstatus" traffic-light indicator.
- Plan service, insurance (*försäkring*), tax (*skatt*) and tyre changes on
  one calendar.
- Manage drivers (*förare*) and their licence classes.
- Report faults (*felrapportering*) and calculate legal trailer loads.
- Register / de-register vehicles (*ställa på/av*) via the Transport Agency.
- Budget & forecast running costs across a fleet.

The company was founded in 2025 by Alexander Paravac and Anton Lindberg and is
based at Sperlingsgatan 16, 302 48 Halmstad. Founders are `anton@` and
`alexander@fordonskollen.com`.

---

## Products represented

A single product with two distinct surface families, both sharing the same
wrapper chrome (header / inner-nav / footer):

1. **Marketing site** — home, *Våra lösningar* (solutions), *Om oss* (about),
   *Priser* (pricing), *Kontakta oss* (contact), privacy. Full-width, image-led
   sections, pricing contact-sales model.
2. **Authenticated app** — dashboard ("Mina fordon"), vehicle list cards,
   add-vehicle forms, calendar, budget, driver register, inspection,
   maintenance, trailer calculator. Dense, table-and-card UI with a secondary
   inner nav strip.

Auth supports **business login** (email + password) and the code references
a commented-out **BankID** private-login flow (currently gated behind
`:dev` env in several places).

---

## Sources

- **Codebase** — `fordonskollen_phoenix_web/` mount (Elixir Phoenix LiveView,
  Tailwind + daisyUI). The truth for every component in this system.
  - Public site routes live under `FordonskollenPhoenixWeb.*Live`
  - Shared chrome: `components/fleet_header_component.ex`, `components/layouts.ex`
  - Core atoms: `components/core_components.ex` (buttons, inputs, flash, icons)
  - Dashboard + vehicle card: `live/dashboard_live.ex`, `live/components/vehicle_list.ex`
- **Images** — `images/` mount: logo, team portraits, marketing photography,
  a small library of vehicle silhouettes (sedan / truck / van / trailer /
  tractor / motorcycle / etc.), and a reklamfilm promo video.
- **Live site** — https://fordonskollen.com (referenced via canonical URLs in
  LiveView mounts but not fetched for this system).

All copy in the codebase is **Swedish** (`<html lang="sv">`).

---

## Index

| File / folder              | What it is                                                                 |
| -------------------------- | -------------------------------------------------------------------------- |
| `README.md`                | This file.                                                                 |
| `SKILL.md`                 | Agent-SKill manifest for invoking this system as a skill.                  |
| `colors_and_type.css`      | CSS variables — colors, type, spacing, shadows, radii.                     |
| `assets/`                  | Logos, team photos, marketing imagery, regbevis illustration, video.       |
| `assets/vehicles/`         | SVG vehicle silhouettes used in the dashboard cards.                       |
| `preview/`                 | HTML cards registered to the Design System tab.                            |
| `ui_kits/marketing_site/`  | Recreation of the public marketing pages (hero, solutions, pricing, etc).  |
| `ui_kits/dashboard_app/`   | Recreation of the authenticated fleet-manager app (dashboard, vehicle list, forms). |

---

## Content fundamentals

All product copy is Swedish. Tone is **utilitarian, direct, concrete, a bit
warm** — clearly written for Swedish SMB owners and fleet managers, not for
consumer hype. No emoji anywhere in the codebase. No exclamation marks in
marketing copy. No buzzwords like "AI", "revolutionary", "game-changing". The
voice is closer to a municipal / B2B SaaS than a consumer app.

### Voice rules

- **Person.** Marketing copy talks about the user as "du" (singular informal
  "you") and about Fordonskollen as "vi" (we). No "ni" (formal plural "you")
  except when addressing a whole company in later pricing/contact copy.
- **Casing.** Sentence case everywhere. Headings are sentence case, never
  Title Case. Some German-style capitalisation would feel wrong. Section
  labels in the dark footer use ALL CAPS (`KONTAKT`, `PARTNER`) with wider
  letter-spacing — that's the only place caps appear.
- **Punctuation.** Swedish characters å/ä/ö used correctly. Occasional em-dash
  in marketing copy. No Oxford comma (follows Swedish conventions). No
  trailing periods on short inline labels.
- **Numbers & dates.** Dates written out like "15 april 2026" in full; table
  rows use `YYYY-MM-DD`. Month names in-code use Swedish (`januari`, `feb`,
  `mars`, etc.). Phone numbers written `+46 70-212 45 64`.
- **Currency.** Kronor (SEK), shown contextually — pricing page explicitly
  avoids listing prices and says "contact us for a quote" instead.
- **No emoji.** Not one in the codebase. Don't invent them.
- **No unicode decoration.** No ✨ ✓ · — replacements. Check-marks are
  rendered as Heroicons SVGs, bullets as `•` in only one or two legal strings.

### Examples lifted from the codebase

- Hero H1: *"Samla dina fordon med ett verktyg."*
- Hero subtitle: *"Håll full koll på besiktning, service och försäkring för
  alla dina fordon."*
- CTA: *"Logga in som företag"*, *"Kom igång med Fordonskollen"*,
  *"Kontakta oss för prisuppgift"*
- Feature card: *"Central dashboard — All fordonsinformation samlad på ett
  ställe för enklare beslut och snabb överblick."*
- Solutions promise: *"Allt du behöver för dina fordon, på ett ställe."*
- Footer copyright: *"© 2026 Fordonskollen Norden AB. Alla rättigheter
  förbehållna."*
- Cookie banner: *"Vi använder cookies för att förbättra din upplevelse."*
- Flash (error): *"Fel e-postadress eller lösenord. Kontrollera dina
  uppgifter och försök igen."*
- Empty state / status: *"Ej planerat"*, *"Okänt"*, *"Avställd"*, *"Itrafik"*.

### Swedish glossary

Words you'll see throughout this system and should keep as-is:

| Swedish             | English                                                 |
| ------------------- | ------------------------------------------------------- |
| Fordon              | Vehicle                                                 |
| Besiktning          | (Periodic) inspection                                   |
| Försäkring          | Insurance                                               |
| Förare              | Driver                                                  |
| Ställa på / av      | Register / de-register with Transportstyrelsen          |
| Körjournal          | Driving log                                             |
| Felrapportering     | Fault reporting                                         |
| Trafikljusstatus    | Traffic-light status (red/yellow/green indicator)       |
| Kalender            | Calendar                                                |
| Budgettering        | Budgeting                                               |
| Avställd / Itrafik  | De-registered / In traffic (active)                     |
| Regbevis            | Registration certificate                                |
| Mina fordon         | My vehicles                                             |
| Mätarställning      | Odometer reading                                        |
| Logga in / ut       | Log in / out                                            |

---

## Visual foundations

### Color

- **One brand accent: forest green `#218E33`**, hover-darker `#1a7029`. Used
  for the logo wordmark, primary CTAs, active nav state, link hovers, status
  dots for "green" vehicles, the "Itrafik" badge bg-tint. Never a gradient —
  always solid fills.
- **Neutrals are Tailwind's gray scale** — `gray-50` page wash, `gray-100`
  strip/inner-header, `gray-200` borders, `gray-600` body, `gray-900`
  headings + the dark footer. No warm-cream or blue-tinted greys.
- **Status palette** is red / yellow / green / gray badges for vehicle
  status, with soft-tint bg + strong fg (e.g. `bg-red-100 text-red-800`).
  Never use the brand green AS a status green — `#22c55e` (Tailwind
  `green-500`) is the status shade; `#218E33` is reserved for brand.
- **Semantic accents** are sparing: `blue-600` for the Förare (driver) badge,
  `red-600` for Admin and destructive, `orange-600` for dev-only features.

### Type

- **Single stack, no custom webfont.** Tailwind default sans-serif stack
  (ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto…). We ship
  **Inter** from Google Fonts as the closest match inside this system.
  ⚠ **Substitution flagged:** if Fordonskollen later adopts a branded
  typeface, drop it into `fonts/` and swap the `--font-sans` value.
- **Scale.** Display `clamp(2.5rem, 5vw, 3.75rem)` extrabold for page heroes;
  `text-3xl`/`text-2xl` bold for section titles; `text-xl` bold for cards;
  `text-base`/`text-sm` for body; `text-xs` for captions. `tracking-tight`
  on all `font-extrabold` headings.
- **Weights used:** 400 / 500 / 600 / 700 / 800. Headings are 700–800,
  labels 500–600, body 400.

### Backgrounds & imagery

- **Photography is warm, everyday, lightly desaturated.** The marketing
  hero is a top-down shot of a parking lot with real vans and cars; dashboard
  / besiktning / förare / logistik are context photos in the same warm-flat
  register. No illustration-style renders, no glow, no 3D product shots.
  ⚠ A few marketing image filenames are prefixed `Claude_` — these were
  likely seeded by an AI pass; treat them as placeholders until the team
  ships final brand imagery.
- **One custom illustration** — `assets/regbevis_illustration.svg` — a simple
  line drawing of a Swedish registration certificate. Not decorative; used
  on the add-vehicle onboarding screen.
- **No repeating patterns, no texture, no noise.** No hand-drawn scribbles.
  Backgrounds are solid gray-50 or white.
- **Gradients are reserved for image→color transitions.** Marketing hero
  fades a photo into `rgba(249,250,251,1)` via a vertical linear-gradient at
  80–100%. The only other gradient is a bottom-of-video dim on the Om oss
  page. No brand or CTA gradients.

### Animation

- **Light, functional, Tailwind-grade transitions.** `transition-colors`,
  `transition-shadow`, `transition-all duration-150–300`. Never bouncy on UI.
- **Hover states** shift color (nav green, darker button bg) or shadow
  (`shadow-sm → shadow-md`). Feature cards on the home page **scale up to
  1.05** on hover (only place scale is used).
- **One deliberate novelty: the gooey search button** on the dashboard —
  a 5-second loop animation that flips between a magnifying-glass icon and
  the text "Klicka här för att filtrera", using a cubic-bezier ease-in-out.
  The auto-animate stops after first click. See `vehicle_list.ex` for the
  full @keyframes. Keep it; it's an intentional brand moment.
- **Scroll-to-top FAB** fades in after 300px scroll, fixed bottom-right,
  reverts to bottom-8 unless within 100px of footer (bumps to bottom-24).
- **No page-enter animations, no scroll-triggered parallax.**

### Shadows & elevation

Four levels, all cool-neutral (`rgba(0,0,0,…)`, no tinted shadows):

- `shadow-xs` — inline controls, inputs.
- `shadow-sm` — default card resting state. `0 1px 3px` blur.
- `shadow-md` — card hover, sticky header. `0 4px 6px`.
- `shadow-lg` — feature cards, CTA prominence. `0 10px 15px`.
- `shadow-xl` — rarely used (image section containers on Våra lösningar).

Cards **never** use an inner shadow. No glow. No colored shadow.

### Borders & corners

- `border border-gray-200` is the default card border. A hairline, always
  paired with a `rounded-*` and a `shadow-sm`.
- **Radius ladder.** `rounded-md` (6px) for small nav pills;
  `rounded-lg` (8px) for inputs, most buttons, status badges;
  `rounded-xl` (12px) for pricing cards, contact cards;
  `rounded-2xl` (16px) for hero feature cards, solutions image frames;
  `rounded-full` for circular avatars, social icons, status dots,
  the gooey search button.
- `rounded-sm` (4px) is used on one place only — the hero login CTAs
  "Logga in som företag" / "Logga in med BankID". Those buttons are
  intentionally squarer to feel more utility-form.

### Cards

The typical card composition:

```
bg-white
rounded-2xl  (or rounded-xl on compact cards)
border border-gray-200
shadow-sm   (hover → shadow-md)
p-6 to p-8
```

Cards **stack vertically** on mobile and **grid** at `md:` or `lg:`. Icon
wells inside cards are `w-12 h-12 rounded-full bg-[#218E33]/10` with a
6×6 brand-green Heroicon inside.

### Layout rules

- **Max widths.** `max-w-5xl` for marketing text sections, `max-w-6xl`
  for feature grids, `max-w-7xl` for header/footer, `max-w-4xl` for
  dashboard content, `max-w-xl` for hero column.
- **Horizontal padding.** Always `px-4 sm:px-6 lg:px-8`. Fixed.
- **Vertical rhythm.** Section padding `py-16` to `py-24` on marketing;
  `py-8` on app screens. Grid gaps `gap-8` on marketing, `gap-4` on app.
- **Sticky chrome.** Outer header `sticky top-0 z-50`; inner-app nav
  `sticky top-[57px] z-40`; cookie banner `fixed bottom-0 z-50`;
  scroll-to-top FAB `fixed bottom-8 right-8 z-40`.

### Transparency & blur

- **Top nav** uses `bg-white/95 backdrop-blur-sm` (marketing variant)
  — a softly translucent white. The authenticated outer header is
  solid `bg-white`.
- `bg-[#218E33]/10` and `/5` tinted wells for icon pills and small
  status backgrounds. Never more than ~15% opacity on tints.
- **Never** translucent cards, translucent modals, or glassmorphism
  beyond those two uses.

### Imagery tone

Photos are **warm-neutral, overcast natural light**, lots of greens
(trees, grass) balancing grey asphalt and truck bodies. Feels Nordic,
practical, a tiny bit cold. Not moody; not glossy. When the marketing
site blends photo → background, it fades into `#f9fafb` (gray-50).

---

## Iconography

- **Heroicons (outline).** Every icon in the product comes from
  [Heroicons](https://heroicons.com). The Phoenix `.icon/1` helper renders
  them by class name, e.g. `<.icon name="hero-magnifying-glass" />`. Stroke
  weight 2, outline style is default; the product occasionally reaches for
  `-solid` or `-mini` suffixes. Size is set via Tailwind (`size-4`, `h-5 w-5`,
  etc). To match in HTML we link Heroicons via inline SVG or the
  [unpkg CDN](https://unpkg.com/heroicons) — see individual UI kits.
- **Vehicle silhouettes.** Custom raster/SVG illustrations in
  `assets/vehicles/` (sedan, truck, van, trailer, tractor, bus, SUV,
  pickup, camper, motorcycle, hatchback, coupe, minivan, station wagon).
  Flat 2-tone fills, consistent side-profile, 40×40 box. Used in the
  vehicle list card thumbnail.
- **Regbevis illustration** — `assets/regbevis_illustration.svg`. One
  custom SVG for the add-vehicle onboarding. Line-art style.
- **Social icons** — Facebook, LinkedIn, Instagram — are inline SVG glyphs
  from their official brand marks, rendered inside a `w-9 h-9 rounded-full
  bg-gray-700` dark-footer chip.
- **Partner logo** — Biluppgifter is rendered as an **inline SVG recreation**
  of their orange tile + white "b" mark, not a raster file. ~32px tile.
- **No emoji.** None in the codebase.
- **No unicode glyph decoration.** `·` / `•` appear only in one or two legal
  strings. Avoid ✓ ✨ →.
- **No icon font.** Heroicons are bundled SVG sprites (via the Phoenix
  `heroicons` dep).

---

## Caveats & substitutions

- **Typeface substitution.** There is no custom webfont in the codebase;
  Tailwind's default system stack handles rendering. I've substituted
  **Inter** in this system as the closest free Google Font equivalent.
  If the brand adopts a specific typeface, drop it in `fonts/` and update
  `--font-sans` in `colors_and_type.css`.
- **AI-seeded imagery.** Several marketing photos are filename-prefixed
  `Claude_*` (team portraits, solutions hero etc.). They render as-is but
  the team may want to replace them with final brand shots.
- **BankID private login.** Commented out in the production code base
  (`<%!-- ... --%>`). Not recreated in the kit.
- **Dev-only nav items.** `Körjournal`, `Besiktning`, `Service`,
  `Släpvagnskalkylator`, `Appinfo` are gated `if env == :dev`. They ship
  as orange-accent items in the kit for completeness but note the flag.

---

## Using this system

Every file here is a plain static asset — no build step. Pull the CSS
variables in from `colors_and_type.css` and lift components out of the
UI kits under `ui_kits/*/`. If you're using this as an Agent Skill in
Claude Code, read `SKILL.md` first.
