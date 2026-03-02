---
name: az104-practice-question
description: "Generate one exam-realistic AZ-104 practice question grounded in Microsoft Learn and validated for syntax."
argument-hint: "skillArea='Manage identities and governance' objective='RBAC role assignments' bloom='Apply' difficulty='medium' itemType='multiple-choice'"
agent: az104-cert-buddy-agent
tools:
  - learn/*
  - context7/*
  - Azure MCP Server/*
---

Generate **ONE** original, exam-realistic **AZ-104** practice question.

## Use this skill

You must follow the workspace skill **az104-item-creator** for item structure, guardrails, and **delivery rules** (Phase 1 / Phase 2 interactive flow).

## Inputs (from chat)

- Skill area: ${input:skillArea:Pick an AZ-104 skill area (or leave blank to choose)}
- Objective: ${input:objective:Specific objective line to measure (optional)}
- Bloom: ${input:bloom:Remember | Understand | Apply | Analyze}
- Difficulty: ${input:difficulty:easy | medium | hard}
- Item type: ${input:itemType:multiple-choice (A-D, single answer)}

## Grounding and validation rules

1. Ground the correct behavior in **Microsoft Learn** using the **learn** MCP tools.
2. If the item includes CLI/PowerShell syntax, confirm with **Context7** tools.
3. If you claim a command/property exists or works in a particular way, sanity-check with **Azure MCP Server** tools.
4. Provide **Microsoft Learn URLs** in the Phase 2 References section.

## Terminology

Always use current Microsoft product names. Never use retired names (for example, "Azure AD" must be "Microsoft Entra ID"). See the full rename table in `.github/copilot-instructions.md`.

## Output format (exact) -- two-phase delivery

### Phase 1 (send first, then STOP and wait for user reply)

#### Metadata

- Exam: AZ-104
- Skill area:
- Objective:
- Bloom:
- Difficulty:

#### Question

<scenario-first stem>

A. <choice>
B. <choice>
C. <choice>
D. <choice>

*(Do NOT reveal the answer. Wait for the user to reply.)*

### Phase 2 (send after the user replies with their choice)

**Result:** <Correct! / Incorrect.> The correct answer is **<A|B|C|D>**.

#### Rationale

- A: <2 sentences. Sentence 1: state correct/incorrect and why. Sentence 2: context, misconception, or contrast with the correct answer.>
- B: <same 2-sentence format>
- C: <same 2-sentence format>
- D: <same 2-sentence format>

#### References

- <Microsoft Learn URL 1>
- <Microsoft Learn URL 2 if needed>

## Style rules

- No contractions.
- No trick wording.
- No negatives unless absolutely required; if used, bold the negative word.
- Distractors must be plausible and real Azure options (no invented services/flags).
- All Azure product names must use current terminology (no retired names).
