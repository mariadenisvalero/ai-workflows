# Figma scaffolding workflow

Verified live against the real "EMAIL MONTH FY27 - TEMPLATE" file (2026-09-28), node-id `135:1308`. If the user points this skill at a different file, re-verify the structure below before assuming it matches — don't build against a guess (see the general lesson in the assembly skill's `figma-workflow.md`: nested frames and field names are easy to get subtly wrong without checking live).

## Finding the master template section
There should be exactly **one** section on the target page named literally **"Email 01 - Email Name (New Template)"** — that's the pristine, never-duplicated-from-since-cleaned master. Find it by name, not by a hardcoded node id (ids are specific to one file and won't match a different month's template file):
```js
const page = await figma.getNodeByIdAsync('<page-id>');
await figma.setCurrentPageAsync(page);
const master = page.findOne(n => n.type === 'SECTION' && n.name === 'Email 01 - Email Name (New Template)');
```
If none is found, or more than one is found, stop and tell the user — don't guess which one is the real master.

## Placeholder strings: verify live, don't trust a prior write-up
Before relying on any placeholder string in this file, re-confirm it against the live section (`master.findAll(n => n.type === 'TEXT').map(t => t.characters)` is enough to eyeball them all at once) rather than assuming this document is still accurate — it's easy for a template to drift from its own documentation, and a silent no-op (search finds nothing, loop does nothing, no error) is exactly the kind of failure that's easy to miss without a screenshot check at the end.

## The section's field structure (by name, so it survives duplication)

Confirmed live layout (2026-09-28): the master section holds **one shared metadata block** for the whole three-email set, plus **three parallel columns**, one per email:

- **`Marketing Brief`** (a FRAME, empty) — ONE per section, shared across all three emails. Gets one image (a screenshot of the brief) placed as its content.
- **`Image Mapping`** (a FRAME, empty) — ONE per section, shared across all three emails. Gets one image (a screenshot of the image-mapping portion of the brief) placed as its content.
- **`Timeline`** (a FRAME) — ONE per section, shared. Contains three label/placeholder TEXT pairs: `Design Review`/`"Today here"`, `Final Delivery`/`"Delivery date here"`, `Email Launch`/`"Launch date here"`. Match each by its exact placeholder string, not by position.
- **Three `SL/PH` frames** — one per email, each an independent FRAME containing its own `SL` label + `"Subjectline here"` placeholder TEXT, and `PH` label + `"Pre-header here"` placeholder TEXT (note the hyphen — verified live; `"Preheader here"` with no hyphen does not match and silently finds nothing). **Match each `SL/PH` frame to its email by x-position, not by document order**: the `SL/PH` frame that shares (approximately) the same x-coordinate as a given `Email design N` frame belongs to that email. Confirmed live: `Email design 1` and its `SL/PH` both sit at `x≈2741`; `Email design 2` and its `SL/PH` at `x≈4681`; `Email design 3` and its `SL/PH` at `x≈6621` (all relative to the section). The column order left-to-right is Teaser → Product Focus → Reminder (confirmed by the "Teaser"/"Product Focus"/"Reminder" labels sitting directly above each column) — but re-verify the labels live rather than assuming the order never changes.
- **Three `Email design 1`, `Email design 2`, `Email design 3` frames** (1726×3515 each, left empty by this skill) — this is the assembly skill's territory. Don't touch them.

## Duplicating and placing the new section
```js
const clone = master.clone();
// position: to the right of every existing EMAIL section on the page, with a gap
const existingSections = page.children.filter(n => n.type === 'SECTION');
const rightmostEdge = Math.max(...existingSections.map(s => s.x + s.width));
clone.x = rightmostEdge + 400; // gap; adjust if the file's own spacing convention differs
clone.y = master.y;
clone.name = '<constructed name per naming-convention.md>';
page.appendChild(clone);
```
Do this **before** editing any of the clone's contents — edit the clone, never the master. Confirm afterward that the master section (`Email 01 - Email Name (New Template)`) still has zero content in its Marketing Brief/Image Mapping frames and its placeholders unchanged — if it doesn't, something edited the wrong node.

## Filling in text fields
Standard pattern (see the assembly skill's `figma-workflow.md` for the general font-loading gotchas — the same substitute-font rules apply here for any text in this template):
```js
const node = clone.findOne(n => n.type === 'TEXT' && n.characters === 'Subjectline here');
await figma.loadFontAsync(node.fontName);
node.characters = '<real SL from the matching email in the Campaign Plan>';
```
Because there are now three `"Subjectline here"` / `"Pre-header here"` placeholder pairs in one section (one per `SL/PH` frame), don't use `findOne` for these two — use `findAll` and match each match's parent frame to the correct email by x-position (see above) before setting its text, or you risk writing the wrong email's SL into the wrong slot. The three Timeline date placeholders remain a single shared set — `findOne` is fine for those.

## Placing the two shared brief screenshots
Use the same `upload_assets` → `curl` POST pattern documented in the assembly skill's `figma-workflow.md` (submit URL, multipart POST, verify `placedOnNodeId` came back before trusting it landed) — the mechanism is identical whether the image is a product photo or a screenshot of a brief. Target the `Marketing Brief` frame and the `Image Mapping` frame directly (they're plain frames, not nested like the module image-fill targets in the assembly skill, so no need to hunt for an inner node — but confirm with a screenshot regardless, since the upload step has a known first-attempt-silently-fails quirk). These are placed once per section, not once per email.

## Final check
Screenshot the whole new section (all three `Email design` columns, not just one) before calling the scaffolding done — confirm the name, all three SL/PH pairs (matched to the right column), the shared timeline dates, and both shared screenshots landed, and that `Email design 1/2/3` are still empty and ready for the assembly skill.
