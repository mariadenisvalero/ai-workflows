# Selecting images from Drive

This closes the gap `module-types.md` left open: when the marketing brief's image mapping gives a pool of candidates instead of pre-labeled selects, this skill looks at the actual photos and picks — it doesn't leave the Email Plan's image fields as `TBD`.

## Finding the right folder
Project photos live under one shared Drive folder, with one subfolder per brand/test (confirmed pattern: e.g. `Kids 1`, `W Polo 1` under the shared root). If the root folder link isn't already known for this project, ask for it once — after that, match the brand/sub-line named in the brief to the subfolder whose name matches (case-insensitively, ignoring a trailing test number).

## Workflow
1. **Check for pre-labeled secondary selects first** (per `module-types.md`) — if the brief already hands you a labeled image for a slot, use it as given, don't second-guess it by browsing further.
2. **List the remaining folder contents** for whatever's left unresolved (the primary pool).
3. **View the actual images**, not just filenames — filenames are often just photo codes (`J002034_S_09_1581 1.png`) or generic asset labels (`Type 1.png`, `Content 2.png`) that tell you nothing about content on their own.
4. **Match each open module slot to a candidate** using what the brief and the Email Plan's copy actually need — a Hero wants the shot that carries the headline's mood; a CTL/WIW slot wants something that pairs sensibly with the Hero look rather than duplicating it.
5. **Apply the standard variety rules** while choosing (from the brand voice files' "story best practices"): don't reuse the same crop twice in one email, mix full-outfit shots with closer crops, prefer a laydown for a module that wants a quieter visual beat rather than another on-figure shot.
6. **Record the actual file** (Drive file id and/or filename) in the Email Plan's Image field for that module — never leave it `TBD` once real candidates have actually been reviewed. If two candidates are genuinely close, pick one and name the runner-up in NOTES so the human can swap it in one step rather than starting the search over.

## When to still say TBD
Only when the folder genuinely doesn't have anything that fits a slot (not just "nothing's obviously perfect") — say so plainly in NOTES rather than forcing a mediocre pick, and describe what kind of shot is actually missing so the human knows what to source.

## Hard rule: the copy must describe what's actually in the assigned photo

Confirmed failure mode (2026-09-25 production run): a module was headlined "Chinos, On Repeat" with a CTA of "Shop Chinos," but the photo assigned to that slot was actually a full-body shot dominated by a blazer and a bag — no chinos clearly visible, and the crop cut off the model's face on top of that. The category name in the brief (or the SL/PH's own product list) is not itself evidence the assigned photo shows that category — it's only evidence the *brief* wants that category covered somewhere in the email.

**Before finalizing any module's headline/CTA, look at the actual photo assigned to that slot (view the image, not just its filename or Drive listing) and check that the copy's claim is literally visible in it**: if the headline names a garment ("Chinos," "The Cardigan"), that garment needs to be the one actually legible in the shot — not merely present somewhere in the brand's fall lineup. If the pool of available photos for a slot doesn't include a clean shot of the category the brief called out, **don't force a mismatched headline onto whatever photo is available** — write the headline to match what IS clearly visible in the photo (garment, mood, styling), and note in the Email Plan's NOTES that the originally-requested category ("chinos") didn't have a matching photo in the folder, so the module was retitled to fit the photo instead. This is the same "don't invent that it happened" honesty already required elsewhere in this pipeline, applied to image/copy coherence specifically.

**This check also applies after any later image substitution** — e.g. if `rl-email-assembly` has to swap a module's photo during assembly (asset upload failed, a better-matching photo was found, a duplicate fallback was used per the never-leave-empty rule in `module-library.md`). A copy/image pairing that was correct at planning time can go stale the moment the image changes; whichever skill makes that swap is responsible for re-checking the module's headline/CTA against the new photo before calling the module done, and updating the copy if the pairing no longer holds — not just flagging the mismatch in a final summary and moving on.

## Hard rule: a crop of a person must never cut off the face

When picking or later cropping a photo for any slot, a full-body or portrait shot of a model must show the face — never crop a person's photo starting anywhere but the top. This is enforced mechanically during assembly (see `module-library.md`'s top-anchored-crop formula), but it starts here at selection time too: when a candidate photo's only usable crop for a slot's aspect ratio would cut off the face (e.g. the shot is extremely tall and narrow relative to the target frame), prefer a different candidate rather than one that forces a bad crop.
