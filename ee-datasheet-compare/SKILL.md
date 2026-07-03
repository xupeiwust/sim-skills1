---
name: ee-datasheet-compare
description: Compare electronic component datasheets, BOM alternates, and replacement candidates for electrical engineering and sourcing decisions. Use when the user asks whether one chip, module, connector, passive, power device, sensor, IC, or PCB component can replace another; asks to compare datasheet PDFs, data sheets, spreadsheets, BOM rows, pinouts, packages, ratings, timing, electrical specs, thermal specs, or external-circuit requirements; or needs a source-backed replacement-risk table for EE review.
---

# EE Datasheet Compare

## Overview

Use this skill to turn datasheets, BOM rows, candidate alternates, and part
selection notes into a concise engineering replacement check. Keep the output
source-backed and decision-oriented: AI may extract and organize evidence, but
the engineer owns final approval.

Prefer a lightweight workflow over building a hosted comparison pipeline. Use
the available file-reading skills or tools for PDFs, spreadsheets, images, and
local files; this skill supplies the EE comparison discipline and report shape.

## Workflow

1. Identify the decision.
   - Original part, candidate part, product/circuit context, and why the
     replacement is being considered.
   - Whether the user wants a drop-in decision, a candidate ranking, or a
     checklist for manual review.
   - Any fixed constraints: PCB footprint cannot change, firmware cannot
     change, same supplier required, temperature grade, automotive/medical
     qualification, cost target, or deadline.

2. Extract only decision-relevant evidence.
   - Prefer manufacturer datasheets, official product pages, BOM sheets, and
     user-provided files.
   - Preserve provenance for every important claim: page/table/figure for PDFs,
     sheet/cell/row for spreadsheets, or URL plus retrieval date for web
     sources.
   - Capture test conditions beside numeric specs. Do not compare bare values
     when temperature, supply, load, frequency, tolerance, package, or revision
     differs materially.

3. Normalize before comparing.
   - Normalize units, prefixes, min/typ/max meanings, absolute maximum versus
     recommended operating values, package names, and pin names.
   - Treat missing values as `unknown`, not as compatible.
   - Treat marketing summaries as lower-confidence than tables, pin diagrams,
     application notes, and official ordering/package tables.

4. Compare with the replacement rubric.
   - Read `references/replacement-rubric.md` when doing a real comparison or
     candidate ranking.
   - Classify each finding as `blocker`, `high`, `medium`, `low`, or `unknown`.
   - Separate facts from judgement. A changed value is a fact; whether it
     matters depends on the circuit context.

5. Deliver a compact EE review artifact.
   - Start with a verdict: `not drop-in`, `likely not drop-in`, `possible with
     review`, `likely compatible`, or `insufficient evidence`.
   - Include a red-flag table first, then supporting comparison rows.
   - End with the shortest useful bench/layout/firmware checks before approval.

## Source Discipline

- Do not claim drop-in compatibility without checking package, pinout, operating
  range, and the most relevant electrical or timing limits.
- Do not use absolute maximum ratings as operating recommendations.
- Do not hide missing evidence. Mark absent datasheet sections, unreadable
  scans, ambiguous package variants, and unclear ordering codes as `unknown`.
- If a file is scanned or extraction quality is poor, say which values need
  manual confirmation from the original PDF image.
- For live availability, lifecycle, price, distributor stock, PCN/EOL status,
  or regulatory status, browse current sources when the user asks for those
  facts or when they are necessary to answer the decision.

## Output Shape

Use this shape unless the user asks for a different artifact:

```markdown
## Verdict
<one-sentence replacement decision with confidence>

## Red Flags
| Severity | Area | Finding | Why It Matters | Source |
|---|---|---|---|---|

## Comparison
| Area | Original | Candidate | Status | Source |
|---|---|---|---|---|

## Required EE Checks
- <bench/layout/firmware/procurement check>
```

For spreadsheet or BOM candidate lists, add a ranked candidate table:

```markdown
| Rank | Candidate | Verdict | Blockers | High Risks | Evidence Gaps |
|---|---|---|---|---|---|
```

## Good Triggers

- "Can this chip replace this one?"
- "Compare these two datasheets for drop-in compatibility."
- "Procurement says this part is out of stock; check this alternate."
- "Read this BOM and flag risky replacements."
- "Compare these MOSFET/op-amp/regulator/MCU connector specs."
- "Make a datasheet comparison table for EE review."
