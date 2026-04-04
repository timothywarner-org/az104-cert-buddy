---
name: az104-study-planner
description: "Create a personalized AZ-104 study plan based on your confidence ratings."
argument-hint: "Rate your confidence: 'identities: Weak, storage: Moderate, compute: Strong, networking: Weak, monitoring: Unknown'"
agent: az104-cert-buddy-agent
tools:
  - az104buddy-mslearn/*
---

# AZ-104 Study Planner

Generate a **personalized AZ-104 study plan** based on your confidence across the five exam skill areas.

## Use this skill

You must follow the workspace skill **az104-study-planner** for workflow, output format, and **delivery rules** (full plan in a single message).

## Inputs (from chat)

- Identities and governance confidence: ${input:identities:Strong | Moderate | Weak | Unknown}
- Storage confidence: ${input:storage:Strong | Moderate | Weak | Unknown}
- Compute confidence: ${input:compute:Strong | Moderate | Weak | Unknown}
- Networking confidence: ${input:networking:Strong | Moderate | Weak | Unknown}
- Monitoring confidence: ${input:monitoring:Strong | Moderate | Weak | Unknown}

## Grounding and validation rules

1. Ground all Microsoft Learn module links using the **Microsoft Learn MCP** server. Do not invent Learn URLs.
2. Use the AZ-104 exam skills outline from `references/az104-objectives.md` for objective mapping.
3. Prioritize weak areas first. Within equal confidence levels, prioritize by exam weight.

## Key rules

- All product names must use current terminology (no retired names).
- No contractions.
- Do not skip any skill area, even if rated Strong.
- Treat "Unknown" the same as "Weak."
