---
name: rl-email-pipeline
description: "The conductor for building a Ralph Lauren email set end to end: remembers the exact 9-step sequence from a marketing brief to a finished set of three emails (Teaser, Product Focus, Reminder) sitting in their Email design 1/2/3 frames, and which of the three RL skills (rl-brand-copy, rl-campaign-scaffolding, rl-email-assembly) handles each step. Use whenever the user wants to run the full RL email pipeline, asks 'build me an email set from this brief,' asks what step comes next, or asks which skill does what in this workflow. This skill holds no domain knowledge of its own — it only sequences the other three."
---

# RL Email Pipeline

This skill does nothing by itself — it's the map, not the territory. Its only job is to keep the sequence straight and load the right skill at the right step, so the sequence doesn't have to be re-derived from scratch each time. Read the three skills' own SKILL.md files for how each step actually gets done; this file just says which one to reach for, in what order.

**Every run produces a set of THREE emails from one brief, not one email.** This replaced the old single-email output entirely (as of the "EMAIL MONTH FY27 - TEMPLATE" master section, node-id `135:1308`, name "Email 01 - Email Name (New Template)"). The three always have fixed roles — see `rl-brand-copy/references/module-types.md` for the shape of each:

1. **Teaser** — Hero only, introduces the story, doesn't sell product yet.
2. **Product Focus** — Hero + 2 modules, the actual shopping email.
3. **Reminder** — Hero (can use a different photo than the other two) + one wrap-up module, closes out the story/collection highlight.

All three share one story, one 2–3-color palette, and end with the same fixed **Core App Banner + Navigation Footer** pair — but each has its own SL/PH, and the Reminder is allowed a different hero photo.

## The steps

| # | Step | Skill |
|---|---|---|
| 1 | Duplicate the "Email 01 - Email Name (New Template)" master section; replace "Email Name" in the title with the real campaign name | `rl-campaign-scaffolding` |
| 2 | Place the ONE shared marketing-brief screenshot into `Marketing Brief`, and the ONE shared image-mapping screenshot into `Image Mapping` | `rl-campaign-scaffolding` |
| 3 | Fill in all THREE Subject Line / Preheader pairs — one per email (Teaser, Product Focus, Reminder), each in its own `SL/PH` frame | `rl-campaign-scaffolding` |
| 4 | Fill in the ONE shared Timeline: creation date = today, delivery date = +1 week, launch date = delivery + 1 week | `rl-campaign-scaffolding` |
| 5 | Read the marketing brief, decide the overarching story, pick real images for the whole set, decide ONE shared 2–3-color palette; write the copy for all three emails | `rl-brand-copy` |
| 6 | Assemble **Email 1 — Teaser**: Header + Hero (story-introducing copy, no product CTA) + Core App Banner + Navigation Footer, placed inside the `Email design 1` frame | `rl-email-assembly` |
| 7 | Assemble **Email 2 — Product Focus**: Header + Hero + Module 1 + Module 2 + CTAs + Core App Banner + Navigation Footer, placed inside the `Email design 2` frame | `rl-email-assembly` |
| 8 | Assemble **Email 3 — Reminder**: Header + Hero (own photo choice, can differ from Email 1/2) + one wrap-up module (collection/story highlight, closing tone) + Core App Banner + Navigation Footer, placed inside the `Email design 3` frame | `rl-email-assembly` |
| 9 | For each of the three, group its module instances into one frame, name it per the fiscal naming convention, and place it at `x: 543, y: 216` **relative to its own `Email design N` frame** — apply the shared palette consistently across all three, not just within one | `rl-email-assembly` |

## How to run it
1. Confirm which section/campaign this is for (or that step 1 is creating a new one) and get the marketing brief.
2. Steps 1–4: follow `rl-campaign-scaffolding`'s own workflow in order. Don't skip ahead to copy or modules before the section exists and its metadata is filled in — later steps assume it's there.
3. Step 5: switch to `rl-brand-copy`. Its output is now a **Campaign Plan** containing all three emails' copy plus the one shared palette (see `email-plan-format.md`) — don't let steps 6–8 start writing copy themselves, that's step 5's job alone.
4. Steps 6–9: switch to `rl-email-assembly`, building the three emails in order (Teaser, then Product Focus, then Reminder), each into its own `Email design N` frame, working from the Campaign Plan produced in step 5 and the section produced in steps 1–4.
5. Do a final screenshot of the **whole section** (all three `Email design` frames together, not just one) before calling the run done, and say plainly which steps were completed vs. skipped.
6. Report **one Figma link to the section** (not three separate links) — the section already contains all three emails, so a single link covering the whole thing is what goes in the final summary/webhook.

## Why three skills and not one
Each one fails differently and gets touched by different kinds of changes: copy/voice guidance changes when brand guidelines update, scaffolding changes if the section template changes, assembly changes when the component library changes. Keeping them separate means a change to one doesn't require re-testing or re-writing the other two.
