# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is a **content-only repository** for an AZ-104 certification study buddy powered by GitHub Copilot agents. There is no application code, no build system, and no tests. The primary artifacts are agent definitions, skill specs, prompt templates, and MCP server configurations.

## Architecture

```
.github/
  agents/az104-cert-buddy-agent.agent.md   # Main Copilot agent definition
  skills/
    az104-item-creator/SKILL.md            # Exam question generation skill
    az104-lab-creator/SKILL.md             # Practice lab generation skill
  prompts/
    az104-practice-questions.prompt.md     # Reusable prompt template
  copilot-instructions.md                  # Copilot workspace instructions
.vscode/mcp.json                           # MCP server definitions (workspace-scoped)
```

### How It Works

The **az104-cert-buddy-agent** orchestrates two skills:
- **az104-item-creator**: Generates exam-realistic AZ-104 practice questions (multiple-choice, scenario-first stems, 4 options, rationale for each).
- **az104-lab-creator**: Generates 10-20 minute self-validating practice labs with prerequisites, tasks, validation gates, troubleshooting, and cleanup.

Both skills enforce a strict grounding chain: **Microsoft Learn first** -> **Context7 for CLI/PowerShell syntax** -> **Azure MCP for reality checks**.

Questions use a **two-phase interactive delivery**: Phase 1 presents only the stem and choices (no answer), then the agent waits for the user to reply. Phase 2 reveals the correct answer, 2-sentence-per-choice rationale, and references.

### MCP Servers

Defined in `.vscode/mcp.json` with IDs:
- `az104buddy-azure` — Azure MCP (validate commands, resource types, properties)
- `az104buddy-context7` — Context7 (version-specific docs/snippets)
- `az104buddy-markitdown` — MarkItDown (convert PDFs/Office docs to markdown)

## Authoring Conventions

- **Skill files**: YAML frontmatter (`name`, `description`) followed by Markdown body. The `name` field in frontmatter is the canonical skill identifier (not the folder name).
- **Agent files**: YAML frontmatter with `tools` and `skills` lists. Tool IDs must match MCP server IDs from `.vscode/mcp.json`.
- **Plain ASCII only** — no curly quotes, no en dashes.
- **No contractions** in any generated content.
- **Microsoft style** — use official UI labels, sentence-style capitalization, and Microsoft instruction formatting.
- **Current terminology only** — never use retired Azure product names (e.g., "Azure AD" -> "Microsoft Entra ID"). A full rename table lives in `copilot-instructions.md`.
- Negatives only when required; if used, **CAP** + **bold** the negative word.
- Distractors in questions must reference real Azure services/flags (never invent fake ones).
- Labs must always include cleanup steps that remove all created resources.
- **Rationale depth** — every choice explanation must be exactly 2 sentences (why correct/incorrect + context).
