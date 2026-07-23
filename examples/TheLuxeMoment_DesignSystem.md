# The Luxe Moment — Design System
**For use in Claude Design and any future design tooling**
**Source of truth: pulled directly from the live `style.css`, `index.html`, and `TheLuxeMoment_BrandVoice.md` — not invented or approximated.**

---

## Brand Summary

The Luxe Moment is a luxury **event styling company** (not a balloon company, not a party supply business) serving the Lehigh Valley, Pennsylvania. Founded by Jillissa Matthews. Full-service: design, sourcing, building, delivery, setup, breakdown, cleanup. The client shows up; Jillissa handles everything else.

Visual personality: quiet luxury. Warm neutrals, not stark black/white. Flat surfaces, no gradients, no glassmorphism, no drop shadows. Sharp/minimal corners (near-zero border-radius). Editorial serif display type paired with a clean sans body. Generous whitespace. Motion is restrained — a single eased transition, never competing effects.

---

## Color Palette

All colors are warm neutrals — no saturated brand color, no blue/purple accent. Contrast comes from value (light vs. dark), not hue.

| Token | Hex | Role |
|---|---|---|
| `--cream` | `#F1EBE1` | Primary light background, primary text-on-dark |
| `--bone` | `#E8E1D4` | Secondary light background (alternating sections) |
| `--stone` | `#C8B89C` | Mid-tone accent, hover states, dividers, swatches |
| `--taupe` | `#9C8A72` | Secondary accent, decorative use |
| `--pebble` | `#6B6258` | Muted body text on light backgrounds, secondary text on dark |
| `--ink` | `#1C1C1E` | Primary text on light backgrounds, primary button fill |
| `--noir` | `#111113` | Darkest background (dark sections, footer) |

**Usage pattern**: light sections alternate `--cream` and `--bone`; dark sections use `--noir` with `--bone`/`--stone` text. Buttons and headlines use `--ink`. Never introduce a hue outside this palette (no blue, green, purple, or bright accent color) — the palette is intentionally monochrome-neutral.

---

## Typography

| Role | Font | Stack |
|---|---|---|
| Display / headlines | **Italiana** | `'Italiana', 'Cormorant Garamond', serif` |
| Serif accents / italics / emphasis | **Cormorant Garamond** | `'Cormorant Garamond', serif` |
| Body / UI / labels | **Inter** | `'Inter', -apple-system, BlinkMacSystemFont, sans-serif` |

- All headings (`h1`–`h4`) use the serif family, weight 500, tight letter-spacing (`-0.01em`).
- `<em>` and italic emphasis always render in Cormorant Garamond italic — used for the single emotional "beat" line in a section (e.g. "You just show up. We handle everything else.").
- Body copy, nav, buttons, and eyebrow labels use Inter.

**Scale (fluid, via `clamp()`):**
- Hero H1: `clamp(2.6rem, 4.4vw + 0.8rem, 4.6rem)`, line-height 1.05
- Section H2: `clamp(2.1rem, 2.6vw + 1rem, 3.1rem)`, line-height 1.1
- Section description / lede paragraph: `1.02rem`–`1.15rem`, line-height ~1.65, color `--pebble`
- Eyebrow label (small caps overline): `0.72rem`, weight 600, letter-spacing `0.22em`, uppercase, color `--pebble`
- Button label: `0.78rem`, weight 600, letter-spacing `0.14em`, uppercase

---

## Spacing & Layout

- Max content width: `1240px`
- Page edge padding: `clamp(1.5rem, 5vw, 4rem)` (fluid, tighter on mobile)
- Section vertical padding: `clamp(4.5rem, 9vw, 7.5rem)` top and bottom
- Section header layout: two-column grid (`1.2fr 1fr`) pairing an eyebrow/label column with a description column, gap `3rem`
- Grids (services, gallery) use `1px` gaps with a translucent border color to create hairline dividers between cards, rather than card shadows or rounded panels

## Motion

- Easing curve used everywhere: `cubic-bezier(0.22, 1, 0.36, 1)`
- Standard transition length: `0.35s`
- One considered motion per element — e.g. a button's icon nudges on hover, a card's image scales 1.04x on hover. Never stack multiple competing effects on one element.
- `prefers-reduced-motion: reduce` is always respected — animations must degrade to instant/no motion, not just shorter motion.

---

## Components

### Buttons
- Flat fill, **1px solid border**, **1px border-radius** (effectively square corners — this is a deliberate brand signature, not an oversight)
- Uppercase label, letter-spacing `0.14em`, size `0.78rem`, weight 600
- **Primary**: `--ink` background, `--cream` text. Hover: background shifts to `--pebble`.
- **Ghost**: transparent background, `--ink` border + text. Hover: fills to `--ink` background, `--cream` text.
- **On-dark variants**: swap `--ink`/`--cream` roles so buttons stay legible on `--noir` sections.
- Icon (arrow) inside primary buttons nudges 3px right on hover/focus.

### Cards / Grid Items
- No drop shadows, no rounded corners, no card "container" styling — separation comes from hairline (1px) dividers between grid cells, using a translucent bone/cream border color.

### Section Backgrounds
Sections alternate between three background states to create rhythm down the page: light (`--cream`/`--bone`), dark (`--noir`), and back to light. Every section declares one of these explicitly — never left to default.

### Eyebrow Labels
Every major section and hero uses a small uppercase "eyebrow" label above its headline, in Inter, `--pebble` color, wide letter-spacing — this is the primary way section context is signaled, not icons or badges.

---

## Voice (for any AI-generated copy in Design)

Full detail lives in `TheLuxeMoment_BrandVoice.md` — summarized here for design-tool context:

- **Warm, confident, personal, direct, specific.** No corporate language, no filler.
- **Never use em dashes.** Full stop, no exceptions.
- Never use: cheap, affordable, budget-friendly, basic, "just balloons," "decorations" (say "installation" or "design"), simple, quick.
- Words that sound like the brand: vision, celebration, every detail, "I handle everything," "you just show up," "from design to breakdown," brought to life.
- Pronoun rule: first-person singular ("I/me/my") only in the founder's own About/bio copy. Everywhere else, "we/us/our."
- Never mention pricing or discounting in brand copy — pricing lives only as "starting at" anchors on a dedicated services/pricing surface.

---

## What NOT to introduce

- No purple/violet, no gradients, no gradient text, no glassmorphism
- No drop shadows or soft card elevation
- No large border-radius / pill shapes (this brand is sharp-cornered)
- No stock "AI luxury" tropes: fake stat blocks, generic "Why Choose Us" sections, emoji in headings
- No hue outside the warm-neutral palette above

---

*Generated from the live codebase, July 2026. If `style.css` changes, update this file to match — it should never drift from what's actually shipped.*
