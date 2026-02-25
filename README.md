# incremental-commit-workflow

A discipline-enforcing Claude Code Skill that ensures method-level commit granularity with clear "what" and "why" in every commit message.

## Problem

When AI agents implement features involving multiple methods, they tend to batch all changes into a single commit. This makes code review difficult and commit history unreadable.

## Solution

This skill enforces:
- **One method, one commit** - each commit modifies exactly one method/function
- **Plan before you code** - produce a commit plan table and get approval before implementation
- **Conventional Commits + why** - every commit message includes a description and the reason for the change
- **Local commits only** - never push to remote without explicit human approval

## Installation

Copy the `incremental-commit-workflow` directory into your Claude Code skills folder:

```bash
# Clone the repository
git clone https://github.com/Chao-Shiun/incremental-commit-workflow.git

# Copy to Claude Code skills directory
cp -r incremental-commit-workflow ~/.claude/skills/
```

Or manually create `~/.claude/skills/incremental-commit-workflow/SKILL.md` with the skill content.

## Usage

This skill activates automatically when Claude Code detects a task requiring modifications to multiple methods or functions.

### Workflow

1. **PLAN** - Claude analyzes the task, lists methods to modify, and presents a commit plan table
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

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI

## License

MIT
