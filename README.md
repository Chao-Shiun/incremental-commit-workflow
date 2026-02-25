# incremental-commit-workflow

A discipline-enforcing workflow for AI-assisted development that ensures method-level commit granularity with clear "what" and "why" in every commit message. Works with **Claude Code**, **Codex**, **Cursor**, **Gemini**, **GitHub Copilot**, **OpenCode**, and other AI coding agents.

## Problem

When AI agents implement features involving multiple methods, they tend to batch all changes into a single commit. This makes code review difficult and commit history unreadable.

## Solution

This workflow enforces:
- **One method, one commit** - each commit modifies exactly one method/function
- **Plan before you code** - produce a commit plan table and get approval before implementation
- **Conventional Commits + why** - every commit message includes a description and the reason for the change
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

This workflow activates when a task requires modifications to multiple methods or functions.

### Workflow

1. **PLAN** - AI analyzes the task, lists methods to modify, and presents a commit plan table
2. **IMPLEMENT** - One method at a time, commit immediately after each change
3. **COMPLETE** - Show commit history summary, wait for push instructions

### Commit Plan Example

| # | Type | Scope | Method/Function | Description | Why |
|---|------|-------|----------------|-------------|-----|
| 1 | feat | UserService | ValidateInput() | Add email format validation | Current impl accepts malformed emails, causing downstream failures |
| 2 | feat | UserService | CreateUser() | Add duplicate check before insert | Users can register twice with same email, violating business rules |
| 3 | test | UserService | ValidateInput_Email | Add email validation unit tests | Verify RFC 5322 compliance and edge cases |

### Commit Message Format

```
<type>(<scope>): <description>

<why - explain the reason for this change>
```

**Types:** `feat`, `fix`, `refactor`, `test`, `docs`, `chore`, `perf`, `style`

**Example:**

```
feat(GameListService): add Redis cache for GetGameList method

Reduce database load during peak hours. Current implementation queries
DB on every request, causing latency spikes with concurrent users.
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
