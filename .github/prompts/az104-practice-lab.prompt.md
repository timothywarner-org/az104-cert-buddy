---
name: az104-practice-lab
description: "Build a hands-on AZ-104 lab with validation and cleanup."
argument-hint: "skillArea='networking' toolPreference='Azure CLI' timebox='15'"
agent: az104-cert-buddy-agent
tools:
  - az104buddy-mslearn/*
---

# AZ-104 Practice Lab

Generate **ONE** short, self-validating **AZ-104** practice lab.

## Use this skill

You must follow the workspace skill **az104-lab-creator** for lab structure, guardrails, workflow, output format, and **delivery rules** (full lab in a single message).

## Inputs (from chat)

- Skill area: ${input:skillArea:Pick an AZ-104 skill area (or leave blank and the agent picks one)}
- Objective: ${input:objective:Specific objective to practice (optional)}
- Tool preference: ${input:toolPreference:Portal | Azure CLI | PowerShell (default Azure CLI)}
- Timebox: ${input:timebox:Duration in minutes (default 15)}

## Grounding and validation rules

1. Ground the lab outcome in **Microsoft Learn** using the **Microsoft Learn MCP** server.
2. If the lab includes CLI or PowerShell syntax, confirm with `microsoft_code_sample_search`.
3. Provide **Microsoft Learn URLs** in the References section.

## Key rules

- Randomize the fictional company name from `references/fictional-companies.md`.
- Follow all style rules from `references/style-guide.md`.
- Default to **Azure CLI** if no tool preference is specified.
- All Azure product names must use current terminology (no retired names).
- No contractions. Cleanup is mandatory.

## Output format

Use the YAML output format defined in the **az104-lab-creator** skill exactly. The output must include: title, objective, skill_area, estimated_time, prerequisites, starting_state, tasks (each with steps and validation), troubleshooting, cleanup (with validation), and references.
