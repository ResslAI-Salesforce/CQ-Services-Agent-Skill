# Proposal Template Editing Guide

## Template Files

| Type | File |
|------|------|
| GxP (validation in scope) | `assets/proposal-gxp.pptx` |
| Non-GxP (no validation) | `assets/proposal-non-gxp.pptx` |

**Always choose exactly one template.** GxP if validation is in scope; non-GxP if it is not.

---

## Real-World Examples to Learn From

The IDEXX and Osmose proposals are good mental models for what a completed deck looks like:
- Large GxP engagement: 3 phases, 9 processes, multiple integrations, migration, 4 languages, ~$1M total
- Smaller non-GxP engagement: 2 phases, 9 processes, 5 integrations, migration, 1 language, ~$324K–$404K total

When in doubt about what a completed slide should look like, refer to those patterns rather than the blank template.

---

## Slide Map

### GxP Template — 28 slides base
### Non-GxP Template — 27 slides base
### Multi-phase engagements add investment slides (one per phase)

| Slide | Title | Edit? | Notes |
|-------|-------|-------|-------|
| 1 | Title slide | ✅ YES | Customer name, date |
| 2 | Agenda | ❌ NO | Boilerplate |
| 3 | "Our Understanding" section divider | ❌ NO | Boilerplate |
| 4 | Our understanding of your needs | ✅ YES | Key customer-specific slide |
| 5 | "Implementation Approach" section divider | ❌ NO | Boilerplate |
| 6 | ABCD Methodology | ❌ NO | Boilerplate |
| 7 | ABCD Steps and Key Milestones | ❌ NO | Boilerplate |
| 8 | CQ Project Resources | ❌ NO | Boilerplate |
| 9 | Client Project Resources | ❌ NO | Boilerplate |
| 10 | Project Team Hierarchy | ❌ NO | Boilerplate |
| 11 | "Implementation Timelines" section divider | ❌ NO | Boilerplate |
| 12 | Implementation Scope | ✅ YES | Phase/process/track breakdown |
| 13 | Proposed Program Timeline | ✅ YES | High-level multi-phase Gantt |
| 14 | Sample Phase 1 Timeline | ✅ YES | Week-by-week Phase 1 detail |
| 15 | Project Activities | ❌ NO | Boilerplate |
| 16 | Project Management Deliverables | ❌ NO | Boilerplate |
| 17 | Migration Deliverables | ⚠️ CONDITIONAL | Keep if migration in scope; remove if not |
| 18 | Project Deliverables | ❌ NO | Boilerplate |
| 19 (GxP) | Validation Deliverables | ⚠️ CONDITIONAL | GxP only; remove if not CQ-led validation |
| 20/19 | "Implementation Investments" section divider | ❌ NO | Boilerplate |
| 21/20 | **Phase 1** Investment | ✅ YES | Core commercial slide |
| 22/21 | **Phase 1 Additional** (integration + migration) | ⚠️ CONDITIONAL | If integration or migration in scope |
| ➕ | **Phase 2** Investment | ✅ DUPLICATE if 2+ phases | Duplicate Phase 1 slide, edit for Phase 2 |
| ➕ | **Phase 2 Additional** | ⚠️ CONDITIONAL | If Phase 2 has integration/migration |
| ➕ | **Phase 3** Investment | ✅ DUPLICATE if 3 phases | Duplicate and edit for Phase 3 |
| last-3 | Additional Services — Language Translation | ⚠️ CONDITIONAL | If language translation in scope |
| last-2 | Total Implementation Investments | ✅ YES | All-phase summary table |
| last-1 | "Assumptions" section divider | ❌ NO | Boilerplate |
| last | In Scope Assumptions | ✅ YES | Customer-specific assumptions |
| last | Out of Scope Assumptions | ✅ YES | Customer-specific out-of-scope |
| final | Footer / website slide | ❌ NO | Boilerplate |

---

## Slide-by-Slide Editing Instructions

### Slide 1 — Title Slide
Replace:
- `for <Customer Name> QMS Implementation` → actual customer name and implementation type
  - e.g. `for IDEXX QMS Implementation` or `for Osmose QEHS Implementation`
- Date → proposal date
- Remove `Insert Customer Logo` text if present

---

### Slide 4 — Our Understanding of Your Needs

Seven numbered items. Fill each one concretely — no placeholders, no "TBD".

| # | Header | How to fill it |
|---|--------|---------------|
| 1 | Current State | 1–2 specific sentences: what systems they use today, what pain they have. e.g. "Numerous systems spread out over multiple sites / Looking to harmonize QMS processes globally" |
| 2 | Looking to Implement the following QMS processes | List every in-scope process by name. e.g. "Document / Digital SOP, Training, Change, Nonconformance, Complaints, CAPA" |
| 3 | Integrations | Name every integration system. e.g. "Integration with SAP DM, Salesforce, Sharepoint, Smartsheet, APEXX, IDEXX.com, Maximo and Transcat C3." — or "Not in scope" |
| 4 | Data Migration | Volume and type. e.g. "60,000 Documents / 200,000 Training Records" — or "Not in scope" |
| 5 | UAT Testing | e.g. "Customer led and CQ Supported UAT" — or "Jointly led by CQ and Customer" |
| 6 | Language Translations | Specific languages. e.g. "Language Translations to 4 Languages included in Scope" — or "Not in scope" |
| 7 | Rollout | e.g. "Managed by Customer, CQ Supported" |

**Key rule:** Never leave placeholder text. Every item must have real content. If a workstream is not in scope, write "Not in scope" explicitly.

---

### Scope Recap Slides (insert after Slide 4)

After the "Our Understanding" slide, insert **one or two slides** that recap the estimate and staffing outputs in slide-friendly language. These are new slides added to the deck — not replacements for existing slides.

**What to include:**

Slide A — **Estimate Recap**
- Configuration effort summary: key requirements identified, total CC hours, padded total
- Role breakdown: CC / SC / SA / PM hours and cost
- Optional workstreams in scope: integration range, migration cost, validation cost, language cost
- Total investment range (low–high)

Slide B — **Delivery Recap** (only if the engagement is complex enough to warrant it)
- Phase structure: Phase 1 / Phase 2 / Phase 3 and what's in each
- Duration: total weeks per phase, overall project duration
- Key milestones: Config lock, UAT, Go-live

**Format guidance:**
- Keep both slides visually simple — bullet points or a clean two-column layout
- Match the visual style of the surrounding slides (font, color, no extra design elements)
- These slides are internal-facing summaries, not polished customer prose — crisp, factual, numbers-forward
- Use `add_slide.py` to insert them after slide 4; do not squeeze them into existing slides

---

### Slide 12 — Implementation Scope

Two tracks — Functional Track and Technical Track.

**Functional Track:**
- Group processes by phase: Phase 1, Phase 2, Phase 3 (only as many phases as exist)
- Each phase lists its processes by name, one per line
- Example (IDEXX):
  - Phase 1: Document / Digital SOPs, Training, Change
  - Phase 2: Deviation / Nonconformance, CAPA, Complaints
  - Phase 3: Audits, Risk / FMEA, Supplier

**Technical Track:**
- Integration row: Name all integration systems (or remove if none)
- Analytics row: "Operational Reports / Operational Dashboards / Management KPIs" — leave as-is
- Migration row: Specific volume. e.g. "~60,000 Documents / Records across multiple sites / 200,000 Training Records" — or remove if no migration
- Language Translations row: Specific languages. e.g. "Language Translations to four languages are included in scope" — or remove if none

**Space constraint:** This slide is compact. Keep process names short. If content overflows, abbreviate — never expand the text box.

---

### Slide 13 — Proposed Program Timeline

Month-by-month Gantt showing all phases.

What to edit:
- Number of month columns: match actual total project duration (IDEXX = 25 months, Osmose = 16 months)
- Phase rows: match actual phases in scope (remove Phase 2/3 rows if single-phase)
- The column header row repeats (M1–Mn appears twice — keep both in sync)
- Remove "Only Applicable to Multi Phase Implementation" note if single-phase

---

### Slide 14 — Sample Phase 1 Timeline

Week-by-week detail for Phase 1.

What to edit:
- Week count columns: match actual Phase 1 duration in weeks (Osmose = 25 weeks / 6 months, IDEXX = 31 weeks / 8 months)
- Month header labels: update to match week count (Month 1 through Month N)
- Activity row shading blocks: adjust to show which weeks each activity spans
- **For GxP:** add a "Validation" row between "Final Testing and Configuration Lock" and "Training" — IDEXX shows this pattern
- **For large projects:** rename "Iteration" rows to "Sprint" if more iterations are needed

Leave row label text unchanged unless adding the Validation row for GxP.

---

### Slide 17 — Migration Deliverables (Conditional)
- Keep if migration is in scope — content is boilerplate, do not edit
- Remove entirely if migration is not in scope

---

### Slide 19 — Validation Deliverables (GxP only, Conditional)
- Keep if CQ-led validation is in scope — content is boilerplate, do not edit
- Remove entirely if not applicable

---

## Investment Slides — Multi-Phase Structure

This is the most complex part. The template has Phase 1 only. For multi-phase engagements, **duplicate** the Phase 1 investment slide and edit it for each additional phase.

### Phase N — Core Implementation Slide

Each phase gets its own investment slide. Fill in:

**Header line:** `Phase N Implementation - up to X to Y Week Project`
— derive week range from staffing plan (IDEXX Phase 1 = 26–30 weeks, Osmose Phase 1 = 22–24 weeks)

**Scope description bullets** (replace the generic bullets with actual scope):
- `Project Manager, SME and configuration support through the entire project`
- `Integration with [SSO system]`
- `System Configuration for [N] processes: [list them]`
- `Integration with [named systems]` — only if integration is in this phase
- `Data Migration of [volume]` — only if migration is in this phase
- `Application Training: [N] consecutive Train-the-Trainer sessions`
- `Deployment to Production`

**Fee range:** derived from estimate (e.g. `$230,000 - $250,000`)

**Validation Services row (GxP only):** include with its fee range; remove for non-GxP

**Hypercare:** update hours and duration. e.g.:
- Small phase: 40 hours / 4 weeks / `$10,000`
- Large phase: 80 hours / 4 weeks / `$20,000`
- Very large phase: 160 hours / 8 weeks / `$20,000`

**Phase total line:** sum of all line items for this phase

### Phase N Additional Services Slide (Conditional)

Only include if this phase has integration OR migration. Duplicate from template.

- **Integration block:** list specific systems and fee range
- **Migration block:** volume, approach summary, duration, fee range
- Remove whichever block is not in scope for this phase

### Language Translation Slide (Conditional)

Include once (not per phase) if language translation is in scope. Update:
- Number of languages in the description: "Language Translations (up to N Languages)"
- Fee: `$4,000 × N languages`
- Total line

### Total Implementation Investments Slide

One summary table with all phases. Build rows to match what's in scope:

| Phase | Costs |
|-------|-------|
| Phase 1 – Implementation Services | $X – $Y |
| Phase 1 – Additional Implementation Services | $X – $Y (if applicable) |
| Phase 2 – Implementation Services | $X – $Y (if multi-phase) |
| Phase 2 – Additional Implementation Services | $X – $Y (if applicable) |
| Phase 3 – Implementation Services | $X – $Y (if 3 phases) |
| Additional Services (Language Translations) | $X (if applicable) |
| **Total Implementation Investments** | **$X – $Y** |

Remove any rows that are not in scope. The total must match the sum of all rows.

---

### In Scope Assumptions Slide

Sections: General, Configuration & Validation (GxP) / Configuration (non-GxP), Training, Integration & Data Migration, Language Translation.

What to make customer-specific:
- **Integration & Data Migration section:** replace generic bullets with specifics:
  - "CQ will support the development of up to N integrations" (name count or systems)
  - "CQ will support migration of up to X Documents, Y Training Records"
- **Language Translation section:** name specific languages or remove if not in scope

Keep all other bullets unchanged.

---

### Out of Scope Assumptions Slide

Standard 5-item numbered list. Keep as-is unless something standard is actually in scope for this customer (then remove that item). May add a customer-specific out-of-scope item if relevant.

---

## Formatting Rules (Critical)

1. **Always start from the template** — unpack and edit the XML, never recreate from scratch
2. **Preserve all visual design** — fonts, colors, paragraph styles, shapes, branding, images, dividers
3. **Body text stays body style** — do not let edited text inherit heading styling
4. **No yellow highlighting** — do not add highlight formatting
5. **Implementation Scope slide is space-constrained** — compress; never overflow
6. **Conditional slides:** remove cleanly from sldIdLst; do not leave empty placeholder slides
7. **Duplicated slides for phases:** use `add_slide.py` to duplicate properly — never manually copy XML files

---

## Generation Workflow

```
1. Determine GxP or non-GxP → pick template file
2. Read /mnt/skills/public/pptx/SKILL.md and /mnt/skills/public/pptx/editing.md
3. Study reference proposals if unsure what a slide should look like
4. Unpack:
   python scripts/office/unpack.py assets/proposal-[gxp|non-gxp].pptx unpacked/
5. Plan slide structure:
   - How many phases? → how many investment slides to add
   - Migration in scope? → keep slide 17
   - Validation in scope? → keep slide 19 (GxP only)
   - Language translation? → keep language slide
6. Add phase slides (for Phase 2, 3):
   python scripts/add_slide.py unpacked/ slide21.xml   # duplicate Phase 1 slide
   (insert new <p:sldId> in presentation.xml at right position)
7. Remove unwanted slides from ppt/presentation.xml sldIdLst + run clean.py
8. Edit each customer-specific slide XML
9. Clean: python scripts/clean.py unpacked/
10. Pack: python scripts/office/pack.py unpacked/ output.pptx --original assets/proposal-[gxp|non-gxp].pptx
11. QA:
    extract-text output.pptx   # check all content, no placeholders
    grep for "<Customer Name>", "Process…", "Process..", "Integrations in Scope", "Insert"
    Visual QA via soffice + pdftoppm
```
