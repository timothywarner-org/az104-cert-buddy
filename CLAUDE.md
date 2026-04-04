# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

Content-only repository for an AZ-104 certification study buddy powered by GitHub Copilot agents. No application code, no build system, no tests. Artifacts are agent definitions, skill specs, prompt templates, and MCP server configurations.

## Architecture

The **az104-cert-buddy-agent** (`.github/agents/`) orchestrates three auto-discovered skills (`.github/skills/`):

- **az104-item-creator** -- Exam-realistic practice questions with two-phase interactive delivery (question first, rationale after user answers). Supports "hint" and "skip" commands.
- **az104-lab-creator** -- 10-20 minute self-validating labs with validation gates and mandatory cleanup.
- **az104-study-planner** -- Personalized study plans based on confidence self-assessment across five AZ-104 skill areas.

Three prompt templates (`.github/prompts/`) provide slash-command entry points: `/az104-practice-question`, `/az104-practice-lab`, `/az104-study-planner`.

All content is grounded via a single MCP server: **az104buddy-mslearn** (`https://learn.microsoft.com/api/mcp`, free, no API key) providing `microsoft_docs_search`, `microsoft_docs_fetch`, and `microsoft_code_sample_search`.

A parallel **AB-900** cert buddy implementation lives in `reference/` as a template. Do not modify it when editing AZ-104 files.

### Skill Discovery (Critical Gotcha)

GitHub Copilot **does not** support a `skills:` field in agent YAML frontmatter. Skills are auto-discovered from `.github/skills/` folders based on `name` and `description` in SKILL.md frontmatter. The agent references skills by name in its Markdown body only.

### Cross-Reference Dependencies

When renaming anything, update all dependents:
- Agent `name` field -> all `.github/prompts/*.prompt.md` `agent:` fields
- Skill `name` field -> agent Markdown body references
- MCP server ID in `.vscode/mcp.json` -> agent and prompt `tools:` lists

## CI Validation

`.github/workflows/validate.yml` runs on PR to `main` (non-blocking, `continue-on-error: true`):

1. **Retired terminology check** -- greps for `Azure AD`, `ADAL`, etc. (excludes copilot-instructions.md and CLAUDE.md which contain the rename table)
2. **Non-ASCII check** -- greps for curly quotes, en dashes, em dashes
3. **Contraction check** -- greps for `don't`, `doesn't`, `won't`, etc. (excludes CONTRIBUTING.md)
4. **Markdown link check** -- validates URLs using mlc-config.json (10s timeout, retries on 429)

## Terminology Rename Table

Always use current names. This table is the most frequently needed reference when editing:

| Retired | Current |
| --- | --- |
| Azure Active Directory (Azure AD) | Microsoft Entra ID |
| Azure AD tenant | Microsoft Entra tenant |
| Azure AD Connect | Microsoft Entra Connect |
| Azure AD B2B / B2C | Microsoft Entra External ID |
| Azure AD Domain Services | Microsoft Entra Domain Services |
| Azure AD Conditional Access | Microsoft Entra Conditional Access |
| Azure AD PIM | Microsoft Entra Privileged Identity Management |
| Azure AD Identity Protection | Microsoft Entra ID Protection |
| Azure AD application registrations | Microsoft Entra app registrations |
| RBAC (standalone) | Azure role-based access control (Azure RBAC) on first use |
| AKS (standalone) | Azure Kubernetes Service (AKS) on first use |
| ASR | Azure Site Recovery on first use |

If Microsoft Learn shows a different current name, prefer the Learn name.

## Authoring Rules

These are enforced across all generated content and are non-negotiable:

- **Plain ASCII only** -- no curly quotes, no en/em dashes. Use `--` instead.
- **No contractions** -- write "do not" not "don't".
- **Microsoft Writing Style Guide** -- `references/style-guide.md` governs voice, capitalization, formatting. Key rules: sentence-style capitalization, **bold** for UI elements, input-neutral verbs (select not click, enter not type), Oxford comma, imperative mood in procedures.
- **Answer randomization** -- correct answer position must be distributed across A/B/C/D, never always the same letter.
- **Fictional company randomization** -- draw from the full 50+ list in `references/fictional-companies.md`, not always Contoso.
- **Rationale depth** -- exactly 2 sentences per choice (why correct/incorrect + context).
- **Distractors must be real** -- reference actual Azure services, flags, features. Never invent fake ones.
- **Labs must include cleanup** -- every lab ends with resource deletion steps.
- **Negatives** -- avoid; if required, **CAP** + **bold** the negative word.

## Default Behaviors

Preserve these when editing agent or skill content:

- Labs default to **Azure CLI** when no tool preference is specified.
- Agent picks a skill area from the AZ-104 study guide when none is specified.
- Labs prefer lowest-cost Azure resources with cost warnings when unavoidable.
- Questions use two-phase delivery: Phase 1 (question only, wait for answer), Phase 2 (evaluation with rationale and references).
