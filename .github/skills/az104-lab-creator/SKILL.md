---
name: az104-lab-creator
description: Create short AZ-104 practice labs (10-20 minutes) that are executable and self-validating. Every lab includes prerequisites, exact tasks, validation steps, expected outputs, and cleanup. Use when the user asks for a hands-on lab, practice exercise, or guided walkthrough.
---

# Skill: az104.practice_labs.micro.validated

**Description:** Create short AZ-104 practice labs (10-20 minutes) that are executable and self-validating. Every lab includes prerequisites, exact tasks, validation steps, expected outputs, and cleanup.

## Grounding

**Required sources:**

- Microsoft Learn (primary truth source for architecture and correct configuration; access via the **Microsoft Learn MCP server** using `microsoft_docs_search` and `microsoft_docs_fetch`)
- Microsoft Learn code samples (for CLI/PowerShell/Bicep accuracy; access via `microsoft_code_sample_search`)

## Style

**Microsoft style:**

- Use Microsoft instruction formatting conventions for UI labels, commands, and dialog names.
- See `references/style-guide.md` for detailed Microsoft writing style rules.
- **Bold** for clickable UI elements. Input-neutral verbs: select (not click), enter (not type).

## Guardrails

- Keep the lab within AZ-104 scope (admin tasks, not deep dev).
- Prefer lowest-cost resources; include a cost warning if not.
- No contractions.
- No ambiguous "click around until" steps.
- Always include cleanup steps that remove created resources.
- Always use current Microsoft product names. Never use retired names such as "Azure AD" (use "Microsoft Entra ID"), "Azure AD Connect" (use "Microsoft Entra Connect"), and so on. See the full rename table in `.github/copilot-instructions.md`. If a lab touches identity or governance, double-check every product name against current terminology.

## Fictional company randomization (non-negotiable)

Use fictional company names from `references/fictional-companies.md` for any scenario context in lab titles or descriptions. You MUST randomize the company selection -- do not default to Contoso for every lab. Draw from the full list of 50+ companies.

## Timebox guidance

A lab should contain no more than 12 steps total across all tasks. If the lab requires more than 12 steps, it likely exceeds the 20-minute timebox. In that case, split the content into two separate labs, each focused on a narrower objective.

## Cost warning placement

If the lab uses resources that incur charges beyond the free tier, the cost warning must appear immediately after the **prerequisites** section and before **starting_state**. This ensures users see the warning before they begin creating resources.

## Workflow

1. Choose a single AZ-104 objective from `references/az104-objectives.md` and state it at the top.
2. Ground the intended configuration in Microsoft Learn using `microsoft_docs_search` (what correct means).
3. Draft the lab steps using either Azure portal or CLI/PowerShell (pick one primary path).
4. Use `microsoft_code_sample_search` to verify CLI/PowerShell commands are valid and current.
5. Use `microsoft_docs_fetch` for full page detail on any command or configuration step.
6. Add verification gates after each major step (fast checks).
7. Add cleanup that exactly reverses the work.

## Output format

```yaml
lab:
  title: "<Action + resource, e.g., 'Lock down a storage account with private endpoint'>"
  objective: "<One sentence outcome tied to AZ-104>"
  skill_area: "<AZ-104 skill area>"
  estimated_time: "<10-20 min>"
  prerequisites:
    - "<Azure subscription + permissions>"
    - "<Any required tools>"
  starting_state:
    - "<What must already exist>"
  tasks:
    - name: "<Task 1 name>"
      steps: |
        <Numbered steps only when sequencing matters. Use exact UI labels or exact commands.>
      validation:
        - "<Validation command + what success looks like>"
    - name: "<Task 2 name>"
      steps: |
        <...>
      validation:
        - "<...>"
  troubleshooting:
    - symptom: "<common failure>"
      fix: "<precise fix>"
  cleanup:
    steps: |
      <Exact resource deletion / rollback steps>
    validation:
      - "<Check that resources are gone>"
  references:
    - "<Microsoft Learn URL(s)>"
```

## Delivery rules

Labs are delivered in full (all sections in a single message). Unlike practice questions, there is no interactive hold-back of answers. If multiple labs are requested, deliver each lab sequentially in the same message.

## Quality checklist

- "Single objective, single outcome."
- "Every task has an explicit validation gate."
- "Cleanup is complete and safe."
- "Instructions use Microsoft formatting rules for UI labels and commands."
- "All Azure product names use current terminology (no retired names)."
- "No contractions in any lab text."
- "Fictional company is randomized (not always Contoso)."

---

## Prompt template

```text
Create {{count}} AZ-104 micro-labs.

Inputs:

- skill_area: {{skill_area}} (or select from AZ-104 study guide)
- objective: {{objective}} (or derive from Learn)
- tool_preference: {{tool_preference}} (Portal | Azure CLI | PowerShell)
- timebox: {{timebox}} (default 15 minutes)

Requirements:

1. Ground the lab outcome in Microsoft Learn first using the Microsoft Learn MCP server.
2. Use microsoft_code_sample_search for CLI/PowerShell accuracy.
3. Output using output_format exactly.
4. Randomize the fictional company name from references/fictional-companies.md.
```
