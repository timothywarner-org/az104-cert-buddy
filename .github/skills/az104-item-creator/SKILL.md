---
name: az104-item-creator
description: Generate AZ-104 practice questions that feel like the real exam without copying it. Every item is grounded in current Microsoft Learn content, uses modern Azure terminology, and follows Microsoft-style exam item rules (scenario-first, plausible distractors, no trick wording). Use when the user asks for practice questions, quiz items, or exam prep.
---

# Skill: az104.practice_questions.exam_realistic

**Description:** Generate AZ-104 practice questions that feel like the real exam without copying it. Every item is grounded in current Microsoft Learn content, uses modern Azure terminology, and follows Microsoft-style exam item rules (scenario-first, plausible distractors, no trick wording).

## Grounding

**Required sources:**

- Microsoft Learn (primary truth source for objectives + features; access via the **Microsoft Learn MCP server** using `microsoft_docs_search` and `microsoft_docs_fetch`)
- Microsoft Learn code samples (for CLI/PowerShell/SDK accuracy; access via `microsoft_code_sample_search`)

**Study guide:**

- Study guide for Exam AZ-104: Microsoft Azure Administrator (skills outline + objective mapping) [Microsoft Learn]
- `references/az104-objectives.md` for the full skills-measured list

## Style

**Microsoft style:**

- Follow Microsoft sentence-style capitalization and UI-label rules.
- See `references/style-guide.md` for detailed Microsoft writing style rules. Key rules for exam items:
  - Sentence-style capitalization everywhere except proper nouns and product names.
  - **Bold** for UI element names.
  - Input-neutral verbs: select (not click), enter (not type).
  - Imperative mood in procedure steps.
  - Oxford comma in all lists.
  - No contractions.
  - on-premises (never on-premise), cloud-native (hyphenated before a noun).
  - Spell out zero through nine; numerals for 10 or greater.
  - **should** for recommendations, **must** for requirements.

## Guardrails

**Exam integrity:**

- Do not recreate or paraphrase real exam questions.
- Do not reference braindumps or leaked content.
- Write original scenarios and original stems every time.

**Terminology:**

- Always use current Microsoft product names. Never use retired names such as "Azure AD" (use "Microsoft Entra ID"), "Azure AD Connect" (use "Microsoft Entra Connect"), and so on. See the full rename table in `.github/copilot-instructions.md`. If a distractor references identity or governance, double-check that every product name is current.

**Item quality:**

- No contractions.
- Avoid negatives; if truly required, **CAP** + **bold** the negative word in the stem.
- Exactly 4 options (A-D) unless the requested item type explicitly differs.
- Exactly 1 correct answer unless the requested item type explicitly differs.
- No "all of the above", "none of the above", or subset answers (no overlap between choices).
- Distractors must be plausible and real (no fake services, fake flags, fake features).

## Answer choice randomization (non-negotiable)

You MUST randomize which letter (A, B, C, or D) is the correct answer for each question. Do not default to any single letter position. Across a set of questions, distribute the correct answer roughly evenly among A, B, C, and D. Before finalizing each question, deliberately vary the correct answer position.

## Fictional company randomization (non-negotiable)

Use fictional company names from `references/fictional-companies.md` for scenario context. You MUST randomize the company selection -- do not default to Contoso for every scenario. Draw from the full list of 50+ companies including Contoso, Fabrikam, Tailwind Traders, Northwind Traders, Litware, A. Datum, Woodgrove Bank, Trey Research, Coho Vineyard, AdventureWorks, Wide World Importers, Blue Yonder Airlines, Fourth Coffee, Humongous Insurance, Alpine Ski House, WingTip Toys, and others.

## Workflow

1. Pull current AZ-104 skill areas from `references/az104-objectives.md` and choose a target objective to measure.
2. Ground the intended correct behavior in Microsoft Learn using `microsoft_docs_search` first, then `microsoft_docs_fetch` if you need full page detail.
3. If the item touches CLI/PowerShell/SDK specifics, invoke `microsoft_code_sample_search` to confirm syntax.
4. Pick a random fictional company from `references/fictional-companies.md` and draft a workplace scenario stem that forces a real admin decision.
5. Randomly assign the correct answer to A, B, C, or D. Write 1 correct answer and 3 distractors based on common-but-wrong admin assumptions.
6. Run a mutual exclusivity check on answer choices.
7. Run a terminology check: confirm every Azure product name matches the current name (see rename table in copilot-instructions.md).
8. Run a candidate clarity check: single skill measured, no trivia, no hidden requirements.
9. Prepare rationale internally but **do not deliver it yet** (see delivery rules below).

## Invalid answer handling

When presenting questions interactively:

- **"hint"**: Provide a clue that eliminates one distractor. Re-present the question with all four choices still visible but the eliminated option noted.
- **"skip"** or **"I do not know"**: Immediately reveal the correct answer and full rationale (Phase 2), then move to the next question.
- **Unrecognized input**: Prompt the user: "Please reply with **A**, **B**, **C**, or **D**. You can also type **hint** for a clue or **skip** to see the answer."

## Progress tracking

When multiple questions are requested:

- Prefix each question with **"Question N of M"** (for example, "Question 3 of 10").
- After the final question, present a summary: total correct, total incorrect, total skipped, and any weak skill areas identified.

## Scenario-first stem guidance

The stem must open with a workplace scenario before asking the question. The scenario establishes context that makes the question feel like a real admin decision.

**Good example:**
> Tailwind Traders has a storage account in the East US region. The security team requires that all data at rest use customer-managed keys stored in Azure Key Vault. You need to configure encryption for the storage account. What should you do?

**Bad example (no scenario):**
> Which encryption option uses customer-managed keys for Azure Storage?

## Plausible distractor guidance

Distractors must reference real Azure services, flags, or features that are genuinely related to the topic but incorrect for the specific scenario.

**Good distractors** (real but wrong):

- A: "Enable infrastructure encryption on the storage account." (Real feature, but does not satisfy the customer-managed key requirement alone.)
- B: "Configure encryption with Microsoft-managed keys." (Real default, but contradicts the requirement.)

**Bad distractors** (fake or implausible):

- A: "Run `az storage enable-cmk --auto`." (Fake flag -- no such command exists.)
- B: "Use Azure Key Safe to store the encryption key." (Fake service -- Azure Key Safe does not exist.)

## Delivery rules (non-negotiable)

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

## Output format

**Phase 1 message (question only):**

- **metadata**
  - exam: AZ-104
  - skill_area: "`<one of the AZ-104 skill areas>`"
  - objective: "`<specific objective line>`"
  - bloom: "`<Remember|Understand|Apply|Analyze>`"
  - difficulty: "`<easy|medium|hard>`"
- **question**
  - stem:
    - `<Scenario + question. Keep it tight. One problem. One decision.>`
  - choices:
    - A: "`<choice>`"
    - B: "`<choice>`"
    - C: "`<choice>`"
    - D: "`<choice>`"

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
  - "Correct answer position is randomized (not always A)."
  - "Fictional company is randomized (not always Contoso)."

---

## Prompt template

You are writing NEW AZ-104 practice questions that feel exam-realistic without copying the exam.

**Inputs:**

- count: {{count}}
- skill_area: {{skill_area}} (or pick from the AZ-104 study guide)
- bloom: {{bloom}}
- constraints: {{constraints}}

**Requirements:**

1. Ground every question in Microsoft Learn first using the Microsoft Learn MCP server.
2. Use `microsoft_code_sample_search` for CLI/PowerShell/SDK accuracy when applicable.
3. Follow guardrails and output_format exactly.
4. Randomize the correct answer position across A, B, C, D.
5. Randomize the fictional company name from references/fictional-companies.md.

Deliver {{count}} items.
