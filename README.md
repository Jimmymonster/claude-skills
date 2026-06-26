# Claude Skills

A collection of custom Claude Code skills created by [Jimmymonster](https://github.com/Jimmymonster).

Skills are small, composable agent behaviors that extend Claude Code with structured workflows. Drop them into your project or user-level `.claude/skills/` folder and invoke them with `/skill-name`.

## Skills

### [Legendary Review](./skills/legendary-review/SKILL.md)

A multi-dimensional, adversarial code review that goes far beyond style nits. Attacks the code from five lenses — correctness, security, performance, maintainability, and test coverage — then delivers a ranked, actionable report with `file:line` citations for every finding.

**Triggers:** `/legendary-review`, "review this", "review my PR", "give me a real code review"

**Severity tiers:** 🔴 Critical · 🟠 Major · 🟡 Minor · 🔵 Suggestions

> **Token warning:** Legendary Review reads the target code **plus its surrounding context** (callers, tests, related modules). On large diffs or directories this can be expensive. Point it at a specific file or diff rather than an entire codebase.

---

## Installation

### Per-project
```bash
mkdir -p .claude/skills/legendary-review
curl -o .claude/skills/legendary-review/SKILL.md \
  https://raw.githubusercontent.com/Jimmymonster/claude-skills/main/skills/legendary-review/SKILL.md
```

### User-level (available in all projects)
```bash
mkdir -p ~/.claude/skills/legendary-review
curl -o ~/.claude/skills/legendary-review/SKILL.md \
  https://raw.githubusercontent.com/Jimmymonster/claude-skills/main/skills/legendary-review/SKILL.md
```
