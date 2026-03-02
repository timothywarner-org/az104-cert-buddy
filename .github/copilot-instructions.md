# Copilot instructions

## Repository purpose

- This repo defines GitHub Copilot agent behavior and AZ-104 skills. There is no app code; the primary artifacts are agent definitions and skill specs.

## Key locations

- Agent definition: .github/agents/az104-cert-buddy-agent.agent.md
- Skills: .github/skills/\*/SKILL.md
- MCP servers (workspace only): .vscode/mcp.json

## Skill and agent conventions

- Each skill file is a single YAML frontmatter block followed by Markdown.
- The skill name used by agents must match the YAML frontmatter name in the skill file, not the folder name.
- Agent files use YAML frontmatter with tools and skills lists. Keep tool IDs scoped to MCP servers defined in .vscode/mcp.json.
- Current skills:
  - az104-item-creator (exam item generation)
  - az104-lab-creator (lab generation)

## Grounding and validation rules

- Questions must be grounded in Microsoft Learn first; use Context7 only when CLI or PowerShell syntax accuracy matters.
- Labs must be validated with Azure MCP for resource existence, flags, and success checks.
- If a request mixes questions and labs, split the output and apply the correct skill to each section.

## MCP server IDs

- az104buddy-azure
- az104buddy-context7
- az104buddy-markitdown

## Terminology (non-negotiable)

Always use current Microsoft product names. Never use a retired name, even if the user does. Silently map to the current name.

| Retired name | Current name |
|---|---|
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

If Microsoft Learn shows a different current name than what appears above, prefer the Learn name.

## Interactive question delivery

When the user asks for a practice question (one or more items):

1. Present **only** the metadata, scenario stem, and answer choices.
2. Do **NOT** include the correct answer, rationale, or references in the same message.
3. Wait for the user to reply with their answer.
4. After the user replies, reveal the correct answer, full rationale (2 sentences per choice), and references.

This rule applies to all question-generation skills and prompts in this workspace.

## Authoring guidance

- Keep instructions and outputs in plain ASCII (avoid curly quotes and en dashes).
- Prefer Microsoft style UI labels and instruction wording.
- No contractions; avoid negatives unless required.
