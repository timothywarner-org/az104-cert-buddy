---
name: az104-study-planner
description: Generates a personalized AZ-104 study plan based on the user's self-assessed confidence across exam skill areas, prioritizing weak areas with estimated hours and Microsoft Learn module links. Use when the user asks for a study plan, is unsure what to study, or wants exam prep guidance.
---

# Skill: az104.study_planner.personalized

**Description:** Generates a personalized AZ-104 study plan based on the user's self-assessed confidence across exam skill areas, prioritizing weak areas with estimated hours and Microsoft Learn module links.

## Grounding

**Required sources:**

- `references/az104-objectives.md` (AZ-104 skills measured, April 2025)
- Microsoft Learn (access via the **Microsoft Learn MCP server** using `microsoft_docs_search` for current Learn module URLs)
- Use `microsoft_docs_fetch` to verify Learn module links are current and active

## Workflow

1. **Present skill areas with weights.** Show the five AZ-104 skill areas and their exam weight percentages:

   | Skill Area | Exam Weight |
   | --- | --- |
   | Manage Azure identities and governance | 20-25% |
   | Implement and manage storage | 15-20% |
   | Deploy and manage Azure compute resources | 20-25% |
   | Implement and manage virtual networking | 15-20% |
   | Monitor and maintain Azure resources | 10-15% |

2. **Ask for confidence ratings.** Ask the user to rate their confidence in each area using one of these levels:
   - **Strong** -- comfortable with most objectives; needs only light review.
   - **Moderate** -- familiar with the concepts but needs targeted practice.
   - **Weak** -- limited experience; needs focused study.
   - **Unknown** -- not sure; treat as weak.

3. **Generate a prioritized study plan.** Based on the user's ratings:
   - Order areas from weakest to strongest.
   - Within equal confidence levels, prioritize areas with higher exam weight.
   - For each area, provide:
     - Estimated study hours (weak: 8-12 hours, moderate: 4-6 hours, strong: 1-2 hours).
     - Two to three specific Microsoft Learn module links (grounded via Microsoft Learn MCP server; do not invent URLs).
     - Key objectives to focus on (from `references/az104-objectives.md`).
   - Include a total estimated hours range at the bottom.

4. **Offer to start practicing.** After presenting the plan, ask: "Would you like to start with practice questions or a hands-on lab on **[first recommended topic]**?"

## Output format

```markdown
## Your Personalized AZ-104 Study Plan

### Priority 1: [Skill Area Name] (exam weight: XX-XX%)

**Your confidence:** [rating]
**Estimated study time:** X-X hours

**Focus objectives:**
- [Objective 1]
- [Objective 2]
- [Objective 3]

**Recommended Microsoft Learn modules:**
- [Module title](URL)
- [Module title](URL)

---

### Priority 2: [Skill Area Name] (exam weight: XX-XX%)

... (repeat for each area)

---

**Total estimated study time:** XX-XX hours

Ready to start? I can generate practice questions or a hands-on lab on **[first recommended topic]**.
```

## Guardrails

- Do not skip any of the five skill areas. Even "strong" areas should appear in the plan with a light review recommendation.
- Do not invent Microsoft Learn module URLs. Use the Microsoft Learn MCP server (`microsoft_docs_search`) to find real, current module links.
- Treat "unknown" confidence the same as "weak."
- Always use current Microsoft product names. See the rename table in `.github/copilot-instructions.md`.
- No contractions.

## Delivery rules

Deliver the full study plan in a single message after the user provides their confidence ratings. Do not split the plan across multiple messages.
