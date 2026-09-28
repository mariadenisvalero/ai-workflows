# Module library — "Test - RLNA Email DS"

This is the real, current source for assembly — a proper component library, not the older fixed-template file. Verified live against `https://www.figma.com/design/TekSLtkllJYAUYZ4ZIFHIQ/Test---RLNA-Email-DS`. If the user gives a different library URL later, re-verify this map before trusting it — library structure is not guaranteed stable across versions.

## Pages
- **01 Header Footer Elements** — one header component per brand/sub-line (`02 Header/Polo`, `02 Header/Home`, `02 Header/Ralph Lauren`, `02 Header/Lauren`, `02 Header/Lauren Petite`, `02 Header/Lauren Woman`, `02 Header/Purple Label`, `02 Header/Collection`, `02 Header/RRL`, plus special-campaign ones like Wimbledon/US Open/Pink Pony/RLX/Baby), navigation/footer components, and fixed banners (Free Shipping, Store Locator, Core App).
- **02 Copy Block** — text style components per tier: `Core/01 H1` through `07 Eyebrow`, and in parallel `Lux Founders/...` and `Lux Sackers/...` for the two Luxe Brands typography tracks (see `luxe-brands.md`). Editing the text inside these still requires the font-substitution technique in `figma-workflow.md` — having a styled component doesn't bypass the plugin's font-loading requirement when you change the characters.
- **03 CTAs** — `01 Primary` (Hero CTA — variants include a `2nd CTA=YES/NO` toggle for a two-button Hero), `02 Secondary`, `03 Tertiary` (both have `CTAs=1/2/3/4...` variants for modules that need more than one button in different layouts — Half Stack, Scale, Stack, 2x2, 1x4).
- **04 Modules** — the actual content modules, detailed below.
- **05 Resources** — campaign-specific one-offs (e.g. an MLB collab); not general-purpose.
- **07 Icons** — social/UI icons.
- **08 Templates Emails complete** — whole pre-assembled emails per brand (a `RLE EMAIL` section holding one COMPONENT per brand/theme: `Virtual Experience`, `CYO` in three variants, `Home Template - Holidays`, `LUX Template - Holidays`, `Purple Label Template - Holidays`, `Collection Template - Holidays`, `Lauren Template - Light`). **Check this page before composing an email module-by-module** — see the dedicated section below.

## Check "08 Templates Emails complete" before building module-by-module

This page has **whole emails already assembled** — logo, Hero, multiple modules, and CTAs all nested together as a single importable component, not just individual pieces from `04 Modules`. A `Dark` variant of the Lauren template likely also exists (matching the Light/Dark axis seen elsewhere in this library) — confirm live rather than assume, and re-check this page's contents each time, since it's the newest, least-explored part of the library and may grow.

**Workflow — one quick pass, then decide, don't linger here**: call `get_metadata` on this page ONCE per email and match by name/brand/theme against the Email Plan's brand and module count/shape (Virtual Experience, CYO ×3, Home/LUX/Purple Label/Collection Holidays, Lauren Light/Dark). This is a name-and-shape match, not a visual audit — don't screenshot or open multiple candidates to compare before deciding.
- Clear match on name+shape: import that ONE component, detach it (same rule as any other module — detach before editing, never after), then swap in the real images, real copy, and real palette module by module within it — much less assembly work than building from scratch, since the logo/Hero/module/CTA structure and spacing is already correct.
- No clear match within that one pass (wrong module count, wrong shape, or only a themed variant like "Holidays" exists for a non-holiday campaign): stop looking and fall back to composing from `04 Modules` as documented below immediately — don't keep scanning for a better fit that probably isn't there.
- Either way, still apply every rule in this file: the font substitution table, the detach-before-edit rule, the stroke sweep, the 788px Hero-image minimum, the `x:181, y:223` placement, and the logo-color-matches-background rule. A pre-assembled template doesn't exempt any of these — it just means the structure is already there instead of being built piece by piece.
- These complete templates skew toward specific seasonal themes (several are literally named "Holidays") — treat the visual theming (not just the module structure) as a starting point to adjust, not something to ship as-is for an unrelated campaign.

## Modules (page "04 Modules")
- **`01 Primary/Primary`** — the Hero. Variants: `DSK/MOB`, `Alignment` (Center/Left/Right), `Light/Dark`, `Core/Lux`. Contains an eyebrow, H1, and body text, with an image layer behind/above. Base variant is full-bleed image (0,0, fills the module).
- **`01 Primary/Text Lock Up + BG`** — the Hero's text block on its own, same variant axes; used when the text lockup needs to be handled separately from the image (e.g. the BG-image + HERO-image two-layer pattern documented in `figma-workflow.md`).
- **`02 Secondary/Left`** and **`02 Secondary/Right`** — single-image module with its own built-in eyebrow, H3 headline, body, and CTA button(s) already in the instance (confirmed by inspection: found `EYEBROW`, `H3 Headline`, body text, and `Button Text` nodes already present). Variants: `DSK/MOB`, `Light/Dark`, `Core/Lux` (and a third `Core/Lux/Sackers` option on some), `Version` (1/2 — different CTA arrangement). Left vs Right controls which side the image sits on.
- **`03 Tertiary/2 Images`** — two images, `Vertical/Horizontal` property. This is the module to use for a **CTL-style "Complete the Look"** side-by-side pair.
- **`03 Tertiary/2 Images - Alternating`** — two images with an `Inset` property (`Inset Left`/`Inset Right`) — a formal variant-driven framed/inset look, unlike most other modules where framing is manual (see below).
- **`03 Tertiary/3 Images`** — three images, has a `Vertical` property.
- **`03 Tertiary/4 Images`** — four images.
- **`04 Image Elements/1 Image`** — a single standalone image, no text — for a pure visual break or a laydown shot with no copy.

## Fixed footer pair: Core App Banner + Navigation Footer — sourced from the TEMPLATE file itself, not the component library

Every email in a three-email set (Teaser, Product Focus, and Reminder alike) ends with the same fixed pair, stacked banner-then-footer — no copy to write for either, no per-email variation:
- **`03 Banner/Core App Banner`**
- **`01 Navigation Footer VWAP/Footer`**

**Confirmed live (2026-09-28):** these are staged as ready-to-clone instances inside a section named **"Footer Emails"** in the *campaign template file itself* (`BiBGFqn3Eqy54ieUUncxZX`, the same file `rl-campaign-scaffolding` duplicates sections in) — **not** in the separate "Test - RLNA Email DS" component library file. Source them with a same-file `findOne`/`.clone()` against that section, the same way the master section itself is found by name, rather than a cross-file `importComponentByKeyAsync` — this avoids the cross-file publish-lag gotcha entirely for these two:
```js
const footerSection = page.findOne(n => n.type === 'SECTION' && n.name === 'Footer Emails');
const banner = footerSection.findOne(n => n.name === '03 Banner/Core App Banner').clone();
const footer = footerSection.findOne(n => n.name === '01 Navigation Footer VWAP/Footer').clone();
```
Place the banner clone first (directly under the last real module), then the footer clone directly under that — matching their relative stacking in the "Footer Emails" section (banner above, footer below). If the user stages more components in that same "Footer Emails" section (or a similarly-named staging section) later — headers, Hero, CTAs, other modules — prefer sourcing those same-file too instead of cross-file import; re-verify live whichever section holds them, don't assume this exact name/location stays fixed forever.

## Critical gotcha: DSK/MOB naming is NOT consistent across component sets
Verified directly: in `04 Modules`, `DSK/MOB=False` = desktop (640px wide), `DSK/MOB=True` = mobile (376px wide). But in `03 CTAs/01 Primary`, it's inverted: `DSK/MOB=yes` = mobile (376px), `DSK/MOB=no` = desktop (640px). **Never trust the variant label text alone — always check the actual component's `width` (640 = desktop, 376 = mobile) before picking a variant**, every single time, for every component set. This cost real debugging time once already.

## Known gotcha: newly-published components can lag behind the library index
As of this session, `02 Secondary` (both Left and Right) failed `importComponentByKeyAsync` with "component not found" — three isolated retries, consistent failure — while `01 Primary`, `03 CTAs/01 Primary`, and the header components imported fine on the first try, in the same library, at the same time. This points to a per-component-set publish/propagation lag, not a scripting error. **Before assuming a component is unusable, retry the import in its own isolated call** (see the next section — don't batch multiple component imports in one script) — and if it still fails, tell the user which specific component set is affected so they can check its publish status directly in Figma, rather than silently falling back to manual construction.

## Critical: detach nested-instance modules BEFORE editing, never after

Confirmed by direct testing on `01 Primary/Primary` (Hero) and `02 Secondary/Left`/`Right`: while a module is still a live component INSTANCE, some of its nested children (the `Image Ratio` image layer, and sometimes an eyebrow text layer) don't reliably show up in `findAll`/`children` — they only become visible and addressable after calling `.detachInstance()` on the module. Separately, `upload_assets` flatly rejects nested-instance-path node ids (`I52:686;548:907` format) — it only accepts plain `123:456` ids, which nested instance children don't have until the module is detached.

**This means: detach every module immediately after creating it, before touching any text or fill on it — never edit an instance's children first and detach afterward.** Confirmed the hard way: editing text/colors on a live instance's nested children, then calling `.detachInstance()` on the parent, **silently reverts those edits back to the component's published defaults** (variable-bound fills in particular reset to their bound default; some text content survives, some doesn't — don't rely on the split, treat it as a full reset). The safe sequence is:
```js
const instance = comp.createInstance();
page.appendChild(instance);
const detached = instance.detachInstance(); // do this immediately, before any edits
// now find image/text nodes on `detached` and edit freely — ids are stable from here on
```
If a module was already edited-then-detached and looks reset (blank white background, placeholder text creeping back in), don't fight it — just re-run the text/fill edits fresh against the current (now-stable) detached structure; the ids will have changed from the pre-detach ones.

## Final assembly placement and polish (confirmed requirements, not optional)

- **Position the assembled frame at `x: 543, y: 216`** inside its matching `Email design 1`/`Email design 2`/`Email design 3` target frame (Teaser → `Email design 1`, Product Focus → `Email design 2`, Reminder → `Email design 3`) — not `(0,0)`. This is a fixed visual-padding requirement, not a style choice, confirmed live against the "EMAIL MONTH FY27 - TEMPLATE" file's new three-email-per-section structure (2026-09-28); it replaces the older `x: 181, y: 223` / `V1` placement rule from the single-email template.
- **Hero image minimum height: 788px.** The base component's `Image Ratio` defaults to 240px tall — always resize it to at least 788 (resize the width-640-tall-240 frame to 640×788 or taller). The parent's vertical auto-layout recalculates the total module height automatically once the image is resized — don't manually recompute it.
- **Convert the final assembled frame to vertical auto-layout** (`layoutMode = 'VERTICAL'`, `itemSpacing = 0`, `primaryAxisSizingMode = 'AUTO'`, `counterAxisSizingMode = 'FIXED'`) once every module is placed — this keeps modules stacked correctly (no manual y-math needed) and means a later height change (like the Hero resize above) propagates automatically instead of leaving gaps or overlaps.
- **Check strokes, not just fills, for every CTA button and underline — and fold the header into the SAME sweep, don't check it separately.** Confirmed repeatedly: `Polo Button` instances and their `Line 1` underline children carry their own `strokes` array (default navy) completely independent of the text/fill colors already edited — recoloring the fill and text is not enough, the button border and the "View All"-style underline will still show the old color unless `.strokes` is set too. The header's background/logo-color rule (above) is easy to forget precisely because it lives in a separate step — build it into the same programmatic check instead of relying on remembering it:
  ```js
  const offPalette = wrapper.findAll(n => n.strokes && n.strokes.length > 0 &&
    n.strokes.some(s => /* compare against the email's chosen palette colors */));
  const headerFill = wrapper.children[0].fills; // header is always the first child
  // assert headerFill matches the palette background color too, not just strokes elsewhere
  ```
  Run this as the actual final step, right before grouping/placement — a rule that's only written down is easy to skip under time pressure; a rule that's also checked in code isn't.

- **Header/logo color must match the header's own background — this is a hard rule, not a suggestion, and it's checked in the same final sweep as strokes (below), not just remembered.** If the header block's background is dark (e.g. the email's palette background color), use the header component's **white** logo variant; if the header's background is light/white, use the **navy** (or brand-default) logo variant. `02 Header/<brand>` is published as a component set with a `Logo Color` variant axis (Navy/Black/White/Yellow seen for Polo) — swap to the correct variant by importing that variant's own key, rather than trying to recolor a Navy instance's logo artwork directly (it's a lockup graphic, not simple flat text). When the email's palette (per `color-palette.md`) applies a dark background to every other module, apply that same background to the header instance too and swap it to the white-logo variant — the header is one of the email's non-photo blocks and follows the same palette consistency rule as everything else, it doesn't get to stay a separate white slab while the rest of the email is on-palette. This exact miss happened once already (header left on its default white/navy while the rest of a Polo email went to a derived brown palette) — that's why it's now part of the automated sweep, not a step to remember on its own.
  ```js
  const comp = await figma.importComponentByKeyAsync('<white-logo variant key>');
  const newHeader = comp.createInstance();
  newHeader.fills = [{ type: 'SOLID', color: /* the email's background color */ }];
  wrapper.insertChild(indexOfOldHeader, newHeader);
  oldHeader.remove();
  ```

## Critical: import one component per tool call, not several in a loop
Confirmed by direct testing — and this generalizes beyond imports, see the box below: if a script imports and places several components in sequence and a **later** one throws, the **earlier** ones do not persist either — the whole script's effects were lost, not just the failing step. Always import + place one component per tool call. Check each result before moving to the next.

## Critical: this holds for ANY multi-step script, not just imports
Re-confirmed with a plain property edit: a script that sets several text nodes' `.characters` in sequence, where a **later** line throws, loses the **earlier** edits too — verified by re-reading the first node afterward and finding it still showed its original placeholder text. Treat every script here as all-or-nothing: if there's any risk a later line throws, either split into separate calls, or wrap the risky part in try/catch so the safe edits aren't lost with it.

## Critical: never set `.visible = false` on a text node inside a component instance
Confirmed by direct testing: doing this can cause the node to be **pruned entirely** — `getNodeByIdAsync` returns null for that id afterward, and it disappears from the instance's `findAll` results. This looks like the instance's internal auto-layout removing a hidden, zero-content child rather than just hiding it. **Don't use `.visible = false` to "turn off" a headline or an extra CTA in these modules.** If a module needs to look lighter (e.g. a brief says "disable the headline, body copy only"), give that text node short, light content instead of trying to hide it — the visual weight goal is met without the structural risk. If a module truly has an unwanted extra element (e.g. a second CTA the brief didn't ask for) and hiding it matters, flag it to the user rather than risking corrupting the instance — this hasn't been solved safely yet.

## Body/eyebrow copy font — a second licensed font beyond Le Jeune Deck/Founders Grotesk Mono
Confirmed by inspection: inside `02 Secondary`'s eyebrow and body text, the font is **Founders Grotesk Text** (not Le Jeune Deck — Le Jeune Deck is only the larger headline styles). This is a third licensed font needing substitution, distinct from the Luxe-only Founders Grotesk Text mentioned in `luxe-brands.md` — it turns out Core-tier modules use it too, just for smaller text roles. **Confirmed standing substitute: Space Grotesk** (pairs naturally with Space Mono — same designer/family line, a coherent substitute pairing rather than two unrelated fonts). Apply it directly in an unattended run — see `figma-workflow.md`'s font-substitution section for why the side-by-side comparison step is skipped during routine runs.

## Two-layer image modules: background frame + main photo frame, in that order

Confirmed with real examples (both from `08 Templates Emails complete` and matching production use): many modules — not just Hero — are built from **two separate image-fill frames stacked**, not one. An outer/larger **background frame** (a texture, a color-blocked photo, or a contextual shot — e.g. a stucco wall with dappled light, a knit-fabric close-up) fills the module's full bounds, and a smaller **main photo frame** sits inset on top of it with a cream/white mat border, holding the actual model/product shot. Named variously across templates — `Image BG` / `LOGO + HERO IMG` → nested `Image HERO`, or `BG` / `04 Image Elements/1 Image` nested inside — don't rely on the exact name, rely on the structure: look for two frames with `IMAGE`-capable fills, where one is at or near the module's full width/height and the other is smaller and inset within it.

**Build order matters: fill the background frame first, then the main photo frame second.** This isn't just a preference — it's the order that matches how these templates are actually constructed, and filling out of order risks targeting the wrong frame when both are still showing empty/checkered placeholders and look similar in the layers panel.

## Critical: an image frame must never be left empty — and how to confirm it actually rendered

**Hard rule (no exceptions): every `IMAGE`-fill frame in an assembled module — background/texture frame and main photo frame alike — must end the run showing a real, rendering photo.** If the true intended asset can't be uploaded (Drive download fails, "session expired", file too large, timeout, whatever the cause), **do not leave the frame empty or broken.** Reuse the `imageHash` of another real photo already confirmed rendering elsewhere in this same email — even if that means the same photo appears twice in one email. A duplicate real photo is always an acceptable fallback; an empty, checkered, or broken image frame is never acceptable, and is not something to flag-and-move-on from — fix it before calling the module done.

```js
// Reusing an already-confirmed-good imageHash as a fallback — no re-upload needed,
// this is a legitimate, documented use of imageHash (see api-reference.md)
node.fills = [{ type: 'IMAGE', scaleMode: 'FILL', imageHash: '<hash from a node already confirmed rendering>' }];
```

**`fills[0].imageHash` being non-null is NOT proof the image renders.** A failed or partial `upload_assets` call can leave a fill pointing at an `imageHash` that resolves to a transparent/grey checkerboard — the node's own `fills` data looks completely normal (valid-looking hash string, `scaleMode: 'FILL'`) while the visual result is broken. This was confirmed directly: a background frame's `fills` showed a well-formed IMAGE paint with a real-looking hash, and `get_screenshot` on that exact node still showed a checkerboard. **Always call `get_screenshot` on the specific image node itself (not just a screenshot of the whole module/section, where a small broken frame can be easy to miss next to a large working one) before considering an image frame done.** Reading `get_metadata`/`fills` alone is not sufficient evidence that an image placement succeeded.

## Cropping a person photo without cutting off the face — always crop from the top down

`upload_assets`'s own `scaleMode` parameter only supports `FILL | FIT | TILE` — it has no `CROP` option. Its default `FILL` behavior **centers** the crop, which reliably cuts into a model's face when the photo is a portrait/full-body shot taller than its target frame (the face sits near the top of the frame, and a centered crop removes roughly equal amounts from top and bottom). **Never leave a person photo on centered/default FILL cropping in a tall image placed into a shorter or squarer box — always follow the upload with an explicit `CROP` + `imageTransform` that anchors to the top,** so the crop only ever removes from the bottom.

Set this via a `node.fills` reassignment (plain Plugin API, not an `upload_assets` parameter) after the image is uploaded and placed:

```js
const node = await figma.getNodeByIdAsync(targetNodeId);
const fill = node.fills[0];
node.fills = [{
  ...fill,
  type: 'IMAGE',
  scaleMode: 'CROP',
  imageHash: uploadedHash,
  // top-anchored crop — see formula below for visibleHeightFraction
  imageTransform: [[1, 0, 0], [0, visibleHeightFraction, 0]],
}];
```

`imageTransform` is a 2×3 affine matrix, same shape/convention as `gradientTransform` (see `plugin-api-patterns.md`): it maps the node's own normalized space `(u, v)` ∈ `[0,1]²` onto the image's normalized space `(x, y)` ∈ `[0,1]²` (0,0 = top-left of the image, 1,1 = bottom-right). `[[a,b,tx],[c,d,ty]]` with no shear (`b=c=0`) gives `imageX = a·u + tx`, `imageY = d·v + ty`.

**Formula for a top-anchored crop** (image taller, relative to its aspect ratio, than the target box — the common case for a portrait/full-body shot going into a square or landscape frame):
1. Cover-scale = `max(box.width / image.width, box.height / image.height)` — this is the scale `FILL` would use.
2. Scaled image height = `image.height × coverScale`.
3. `visibleHeightFraction = box.height / scaledImageHeight` (always ≤ 1 when the image is the taller/cropped axis).
4. `imageTransform = [[1, 0, 0], [0, visibleHeightFraction, 0]]` — full width visible, and only the **top** `visibleHeightFraction` of the image's height is sampled, so the crop only ever removes from the bottom.

**Worked example (confirmed working in production, 2026-09-25):** a 1040×1300px portrait photo placed into a 560×560 square frame. Cover-scale = `560/1040 = 0.5385` (image is scaled to fill by width). Scaled height = `1300 × 0.5385 = 700px`. `visibleHeightFraction = 560/700 = 0.8`. Result: `imageTransform: [[1, 0, 0], [0, 0.8, 0]]` — shows the full width and the top 80% of the image (face fully visible, only the lower body/legs cropped off).

If the box is instead wider than the image relative to its aspect (cropping horizontally, not vertically), apply the same logic on the horizontal axis and center it (`tx = (1 - visibleWidthFraction) / 2`) — a face is rarely off to one extreme side horizontally the way it's reliably near the top vertically. When in doubt, or when both axes crop, still keep `ty = 0` (never crop from the top) for any photo with a person in it.

This only applies to the **main photo frame** (the one holding the actual model/product shot) — a background/texture frame's crop position doesn't matter the same way and can stay on plain `FILL`.

## Composing an email from these modules
This is the confirmed pattern (matches production usage, given directly by the user):

**Hero** = Header component + full-bleed image + `01 Primary/Primary` (headline + body) + `03 CTAs/01 Primary` (one or two CTAs, via the `2nd CTA` variant).

**Module 2 and beyond** = full-bleed image + a copy block + one or two CTAs. Two ways to build the copy block:
- The `02 Secondary/Left` or `/Right` module (once its import issue is resolved) — comes with eyebrow/H3/body/CTA already built in.
- Or reuse `01 Primary/Primary`'s text lockup with a **short, light headline** instead of a long one, for a visually lighter module — don't try to hide the headline via `.visible = false` (see the critical gotcha above; that silently prunes the node). `01 Primary` has no headline-toggle property in its variant list, so a short headline is the safe way to get a lighter-feeling module, not a different component pick.
- Sometimes Module 2 uses **two images** instead of one — reach for `03 Tertiary/2 Images` in that case instead of a single-image module.

**Background frame + main photo frame, when a module has both**: see the dedicated section above — fill the background frame first, then the main photo frame. Only fall back to manual framing (below) when a module genuinely has just one image frame to work with.

**Framed vs. bleed photo treatment (single-image-frame modules only)**: most single-image modules are full-bleed by default. A decorative mat/frame look (image inset with margin, optionally a stroke on its frame, optionally the module's own background swapped from solid color to a texture/pattern image) is **manual construction on top of the base component** — resize and reposition the image layer inward, don't expect a variant to do it. The one confirmed exception is `03 Tertiary/2 Images - Alternating`, which has this built in as its `Inset` variant property. Check each module's variant list for an Inset/framing-related property before assuming it needs the manual treatment — and check for a second, pre-built background frame (above) before assuming manual construction is needed at all.

## Grouping and placement (the last steps of assembly)
Once every module instance for one email is placed and stacked (each module's `y` = previous module's `y + height`, matching the pattern used successfully in this session's test build) — remembering to place **Core App Banner + Navigation Footer** as the last module in the stack, every email:
1. Select all the module instances that make up this one email (Hero + its modules, if any + the Core App Banner + Navigation Footer pair) and wrap them in a single FRAME.
2. Name that frame per the fiscal naming convention (see the `rl-campaign-scaffolding` skill's `naming-convention.md` — same formula).
3. Move that frame to be a child of the scaffolded section's **matching `Email design N` frame** — `Email design 1` for the Teaser, `Email design 2` for Product Focus, `Email design 3` for the Reminder — at `x: 543, y: 216` (see above). Each of the three frames was left empty by `rl-campaign-scaffolding` specifically for this. Do this once per email, three times per run, never mixing one email's modules into another's frame.

## Relationship to the older "EMAIL TEMPLATES (Copy)" library
That file (fixed per-brand-gender templates like `W - POLO template`, `M-POLO Template`) still exists and still works via `importComponentByKeyAsync` the same way — but it's a fixed 2-module structure with no CTL/multi-image option, superseded by this proper component library now that it's published. Prefer this library going forward; fall back to the old one only if the user explicitly asks for one of its specific themed variants (e.g. a Holiday version) that doesn't have an equivalent here yet.
