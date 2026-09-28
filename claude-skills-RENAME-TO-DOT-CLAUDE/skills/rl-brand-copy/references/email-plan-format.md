# Campaign Plan — output format

This is what this skill hands off. The campaign-scaffolding skill and the assembly skill both consume this, so its shape needs to stay predictable — don't freelance the structure, even when the content varies a lot campaign to campaign. **One Campaign Plan always covers all three emails** (Teaser, Product Focus, Reminder) from one brief — never output a single email plan on its own.

## Template

```
CAMPAIGN PLAN — <brand> <sub-line>

PALETTE (shared across all three emails)
- Background: <hex or descriptive color> — sampled from <what in the photos>
- Text/CTA: <hex or descriptive color>
- Accent (optional): <hex or descriptive color> — <where used>

=== EMAIL 1 — TEASER ===
SL: <subject line>
PH: <preheader>

HERO
- Media: image | gif
- Image: <filename/id, or "TBD — choose from: [list]" if not yet narrowed down>
- Headline: <text — story-introducing, not a sales pitch>
- Body: <text>
- CTA: <soft CTA text, or "none" — Teaser CTAs are never a direct shop CTA>

FOOTER: Core App Banner + Navigation Footer (fixed pair, no copy to write)

=== EMAIL 2 — PRODUCT FOCUS ===
SL: <subject line>
PH: <preheader>

HERO
- Media: image | gif
- Image: <filename/id>
- Headline: <text>
- Body: <text>
- CTA(s): <text> [+ <second CTA text> if the brief calls for two]

MODULE 1 — type: <generic | CTL | WIW | CYO | fixed:social | fixed:store-locator | fixed:sizes | fixed:lauren-look-banner>
- Image(s): <filename/id per slot — for CTL/WIW list each labeled image against its slot>
- Title: <text, omit for CTL/WIW/fixed modules that don't carry one>
- Body: <text, omit where the shape doesn't call for it>
- CTA(s)/label(s): <text — for CTL/WIW this is the per-image label>

MODULE 2 — (same shape as above)

FOOTER: Core App Banner + Navigation Footer (fixed pair, no copy to write)

=== EMAIL 3 — REMINDER ===
SL: <subject line>
PH: <preheader>

HERO
- Media: image | gif
- Image: <filename/id — can differ from Email 1's and Email 2's Hero image; note if it's reused or new>
- Headline: <text — closing/recap tone>
- Body: <text>
- CTA: <direct CTA text — Reminder CTAs point back to shop/browse>

WRAP-UP MODULE — type: <generic | CTL | WIW>
- Image(s): <filename/id per slot>
- Title: <text, omit where the shape doesn't call for it>
- Body: <text, omit where the shape doesn't call for it>
- CTA(s)/label(s): <text>

FOOTER: Core App Banner + Navigation Footer (fixed pair, no copy to write)

NOTES
- <anything the human should sanity-check across the whole set: an image the mapping didn't clearly assign, a module-shape judgment call within Email 2, a voice/tone decision that could go either way, a sustainability or sensitivity flag from the compliance references, whether Email 3 reused an earlier Hero image or picked a new one and why>
```

## Rules for filling it in

- **SL/PH**: write these in-brand for each of the three emails, respecting the character-count guidance in `copy-voice.md` (Performance/social has explicit limits; RLE SL/PH don't have a hard number in the source material, so keep them tight and scannable rather than padding them out). The three SL/PH pairs must read as three distinct messages, not the same line three times — a Teaser SL should read differently from a Product Focus SL for the same story.
- **Image references**: use whatever identifier the mapping gave (filename, or a short description if the mapping only gave a description). For anything not pre-labeled, actually browse the project's Drive folder and pick a real file — see `image-selection.md`. Never invent a filename, and don't default to `TBD` as a shortcut; only use it when the folder genuinely has nothing that fits, and say what's missing. Pick enough distinct images to cover all three Heroes plus Email 2's two modules plus Email 3's wrap-up — reuse across emails is allowed (especially the Reminder reusing an earlier Hero) but should be a deliberate choice, noted in NOTES, not a default because nothing else was picked.
- **Headline/body/CTA copy**: written in the brand's actual voice (read the brand voice file + `master-brand.md` first), following `copy-voice.md`'s anatomy and length calibration, and cleared against `sensitivity-and-compliance.md` (banned words) and, if a sustainability claim is involved, `sustainability-claims.md`.
- **Email 2's module count**: always exactly 2 (per `module-types.md`'s fixed three-email structure) — following the brief's explicit structure when it has one, otherwise proposing a reasonable 2-module plan and flagging that it's a proposal in NOTES.
- **Palette**: decide it per `color-palette.md` — 2–3 colors maximum, shared across **all three emails**, derived from the actual selected images as a set, not a brand default and not invented, and not re-derived separately per email.
- **Always end with one shared NOTES section**, even if it's just "no open questions" — the human reviewing this should never have to guess whether something was a deliberate choice or an oversight, across any of the three emails.

## What this format is NOT for
It's a plan, not final production text formatted for Figma insertion — the assembly skill is responsible for actually placing this copy into the right nodes for each of the three `Email design N` frames, applying the CTA character-count/width limits (see the assembly skill's brand files), and flagging if a CTA is too long for its frame at build time.
