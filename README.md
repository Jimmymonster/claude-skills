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

### [OWASP API](./skills/owasp-api/SKILL.md)

Audits API code against the OWASP API Security Top 10 (2023). Reads route handlers, auth middleware, serializers, and config, then delivers a ranked vulnerability report with `file:line` citations and concrete fixes for every finding.

**Triggers:** `/owasp-api`, "check my API security", "audit my API", "OWASP check", "find security vulnerabilities in my API"

**Covers:** API1 Broken Object Auth · API2 Broken Authentication · API3 Property Auth · API4 Resource Consumption · API5 Function Auth · API6 Business Flow Abuse · API7 SSRF · API8 Misconfiguration · API9 Inventory · API10 Unsafe API Consumption

> **Token warning:** OWASP API maps the entire API surface before auditing — routes, middleware, validators, DB queries. Point it at a specific service or directory on large monorepos to avoid excessive token usage.

---

## Installation

### Per-project
```bash
# legendary-review
mkdir -p .claude/skills/legendary-review
curl -o .claude/skills/legendary-review/SKILL.md \
  https://raw.githubusercontent.com/Jimmymonster/claude-skills/master/skills/legendary-review/SKILL.md

# owasp-api
mkdir -p .claude/skills/owasp-api
curl -o .claude/skills/owasp-api/SKILL.md \
  https://raw.githubusercontent.com/Jimmymonster/claude-skills/master/skills/owasp-api/SKILL.md
```

### User-level (available in all projects)
```bash
# legendary-review
mkdir -p ~/.claude/skills/legendary-review
curl -o ~/.claude/skills/legendary-review/SKILL.md \
  https://raw.githubusercontent.com/Jimmymonster/claude-skills/master/skills/legendary-review/SKILL.md

# owasp-api
mkdir -p ~/.claude/skills/owasp-api
curl -o ~/.claude/skills/owasp-api/SKILL.md \
  https://raw.githubusercontent.com/Jimmymonster/claude-skills/master/skills/owasp-api/SKILL.md
```
