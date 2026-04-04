# Copilot instructions

## Repository purpose

- This repo defines GitHub Copilot agent behavior and AZ-104 skills. There is no app code; the primary artifacts are agent definitions and skill specs.

## Key locations

- Agent definition: .github/agents/az104-cert-buddy-agent.agent.md
- Skills: .github/skills/\*/SKILL.md
- Prompts: .github/prompts/az104-practice-questions.prompt.md, .github/prompts/az104-practice-lab.prompt.md, .github/prompts/az104-study-planner.prompt.md
- MCP servers (workspace only): .vscode/mcp.json
- Microsoft Writing Style Guide: references/style-guide.md
- Fictional companies: references/fictional-companies.md
- AZ-104 objectives: references/az104-objectives.md

## Skill and agent conventions

- Each skill file is a single YAML frontmatter block followed by Markdown.
- The skill name used by agents must match the YAML frontmatter name in the skill file, not the folder name.
- Skills are auto-discovered from `.github/skills/` folders. The agent references them by name in its Markdown body; there is no `skills:` field in agent YAML frontmatter.
- Agent files use YAML frontmatter with tools lists. Keep tool IDs scoped to MCP servers defined in .vscode/mcp.json.
- Prompt files (in .github/prompts/) reference agents by the `name` field in the agent's YAML frontmatter via the `agent:` key. The current value is `az104-cert-buddy-agent`. If the agent is renamed, update all prompt files that reference it.
- Current skills:
  - az104-item-creator (exam item generation)
  - az104-lab-creator (lab generation)
  - az104-study-planner (personalized study plan generation)

## MCP server IDs

- az104buddy-mslearn -- Microsoft Learn MCP (free, no API key; provides microsoft_docs_search, microsoft_docs_fetch, microsoft_code_sample_search)

### How users set up the MCP server

The Microsoft Learn MCP server requires no API keys, logins, or sign-ups. It is configured in `.vscode/mcp.json` as an HTTP server pointing to `https://learn.microsoft.com/api/mcp`. Users can also search for "@mcp learn" in the VS Code Extensions marketplace for one-click install.

## Grounding and validation rules

- Questions must be grounded in Microsoft Learn content first. Use the Microsoft Learn MCP server to retrieve current Learn documentation and code samples.
- Labs must be grounded in Microsoft Learn for correct configuration and CLI/PowerShell syntax.
- If a request mixes questions and labs, split the output and apply the correct skill to each section.
- Grounding chain: Microsoft Learn MCP (`microsoft_docs_search` for discovery, `microsoft_docs_fetch` for full detail, `microsoft_code_sample_search` for code accuracy).

## Answer choice randomization (non-negotiable)

When generating practice questions, the correct answer MUST be randomized across A, B, C, and D. Never default to any single letter position.

## Fictional company randomization (non-negotiable)

Use fictional company names from references/fictional-companies.md for scenario context. Randomize the company selection across the full list of 50+ companies. Do not default to Contoso for every scenario.

## Terminology (non-negotiable)

Always use current Microsoft product names. Never use a retired name, even if the user does. Silently map to the current name.

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
- Prefer Microsoft style UI labels and instruction wording per references/style-guide.md.
- No contractions; avoid negatives unless required.
- Follow the Microsoft Writing Style Guide rules in references/style-guide.md for all generated content.
