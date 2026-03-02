# AZ-104 Cert Buddy

An AI-powered study companion for the **Microsoft Azure Administrator (AZ-104)** certification exam, built entirely on GitHub Copilot agents. There is no application code here -- just agent definitions, skill specs, prompt templates, and MCP server configurations that turn GitHub Copilot Chat into an interactive exam coach.

The agent generates **exam-realistic practice questions** with interactive answer evaluation and **short practice labs** (10-20 minutes) with built-in validation and cleanup. All content is original and grounded in Microsoft Learn documentation.

## Prerequisites

You will need the following before getting started:

| Requirement | Purpose |
|---|---|
| **VS Code** | IDE with Copilot Chat support |
| **GitHub Copilot Chat extension** | Runs the agent and skills inside VS Code |
| **Node.js** (for `npx`) | Launches the Context7 MCP server |
| **Python** (for `uvx`) | Launches the MarkItDown MCP server |
| **.NET** (for `dnx`) | Launches the Azure MCP server |
| **Context7 API key** | Required by the Context7 MCP server for version-specific documentation lookups |
| **Azure subscription** | Needed only if you want to run practice labs against real Azure resources |

## Quick Start

1. Clone the repository:

   ```bash
   git clone https://github.com/<your-org>/az104-cert-buddy.git
   ```

2. Open the folder in VS Code:

   ```bash
   code az104-cert-buddy
   ```

3. The three MCP servers are defined in `.vscode/mcp.json` and will auto-configure when the workspace loads. VS Code will prompt you for your Context7 API key on first use.

4. Open **GitHub Copilot Chat** and invoke the agent:

   ```
   @az104-cert-buddy-agent Generate 5 practice questions on Azure RBAC
   ```

   Or use the included prompt template from `.github/prompts/az104-practice-questions.prompt.md` to generate a single question with guided input fields for skill area, objective, Bloom level, difficulty, and item type.

## How to Use It

### Practice Questions

Ask the agent for one or more practice questions on any AZ-104 topic. The interaction follows a two-phase flow:

1. **Phase 1 -- Question:** The agent presents metadata (skill area, objective, Bloom level), a scenario-based stem, and four answer choices (A-D). It does **not** reveal the correct answer.
2. **You answer:** Type your choice in the chat (for example, "B").
3. **Phase 2 -- Evaluation:** The agent tells you whether you were correct or incorrect, reveals the correct answer, provides a two-sentence rationale for every choice, and lists Microsoft Learn reference URLs.

If you requested multiple questions, the agent delivers them one at a time, repeating this cycle for each question.

Example prompts:

```
@az104-cert-buddy-agent Generate 3 questions on virtual network peering
@az104-cert-buddy-agent Give me a hard Apply-level question about storage account access keys
@az104-cert-buddy-agent Quiz me on Microsoft Entra Conditional Access
```

### Practice Labs

Ask the agent for a hands-on lab. Labs are scoped to 10-20 minutes and include:

- A single AZ-104 objective
- Prerequisites and starting state
- Numbered task steps (Azure CLI by default, or Portal/PowerShell if you specify)
- Validation gates after each major step so you can confirm success
- Troubleshooting tips for common failures
- Cleanup steps that remove all created resources

Example prompts:

```
@az104-cert-buddy-agent Create a 15-minute lab on locking down a storage account with a private endpoint
@az104-cert-buddy-agent Build a lab on configuring Azure DNS using PowerShell
```

### Providing Reference Material

If you have study notes, PDFs, or Office documents you want the agent to incorporate, mention them in chat. The agent uses the **MarkItDown** MCP server to convert those files into markdown and then grounds any claims against Microsoft Learn.

## Skills

The agent orchestrates two skills, each defined in its own `SKILL.md` file:

### az104-item-creator

**Location:** `.github/skills/az104-item-creator/SKILL.md`

Generates exam-realistic AZ-104 practice questions. Each question includes:

- A workplace scenario stem (using fictional companies like Contoso, Fabrikam, or Tailwind Traders)
- Four answer choices (A-D) with exactly one correct answer
- Plausible distractors based on common admin misconceptions (no fake services or flags)
- A two-sentence rationale per choice (delivered only after you answer)
- Microsoft Learn references

The skill enforces a quality checklist covering scenario realism, single-skill measurement, terminology correctness, grammar parallelism, and rationale depth.

### az104-lab-creator

**Location:** `.github/skills/az104-lab-creator/SKILL.md`

Generates short, self-validating practice labs. Each lab includes:

- A single AZ-104 objective and estimated completion time
- Prerequisites and required permissions
- Step-by-step tasks with exact commands or UI instructions
- Validation gates (Azure MCP queries that confirm each step succeeded)
- Troubleshooting entries for common failures
- Complete cleanup steps that reverse all changes

Labs prefer the lowest-cost Azure resources and include a cost warning when that is not possible.

## MCP Servers

Three Model Context Protocol servers are configured in `.vscode/mcp.json`. They start automatically when the workspace loads.

| Server ID | Technology | Purpose |
|---|---|---|
| `az104buddy-context7` | `npx @upstash/context7-mcp@1.0.31` | Retrieves version-specific documentation and code snippets for Azure CLI, Az PowerShell, Bicep, and ARM templates. Ensures command syntax is current and accurate. |
| `az104buddy-markitdown` | `uvx markitdown-mcp@0.0.1a4` | Converts PDFs, Word documents, and other file formats into markdown so the agent can ingest your reference material. |
| `az104buddy-azure` | `dnx Azure.Mcp@2.0.0-beta.23` | Connects to Azure to validate that resource types, CLI commands, properties, and flags actually exist. Used by the lab skill to confirm that validation queries detect success or failure correctly. |

## Key Rules

The agent enforces several non-negotiable rules across all generated content:

- **Current terminology only.** Retired Azure product names are never used. For example, "Azure AD" is always replaced with "Microsoft Entra ID." A full rename table is maintained in `.github/copilot-instructions.md`.
- **Grounded in Microsoft Learn.** Every question and lab is grounded in official Microsoft Learn documentation before any other source is consulted. Microsoft Learn URLs are included as references.
- **Original content only.** The agent does not recreate, paraphrase, or reference real exam questions, braindumps, or leaked content. Every scenario and stem is written from scratch.
- **No contractions.** All generated text avoids contractions.
- **No trick wording.** Negative words are avoided; when necessary, they are bolded and capitalized.
- **Distractors must be real.** Wrong answer choices reference actual Azure services, flags, and features -- never invented ones.
- **Labs include cleanup.** Every practice lab ends with steps that remove all resources created during the exercise.

## Repository Structure

```
.github/
  agents/
    az104-cert-buddy-agent.agent.md    Main Copilot agent definition
  skills/
    az104-item-creator/SKILL.md        Exam question generation skill
    az104-lab-creator/SKILL.md         Practice lab generation skill
  prompts/
    az104-practice-questions.prompt.md  Reusable prompt template for single questions
  copilot-instructions.md              Workspace-level Copilot instructions and rename table
.vscode/
  mcp.json                             MCP server definitions (workspace-scoped)
CLAUDE.md                              Guidance for Claude Code when editing this repo
```
