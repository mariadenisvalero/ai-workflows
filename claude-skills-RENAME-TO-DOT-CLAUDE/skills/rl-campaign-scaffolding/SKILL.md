---
name: rl-campaign-scaffolding
description: "Duplicate the master email section in the Ralph Lauren monthly Figma template and set it up for a new set of three campaign emails (Teaser, Product Focus, Reminder) — correct fiscal-naming convention, the three subject lines/preheaders, timeline dates, and the shared marketing-brief/image-mapping screenshots. Use whenever the user wants to start a new RL email set in the Figma campaign file, create/duplicate a section for a specific fiscal week and brand, or asks to set up a set's metadata (SL, PH, timeline, brief) before the actual modules get built. This is the organizational layer between planning (rl-brand-copy) and building (the Figma assembly skill) — it never touches the Email design 1/2/3 module content itself."
---

# RL Campaign Scaffolding

Duplicates the master **"Email 01 - Email Name (New Template)"** section in the monthly Figma template, names it correctly, and fills in everything that's pure metadata for the whole three-email set: the ONE shared Marketing Brief/Image Mapping screenshots, the ONE shared Timeline, and all THREE SL/PH pairs (Teaser, Product Focus, Reminder — one per email). Leaves the three `Email design 1/2/3` frames empty — that's the Figma assembly skill's job, working from this section once it exists.

**One section now always holds a set of three emails, not one.** See `rl-email-pipeline`'s own SKILL.md for why and what each of the three is for.

## Workflow

1. **Gather what's needed**: fiscal year, month, fiscal week (ask — never compute this), region (default RLNA, confirm if different), brand + gender code (only `M-Polo` is confirmed so far — ask for the exact fragment the first time any other brand/sub-line comes up, per `naming-convention.md`), campaign name, the three SL + PH pairs (usually from a `rl-brand-copy` Campaign Plan — one per email in the set), the three timeline dates (Design/Final Delivery/Launch, shared across the whole set), and the two shared brief screenshots (Marketing Brief + Image Mapping — one of each for the whole set, not per email).
2. **Build the section name** per `naming-convention.md` — one name for the whole section/set, same formula as before.
3. **Find the master section, duplicate it, place and rename the copy** per `figma-scaffolding-workflow.md` — always edit the clone, never the master.
4. **Fill in the shared Timeline and both shared screenshots, then all three SL/PH pairs** — match each `SL/PH` frame to its corresponding `Email design N` frame by shared x-position (see `figma-scaffolding-workflow.md`), not by assuming a fixed order.
5. **Confirm with a screenshot of the whole new section** (all three columns) before calling it done, and confirm the master section is still untouched.

## Reference index
- `naming-convention.md` — the fiscal-naming formula, with what's confirmed vs. what still needs to be asked per brand/region.
- `figma-scaffolding-workflow.md` — the technical how-to: finding the master by name, cloning, placement, filling text fields, placing the two shared screenshots, matching each SL/PH pair to its email.

## What this skill deliberately does not do
No copy decisions (that's `rl-brand-copy` — this skill takes the three SL/PH pairs and the brief as given, it doesn't write them), no module building, no product photo sourcing/cropping, no font substitution logic beyond the basic text-field fills here (the assembly skill owns all of that for the `Email design 1/2/3` content).
