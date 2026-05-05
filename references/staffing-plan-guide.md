# Staffing Plan Template Guide

## Template File

`assets/staffing-plan-template.xlsx` — single sheet, "Per Phase (Non-GxP)".

There is one template. GxP vs non-GxP is handled by populating or zeroing the Validation Specialist row — do not create two separate files.

---

## Template Structure

| Area | Rows/Cols | What it contains |
|------|-----------|-----------------|
| Process label | A1, B1:D1 (merged), B2:D2 | Customer name + processes in scope |
| Config lock marker | P3 (in template) | "Config lock" label in red — repositioned per engagement |
| Phase labels | Row 4, cols F onward | Phase band names above the timeline |
| Column headers | Row 5 | Role / Total Hrs / Rate / Total / Role repeat / weekly dates |
| Role rows | Rows 6–13 | One row per role |
| Total | Row 14 | =SUM(D6:D13) |

### Role rows (A–E fixed, F onward = weekly hours)

| Row | Role | Rate |
|-----|------|------|
| 6 | Project Manager | $150/hr |
| 7 | Solution Architect | $125/hr |
| 8 | Technical Architect | $150/hr |
| 9 | Solution Consultant | $125/hr |
| 10 | Configuration Consultants | $60/hr |
| 11 | Validation Specialist | $125/hr |
| 12 | Hypercare | $125/hr |
| 13 | Rollout Support | $125/hr |

Column layout per role row:
- A = role name
- B = `=SUM(F{row}:{last_col}{row})` — auto-totals all weekly hours
- C = rate (hardcoded)
- D = `=B{row}*C{row}` — total cost
- E = role name repeated (display column)
- F onward = weekly hours (one column per week, one week = 40 hrs max for CC)

---

## How to Generate a Staffing Plan

### Step 1 — Gather inputs from the approved estimate

```
cc_hours        = padded CC hours (Base CC × 1.33, rounded)
sc_hours        = cc_hours × 0.50 (rounded)
sa_hours        = cc_hours × 0.20 (rounded)
pm_hours        = cc_hours × 0.20 (rounded)
ta_hours        = TA hours from integration estimate (0 if not in scope)
validation_hours= hours from validation estimate (0 if not GxP)
hypercare_hours = 40 (standard) or as specified
rollout_hours   = 0 unless rollout support is in scope
```

**These numbers must match the estimate exactly. The staffing plan is not a second estimate.**

### Step 2 — Calculate project duration

```
n_config_weeks = ceil(cc_hours / 40)    # assumes 1 FTE CC
```

### Step 3 — Run the generation script

```bash
python scripts/generate_staffing_plan.py '<json>' output.xlsx
```

JSON input format:
```json
{
  "customer_name": "Acme Corp",
  "processes": ["Document", "Training", "Change"],
  "is_gxp": false,
  "start_date": "2026-06-02",
  "roles": {
    "cc_hours": 380,
    "sc_hours": 190,
    "sa_hours": 76,
    "pm_hours": 76,
    "ta_hours": 0,
    "validation_hours": 0
  },
  "hypercare_hours": 40,
  "rollout_hours": 0
}
```

### Step 4 — Recalculate formulas

```bash
python scripts/recalc.py output.xlsx
```

Check the output JSON for any formula errors. Fix before delivering.

### Step 5 — Copy to outputs

```bash
cp output.xlsx /mnt/user-data/outputs/staffing-plan-<customer>.xlsx
```

---

## Phase Structure Logic

The script proportionally sizes phases based on config effort:

| Phase | Weeks formula | Minimum |
|-------|--------------|---------|
| Prep & OOB demo | 10% of config weeks | 1 |
| Design / Workshop | 15% of config weeks | 1 |
| Config (core) | Remaining config weeks | 1 |
| UAT / Testing | 20% of config weeks | 1 |
| Go-live | Always 1 week | 1 |
| Hypercare | ceil(hypercare_hours / 10) | 1 (if in scope) |

---

## Role Distribution Logic

| Role | Where hours are concentrated |
|------|------------------------------|
| PM | Steady run-rate across all phases (except hypercare) |
| SA | Heavy in Prep (40%) + Design (35%), taper in Config (25%), done by UAT |
| SC | Active from Design through UAT |
| CC | Concentrated in Config phase |
| TA | Spread across Config + UAT (if integration in scope) |
| Validation | UAT phase only (GxP only) |
| Hypercare | Hypercare phase only |
| Rollout | Hypercare zone (if in scope) |

---

## GxP vs Non-GxP

- **Non-GxP:** `is_gxp: false` → Validation Specialist row stays all zeros
- **GxP:** `is_gxp: true` → Validation hours are distributed across UAT phase weeks

Never generate two separate files. The sheet name updates automatically ("Staffing Plan (GxP)" vs "Staffing Plan (Non-GxP)").

---

## Validation Checklist

After generating, verify:
- [ ] `scripts/recalc.py` returns `"status": "success"` (zero formula errors)
- [ ] Column B total for CC row = `cc_hours` from the estimate
- [ ] Column B total for SC row = `sc_hours` from the estimate
- [ ] Column B total for SA row = `sa_hours` from the estimate
- [ ] Column B total for PM row = `pm_hours` from the estimate
- [ ] D14 total matches the total cost from the estimate (within rounding)
- [ ] Validation Specialist row is all zeros for non-GxP
- [ ] Phase labels are readable and positioned correctly
- [ ] No weeks have CC hours > 40 (1 FTE limit)
