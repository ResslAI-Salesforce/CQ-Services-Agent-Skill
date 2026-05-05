# CQ Services Workflow Skill

A Claude skill that orchestrates a **3-stage services engagement workflow** for ComplianceQuest/Salesforce pre-sales teams.

## What It Does

Given a pre-scoping questionnaire or call transcript, this skill guides Claude through generating three professional deliverables in sequence:

| Stage | Output | Format |
|-------|--------|--------|
| 1 | Configuration Estimate + Staffing Plan | Inline summary + `.xlsx` |
| 2 | Customer Proposal Deck | `.pptx` |
| 3 | Statement of Work | `.docx` |

Each stage requires explicit user approval before advancing. The skill maintains engagement context across the full conversation.

---

## Repo Structure

```
cq-services-workflow/
├── SKILL.md                          # Skill definition + full workflow instructions
├── cq-services-workflow.skill        # Packaged skill file (install this)
├── assets/
│   ├── proposal-gxp.pptx             # Proposal template (GxP/validation engagements)
│   ├── proposal-non-gxp.pptx         # Proposal template (non-GxP engagements)
│   ├── sow-template.docx             # Statement of Work template
│   ├── staffing-plan-template.xlsx   # Staffing plan base workbook
│   └── rate-card.docx                # Role rate reference
├── references/
│   ├── proposal-template-guide.md    # Slide-by-slide editing instructions
│   ├── sow-template-guide.md         # Section-by-section SOW editing instructions
│   └── staffing-plan-guide.md        # Staffing plan structure + validation checklist
└── scripts/
    └── generate_staffing_plan.py     # Staffing plan generation script
```

---

## Installation

### Option A: Install the `.skill` file (recommended)
1. Download `cq-services-workflow.skill`
2. In Claude, go to **Settings → Skills → Install from file**
3. Select the `.skill` file

### Option B: Manual install
Copy the entire `cq-services-workflow/` folder into your Claude skills directory (typically `~/.claude/skills/`).

---

## Trigger Phrases

Claude will activate this skill when you say things like:
- "Here's the pre-scoping questionnaire for [Customer]"
- "Generate the estimate"
- "Approved — generate the proposal"
- "Generate the SOW"
- "Proceed to the next stage"

It also triggers when you upload a call transcript or scoping notes alongside an engagement request.

---

## Workflow Overview

```
[Questionnaire / Transcript]
         │
         ▼
  Stage 1: Estimate + Staffing Plan  ──► User approves
         │
         ▼
  Stage 2: Proposal Deck             ──► User approves
         │
         ▼
  Stage 3: Statement of Work
```

The skill enforces approval gates — it will not advance stages without explicit sign-off.

---

## Estimation Logic

- Requirements are broken down individually with work-item-level hour ranges
- Complexity anchors: new field (~1 hr), quick action (2–3 hrs), CQ approval (2.5–4 days), etc.
- Padded CC hours = Base CC × 1.33
- SC = 50% of padded CC | SA = 20% | PM = 20%
- Optional workstreams: Integration (TA), Data Migration, Validation (33% of config), Language Translation ($4k/language)

---

## Dependencies

This skill references these other public skills (must be installed alongside it):
- `xlsx` — for staffing plan generation
- `pptx` — for proposal deck editing
- `docx` — for SOW generation

---

## Maintainers

ComplianceQuest Pre-Sales Team
