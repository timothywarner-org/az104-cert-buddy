<!-- markdownlint-disable MD041 -->
<p align="center">
  <img src="images/AZ104-Cert-Buddy.png" alt="AZ-104 Cert Buddy banner" width="480" />
</p>
<!-- markdownlint-enable MD041 -->

<p align="center">

[![AZ-104 Exam](https://img.shields.io/badge/Exam-AZ--104-0078D4?style=for-the-badge&logo=microsoft-azure&logoColor=white)](https://learn.microsoft.com/en-us/credentials/certifications/exams/az-104)
[![Azure Administrator](https://img.shields.io/badge/Cert-Azure_Administrator_Associate-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)](https://learn.microsoft.com/en-us/credentials/certifications/azure-administrator/)
[![Microsoft Learn](https://img.shields.io/badge/Grounded_in-Microsoft_Learn-258fdb?style=for-the-badge&logo=microsoft&logoColor=white)](https://learn.microsoft.com/)
[![YouTube](https://img.shields.io/badge/Watch-YouTube_Video-FF0000?style=for-the-badge&logo=youtube&logoColor=white)](https://www.youtube.com/watch?v=9RZWTgNTy90)
[![TechTrainerTim.com](https://img.shields.io/badge/TechTrainerTim.com-Visit_Site-2ea44f?style=for-the-badge&logo=google-chrome&logoColor=white)](https://TechTrainerTim.com)

</p>

# AZ-104 Cert Buddy

An AI-powered study companion for the **Microsoft Azure Administrator (AZ-104)** certification exam, built entirely on GitHub Copilot agents. There is no application code here -- just agent definitions, skill specs, prompt templates, and MCP server configurations that turn GitHub Copilot Chat into an interactive exam coach.

The agent generates **exam-realistic practice questions** with interactive answer evaluation, **short practice labs** (10-20 minutes) with built-in validation and cleanup, and **personalized study plans** based on your self-assessed confidence. All content is original and grounded in Microsoft Learn documentation.

## Azure Knowledge Prerequisites

New to Azure? Consider completing the [AZ-900: Azure Fundamentals](https://learn.microsoft.com/en-us/credentials/certifications/azure-fundamentals/) certification first. The free [Azure Fundamentals learning path](https://learn.microsoft.com/en-us/training/paths/az-900-describe-cloud-concepts/) on Microsoft Learn covers the foundational concepts that AZ-104 builds on.

If you already have hands-on Azure experience, you can proceed directly with this study buddy.

## Prerequisites

| Requirement | Purpose |
| --- | --- |
| **VS Code** | IDE with Copilot Chat support |
| **GitHub Copilot Chat extension** | Runs the agent and skills inside VS Code |
| **Azure subscription** | Needed only if you want to run practice labs against real Azure resources |

No API keys, Node.js, or Python are required. The Microsoft Learn MCP server is a free hosted service with no sign-up.

## Quick Start

1. Clone the repository:

   ```bash
   git clone https://github.com/timothywarner-org/az104-cert-buddy.git
   ```

2. Open the folder in VS Code:

   ```bash
   code az104-cert-buddy
   ```

3. The Microsoft Learn MCP server is defined in `.vscode/mcp.json` and auto-configures when the workspace loads. No API keys or setup is required.

4. Open **GitHub Copilot Chat** and invoke the agent:

   ```text
   @az104-cert-buddy-agent Generate 5 practice questions on Azure RBAC
   ```

   Or use the included prompt templates via slash commands. Type `/` in Copilot Chat to see them:

   ```text
   /az104-practice-question
   /az104-practice-lab
   /az104-study-planner
   ```

   Slash commands open guided input fields (skill area, objective, difficulty, etc.) before generating content.

### Verify Setup

After opening the workspace, confirm that the MCP server started successfully:

1. Open the **Output** panel in VS Code (**View > Output**).
2. Select the MCP server `az104buddy-mslearn` from the dropdown.
3. Verify that the server shows a successful startup message with no errors.

If the server fails to start, see the [Troubleshooting](#troubleshooting) section below.

## How to Use It

### Slash Commands vs. Agent Invocation

You can interact with the cert buddy in two ways:

| Method | Syntax | Best For |
| --- | --- | --- |
| **Slash command** | `/az104-practice-question` | Guided input fields for a single question or lab |
| **Agent mention** | `@az104-cert-buddy-agent Generate 5 questions on RBAC` | Free-form requests, multiple questions, or study plans |

Both methods use the same agent and skills. Slash commands provide a form-like input experience; agent mentions allow natural language requests.

### Practice Questions

Ask the agent for one or more practice questions on any AZ-104 topic. The interaction follows a two-phase flow:

1. **Phase 1 -- Question:** The agent presents metadata (skill area, objective, Bloom level), a scenario-based stem, and four answer choices (A-D). It does **not** reveal the correct answer.
2. **You answer:** Type your choice in the chat (for example, "B"). You can also type **"hint"** for a clue or **"skip"** to see the answer.
3. **Phase 2 -- Evaluation:** The agent tells you whether you were correct or incorrect, reveals the correct answer, provides a two-sentence rationale for every choice, and lists Microsoft Learn reference URLs.

If you requested multiple questions, the agent delivers them one at a time with progress tracking ("Question 2 of 5") and presents a summary at the end.

Example prompts (via agent):

```text
@az104-cert-buddy-agent Generate 3 questions on virtual network peering
@az104-cert-buddy-agent Give me a hard Apply-level question about storage account access keys
@az104-cert-buddy-agent Quiz me on Microsoft Entra Conditional Access
```

Or use the slash command for a guided single-question flow:

```text
/az104-practice-question
```

### Practice Labs

Ask the agent for a hands-on lab. Labs are scoped to 10-20 minutes and include:

- A single AZ-104 objective
- Prerequisites and starting state
- Numbered task steps (Azure CLI by default, or Portal/PowerShell if you specify)
- Validation gates after each major step so you can confirm success
- Troubleshooting tips for common failures
- Cleanup steps that remove all created resources

Example prompts (via agent):

```text
@az104-cert-buddy-agent Create a 15-minute lab on locking down a storage account with a private endpoint
@az104-cert-buddy-agent Build a lab on configuring Azure DNS using PowerShell
```

Or use the slash command for a guided lab flow:

```text
/az104-practice-lab
```

### Study Plans

Not sure what to study? Ask the agent for a personalized study plan:

```text
@az104-cert-buddy-agent I do not know what to study
@az104-cert-buddy-agent Create a study plan for me
```

Or use the slash command:

```text
/az104-study-planner
```

The agent presents the five AZ-104 skill areas with exam weights, asks you to rate your confidence in each, then generates a prioritized plan with estimated hours and Microsoft Learn module links.

## Skills

The agent orchestrates three skills, each defined in its own `SKILL.md` file and auto-discovered from `.github/skills/`:

### az104-item-creator

**Location:** `.github/skills/az104-item-creator/SKILL.md`

Generates exam-realistic AZ-104 practice questions. Each question includes:

- A workplace scenario stem (using randomized fictional companies from a list of 50+)
- Four answer choices (A-D) with exactly one correct answer, randomized position
- Plausible distractors based on common admin misconceptions (no fake services or flags)
- A two-sentence rationale per choice (delivered only after you answer)
- Microsoft Learn references
- Support for "hint" (eliminate a distractor) and "skip" (reveal the answer)

The skill enforces a quality checklist covering scenario realism, single-skill measurement, terminology correctness, grammar parallelism, rationale depth, answer position randomization, and fictional company variety.

### az104-lab-creator

**Location:** `.github/skills/az104-lab-creator/SKILL.md`

Generates short, self-validating practice labs. Each lab includes:

- A single AZ-104 objective and estimated completion time
- Prerequisites and required permissions
- Step-by-step tasks with exact commands or UI instructions
- Validation gates that confirm each step succeeded
- Troubleshooting entries for common failures
- Complete cleanup steps that reverse all changes

Labs prefer the lowest-cost Azure resources and include a cost warning when that is not possible. Labs with more than 12 steps are split into two separate labs to stay within the 20-minute timebox.

### az104-study-planner

**Location:** `.github/skills/az104-study-planner/SKILL.md`

Generates a personalized study plan based on your self-assessed confidence across the five AZ-104 skill areas. The plan prioritizes weak areas first, provides estimated study hours, and links to specific Microsoft Learn modules.

## MCP Server

One Model Context Protocol server is configured in `.vscode/mcp.json`. It starts automatically when the workspace loads.

| Server ID | Technology | Purpose |
| --- | --- | --- |
| `az104buddy-mslearn` | `https://learn.microsoft.com/api/mcp` (HTTP) | Free Microsoft Learn MCP server -- no API keys, no sign-up. Provides `microsoft_docs_search` (search Learn docs), `microsoft_docs_fetch` (fetch full pages), and `microsoft_code_sample_search` (find official code samples). Grounds all content in official Microsoft documentation. |

## Exam Resources

| Resource | Link |
| --- | --- |
| Azure Administrator Associate certification | [Certification page](https://learn.microsoft.com/en-us/credentials/certifications/azure-administrator/) |
| AZ-104 exam page | [Exam details](https://learn.microsoft.com/en-us/credentials/certifications/exams/az-104) |
| AZ-104 study guide | [Study guide](https://learn.microsoft.com/credentials/certifications/resources/study-guides/az-104) |
| Free practice assessment | [Practice assessment](https://learn.microsoft.com/en-us/credentials/certifications/azure-administrator/practice/assessment?assessment-type=practice&assessmentId=21&practice-assessment-type=certification) |
| Schedule your exam | [Register and schedule](https://learn.microsoft.com/en-us/credentials/certifications/register-schedule-exam) |
| Exam policies and FAQs | [Exam policies](https://learn.microsoft.com/credentials/certifications/certification-exam-policies) |
| Frequently asked questions | [FAQ](https://learn.microsoft.com/en-us/credentials/certifications/frequently-asked-questions) |
| Exam offers (Exam Replay) | [Deals](https://learn.microsoft.com/credentials/certifications/deals) |
| MeasureUp practice tests | [MeasureUp](https://www.measureup.com/microsoft.html?product_line=251) |
| Student discounts | [Student pricing](https://learn.microsoft.com/en-us/credentials/certifications/student-discounts) |
| Exam scoring and reports | [Scoring details](https://learn.microsoft.com/en-us/credentials/certifications/exam-scoring-reports) |
| Online proctored exams | [Online exams](https://learn.microsoft.com/en-us/credentials/certifications/online-exams) |

## Key Rules

The agent enforces several non-negotiable rules across all generated content:

- **Current terminology only.** Retired Azure product names are never used. For example, "Azure AD" is always replaced with "Microsoft Entra ID." A full rename table is maintained in `.github/copilot-instructions.md`.
- **Grounded in Microsoft Learn.** Every question and lab is grounded in official Microsoft Learn documentation (accessed via the Microsoft Learn MCP server) before any other source is consulted. Microsoft Learn URLs are included as references.
- **Original content only.** The agent does not recreate, paraphrase, or reference real exam questions, braindumps, or leaked content. Every scenario and stem is written from scratch.
- **No contractions.** All generated text avoids contractions.
- **No trick wording.** Negative words are avoided; when necessary, they are bolded and capitalized.
- **Distractors must be real.** Wrong answer choices reference actual Azure services, flags, and features -- never invented ones.
- **Labs include cleanup.** Every practice lab ends with steps that remove all resources created during the exercise.
- **Answer randomization.** The correct answer position is randomized across A, B, C, and D -- never always the same letter.
- **Company randomization.** Fictional company names are drawn from a list of 50+ Microsoft companies, not always Contoso.
- **Microsoft style.** All content follows the Microsoft Writing Style Guide (sentence-style capitalization, input-neutral verbs, Oxford comma, etc.).

## Troubleshooting

| Problem | Cause | Fix |
| --- | --- | --- |
| MCP server fails to start | Network connectivity issue | The Microsoft Learn MCP server is hosted at `learn.microsoft.com/api/mcp` -- verify you have internet access |
| Agent does not appear in Copilot Chat | Extension not installed or workspace not loaded | Install the **GitHub Copilot Chat** extension and open this folder as a workspace |
| Slash commands do not appear | Copilot Chat not recognizing prompt files | Close and reopen VS Code; ensure `.github/prompts/` files are present |
| "Tool not found" errors | MCP server ID mismatch | Verify that tool IDs in agent/prompt files match server IDs in `.vscode/mcp.json` |

## Repository Structure

```text
.github/
  agents/
    az104-cert-buddy-agent.agent.md    Main Copilot agent definition
  skills/
    az104-item-creator/SKILL.md        Exam question generation skill
    az104-lab-creator/SKILL.md         Practice lab generation skill
    az104-study-planner/SKILL.md       Personalized study plan skill
  prompts/
    az104-practice-questions.prompt.md  Prompt template for single practice questions
    az104-practice-lab.prompt.md        Prompt template for practice labs
    az104-study-planner.prompt.md       Prompt template for study plans
  copilot-instructions.md              Workspace-level Copilot instructions and rename table
  workflows/
    validate.yml                       CI validation pipeline (non-blocking)
.vscode/
  mcp.json                             MCP server definitions (workspace-scoped)
  extensions.json                      Recommended VS Code extensions
.editorconfig                          Editor formatting consistency
.gitignore                             Git ignore rules
CLAUDE.md                              Guidance for Claude Code when editing this repo
CONTRIBUTING.md                        Contribution guidelines and PR process
LICENSE                                Repository license
SECURITY.md                            Security policy and vulnerability reporting
references/
  az104-objectives.md                  AZ-104 skills-measured reference (April 2025)
  fictional-companies.md               Microsoft fictional company names for scenarios
  style-guide.md                       Microsoft Writing Style Guide key principles
  style-guide.pdf                      Microsoft Writing Style Guide (full source PDF)
reference/                             AB-900 cert buddy reference implementation
```

## Help and Support

- **Issues:** [Report a bug or request a feature](https://github.com/timothywarner-org/az104-cert-buddy/issues)
- **Contributing:** See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines
- **Security:** See [SECURITY.md](SECURITY.md) for vulnerability reporting
