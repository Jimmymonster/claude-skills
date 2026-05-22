# Claude Skills

A collection of custom Claude Code skills created by [Jimmymonster](https://github.com/Jimmymonster).

Skills are small, composable agent behaviors that extend Claude Code with structured workflows. Drop them into your project or user-level `.claude/skills/` folder and invoke them with `/skill-name`.

## Skills

### [RU-Imposter](./skills/ru-imposter/SKILL.md)

> **Do you vibe-code all day but quietly panic when someone asks how your own system actually works?**
> This skill will find out — and help you fix that.

A classroom-style interview that verifies whether you *actually* understand the codebase — not just its surface. Grounds every question in real file paths and function names. Ends with an honest report of strengths and gaps.

**Triggers:** `/RU-Imposter`, "quiz me", "test my understanding", "am I an imposter", "interview me about the code"

**Phases:**
1. Scope & orientation — pick the area, report your confidence
2. Socratic interview — 5–10 questions across Recall / Comprehension / Application tiers
3. Final report — strengths with `file:line` citations, gaps, and a one-line verdict

> **Token warning:** RU-Imposter explores the entire codebase before asking its first question. On large repos this can burn a significant number of tokens. To keep costs down, be specific about the area you want to be tested on (e.g. "the auth module" or `src/payments/`) rather than saying "the whole project".

---

## Installation

### Per-project
```bash
mkdir -p .claude/skills/ru-imposter
curl -o .claude/skills/ru-imposter/SKILL.md \
  https://raw.githubusercontent.com/Jimmymonster/claude-skills/main/skills/ru-imposter/SKILL.md
```

### User-level (available in all projects)
```bash
mkdir -p ~/.claude/skills/ru-imposter
curl -o ~/.claude/skills/ru-imposter/SKILL.md \
  https://raw.githubusercontent.com/Jimmymonster/claude-skills/main/skills/ru-imposter/SKILL.md
```
