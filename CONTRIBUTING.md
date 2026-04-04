# Contributing to AZ-104 Cert Buddy

Thank you for your interest in contributing to this project. AZ-104 Cert Buddy is an educational resource that helps people prepare for the Microsoft Azure Administrator certification exam using GitHub Copilot agents. Contributions of all kinds are welcome -- whether you are fixing a typo, updating terminology, proposing a new skill, or improving reference material.

## How to Contribute

### Report Issues

If you find something that is incorrect, outdated, or broken, please open a GitHub issue. Common examples include:

- **Outdated Azure terminology** -- a retired product name that should use the current Microsoft name (for example, "Azure AD" should be "Microsoft Entra ID")
- **Incorrect Azure behavior** -- a question rationale or lab step that does not match current Azure functionality
- **Broken MCP configurations** -- a server definition in `.vscode/mcp.json` that fails to start or uses an outdated package version
- **Inaccurate references** -- a Microsoft Learn URL that no longer resolves or points to the wrong topic

When opening an issue, include enough detail to locate and reproduce the problem (file path, line number, expected vs. actual behavior).

### Suggest New Skills or Prompt Templates

Have an idea for a new agent skill or prompt template? Open an issue describing:

- What the skill or prompt would do
- Which AZ-104 objective(s) it would cover
- How it would differ from the existing `az104-item-creator`, `az104-lab-creator`, and `az104-study-planner` skills

### Improve Reference Documents

The `references/` directory contains supporting material used by the agent:

- `az104-objectives.md` -- AZ-104 skills-measured reference
- `fictional-companies.md` -- Microsoft fictional company names for scenarios
- `style-guide.md` -- Microsoft Writing Style Guide key principles

If any of these documents are out of date or incomplete, submit a pull request with corrections.

### Fix Documentation

Improvements to the README, this file, CLAUDE.md, or any other documentation are always appreciated.

## Contribution Guidelines

All contributions must follow these rules. These are the same rules the agent itself enforces.

### Terminology

Always use current Microsoft product names. Never use a retired name, even if it appears in user-facing text you are quoting. The following table lists common renames:

| Retired name | Current name |
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

If Microsoft Learn shows a different current name than what appears above, prefer the Learn name. The full rename table lives in `.github/copilot-instructions.md`.

### Writing Style

- **No contractions** in any generated content or instructions. Write "do not" instead of "don't," "cannot" instead of "can't," and so on.
- **Plain ASCII only.** Do not use curly quotes, en dashes, em dashes, or other non-ASCII characters. Use straight quotes and double hyphens (`--`) instead.
- **Microsoft style formatting.** Use **bold** for UI labels (for example, **Resource group**, **Save**). Use sentence-style capitalization for headings and labels.
- **Avoid negatives** unless required. When a negative word is necessary, **bold** and CAPITALIZE it (for example, "Which option does **NOT** support...").

### Content Integrity

- **Questions must be original.** Do not copy, paraphrase, or reference real exam questions, braindumps, or leaked content. Every scenario and stem must be written from scratch.
- **Distractors must be real.** Wrong answer choices must reference actual Azure services, flags, and features. Never invent fake Azure services or CLI parameters.
- **Labs must include cleanup steps.** Every practice lab must end with steps that remove all resources created during the exercise.
- **All claims must be groundable in Microsoft Learn.** If you cannot find a supporting reference on Microsoft Learn, do not include the claim.

## File Conventions

### Skill Files

Skill files live in `.github/skills/<skill-name>/SKILL.md`. Each file must have:

- **YAML frontmatter** with at least `name` and `description` fields
- **Markdown body** containing the skill specification

The `name` field in the YAML frontmatter is the canonical skill identifier. Agents and prompts reference skills by this name, not by the folder name.

Example structure:

```yaml
---
name: az104-item-creator
description: Generates exam-realistic AZ-104 practice questions
---

# Skill body in Markdown
```

### Prompt Files

Prompt files live in `.github/prompts/` and use the `.prompt.md` extension. Each file must have:

- **YAML frontmatter** with `name`, `description`, `agent`, and `tools` fields
- **Markdown body** containing the prompt template

The `agent` field must reference the agent by its frontmatter `name` (currently `az104-cert-buddy-agent`).

### MCP Server References

Tool IDs used in agent and prompt files must match the server IDs defined in `.vscode/mcp.json`. The current server ID is:

- `az104buddy-mslearn` -- Microsoft Learn MCP (free, no API key)

## Pull Request Process

1. **Fork the repository** and create a feature branch from `main`. Use a descriptive branch name (for example, `fix/entra-terminology` or `add/networking-skill`).

2. **Keep changes focused.** Submit one skill, one prompt template, or one logical fix per pull request. Small, focused pull requests are easier to review.

3. **Describe what you changed and why.** In the pull request description, explain the motivation for the change and summarize what files were modified.

4. **Verify Azure product names.** Before submitting, check that all Azure product names in your changes match the current terminology table above.

5. **Check ASCII compliance.** Make sure your changes do not introduce curly quotes, en dashes, or other non-ASCII characters.

6. **Test MCP references.** If your change adds or modifies tool references, confirm that the tool IDs match the server IDs in `.vscode/mcp.json`.

## Code of Conduct

This is an educational project built to help people learn and succeed. All contributors are expected to:

- **Be respectful.** Treat every contributor and learner with courtesy and professionalism.
- **Be constructive.** Offer specific, actionable feedback. Explain the "why" behind suggestions.
- **Be supportive of learners.** Remember that this project serves people at all experience levels. Avoid dismissive language or assumptions about what others should already know.
- **Be collaborative.** Work together toward the shared goal of producing accurate, helpful study material.

Harassment, discrimination, and disrespectful behavior will not be tolerated.

## Questions

If you have questions about contributing, reach out to the maintainer:

- **Name:** Tim Warner
- **Email:** <tim@techtrainertim.com>
- **Website:** [TechTrainerTim.com](https://TechTrainerTim.com)

Thank you for helping make AZ-104 Cert Buddy a better resource for everyone preparing for the Azure Administrator certification.
