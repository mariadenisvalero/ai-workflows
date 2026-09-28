# Figma workflow — tested mechanics for building RL emails

Everything here was verified working in an actual build session (Polo W/M templates, FY27 campaign). Follow it as written; the gotchas below cost real time to discover.

## File layout

- **Source library**: the file usually called "EMAIL TEMPLATES (Copy)" (or similarly named — ask if unsure) holds the master template COMPONENTS per brand, on a page typically called "All Templates". Look for sections named things like `W-POLO`, `M-POLO`, `POLO MULTI`, `POLO TOWEL`, etc. Inside each section is a COMPONENT node (e.g. `W - POLO template`, `M-POLO Template`) — these are real published library components with a `.key`.
- **Working file**: the actual campaign build (e.g. "EMAIL MONTH FY27 - TEMPLATE"), on whatever page matches the send month/season.
- Get every file's key from its URL before starting; Figma tools need an explicit file key or URL, there's no search-by-name.

## Bringing a base template into the working file

**Preferred method — component import by key** (clean, no manual reconstruction):

1. In the source library file, find the template component and read its `.key`:
   ```js
   const node = await figma.getNodeByIdAsync('<node-id-in-source-file>');
   return { name: node.name, type: node.type, key: node.key };
   ```
   (must be `type: "COMPONENT"` and have a non-null `key` — that means it's published to the library.)
2. Switch to the working file (different `fileKey` on the tool call) and import + place it:
   ```js
   const page = await figma.getNodeByIdAsync('<target-page-id>');
   await figma.setCurrentPageAsync(page);
   const comp = await figma.importComponentByKeyAsync('<the key from step 1>');
   const instance = comp.createInstance();
   instance.x = ...; instance.y = ...; // position next to existing work
   page.appendChild(instance);
   ```
   This worked directly, first try, once the team's components were actually published as a library — no copy-paste, no cross-file node reconstruction needed. If `importComponentByKeyAsync` throws, the component likely isn't published to a library the working file can see — fall back to asking the user for manual copy-paste, or reconstruct node-by-node (slower, only as a last resort).

## Finding the real image-fill target inside a module

Modules are nested — the frame named e.g. `04 Image Elements/1 Image` or `HERO` is usually a wrapper, not the fill target. Pattern seen repeatedly:
- Outer frame (module bounds, e.g. 640×600) contains a background/ratio frame at full size (often literally named "Image Ratio" or similar) **and** an inner frame nested a level or two down (e.g. at `x:40,y:40` with the visible photo size) whose `.fills` array is `type: "IMAGE"`.
- Always query `.fills` on candidate nodes before uploading — don't guess by name:
  ```js
  const n = await figma.getNodeByIdAsync(id);
  console.log(n.fills); // look for {type: "IMAGE", ...}
  ```
- The Hero photo and a module's photo follow the same nested pattern; so does a plain background-color frame (e.g. `BG IMG`) that sits behind a hero photo for parallax/framing effect.

## Uploading a campaign image into a module

```
Artifact/Figma upload_assets({ fileKey, nodeIds: ["<target node id>"], scaleMode: "FILL", count: 1 })
→ returns { submitUrl, targetNodeId }
```
Then POST the actual file (multipart, field name `file`, with `type` and `filename` set) to `submitUrl` via `curl` in the bash/container tool — **not** a fetch from inside the Figma plugin sandbox:
```bash
curl -s -X POST -F "file=@image.png;type=image/png;filename=whatever.png" "<submitUrl>&scaleMode=FILL"
```

**Known bug — verify before moving on.** The first POST sometimes returns `{"success": true, "imageHash": "..."}` **without** a `placedOnNodeId` field, and the node's fill is silently *not* actually updated (confirmed by re-reading `node.fills` — still shows the old fill, or the image hash doesn't resolve via `figma.getImageByHash()`). When this happens:
1. Call `upload_assets` again for the same node (new `submitUrl`).
2. POST again.
3. This second attempt returns `placedOnNodeId` matching the target — that's the actual confirmation the fill committed.
4. Verify visually with `get_screenshot` on the node regardless — don't trust the JSON response alone.

`mcp.figma.com` needs to be an allowed egress domain for the `curl` upload step to work at all; if it's blocked, tell the user their network/egress settings need that domain added — this isn't fixable from inside the conversation.

**If the real intended asset fails to upload (Drive download times out, "session expired," file too large) — do not leave that frame empty or move on with a broken fill.** Reuse the `imageHash` of another photo already confirmed rendering elsewhere in this same email as a fallback, even if it means a duplicate. Confirmed hard rule and exact technique: see `module-library.md`'s "an image frame must never be left empty" section — including why `fills[0].imageHash` being non-null is not, by itself, proof the image renders (a broken upload can leave a well-formed-looking fill that still screenshots as a checkerboard).

**Default `scaleMode: 'FILL'` centers the crop, which reliably cuts off a model's face on a tall photo going into a shorter/squarer frame.** For any module photo with a person in it, follow the upload with a `node.fills` reassignment setting `scaleMode: 'CROP'` and a top-anchored `imageTransform` — see `module-library.md`'s cropping section for the exact formula and a worked example. Don't ship a person photo on default centered FILL without checking whether it cuts off the face.

## The two licensed brand fonts aren't loadable — plan for it every time

`Le Jeune Deck` (headline/body, Polo/Lauren/Home) and `Founders Grotesk Mono` / `Founders Grotesk Text` (CTAs / Luxe Brands headline) are licensed fonts. **They will not load via the Figma plugin API in this environment**, confirmed by:
- `figma.listAvailableFontsAsync()` returns a fixed catalog (~8,900 fonts, Google Fonts + system) with zero matches for "jeune", "founders", "deck", "grotesk" — before AND after the user uploads the font to their org's Font Management (Enterprise). The total available-font count doesn't change even after that upload, which is the tell: plugin font loading uses a separate, restricted catalog from what a human sees in the Figma desktop/web editor, and org Font Management doesn't bridge that gap. This is a Figma platform limitation, not a user permissions problem — don't spend time troubleshooting it further; move straight to substitution.
- `figma.loadFontAsync({family: "Le Jeune Deck", style: "Regular"})` throws `"The font family ... does not exist"`.

**What actually works — substitute + confirm, when a human is actually present to look at the comparison:**
1. Check availability of a couple of close candidates with `listAvailableFontsAsync()` filtered by family name.
2. Build a small off-canvas test frame with the real copy in each candidate, side by side, labelled. Screenshot it.
3. Let the user pick — don't decide alone for a client brand deliverable.
4. Apply the chosen font to the real nodes, delete the test frame.

**In an unattended routine run, skip steps 1–3 entirely for any font listed below.** Nobody is watching the session to approve a comparison frame, so building one only spends time without ever getting a real answer — apply the confirmed/default substitute directly from the table and move on. This is a permanent pipeline default now, not a "this session only" result — don't re-run the comparison test on a future build just because it's a new session.

**Confirmed substitutes (standing pipeline defaults — apply directly, no re-testing):**
- Le Jeune Deck Regular → **Playfair Display** (Regular for body copy, Medium for headline weight/impact)
- Founders Grotesk Mono → **Space Mono** Regular
- Founders Grotesk Text, Core-tier body/eyebrow (e.g. `02 Secondary`) → **Space Grotesk** (see `module-library.md`)

**Still genuinely unresolved — no human comparison has happened yet:**
- Founders Grotesk Text, Luxe Brands headline/body — no candidate has been picked or seen by a human. In an unattended run, use **Work Sans** as the working default (first candidate, not yet validated) so the build doesn't stall, and list it explicitly under "decisions made without brief input" in the final summary so a human can compare it against IBM Plex Sans (or anything else) later and lock in a real answer. Once a human does confirm one, move it up into the confirmed list above and stop flagging it.

Always load the font explicitly before setting `.characters` or `.fontName`:
```js
await figma.loadFontAsync({ family: 'Playfair Display', style: 'Medium' });
node.fontName = { family: 'Playfair Display', style: 'Medium' };
node.characters = "...";
```

## Module background color — apply the Email Plan's palette, don't invent one per module

The Email Plan from `rl-brand-copy` now includes a decided **PALETTE** section (2–3 colors max, shared across the whole email — see that skill's `color-palette.md`). This skill's job is to apply that palette consistently and verify it's actually legible where it lands — not to re-derive a color from scratch for each module individually. If a module's own photo suggests a different color than the plan's palette, use the plan's color anyway (that's the whole point of deciding it once for the email) — flag the mismatch to the user rather than quietly drifting off-palette.

1. If the Email Plan already gives a specific hex for the background/text colors, use those directly — skip to step 3 to verify contrast.
2. If the plan only describes the color qualitatively (e.g. "olive, from the jacket and wood tones") rather than a hex, sample it: crop a clean patch of the relevant garment/background from the actual campaign photo (avoid skin, hair, hands, background bleed) and average its RGB (PIL: crop → average pixel value).
3. Whichever way you got the color, verify contrast against the text color with real WCAG math before applying — don't eyeball it, and don't do a blind percentage darken/lighten (that can undo an intentional match to the photo's actual tone). Check the *before* contrast ratio if adjusting an existing color; match or exceed it:
   ```python
   def srgb_to_linear(c):
       c = c/255
       return c/12.92 if c <= 0.03928 else ((c+0.055)/1.055)**2.4

   def luminance(rgb):
       r,g,b = rgb
       return 0.2126*srgb_to_linear(r)+0.7152*srgb_to_linear(g)+0.0722*srgb_to_linear(b)
   def contrast(c1,c2):
       L1,L2 = luminance(c1), luminance(c2)
       L1,L2 = max(L1,L2), min(L1,L2)
       return (L1+0.05)/(L2+0.05)
   ```
4. Find every node that shares the *exact* same background color (Figma email builds routinely repeat one raw RGB value across several sibling instances — the image-ratio frame, the headline instance, the body instance, the CTA instance — with no shared variable binding). Update all of them together, and across **every module in the email**, not just within one module — the palette is per-email, not per-module. Check first:
   ```js
   // compare n.fills[0].color across candidate sibling node ids
   ```

## Cropping a photo down to a pure texture/background fill

When a module wants "just the fabric/color, no figure" as a background:
1. Sample candidate crop regions with PIL, viewing each before committing — first attempts almost always catch a sliver of skin, background, or an accessory at the edge; iterate down to a genuinely clean patch.
2. Crop at (or close to) the target frame's aspect ratio so Figma's `FILL` scaleMode doesn't do a surprise crop of its own.
3. Upscale the clean crop with `Image.LANCZOS` before uploading (a small clean source patch upscaled beats a hastily-cropped bigger area with contamination at the edges).

## General sequencing

Build and confirm one module at a time — screenshot after every image swap, every text edit, and every color change, using `get_screenshot` scoped to that module's own node id. These templates nest frames deeply and repeat background colors across siblings; changes that look right in isolation can be applied to the wrong node without a screenshot catching it immediately.
