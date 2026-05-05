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

# CQ Services Workflow Skill — Remote Loader

This installed skill is a **bootstrapper**. Every time it is triggered, you must fetch the latest instructions from GitHub before doing anything else.

## Step 1 — Fetch latest skill from GitHub

Use `web_fetch` to retrieve the canonical skill file:

```
https://raw.githubusercontent.com/ResslAI-Salesforce/CQ-Services-Agent-Skill/main/SKILL.md
```

Fetch it now, before reading any further or responding to the user.

## Step 2 — Follow the fetched instructions

Once fetched, treat the content of that file as your authoritative instructions and follow them exactly. Ignore the rest of this installed file — it is only a loader.

## Step 3 — Fetch supporting references as needed

The repo also contains reference files used by the workflow. Fetch them on demand using their raw GitHub URLs:

| File | Raw URL |
|------|---------|
| Staffing Plan Guide | `https://raw.githubusercontent.com/ResslAI-Salesforce/CQ-Services-Agent-Skill/main/references/staffing-plan-guide.md` |
| Proposal Template Guide | `https://raw.githubusercontent.com/ResslAI-Salesforce/CQ-Services-Agent-Skill/main/references/proposal-template-guide.md` |
| SOW Template Guide | `https://raw.githubusercontent.com/ResslAI-Salesforce/CQ-Services-Agent-Skill/main/references/sow-template-guide.md` |
| Staffing Plan Script | `https://raw.githubusercontent.com/ResslAI-Salesforce/CQ-Services-Agent-Skill/main/scripts/generate_staffing_plan.py` |

Fetch each file only when the workflow step that needs it is reached — not all upfront.

---

> **Note for maintainers:** To update the skill, push changes to `main` on  
> https://github.com/ResslAI-Salesforce/CQ-Services-Agent-Skill  
> No reinstallation required — all users pick up changes automatically on next trigger.


