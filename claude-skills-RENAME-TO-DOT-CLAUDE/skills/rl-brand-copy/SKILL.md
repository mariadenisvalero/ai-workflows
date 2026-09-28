---
name: rl-brand-copy
description: "Turn a Ralph Lauren marketing brief into a full Campaign Plan — one shared story and palette split across a set of three emails (Teaser, Product Focus, Reminder), each with its own module structure and on-brand copy (SL, PH, headlines, body, CTAs) — for Polo, Lauren, Home, or Luxe Brands (Purple Label/Collection). Use whenever the user gives a marketing brief, a story to tell, or an image mapping and wants an RL email set planned or written, or asks which modules an email should have, or asks about RL brand voice, tone, banned words, or sustainability-claim wording. This is the copy/story-planning layer only — it does not touch Figma. For actually building the emails in Figma, hand its output to the Figma assembly skill."
---

# RL Brand & Copy

Turns a marketing brief (+ image mapping, if given) into a structured **Campaign Plan**: one shared story and 2–3-color palette, split across three fixed-role emails — Teaser, Product Focus, Reminder — with the module structure and on-brand copy for each. Pure planning and writing — no Figma. Its output feeds the campaign-scaffolding skill (which creates the section and drops the three SL/PH pairs + shared brief into it) and the assembly skill (which builds the actual modules for all three emails from this plan).

**Every brief now produces all three emails, not one.** See `rl-email-pipeline`'s own SKILL.md for the full sequence this fits into.

## Workflow

1. **Identify brand + sub-line.** If the brief doesn't say, ask — Polo/Lauren/Home/Purple Label/Collection each have a genuinely different voice (see `master-brand.md` for the shared umbrella, then the brand's own file: `polo.md`, `lauren.md`, `home.md`, `luxe-brands.md`).
2. **Read the brief for explicit structure first.** Briefs often already say "Hero — [copy], CTA. Complete the Look — [item], [item]" for the product-focused email — when they do, that *is* your Product Focus module plan; don't second-guess it. See `module-types.md`'s worked example.
3. **Read the image mapping for pre-labeled (secondary) selects before assigning anything yourself.** A labeled image (e.g. one named "Bags") is telling you both the image *and* the module slot it belongs in — this feeds the Product Focus email. For anything left open (the "primary" pool), actually browse the project's Drive folder and pick — see `image-selection.md`. Don't leave an image field as `TBD` when real candidates were available to look at. Pick enough images across the pool to cover all three emails: at minimum, one Hero-worthy shot for the Teaser, enough for Product Focus's Hero + 2 modules, and at least one shot for the Reminder — the Reminder's hero photo is allowed to differ from the Teaser's and Product Focus's (a fresh angle on the same story), it doesn't have to reuse either.
4. **Decide the three emails' module shapes** per `module-types.md`'s fixed three-email structure — this is no longer an open module-count judgment call the way a single email used to be: Teaser is always Hero-only, Product Focus is always Hero + 2 modules, Reminder is always Hero + one wrap-up module. What *is* still a judgment call: which images and which specific copy angle each gets. Then decide the campaign's color palette per `color-palette.md` — 2–3 colors max, derived from the images actually selected across the whole set, shared consistently across all three emails (not re-derived per email).
5. **Write the copy for all three emails** in the brand's actual voice — read the brand's voice file, `master-brand.md`, and `copy-voice.md` (email anatomy + length calibration) before drafting. For product-focused copy specifically, also check `pdp-product-copy.md`. Keep the three emails' voices distinct in *purpose* (teaser intrigue vs. shopping directness vs. closing urgency) while staying the same story and the same brand voice throughout.
6. **Clear it against compliance** — `sensitivity-and-compliance.md` for banned/cautioned words and casting language, `sustainability-claims.md` if any sustainability claim is even implied. Never write a sustainability claim from scratch.
7. **Output the Campaign Plan** in the exact structure in `email-plan-format.md`, ending with one shared NOTES section naming every open judgment call across all three emails.

## Reference index

- `master-brand.md` — the tone every sub-brand inherits (Sophisticated/Concise/Romantic/Confident, narrator rule, recurring campaign themes). Read once, before any brand file.
- `polo.md`, `lauren.md`, `home.md`, `luxe-brands.md` — brand-specific voice, story, tone pillars, and known fixed-shape modules for that brand.
- `module-types.md` — the catalog of module shapes (Hero, generic, CTL, WIW, CYO, fixed modules) and the fixed three-email structure (Teaser/Product Focus/Reminder).
- `color-palette.md` — how to decide the campaign's 2–3-color palette from the actual selected images, shared consistently across all three emails. Required for every campaign, not optional polish.
- `image-selection.md` — how to find the project's Drive folder and actually pick real images for open module slots across all three emails, instead of leaving them `TBD`.
- `email-plan-format.md` — the exact Campaign Plan output structure (shared palette + three email blocks). Don't freelance this shape.
- `copy-voice.md` — RLE email anatomy (SL/PH → Hed/Dek/CTA → per-module), Women's Polo's voice evolution in depth, worked examples, Performance/social copy rules and character limits.
- `pdp-product-copy.md` — product-detail-page romance-copy length/key-words per brand and the spec-bullet structure per product category. Use for the Product Focus email's copy, or as a sanity check on product-heavy module copy.
- `naming-products.md` — how generic and proprietary product names get built (rarely needed for email copy itself, but useful if a brief references a product by an informal name).
- `sensitivity-and-compliance.md` — banned/cautioned words, casting language, cultural-appropriation cautions, body-image standards, flag-usage rules, regional content restrictions, social character limits, and the Polo/Luxe social approval workflows.
- `sustainability-claims.md` — legally vetted claim language by material/process. Mandatory reference before writing any sustainability-adjacent line.
- `quotes.md` — sourced, first-person Ralph Lauren quotes for the rare case a module wants a real quote (see the narrator rule in `master-brand.md` — quotes are the one place first-person is allowed, and only genuine sourced ones).
- `emergency-comms.md` — the shape of a crisis-comms toolkit, not reusable wording. Flag any real crisis-comms request to the user for sign-off rather than drafting it solo.
- `glossary.md` — RL abbreviations (SL, PH, CTL, WIW, CYO, BAU, MWK, FOB, FPO, fiscal quarters, etc.) — briefs and image mappings use these natively.
- `rrl.md`, `fragrance.md` — voice-only references for two brands outside the current Figma build scope, in case a brief touches them.

## What this skill deliberately does not do
No Figma access, no image upload/cropping, no font handling, no color-matching, no section-naming/scaffolding. Those all live in the Figma assembly and campaign-scaffolding skills, which consume this skill's Campaign Plan as their input.
