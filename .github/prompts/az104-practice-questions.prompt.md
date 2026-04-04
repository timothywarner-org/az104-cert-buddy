---
name: az104-practice-question
description: "Quiz me on AZ-104 topics with exam-realistic questions."
argument-hint: "skillArea='storage' bloom='Apply' difficulty='medium'"
agent: az104-cert-buddy-agent
tools:
  - az104buddy-mslearn/*
---

# AZ-104 Practice Question

Generate **ONE** original, exam-realistic **AZ-104** practice question.

## Use this skill

You must follow the workspace skill **az104-item-creator** for item structure, guardrails, and **delivery rules** (Phase 1 / Phase 2 interactive flow).

## Inputs (from chat)

- Skill area: ${input:skillArea:Pick an AZ-104 skill area (or leave blank and the agent picks one)}
- Objective: ${input:objective:Specific objective line to measure (optional)}
- Bloom: ${input:bloom:Remember | Understand | Apply | Analyze}
- Difficulty: ${input:difficulty:easy | medium | hard}

## Grounding and validation rules

1. Ground the correct behavior in **Microsoft Learn** using the **Microsoft Learn MCP** server (`microsoft_docs_search`, then `microsoft_docs_fetch` for detail).
2. If the item includes CLI/PowerShell syntax, confirm with `microsoft_code_sample_search`.
3. Provide **Microsoft Learn URLs** in the Phase 2 References section.

## Key rules

- Randomize the correct answer position across A, B, C, D.
- Randomize the fictional company name from `references/fictional-companies.md`.
- Follow all style rules from `references/style-guide.md`.
- All Azure product names must use current terminology (no retired names).
- No contractions. No trick wording. No fake services or flags.

## Output format (exact) -- two-phase delivery

### Phase 1 (send first, then STOP and wait for user reply)

#### Metadata

- Exam: AZ-104
- Skill area:
- Objective:
- Bloom:
- Difficulty:

#### Question

`<scenario-first stem>`

A. `<choice>`
B. `<choice>`
C. `<choice>`
D. `<choice>`

_(Do NOT reveal the answer. Wait for the user to reply.)_

### Phase 2 (send after the user replies with their choice)

**Result:** <Correct! / Incorrect.> The correct answer is **<A|B|C|D>**.

#### Rationale

- A: <2 sentences>
- B: <2 sentences>
- C: <2 sentences>
- D: <2 sentences>

#### References

- <Microsoft Learn URL 1>
- <Microsoft Learn URL 2 if needed>
