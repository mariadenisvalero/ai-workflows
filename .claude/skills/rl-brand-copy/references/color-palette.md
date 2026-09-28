# Color palette — deciding it, not just applying it

Every email needs its own small, coherent color palette for every non-photo block (copy backgrounds, CTA backgrounds, and the text sitting on them) — this is a real, required step, not an optional polish pass. Confirmed by real examples: a Kids "Camp to Classroom" email used one moss/olive green across its Hero *and* its Girls module even though the two used completely different photos; a Baby "Gifts for Baby" email used cream + navy instead — same brand family, deliberately different palette, because the photography told a different color story.

## The rule
- **Maximum 2–3 colors total per email** (usually: one background color, one text/CTA color, optionally one accent) — not per module, per **email**. Every module's copy block draws from this same small set.
- **The palette must come from the actual campaign photography**, not be invented or defaulted to a generic brand color. Look at every image selected for this email (via `image-selection.md`) together, as a set, and find the color(s) that actually recur across them — a wood tone, a garment color, a landscape tone — the way "Camp to Classroom"'s olive came from both the boy's jacket and the wood backdrop in the Hero, then got reapplied (not re-derived from scratch) to the Girls module even though that photo doesn't share the same jacket.
- **Every module in the email uses the same palette** — this is what makes a multi-module email read as one cohesive piece instead of a set of unrelated blocks. Don't let one module's background color drift to a different hue just because its own photo happens to feature a different dominant color; the palette is decided once, for the whole email, from the full set of images together.
- **A brand's default color** (the hex values in the visual/build spec — e.g. Polo's navy) is the fallback only when there's no specific campaign photography driving a different choice, per the brand guidelines' own best-practice note. A real campaign with real photography should usually derive its own palette rather than defaulting. **Exception: Lauren** — always uses its own fixed palette (cream `#F7F1EB` background, near-black `#1D1D1D` text/logo) regardless of the campaign's photography; see `lauren.md`. Don't derive a Lauren palette from photos the way you would for Polo or Home.
- **Legibility is non-negotiable** — whatever the palette, the actual applied color must keep real contrast against its text (this skill proposes the color; `rl-email-assembly` is responsible for the WCAG contrast math and darkening/lightening as needed while keeping the same hue — see `figma-workflow.md`'s color-matching technique in that skill).

## What to put in the Email Plan
Propose the palette explicitly, as part of planning, before or alongside picking images — not as an afterthought once modules are already speced. State each color's role and where it came from:
```
PALETTE
- Background: <hex or descriptive color> — sampled from <what in the photos>
- Text/CTA: <hex or descriptive color> — <white/cream/navy, whatever reads correctly against the background>
- Accent (optional): <hex or descriptive color> — <where it's used, sparingly>
```
`rl-email-assembly` takes this as given and applies it consistently across every module, running the actual pixel-sampling and contrast math against it rather than each module inventing its own color independently.
