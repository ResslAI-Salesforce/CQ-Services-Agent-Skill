---
name: cq-services-workflow
description: >
  Orchestrates a 3-stage services engagement workflow for ComplianceQuest/Salesforce pre-sales teams.
  Use this skill whenever a user provides a pre-scoping questionnaire, call transcript, or scoping notes
  and wants to generate a Configuration Estimate, Staffing Plan, Customer Proposal, or Statement of Work.
  Also trigger when the user says things like "generate the estimate", "approved", "generate the proposal",
  "generate the SOW", "proceed to proposal", or provides source documents for a services engagement.
  This skill manages shared engagement context across the conversation, enforces approval gates between stages,
  and produces professionally formatted customer-facing documents (Excel staffing plan, PowerPoint proposal deck,
  Word SOW). Trigger even for partial inputs — the skill can proceed with questionnaire only or transcript only.
---

# CQ Services Workflow Skill

A staged document-generation workflow for CQ/Salesforce services engagements. Produces three deliverables in sequence: (1) Configuration Estimate + Staffing Plan, (2) Customer Proposal, (3) Statement of Work.

---

## Shared Engagement Context

Maintain this state mentally across the entire conversation. Never reset it between turns. Update it as new material arrives.

```json
{
  "engagement_context": {
    "customer_name": "",
    "questionnaire_text": "",
    "transcript_text": "",
    "assumptions": [],
    "open_questions": [],
    "artifacts": {
      "proposal_template": null,
      "sow_template": null
    },
    "latest_outputs": {
      "configuration_estimate": null,
      "staffing_plan": null,
      "proposal_document": null,
      "sow_document": null
    },
    "approval_status": {
      "estimate_approved": false,
      "proposal_approved": false
    }
  }
}
```

**Key rules:**
- New source material updates the context — it does not wipe it
- Structured outputs are more authoritative than older prose summaries
- Downstream outputs must always use the latest approved upstream outputs
- If new questionnaire/transcript arrives after a stage is complete, re-evaluate upstream stages before regenerating downstream ones

---

## Workflow Stages

```
[Source Material] → Stage 1: Estimate + Staffing Plan
                              ↓ (user approves)
                    Stage 2: Proposal Deck
                              ↓ (user approves)
                    Stage 3: Statement of Work
```

**Advance only when:**
- User explicitly approves ("approved", "looks good", "go ahead", "proceed", "green light", "move forward"), OR
- User explicitly asks to generate the next document

**If user asks for revisions:** revise the current stage, do not advance.

---

## Stage Routing Logic

On each user message, determine the current state:

| Condition | Action |
|---|---|
| No estimate exists | Run Stage 1 |
| Estimate exists, not approved | Revise estimate or wait |
| Estimate approved, no proposal | Run Stage 2 |
| Proposal exists, not approved | Revise proposal or wait |
| Proposal approved, no SOW | Run Stage 3 |
| New source docs arrive at any stage | Update context, re-evaluate affected stages |

---

## Stage 1: Configuration Estimate + Staffing Plan

### Input
- Pre-scoping questionnaire (preferred)
- Call transcript / scoping notes
- User instructions
- Can proceed with either one alone; state assumptions if context is thin

### Requirement-Level Estimation

For **each customer requirement**, produce:

```
Requirement: <name>
Business summary: <what the customer wants>
Technical basis: <what this means in Salesforce/CQ terms>
Work items:
  - <item>: <low>–<high> hrs  (reason: <complexity basis>)
  - <item>: <low>–<high> hrs
Requirement CC hours: <low>–<high>
```

Work item complexity anchors:
- New field: ~1 hr
- Field + UI config: ~3 hrs
- Quick action: 2–3 hrs
- Validation rule: 3–4 hrs
- RT flow / Apex logic: 1–2 days
- CQ approval: 2.5–4 days depending on complexity
- Simple declarative config (Level 1): small items
- Moderate logic/configuration (Level 2): mid-range items
- Heavy logic with dependencies (Level 3): larger items, split if >5 days

**Rule:** If a work item exceeds ~5 days, split it. Ranges must be tight — don't widen just because the project matters. Totals come from work-item rollups, not top-down intuition.

### Estimate Math (deterministic)

```
Base CC hours     = sum of all requirement-level CC hours
Padded CC hours   = Base CC × 1.33  (round to whole number)
SC hours          = Padded CC × 0.50
SA hours          = Padded CC × 0.20
PM hours          = Padded CC × 0.20
```

Rates:
- CC: $60/hr | SC/SA: $125/hr | PM: $150/hr | TA: $150/hr | Validation: $125/hr

Optional workstreams (include only if supported by source material):
- **Integration:** TA hours at $150/hr, produce lower/upper range
- **Data migration:** Fixed setup = 32 hrs × $150/hr = $4,800. Variable = total record volume × $0.30/record
- **Validation:** 33% of configuration subtotal (before validation/translation) → convert to hours at $125/hr
- **Language translation:** $4,000/language

Round all final hours and costs to whole numbers.

### Estimate Output Style

Produce a crisp summary with these sections:
1. **Commercial Summary** — total investment range, duration, customer name
2. **Configuration Logic** — requirements list, technical basis, requirement-level CC hours, base CC total, 33% pad, SC/SA/PM additions, resulting totals
3. **Additional Workstreams** — only those in scope
4. **Assumptions & Constraints**

The Configuration Logic section is the most important. It should read: "Here is how the estimate was derived from requirements" — not a narrative summary.

Then attach a **Staffing Plan Excel workbook** (see Stage 1.5 below).

---

## Stage 1.5: Staffing Plan (Excel Workbook)

The staffing plan is a scheduling expression of the estimate — not a second estimate.

**Role totals must exactly match the estimate math above.**

### Generation

**Read before generating:**
1. `/mnt/skills/public/xlsx/SKILL.md`
2. `references/staffing-plan-guide.md` ← full structure, logic, and validation checklist

**Use the generation script** — do not hand-code the spreadsheet:

```bash
python scripts/generate_staffing_plan.py '<json>' output.xlsx
python scripts/recalc.py output.xlsx
```

The script takes the estimate role-hours, computes proportional phase durations, distributes hours per role across weeks, handles GxP vs non-GxP (Validation Specialist row), adjusts column count to match project duration, and outputs a properly formatted workbook based on the bundled template.

JSON input (build from estimate outputs):
```json
{
  "customer_name": "...",
  "processes": ["...", "..."],
  "is_gxp": false,
  "start_date": "YYYY-MM-DD",
  "roles": {
    "cc_hours": <padded CC>,
    "sc_hours": <SC>,
    "sa_hours": <SA>,
    "pm_hours": <PM>,
    "ta_hours": <TA or 0>,
    "validation_hours": <val or 0>
  },
  "hypercare_hours": 40,
  "rollout_hours": 0
}
```

After running, verify role totals match the estimate exactly (checklist in staffing-plan-guide.md).

---

## Stage 2: Proposal Document

### Input
- Approved estimate + staffing plan (required)
- Questionnaire + transcript (from context)
- Optional proposal template/source deck

### Truth Priority
1. Latest approved structured estimate/staffing outputs
2. Questionnaire and transcript
3. Earlier prose summaries

### Content Rules

- Determine GxP vs non-GxP from validation scope and customer type
- Stay tightly aligned to approved estimate and staffing plan
- Reuse same process names, workstream names, delivery language
- Do not invent integrations, migration, validation, analytics, or processes not in scope
- If something is optional or out of scope upstream, reflect that plainly
- If the implementation scope slide is crowded, compress text — do not overflow

### Boilerplate slides (generally leave unchanged):
- Agile/ABCD methodology slides
- Project activities boilerplate
- Project management/governance boilerplate

### Customer-specific slides (edit these):
- Understanding / current state
- Implementation scope
- Timeline / phase timeline
- Deliverables
- Implementation investment / commercial
- Assumptions
- In-scope / out-of-scope

### Template Editing Rules (CRITICAL)

**Before generating, read in this order:**
1. `/mnt/skills/public/pptx/SKILL.md`
2. `/mnt/skills/public/pptx/editing.md`
3. `references/proposal-template-guide.md` ← slide-by-slide instructions for these specific templates

**Template files:**
- `assets/proposal-gxp.pptx` — use when validation is in scope
- `assets/proposal-non-gxp.pptx` — use when validation is NOT in scope

The proposal-template-guide.md contains:
- Complete slide map: which of the 27–28 slides to edit, leave alone, or conditionally remove
- Slide-by-slide editing instructions for every customer-specific slide
- Formatting rules and generation workflow (unpack → edit → clean → pack → QA)

Never recreate the deck from scratch. Always unpack and edit the template XML.

### Output

- Updated proposal content summary (brief)
- Proposal deck as `.pptx` attachment
- Brief written note: customer type, estimated investment, estimated duration, scope recap, next step

---

## Stage 3: Statement of Work

### Input
- Approved proposal (required)
- Approved estimate + staffing plan
- Questionnaire + transcript (from context)
- Optional SOW template/source document

### Content Rules

The SOW is contract-ready. Wording should be contractual and crisp — not salesy.

Source priority:
- Proposal → strongest source for scope, phasing, deliverables
- Estimate/staffing → strongest source for commercial posture and duration

Include these sections (suppress entirely if not applicable):
- Customer name + scope overview
- Phase/module structure
- Pricing summary
- Integration narrative (if in scope)
- Migration narrative + scope table (if in scope)
- Validation narrative (if in scope)
- Language translation narrative (if in scope)
- Duration narrative
- Appendix summary

**Explicit rule:** If a workstream (e.g., language translation) is not in scope, omit that section entirely — do not force filler content.

Do not invent missing details. If something is unknown, note it will be finalized in contracting.

### Template Editing Rules (CRITICAL)

**Before generating, read in this order:**
1. `/mnt/skills/public/docx/SKILL.md`
2. `references/sow-template-guide.md` ← section-by-section editing instructions for this specific template

**Template file:** `assets/sow-template.docx`

The sow-template-guide.md contains:
- A full section map (edit / leave alone / conditional)
- Instructions for every section: pricing table, scope module list, migration scope table, duration, Appendix A
- A conditional section summary: which sections to keep or remove based on what is in scope
- The exact generation workflow (unpack → edit → pack → QA)

Key rule: if a workstream (integration, migration, validation, language translation) is not in scope, remove its section entirely. Never leave placeholder or filler text.

### Output

- SOW as `.docx` attachment

---

## Regeneration Rules

If new source material arrives after a stage is complete:
1. Update the shared engagement context
2. Re-evaluate estimate if questionnaire/transcript changed materially
3. Re-evaluate staffing if estimate changed
4. Regenerate proposal/SOW based on latest approved upstream content
5. Inform the user what changed and what was regenerated

---

## Edge Cases

| Situation | Behavior |
|---|---|
| Questionnaire only | Proceed, state assumptions |
| Transcript only | Proceed, state assumptions |
| Both available | Use both |
| Ambiguous approval | Ask once before advancing |
| User asks for revisions | Revise current stage, do not advance |
| New docs arrive mid-flow | Update context, re-evaluate affected upstream stages |
| Section not applicable | Omit cleanly from SOW/proposal |

---

## Non-Goals

- Do not act as a general-purpose assistant during workflow
- Do not invent scope not supported by source material
- Do not generate arbitrary broad estimate ranges
- Do not recreate PPTX/DOCX visuals from scratch
- Do not change template styling unnecessarily
- Do not generate both GxP and non-GxP staffing plans
- Do not force out-of-scope sections into proposal or SOW
- Do not advance stages without approval or explicit instruction

---

## Document Generation

Before generating any file:
- Excel: read `/mnt/skills/public/xlsx/SKILL.md`
- PowerPoint: read `/mnt/skills/public/pptx/SKILL.md`
- Word: read `/mnt/skills/public/docx/SKILL.md`
