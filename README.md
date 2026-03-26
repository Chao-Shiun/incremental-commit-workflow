# incremental-commit-workflow

A discipline-enforcing workflow for AI-assisted development that ensures logical-unit-level commit granularity with build verification after every commit, requirement traceability in commit messages, and clear "what" and "why" in every commit. Works with **Claude Code**, **Codex**, **Cursor**, **Gemini**, **GitHub Copilot**, **OpenCode**, and other AI coding agents.

## Problem

When AI agents implement features involving multiple APIs or Jobs, they tend to batch all changes into a single commit. This makes code review difficult, commit history unreadable, and build failures harder to diagnose.

## Solution

This workflow enforces:
- **Commit by logical unit** - new creations (repository, service, API) get independent commits; modifications to existing logic commit per API endpoint or Job
- **Plan before you code** - produce a commit plan table with requirement links and get approval before implementation
- **Build after every commit** - each commit must compile successfully before proceeding to the next
- **Conventional Commits + requirement link** - every commit message includes a description and which part of the requirement drives the change
- **Local commits only** - never push to remote without explicit human approval

## Installation

### Claude Code

Copy the directory into your Claude Code skills folder:

```bash
git clone https://github.com/Chao-Shiun/incremental-commit-workflow.git
cp -r incremental-commit-workflow ~/.claude/skills/
```

Or manually create `~/.claude/skills/incremental-commit-workflow/SKILL.md` with the skill content.

### Cursor

Add the workflow rules to your Cursor rules file (`.cursor/rules/incremental-commit.mdc`), or paste the content of `SKILL.md` into **Cursor Settings > Rules for AI**.

### GitHub Copilot

Add the workflow instructions to your repository's `.github/copilot-instructions.md`, or include them in your prompt when using Copilot Chat.

### Codex (OpenAI)

Include the workflow instructions in your `AGENTS.md` or system prompt configuration file used by Codex.

### Gemini (Google)

Paste the workflow rules into your Gemini prompt prefix, or include them in your project's AI instruction file (e.g., `.gemini/style-guide.md`).

### OpenCode

Add the workflow instructions to your OpenCode configuration via `AGENTS.md` or the system prompt in your OpenCode settings.

### Other AI Tools

The core workflow is defined in `SKILL.md`. Copy its content into whatever system prompt, rules file, or instruction mechanism your AI tool supports.

## Usage

This workflow activates when a task requires modifications to multiple APIs, Jobs, or creation of new components.

### Commit Granularity

**Scenario 1: New Creations** - Each new component (service, API endpoint, Job) gets its own independent commit.

**Scenario 2: Modifying Existing Logic** - All related changes to one API endpoint or Job go in one commit (controller + service + repository + DTO as a single unit).

### Workflow

1. **IDENTIFY** - Confirm ticket number and requirement details
2. **PLAN** - Identify APIs/Jobs affected, present commit plan table with requirement links
3. **IMPLEMENT** - Complete one logical unit (API/Job/new component)
4. **COMMIT** - Stage and commit with requirement traceability
5. **BUILD** - Run build/compile — must pass before continuing
6. **REPEAT** - Back to IMPLEMENT for next unit
7. **COMPLETE** - Show commit history summary, wait for push instructions

### Commit Plan Example

| # | Type | Scope | Scenario | Unit | Description | Requirement Link |
|---|------|-------|----------|------|-------------|-----------------|
| 1 | feat | PT-1234 | New | GameListService | Create new service for game list API | Requirement 2.1: provide game listing endpoint |
| 2 | feat | PT-1234 | New | GET /api/games | Create game list API endpoint | Requirement 2.1: provide game listing endpoint |
| 3 | feat | PT-1234 | Modify | GET /api/users | Add game count field to user profile API | Requirement 2.3: show user's game count on profile |

### Commit Message Format

```
<type>(<ticket-number>): <description>

<which requirement drives this change and why it is needed>
```

**Types:** `feat`, `fix`, `refactor`, `test`, `docs`, `chore`, `perf`, `style`

**Example:**

```
feat(PT-1234): add game count field to user profile API

Requirement 2.3 specifies that user profiles must show the number of
games owned. Modified controller, service, and DTO together as one
cohesive change to GET /api/users.
```

## Compatibility

| AI Tool | Integration Method |
|---------|-------------------|
| Claude Code | `~/.claude/skills/` directory (auto-discovery) |
| Cursor | `.cursor/rules/*.mdc` or Settings > Rules for AI |
| GitHub Copilot | `.github/copilot-instructions.md` or Copilot Chat prompt |
| Codex (OpenAI) | `AGENTS.md` or system prompt config |
| Gemini (Google) | Prompt prefix or project AI instruction file |
| OpenCode | `AGENTS.md` or system prompt settings |
| Other | Copy `SKILL.md` content into system prompt or rules file |

## License

This project is licensed under the MIT License. See `LICENSE` for details.
