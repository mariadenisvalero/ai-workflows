# Section naming convention

Confirmed by the user against a real file-naming example (originally documented for standalone Figma/COMPS/Slices files, now reused as the **section name** inside the shared monthly template):

```
{Year}{Month}FW{FiscalWeek}_{Region}_{Brand-Gender-code}-{Campaign-name}
```

Real example: `2024NovFW2_RLNA_M-Polo-Season-of-Sweaters-Holiday`

Breaking that down:
- `2024Nov` — calendar year + month name (not zero-padded, month spelled out, no separator between them)
- `FW2` — **fiscal week**, not calendar week (RL fiscal year: Q1 Apr/May/Jun, Q2 Jul/Aug/Sep, Q3 Oct/Nov/Dec, Q4 Jan/Feb/Mar — see `glossary.md`; the fiscal-week number itself isn't in any source doc this skill has, so **always take it as a direct input, never compute it**)
- `RLNA` — region code. `RLNA` = Ralph Lauren North America is the one confirmed value. Ask if a campaign is for a different region rather than assuming RLNA by default.
- `M-Polo` — brand + gender code. **Only this one combination is confirmed** (Men's Polo). Don't invent codes for other brand/gender combinations (e.g. guessing "W-Lauren" or "RLH" for Home) — ask the user for the exact fragment they want the first time each brand/sub-line comes up, then it's safe to reuse.
- `Season-of-Sweaters-Holiday` — campaign name, Title-Case-With-Hyphens-Instead-Of-Spaces, however many words it takes to be recognizable.

## Building the name for a new section
Take: fiscal year, month, fiscal week, region, brand-gender code, campaign name. All six are direct inputs — either the user gives them, or enough of them come from the Skill 1 Email Plan (brand, sub-line, implied campaign name from the brief) that only the fiscal/timeline pieces need asking. **Never guess the fiscal week number or an unconfirmed brand-gender code** — ask once, and treat the answer as reusable for that combination going forward.
