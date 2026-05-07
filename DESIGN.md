# Shemesh — Design System

> This is the visual decisions document. Filled in collaboratively as the design solidifies. The job of this file is to lock decisions so they don't drift session to session.
>
> **Status:** Brainstorm complete 2026-05-06. All decisions locked. Open input from Avihay: list of pension companies + their Hebrew names + logos for the trust ticker.

## Aesthetic Family

> One or two named references that capture the desired feel. Be specific. "Modern and clean" is useless; "Stripe Press editorial crossed with Tel Aviv signage typography" is useful.

**LOCKED 2026-05-06:** *Tel Aviv shuk-sign warmth + editorial-asymmetric scale + illustration/texture art layer.*

Three threads woven together:

1. **Base — hand-painted Israeli signage.** Warm yellow→ochre→burnt-orange dominant. Ink-black type at heroic sizes. Painterly, confident, anti-corporate. References: Carmel Market hand-painted signs, Israeli folk-modern posters, kibbutz silkscreens.
2. **Composition — editorial-agency scale.** Asymmetric modular grid, generous whitespace, full-bleed and text-heavy sections alternating, type-as-hierarchy at display scale. Reference: [rpacomunicacion.com](https://rpacomunicacion.com/).
3. **Art layer — illustration + texture.** Hand-drawn sun-motif fragments (rays, arcs, halos), dithering/grain overlays for atmosphere instead of CSS gradients, sharp restrained accent colors. Reference: [patternbreak.ing](https://patternbreak.ing/).

**Hard rules pulled from these references:**
- No CSS gradient hero. Use solid color blocks + texture (dither/grain).
- No emoji icons anywhere — replaced by hand-drawn SVG illustration.
- No symmetrical "centered hero." Type and image break the grid intentionally.
- Color is committed: one warm dominant (sun) + ink + one sharp accent. No pastel evenness.

**Anti-rule:** The current site (`#1D3D6B` navy + `#F5A623` gold + emojis + CSS gradients + pulsing CTA) is exactly what we are escaping.

## Typography

> Pick one for display, one for body. Commit. Don't switch fonts mid-redesign.

**LOCKED 2026-05-06:** Two-font Google Fonts system — Karantina (display) + Assistant (body). Used across all three languages (Hebrew / Arabic / English) — both fonts have full Latin and good Hebrew coverage. Arabic still routes to Noto Kufi Arabic (existing rule).

| Role | Font | Weights to load |
|---|---|---|
| Hebrew display (hero, section titles) | **Karantina** | 700, 900 (Black for hero, Bold for sections) |
| Hebrew body | **Assistant** | 400, 600, 700 |
| Hebrew tabular numerals (form fields, IDs, dates, stats) | **Assistant** with `font-variant-numeric: tabular-nums` | 400, 600 |
| Latin display | **Karantina** Latin glyphs | 700, 900 |
| Latin body | **Assistant** Latin glyphs | 400, 600 |
| Arabic body/display | **Noto Kufi Arabic** (existing) | 400, 600, 700 |

**Sizes (mobile / desktop):**
- Hero display (Karantina 900): `clamp(48px, 12vw, 140px)` — heroic poster scale.
- Section title (Karantina 700): `clamp(32px, 6vw, 64px)`.
- Sub-display / pull quote (Karantina 700): `28px / 40px`.
- Body large (Assistant 400): `18px / 20px`.
- Body (Assistant 400): `16px / 17px`.
- Caption / form labels (Assistant 600): `14px`.

**Anti-rules:**
- No Inter, Roboto, Arial, or system fonts.
- No "Hebrew fallback of a Latin font" — Hebrew gets first-class typography. Karantina IS Hebrew-first.
- No mixing more than these three families. Don't reach for a fourth even for "just one heading."
- Numerals in form fields use tabular variant so the form doesn't shift width as users type.

## Color Tokens

> CSS variables. Commit to dominant + accent. No evenly-distributed palettes.

**LOCKED 2026-05-06:** "Sun on paper" — committed warm-dominant palette, ink type, ember as sharp single-purpose accent.

```css
:root {
  --bg-paper:      #F2EAD3;  /* page background — sun-bleached parchment */
  --bg-paper-deep: #E8DDC0;  /* cards, alt sections */
  --ink:           #19140C;  /* primary type, rules, borders (warm near-black) */
  --ink-muted:     #6B5C45;  /* secondary type */
  --sun:           #F5B82E;  /* dominant brand accent — the literal sun */
  --sun-deep:      #D49216;  /* hover/pressed for sun elements */
  --ember:         #C4501C;  /* sharp accent — CTAs, success peak, "one memorable thing". Used sparingly. */
  --success:       #3F7A3F;  /* form success state */
  --error:         #A8261A;  /* form error state */
}
```

**Anti-rules:**
- No purple, no navy, no grey-blue. The only cool note in the system is the muted-ink (which is brown, not blue).
- No CSS gradient as fill. Texture is dither/grain, not gradient.
- Sun is dominant; ember is the sharp accent. **Nothing else gets to be a third color.**
- Pure `#000` is forbidden — use `--ink` so type sits inside the warm family.

## Spacing Scale

> Commit to a scale. Mixing arbitrary pixel values is what makes layouts feel sloppy.

**LOCKED 2026-05-06:** 4px base, exponential rhythm.

```css
:root {
  --space-1:  4px;   /* tight, inline */
  --space-2:  8px;
  --space-3: 16px;
  --space-4: 32px;
  --space-5: 56px;   /* section padding */
  --space-6: 96px;   /* major section gap */
}
```

Use only these tokens. Mixing arbitrary px values is the fastest way to make the page feel sloppy — if you need a value not on the scale, ask whether the scale is wrong, not the case.

## Motion Principles

> One well-orchestrated moment beats scattered micro-interactions.

**LOCKED 2026-05-06:** *one peak — form submit. Everything else is restrained.*

- **The peak — form submit (~1.4s total):** when `successMsg` reveals, hand-drawn sun-rays radiate outward from the form card; `--ember` flashes once on the success icon; success copy fades in. No spinner, no progress bar. This is the emotional payoff and the page's signature motion moment.
- **Page load:** content appears, period. No staggered reveals, no fade-ups, no type-in animations. Confidence beats choreography.
- **Pension ticker:** continuous slow RTL marquee, ~40s/loop. Pauses on `:hover` and `touchstart`; resumes on `touchend`.
- **Hover states:** restrained — 100ms color or border-color shift only. **No transforms** (no translate-Y on cards, no scale on buttons).
- **Scroll-triggered animations:** none. Scroll-fade-in is anti-slop default; we skip it.
- **Reduced motion (`prefers-reduced-motion: reduce`):** kills the form-submit ray animation (success state appears instantly), freezes the ticker (becomes a static row), keeps hover transitions.

## RTL-Specific Decisions

- Form labels right-aligned ✓ (Hebrew default)
- Icons with directional meaning (arrows, carets) flipped horizontally
- Phone input: LTR inside an RTL container — wrap in `dir="ltr"` and test in Safari iOS
- ID input: LTR for digit entry, but parsing is RTL-safe
- Dates: order is DD/MM/YYYY in Hebrew context, not MM/DD/YYYY
- Logo placement: aesthetic decision, but RTL convention is right-side anchor

## Required Components

> Locked-in structural requirements. Distinct from "section order" — these are non-negotiable elements wherever they end up.

### Pension-companies trust ticker (LOCKED 2026-05-06)

- Horizontal marquee scrolling **right-to-left** (natural Hebrew reading direction).
- Shows the **logo + Hebrew name** of every Israeli pension company Shemesh works with.
- **Touchable but NOT clickable on mobile.** Touch interactions allowed (drag/scrub to slow or pause); a tap does not navigate anywhere. No `<a href>` wrapping the items — these are not endorsements or affiliate links.
- Implementation notes:
  - CSS `@keyframes` translateX marquee, duplicated track for seamless loop.
  - Pause animation on `:hover` and on `touchstart`. Resume on `touchend`.
  - Respect `prefers-reduced-motion: reduce` — switch to a slow auto-paginate or static grid in that case.
  - Logos in greyscale or single-color treatment so the warm palette stays dominant; full color reserved for the brand.
  - Pension company list TBD — Avihay to provide.

## Spatial Composition

> How is the page composed? Symmetrical or asymmetrical? Grid-strict or grid-breaking?

**LOCKED 2026-05-06:** asymmetric, grid-breaking, editorial-agency scale per the [rpacomunicacion.com](https://rpacomunicacion.com/) reference. Type at heroic display sizes, generous whitespace, full-bleed sections alternating with text-heavy zones. **No** centered-hero / 3-col-features / footer template.

| Section | Treatment |
|---|---|
| Hero | Asymmetric. Karantina display title anchors **right** (RTL). Oversized — `clamp(48px, 12vw, 140px)`. Hand-drawn sun illustration breaks out of the right margin. Sub-line and CTA float left with generous whitespace. **No gradient. No wave SVG separator.** |
| Pension ticker | Strip directly under hero. RTL marquee, logo + Hebrew name, ink-on-paper, no card chrome. Touchable, not clickable. |
| How it works (3 steps) | Editorial: massive numerals (Karantina 900) at left of each step, hand-drawn arc/ray illustration mid-card, copy at right. Off-grid — steps stagger vertically by ~40px. Replaces the emoji-card grid. |
| Why us (5 cards) | Asymmetric magazine layout, **not** a uniform grid. One card double-width as the lead, the other four sit around it. Each card carries a small hand-drawn sun-fragment instead of an emoji. |
| Form | Destination block. Paper card on `--bg-paper-deep` section. Form-submit triggers the orchestrated motion peak. |
| Footer | Minimal: one line of legal + Facebook icon + copyright. Deliberately small — the form is the closer, not the footer. |

## Backgrounds & Visual Details

> Atmosphere comes from texture, not solid colors.

**LOCKED 2026-05-06:** two atmospheric layers, no more.

1. **Subtle paper grain** — SVG `<feTurbulence>` filter rendered inline, applied to `--bg-paper` at low opacity (~0.06). Gives the parchment authority without an image asset. No build step required.
2. **Sun motif as illustration system** — hand-drawn SVGs (rays, half-arcs, dotted halos), reused across hero, step cards, why-us cards, and the form-submit reveal. One asset family, recoloured between `--ink`, `--sun`, and `--ember` depending on context.

**Explicitly excluded:**
- ❌ Gradient meshes (anti-rule)
- ❌ Geometric patterns or Hebrew letterform textures (over-precious)
- ❌ Custom cursor (gimmick)
- ❌ Layered transparencies on the hero (we're using solid color blocks + grain)

## The "One Memorable Thing"

> What's the single detail someone will remember after closing the tab?

**LOCKED 2026-05-06:** *the form-submit sun-ray reveal.*

When the form submits successfully, hand-drawn sun-rays radiate outward from the form card; the success icon flashes once in `--ember`; the success copy fades in. ~1.4s, one moment, no spinner.

Why this specifically:
- The whole site builds toward submitting the form (claiming the money owed) — that's the emotional peak, so that's where the motion goes.
- It's the only place in the design that uses motion with intent.
- It makes the brand name (sun) literal at exactly the right beat. The user's own action *makes the sun appear*.
- It's non-trivial to copy. Most landing pages do nothing on form submit; some do confetti (cliché). Sun-rays radiating from the form, in the brand's exact yellow, on a parchment background, is unique to Shemesh and impossible to mistake for a template.

Everything else stays restrained so this moment can land.

## What This Design Is NOT

> Define by exclusion as well as inclusion. Saying "it's not X" prevents drift.

**LOCKED 2026-05-06:**

- **NOT a navy-corporate-financial-services template.** No `#1D3D6B`. No "trustworthy bank blue." Shemesh is specifically *not* the bank — it's the friend who knows the system.
- **NOT "modern minimalist" / Aesop-lite.** Restraint without warmth reads as cold. The base palette is warm parchment, not white-on-white.
- **NOT Apple-knockoff.** No oversized hero photography of nothing. No glassy navbar.
- **NOT an emoji-driven feature list.** Every emoji on the current site is being deleted. Hand-drawn SVG illustration replaces them.
- **NOT a CSS-gradient party.** No gradient on hero, no gradient on buttons, no gradient on cards, no gradient overlays. Solid color + paper grain only.
- **NOT glassmorphism / frosted-glass / blur-backdrop.** Period.
- **NOT trust-theatre stock photography** (handshakes, smiling agents, briefcases). Visual trust comes from typography, the pension-ticker, and craft — not stock images.
- **NOT "festive" with confetti / ribbons / sparkles** on form submit. The sun-ray reveal is the whole moment.
- **NOT every-effect-I-knew.** Pulse animations, fade-ins on scroll, hover-translate, shadow-lift — all gone. One orchestrated peak, restraint everywhere else.

## Decision Log

> Append decisions as they're made so future sessions can see the chain.

- `[2026-04-XX]` — Initial skeleton created. Values pending brainstorm phase.
- `[2026-05-06]` — Aesthetic family locked: Tel Aviv shuk-sign warmth + editorial-asymmetric scale (rpacomunicacion.com) + illustration/texture art layer (patternbreak.ing). Hard rules: no CSS gradient hero, no emoji icons, no symmetrical centered hero, committed warm-dominant palette.
- `[2026-05-06]` — Pension-companies trust ticker locked as a required component: RTL marquee, logo + Hebrew name, touchable but not clickable on mobile.
- `[2026-05-06]` — Phone format ground truth confirmed: webhook receives `0500000000` (10 digits, no dash). CLAUDE.md updated to match reality.
- `[2026-05-06]` — Typography locked: Karantina (display) + Assistant (body). Two-font Google Fonts system across all three languages.
- `[2026-05-06]` — Color palette locked: "Sun on paper". `--bg-paper #F2EAD3`, `--ink #19140C`, `--sun #F5B82E` dominant, `--ember #C4501C` sharp accent.
- `[2026-05-06]` — Spacing scale locked: 4px base — 4 / 8 / 16 / 32 / 56 / 96.
- `[2026-05-06]` — Motion principles locked: one peak (form-submit sun-ray reveal, ~1.4s); everything else restrained. No scroll-triggered fades, no card-hover transforms.
- `[2026-05-06]` — Spatial composition locked: asymmetric editorial scale per rpa reference. Hero anchors right with sun illustration breaking out; pension ticker under hero; how-it-works as off-grid editorial; why-us as asymmetric magazine layout; form is the closer; minimal footer.
- `[2026-05-06]` — Atmosphere locked: SVG paper grain + hand-drawn sun-motif illustration system. Only two atmospheric layers.
- `[2026-05-06]` — "One memorable thing" locked: form-submit sun-ray reveal. The site's only orchestrated motion moment.
- `[2026-05-06]` — "What this is NOT" locked: explicit exclusions of navy-corporate, Apple-minimalist, emoji-driven, gradient-everywhere, glassmorphism, trust-theatre stock photography, and effect-pile-on patterns.
