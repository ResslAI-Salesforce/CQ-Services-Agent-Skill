# SOW Template Editing Guide

## Template File

`assets/sow-template.docx` — always edit this file; never build a SOW from scratch.

---

## Document Structure Overview

| Section | Title | Edit? |
|---------|-------|-------|
| Header | Statement of Work SOW-1 / Customer Name | ✅ YES |
| Preamble | Legal intro (MSA reference) | ✅ PARTIAL — customer name only |
| 1 | Scope | ✅ YES — phase/module list |
| 2 | Professional Services Estimate | ✅ YES — pricing table + duration |
| 3.1 | Project Kick-off | ❌ NO — boilerplate |
| 3.2 | Solution Design and Configuration | ❌ NO — boilerplate |
| 3.3 | Integration with Third-Party Systems | ⚠️ CONDITIONAL — keep/edit if in scope, suppress if not |
| 3.4 | Data Migration (CQ Led) | ⚠️ CONDITIONAL — use CQ-led OR Customer-led, not both |
| 3.4 | Data Migration (Customer Led) | ⚠️ CONDITIONAL — use only if customer is leading migration |
| 3.4 | Validation | ⚠️ CONDITIONAL — keep if GxP/validation in scope, suppress if not |
| 3.6 | Training and Deployment | ❌ NO — boilerplate |
| 3.7 | Hypercare and Stabilization | ❌ NO — boilerplate |
| 3.8 | Language Translations | ⚠️ CONDITIONAL — keep if in scope, suppress entirely if not |
| 4.1 | CQ Resourcing | ❌ NO — boilerplate (remove Validation Specialist bullet if non-GxP) |
| 4.2 | Customer Resourcing | ❌ NO — boilerplate |
| 5 | Project Deliverables | ✅ PARTIAL — remove validation rows if non-GxP |
| 6 | Project Assumptions | ✅ PARTIAL — update duration, language line |
| Payment Terms | Payment Terms & Schedule | ❌ NO — boilerplate |
| Signature Block | Parties' signatures | ✅ YES — customer name only |
| Appendix A | Scoping summary | ✅ YES — fill with engagement summary |

---

## Section-by-Section Editing Instructions

### Title / Header

Replace:
- `<Customer Name>` → actual customer name (appears in title, preamble, and signature block)
- `Customer Name ("Customer")` in preamble → e.g. `Acme Corp ("Customer")`
- `_____________________` (MSA date blank) → leave as-is or fill if known

Do not change any legal language in the preamble.

---

### Section 1 — Scope

Update the module/phase bullet list to reflect the actual engagement.

Pattern to follow:
```
- Phase 1
  - [Module 1]
  - [Module 2]
- Phase 2
  - [Module 3]
  - [Module 4]
```

Use the approved estimate/proposal as the source of truth for which modules belong in which phase. Remove phases that are not in scope. Add phases if needed.

Do not change the surrounding scope narrative ("Customer is seeking an implementation of CQQMS...") — edit only the bullet list.

---

### Section 2 — Professional Services Estimate

This section contains:
- A pricing table placeholder: `**Insert Pricing Table**`
- A discount note
- Payment notes (boilerplate)

**Replace `**Insert Pricing Table**` with an actual table.** Format:

| Role | Hours | Rate | Estimated Cost |
|------|-------|------|----------------|
| Configuration Consultant (CC) | X hrs | $60/hr | $X |
| Solution Consultant (SC) | X hrs | $125/hr | $X |
| Solution Architect (SA) | X hrs | $125/hr | $X |
| Project Manager (PM) | X hrs | $150/hr | $X |
| Technical Architect (TA) | X hrs | $150/hr | $X (if integration in scope) |
| Validation Consultant | X hrs | $125/hr | $X (if validation in scope) |
| **Configuration Subtotal** | | | **$X** |
| Data Migration (setup + variable) | | | $X (if in scope) |
| Language Translation | X languages | $4,000/language | $X (if in scope) |
| **Total Estimated Investment** | | | **$X – $X** |

Rates source: CC $60/hr · SC/SA $125/hr · PM $150/hr · TA $150/hr · Validation $125/hr. These take precedence over the rate card document in case of any conflict.

**Do not change** the discount note paragraph or any of the payment notes bullet points below the table.

---

### Section 3.1 — Project Kick-off

Leave entirely unchanged. Boilerplate.

---

### Section 3.2 — Solution Design and Configuration

Leave entirely unchanged. Boilerplate.

---

### Section 3.3 — Integration with Third-Party Systems

**If integration IS in scope:**
- Keep this section
- Replace `- Description of Integrations` bullet with actual integration descriptions, e.g.:
  - `Integration with Azure SSO for user authentication`
  - `Integration with Workday for user provisioning`
- Leave all other bullets unchanged (boilerplate responsibilities)

**If integration is NOT in scope:**
- Remove this entire section from the document

---

### Section 3.4 — Data Migration

The template contains two versions of 3.4:
- **CQ Led** — CQ manages the migration
- **Customer Led** — Customer manages the migration

**Use only ONE version.** Remove the other entirely.

**If CQ-led migration IS in scope:**
- Keep the CQ Led section
- Remove the Customer Led section
- Update the **Migration Scope table**:

| Migration Scope | In/Out | Volume |
|----------------|--------|--------|
| [Record type 1] | In-Scope | Approximately X records from [source system] |
| [Record type 2] | In-Scope | Approximately X records from [source system] |

Use volumes from the questionnaire/transcript. If volumes are unknown, write "To be confirmed during discovery."

- Leave all other migration narrative bullets unchanged (boilerplate)

**If migration is NOT in scope:**
- Remove both 3.4 sections entirely

---

### Section 3.4 — Validation

**If validation IS in scope (GxP engagement):**
- Keep this section
- Leave all content unchanged — it is boilerplate

**If validation is NOT in scope (non-GxP engagement):**
- Remove this section entirely

---

### Section 3.6 — Training and Deployment

Leave entirely unchanged. Boilerplate.

---

### Section 3.7 — Hypercare and Stabilization

Leave entirely unchanged. Boilerplate.

---

### Section 3.8 — Language Translations

**If language translation IS in scope:**
- Keep this section
- Update the opening line to reflect actual number of languages:
  - e.g. `Language translations of up to two (2) languages are included in the Scope of this SOW.`
- Leave all other bullets unchanged — boilerplate

**If language translation is NOT in scope:**
- Remove this section entirely — do not leave it with placeholder or N/A text

---

### Section 4.1 — CQ Resourcing

Leave mostly unchanged — boilerplate role descriptions.

**One conditional edit:**
- If this is a non-GxP engagement (no validation), remove the `Validation Specialist` bullet from Section 4.1
- If GxP, leave the `Validation Specialist` bullet as-is

---

### Section 4.2 — Customer Resourcing

Leave entirely unchanged. Boilerplate.

---

### Section 5 — Project Deliverables

The deliverables table has mostly boilerplate rows.

**Conditional rows to remove for non-GxP:**
- Risk Assessment
- Installation Qualification (IQs)
- Operational Qualifications (OQs)
- PQ Scripts (Updated to Customer's Configurations)

Keep all other rows unchanged.

---

### Section 6 — Project Assumptions

This section is mostly boilerplate. Make only two targeted edits:

1. **Duration:** Find and update:
   - `The approximate duration of the implementation is twenty (20) months` → update to actual duration from staffing plan (e.g., `six (6) months`)
   - `Any changes to the project's duration of twenty (20) months` → update the same number

2. **Language translation:** Find and update or remove:
   - `Language translation of the CQQMS System is out of scope of this Order Form.` → remove this line if translation IS in scope
   - If translation is not in scope, leave this line as-is

Leave all other assumption bullets unchanged.

---

### Signature Block

Update both cells in the signature table:
- Right cell: Replace `CUSTOMER NAME` with actual customer name

Left cell (ComplianceQuest side) — leave unchanged.

---

### Appendix A

Appendix A is a synthesis of the pre-scoping questionnaire and the scoping call. It is the narrative foundation for the implementation — not a marketing summary and not a copy-paste of raw notes.

Write it as structured prose with the following coverage:

**1. Customer Background and Current State** (2–3 sentences)
- Who the customer is, their industry, size, or regulatory context if known
- What systems or processes they use today
- The core pain point driving the engagement

**2. Processes in Scope** (1–2 sentences or a short bulleted list)
- Which QMS/QEHS modules they want to implement
- How they are grouped into phases if multi-phase

**3. Technical Scope** (1 paragraph, only include items that are in scope)
- Integration: which systems, what the integration purpose is
- Data migration: what types of records, approximate volumes, source system(s)
- Validation: whether CQ-led or customer-led, high-level approach
- Language translation: which languages

**4. Key Constraints and Assumptions Surfaced During Scoping** (2–4 bullet points)
- Any commitments the customer made (e.g. "customer will harmonize processes prior to kick-off")
- Any significant unknowns flagged during the call that will be resolved in discovery
- Timeline constraints if mentioned

**Tone and length:** Contractual and factual. 300–500 words is appropriate. Do not use marketing language. Do not invent details — if something was not discussed, omit it rather than speculate. If a key detail is unknown (e.g. exact record volumes), note it will be confirmed during the migration planning phase.

---

## Conditional Section Summary

| Workstream | If In Scope | If Out of Scope |
|------------|-------------|-----------------|
| Integration (3.3) | Keep + edit integration list | Remove entire section |
| Data Migration (3.4) | Keep CQ-led OR Customer-led + fill scope table | Remove both 3.4 variants |
| Validation (3.4) | Keep | Remove entire section |
| Language Translation (3.8) | Keep + update language count | Remove entire section |
| Validation Specialist (4.1) | Keep bullet | Remove bullet |
| Validation deliverable rows (5) | Keep rows | Remove rows |
| Duration assumption (6) | Update to actual months | — |
| Language out-of-scope assumption (6) | Remove line | Keep line |

**Rule:** If a workstream is not in scope, remove its section cleanly. Never leave a section with placeholder, N/A, or empty content.

---

## Generation Workflow

```
1. Read docx SKILL.md → read this guide
2. Unpack template:
   python scripts/office/unpack.py assets/sow-template.docx unpacked/
3. Edit word/document.xml:
   a. Replace all instances of <Customer Name> / Customer Name
   b. Update Section 1 module/phase bullet list
   c. Replace pricing table placeholder in Section 2
   d. Update duration in Section 6
   e. Handle all conditional sections (keep/remove per scope)
   f. Fill Appendix A
4. Pack:
   python scripts/office/pack.py unpacked/ output.docx --original assets/sow-template.docx
5. QA:
   extract-text output.docx — verify customer name, pricing, duration, no leftover placeholders
   Check: grep for "<Customer Name>", "Insert Pricing Table", "twenty (20) months" (if duration changed)
```
