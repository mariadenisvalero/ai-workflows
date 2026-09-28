# Polo — visual/build reference

For voice, tone, and story, see the `rl-brand-copy` skill's `polo.md` instead — this file is visual build spec only.

## Typography
- **Headline & body copy**: Le Jeune Deck Regular (licensed font — see `figma-workflow.md` for the loadable substitute)
- **CTAs**: Founders Grotesk Mono (licensed font — see `figma-workflow.md` for the loadable substitute)

## Colors
- Navy Blue Background: `#041E3A` (R4 G30 B58)
- Navy Blue Logo: `#00295C` (R0 G41 B92)
- Yellow: `#FFEA00` (R255 G234 B0)

## Logo & sub-lines
Always the Polo logo on top (over a flat color background or a texture — must stay readable either way). Three sub-audiences, each with its own header component in the design-system library (`02 Header/Polo`; check for a Kids-specific variant if one exists at build time):
- **Polo Kids** — note: kids under ~2 years old are considered babies for these emails, and babies go with the plain **RL logotype** instead of the Polo logo.
- **Polo Men**
- **Polo Women**

## Type scale
- H1 Headline: 60pt, line 60pt
- Large Body: 18pt, line 28pt
- Hero CTA: Founders Grotesk Mono 11pt, line 10pt — two style options (boxed frame, or plain underlined)
- H3 Title (titles for modules below the Hero): 28pt, line 40pt
- Secondary CTA: Founders Grotesk Mono 11pt, line 10pt, always underlined

## Sizes
- Desktop: email width 640px, content width 540px
- Mobile: overall width 376px, content width 296px

## Padding
- Hero CTA frame: 200px min, expands to 316px max; max 34 characters
- 40px padding below a CTA, 40px between multiple CTAs
- Padding is built into Primary modules already; if using a standalone image with no padding built in, slice the padding as white space under the image instead
- Secondary CTA underline matches the copy's text width exactly; 30px padding above and below

## Fixed brand modules (don't restructure, only recolor)
- **CYO (Create Your Own) module**: the picture must be Polos customized with a lockup or something different from a typical Polo; keep the copy close to the CTA (both directly below the picture) — never put copy above the picture and the CTA below it, unless specifically requested.
- **Social Module**: H3 "Follow Us" + a 2x3 grid of Polo-pony placeholder squares + social icon row + a closing line at 18px. Structural — don't change the grey squares' color or layout, only overall email color if needed.
- **Store Locator Module**: address block (Le Jeune Deck Medium for the brand name, Founders Grotesk Text for address/hours, Founders Grotesk Mono for the CTA) beside a map. Don't change the grey of the map; the pin color can be changed.
