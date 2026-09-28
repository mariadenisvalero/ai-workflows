# Luxe Brands (Purple Label / Collection) — visual/build reference

For voice, tone, and story, see the `rl-brand-copy` skill's `luxe-brands.md` instead — this file is visual build spec only. This is the one brand group that does NOT use Le Jeune Deck for headline/body. Don't default to the other three brands' spec here.

## Typography
- **Headline & body copy**: Founders Grotesk Text — **but the "Test - RLNA Email DS" library shows there are actually two Luxe typography tracks**, not one: `Lux Founders/...` and `Lux Sackers/...` (a font family we hadn't encountered before this library), each with their own full type-scale component set (01 H1 through 07 Eyebrow), and Sackers has an ALL CAPS variant on Body Large. Confirm with the user (or the brief) which track a given Luxe campaign wants before building — don't assume Founders by default now that a second real option exists. Neither is loadable via the plugin API; run the side-by-side substitute-font comparison in `figma-workflow.md` for whichever track is actually needed, separately.
- **CTAs**: Founders Grotesk Mono (licensed font — see `figma-workflow.md` for the loadable substitute, Space Mono)

## Colors
- RL Black: `#221F20` (R34 G31 B32)

## Logo & sub-lines
Check the marketing brief for which logo goes on top (both exist as their own header components in the library):
- **Purple Label** — "RALPH LAUREN / PURPLE LABEL" (`02 Header/Purple Label`)
- **Collection** — "RALPH LAUREN / COLLECTION" (`02 Header/Collection`)

## Product naming (Collection specifically)
`[STYLE NUMBER] [COLOR] [SURFACE TREATMENT if any] [FABRIC, exact content] [ITEM NAME if applicable] [SILHOUETTE] [PRICE]` — e.g. *"790750633001 CLASSIC CHAIRMAN NAVY/GREY MÉLANGE EMBROIDERED WOOL-ELASTANE WARRINGTON COAT $5,995."* Multi-hue colors use `/` with no spaces (`BLACK/RED`); "Polymide" is written as NYLON; polyester is written as LUXURY POLYESTER (the one approved "luxury" exception — it's a fiber-content term, not marketing copy); items are always singular (TROUSER not TROUSERS); price has no `.00` and no `USD` (`$2,500`); always include the legal disclaimer at the end.

## Type scale (all smaller than the other three brands)
- H1 Headline: 40pt, line 51pt
- Large Body: 18pt, line 28pt
- Hero CTA: Founders Grotesk Mono 11pt, line 10pt — **underlined only, no boxed option** (unlike Polo/Lauren/Home which offer both)
- H2 Titles (titles for modules below the Hero): 32pt, line 42pt
- Secondary CTA: Founders Grotesk Mono 11pt, line 10pt, always underlined

## Sizes
Same as every other brand:
- Desktop: email width 640px, content width 540px
- Mobile: overall width 376px, content width 296px

## Padding — the one place Luxe Brands differs structurally
- **Extra 20px is always added above the Hero copy and above every new module** — this is unique to Luxe Brands, none of the other three brands do this.
- Hero CTA frame: 200px min, expands to 316px max; max 34 characters
- **30px** padding below a CTA, **30px** between multiple CTAs (the other three brands use 40px here — don't copy that number over)
- Secondary CTA underline matches the copy's text width exactly; 30px padding above and below

## Module library variant note
Every module component in `04 Modules` carries a `Core/Lux` variant axis (and some also show a third `Core/Lux/Sackers` option — see `02 Secondary/Left` and `Right`) — always select the Lux (or Lux/Sackers) variant for these two brands, never the Core default, or the module will render in the wrong typography/spacing entirely.
