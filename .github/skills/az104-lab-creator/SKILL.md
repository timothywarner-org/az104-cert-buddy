---
name: az104-lab-creator
description: Create short AZ-104 practice labs (10-20 minutes) that are executable and self-validating. Every lab includes prerequisites, exact tasks, validation steps, expected outputs, and cleanup. Use Azure MCP to validate feasibility (commands, resource types, properties, and outcomes).
---

## Skill: az104.practice_labs.micro.validated

**Description:** Create short AZ-104 practice labs (10-20 minutes) that are executable and self-validating. Every lab includes prerequisites, exact tasks, validation steps, expected outputs, and cleanup. Use Azure MCP to validate feasibility (commands, resource types, properties, and outcomes).

### Grounding

**Required sources:**

- Microsoft Learn (architecture + what is correct)
- Azure MCP (validate that steps actually work as written)
- Context7 MCP (if lab uses CLI/PowerShell modules or SDK snippets)

### Style

**Microsoft style:**

- Use Microsoft instruction formatting conventions for UI labels, commands, and dialog names.

### Guardrails

- Keep the lab within AZ-104 scope (admin tasks, not deep dev).
- Prefer lowest-cost resources; include a cost warning if not.
- No contractions.
- No ambiguous "click around until" steps.
- Always include cleanup steps that remove created resources.
- Always use current Microsoft product names. Never use retired names such as "Azure AD" (use "Microsoft Entra ID"), "Azure AD Connect" (use "Microsoft Entra Connect"), and so on. See the full rename table in `.github/copilot-instructions.md`. If a lab touches identity or governance, double-check every product name against current terminology.

### Workflow

1. Choose a single AZ-104 objective and state it at the top.
2. Ground the intended configuration in Microsoft Learn (what correct means).
3. Draft the lab steps using either Azure portal or CLI/PowerShell (pick one primary path).
4. Invoke Azure MCP to validate:
   - Resource providers/features exist.
   - CLI/PowerShell commands are valid.
   - Properties/flags used are real.
   - Validation queries actually detect success/failure.
5. Add verification gates after each major step (fast checks).
6. Add cleanup that exactly reverses the work.

### Output format

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
        - "<Azure MCP validation query/command + what success looks like>"
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
      - "<Azure MCP check that resources are gone>"
  references:
    - "<Microsoft Learn URL(s)>"
```

### Quality checklist

- "Single objective, single outcome."
- "Every task has an explicit validation gate."
- "Azure MCP confirms feasibility and correctness."
- "Cleanup is complete and safe."
- "Instructions use Microsoft formatting rules for UI labels and commands."

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

1. Ground the lab outcome in Microsoft Learn first.
2. Use Azure MCP to validate commands, resource properties, and success checks.
3. Use Context7 MCP for any CLI/PowerShell module specifics.
4. Output using output_format exactly.
```
