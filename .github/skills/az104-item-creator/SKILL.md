---
name: az104-item-creator
description: Generate AZ-104 practice questions that feel like the real exam without copying it. Every item is grounded in current Microsoft Learn content, uses modern Azure terminology, and follows Microsoft-style exam item rules (scenario-first, plausible distractors, no trick wording).
---

## Skill: az104.practice_questions.exam_realistic

**Description:** Generate AZ-104 practice questions that feel like the real exam without copying it. Every item is grounded in current Microsoft Learn content, uses modern Azure terminology, and follows Microsoft-style exam item rules (scenario-first, plausible distractors, no trick wording).

### Grounding

**Required sources:**

- Microsoft Learn (primary truth source for objectives + features)
- Context7 MCP (version-specific docs/snippets for CLI/PowerShell/SDK accuracy)
- Azure MCP (reality-check commands, feature existence, portal/CLI names when needed)

**Study guide:**

- Study guide for Exam AZ-104: Microsoft Azure Administrator (skills outline + objective mapping) [Microsoft Learn]

### Style

**Microsoft style:**

- Follow Microsoft sentence-style capitalization and UI-label rules.

### Guardrails

**Exam integrity:**

- Do not recreate or paraphrase real exam questions.
- Do not reference braindumps or leaked content.
- Write original scenarios and original stems every time.

**Terminology:**

- Always use current Microsoft product names. Never use retired names such as “Azure AD” (use “Microsoft Entra ID”), “Azure AD Connect” (use “Microsoft Entra Connect”), and so on. See the full rename table in `.github/copilot-instructions.md`. If a distractor references identity or governance, double-check that every product name is current.

**Item quality:**

- No contractions.
- Avoid negatives; if truly required, **CAP** + **bold** the negative word in the stem.
- Exactly 4 options (A-D) unless the requested item type explicitly differs.
- Exactly 1 correct answer unless the requested item type explicitly differs.
- No “all of the above”, “none of the above”, or subset answers (no overlap between choices).
- Distractors must be plausible and real (no fake services, fake flags, fake features).

### Workflow

1. Pull current AZ-104 skill areas and choose a target objective to measure.
2. Ground the intended correct behavior in Microsoft Learn.
3. If the item touches CLI/PowerShell/SDK specifics, invoke Context7 MCP to confirm syntax and version drift.
4. Draft a workplace scenario stem (Contoso/Fabrikam/Tailwind/etc.) that forces a real admin decision.
5. Write 1 correct answer and 3 distractors based on common-but-wrong admin assumptions.
6. Run a mutual exclusivity check on answer choices.
7. Run a terminology check: confirm every Azure product name matches the current name (see rename table in copilot-instructions.md).
8. Run a candidate clarity check: single skill measured, no trivia, no hidden requirements.
9. Prepare rationale internally but **do not deliver it yet** (see delivery rules below).

### Delivery rules (non-negotiable)

When presenting a question to the user:

**Phase 1 -- Question only:**
- Show metadata, scenario stem, and choices (A-D).
- Do **NOT** include correct_answer, rationale, or references.
- End the message and wait for the user to reply.

**Phase 2 -- Evaluation:**
- After the user replies with their answer, show:
  - Whether they were correct or incorrect.
  - The correct answer letter.
  - Full rationale for every choice (see rationale depth below).
  - References (Microsoft Learn URLs).

If multiple questions were requested, repeat this Phase 1 / Phase 2 cycle for each question sequentially.

### Output format

**Phase 1 message (question only):**

- **metadata**
  - exam: AZ-104
  - skill_area: "<one of the AZ-104 skill areas>"
  - objective: "<specific objective line>"
  - bloom: "<Remember|Understand|Apply|Analyze>"
- **question**
  - stem:
    - <Scenario + question. Keep it tight. One problem. One decision.>
  - choices:
    - A: "<choice>"
    - B: "<choice>"
    - C: "<choice>"
    - D: "<choice>"

*(Stop here. Wait for the user to answer.)*

**Phase 2 message (evaluation, after user replies):**

- **result:** "<Correct! / Incorrect.> The correct answer is <A|B|C|D>."
- **rationale:**
  - A: "<2-sentence explanation. Sentence 1: state whether correct or incorrect and why. Sentence 2: add context -- when this option would apply, the misconception it tests, or how it differs from the correct answer.>"
  - B: "<same 2-sentence format>"
  - C: "<same 2-sentence format>"
  - D: "<same 2-sentence format>"
- **references:**
  - "<Microsoft Learn URL 1>"
  - "<Microsoft Learn URL 2 if needed>"
- **quality_checklist:**
  - "Scenario is realistic for an Azure admin."
  - "Exactly one skill is being measured."
  - "Correct answer is unambiguously correct given Learn docs."
  - "Distractors are plausible, real, and unambiguously wrong."
  - "No contractions; minimal negatives; no trick phrasing."
  - "Choices are parallel in grammar and scope."
  - "At least one Microsoft Learn reference is included."
  - "All Azure product names use current terminology (no retired names)."
  - "Each rationale entry is exactly 2 sentences."

---

## Prompt template

You are writing NEW AZ-104 practice questions that feel exam-realistic without copying the exam.

**Inputs:**

- count: {{count}}
- skill_area: {{skill_area}} (or pick from the AZ-104 study guide)
- bloom: {{bloom}}
- constraints: {{constraints}}

**Requirements:**

1. Ground every question in Microsoft Learn first.
2. Use Context7 MCP for CLI/PowerShell/SDK accuracy when applicable.
3. Use Azure MCP only when you need to verify real-world existence or validate a claim.
4. Follow guardrails and output_format exactly.

Deliver {{count}} items.
