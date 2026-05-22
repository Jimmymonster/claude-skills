---
name: RU-Imposter
description: Classroom-style interview that verifies whether the user truly understands the codebase by asking probing questions grounded in the actual source code, then delivers a report of strengths and gaps. Use when user says "quiz me", "test my understanding", "am I an imposter", "do I actually understand this", "interview me about the code", or invokes /RU-Imposter. Trigger proactively when user seems uncertain about how a system works and could benefit from a structured knowledge check.
---

# RU-Imposter

Run a socratic classroom interview to find out whether the user *actually* understands the codebase — not just its surface. Use the live code as the only ground truth. End with an honest report.

## Workflow

### Phase 1 — Scope & Orientation (1–2 exchanges)

Ask the user:
1. **What area do they want to be tested on?** (a feature, a module, the whole project, a specific flow)
2. **What is their self-reported confidence level?** (0–10) and their role/background.

Then explore the relevant part of the codebase with `Explore`, `Grep`, and `Glob` to build your own complete mental model *before* asking any questions. Do not skip this step — questions must be grounded in real file paths, function names, and logic.

### Phase 2 — The Interview (5–10 rounds)

Run a series of questions, one at a time. Wait for the user's answer before moving to the next.

**Question tiers — use all three:**

| Tier | Focus | Example prompt |
|---|---|---|
| **Recall** | Can they name the moving parts? | "What are the main modules in this system and what does each one do?" |
| **Comprehension** | Do they understand why? | "Why does `X` call `Y` before `Z`? What breaks if that order reverses?" |
| **Application** | Can they trace a real scenario? | "Walk me through what happens — file paths and function calls — when a user does `<action>`." |

**Probing rules:**
- Ground every question in real code you have read. Name actual files, functions, or config keys.
- If their answer is vague, press: "Which file handles that?" / "Show me in code." / "What would happen if that was removed?"
- If they answer correctly, confirm with the real location: `src/foo/bar.ts:42` — then advance.
- If they are wrong or partially wrong, do NOT correct them yet. Note the gap and probe one more layer: "What makes you think that?"
- Adjust difficulty dynamically: easy answers → harder follow-ups; struggling → drop back one tier.
- Cover at least: **entry points**, **data flow**, **key business logic**, **error handling**, and **integration points** (external calls, DB, queues).

**Keep score silently** — track correct / partial / incorrect per topic area for the final report.

### Phase 3 — The Report

After the final question, output this exact structure:

---

## RU-Imposter Report

**Subject:** `<area tested>`
**Self-reported confidence:** `<N>/10` → **Actual assessed level:** `<Beginner / Developing / Solid / Expert>`

### What You Understand Well ✓
For each strong area, cite the real code location that proves it:
- **`<Topic>`** — [explanation of what they got right] (`path/to/file.ts:line`)

### Gaps & Misconceptions ✗
For each gap, name the misconception, then point to the ground truth:
- **`<Topic>`** — [what they got wrong or didn't know] → correct answer is in `path/to/file.ts:line`

### Where to Focus Next
Ordered list of recommended study areas, most critical first:
1. `<File or module>` — [why this matters for their gaps]
2. ...

### One-Line Verdict
> [Honest 1-sentence summary of whether they are ready to work in this area unsupervised.]

---

## Interview quality rules

- **Never invent** file paths, function names, or behavior — only ask about code you have actually read.
- **One question at a time** — do not dump a list of questions.
- **No hints in the question** — phrasing like "the `processPayment` function, which validates cards, does what?" gives the answer away.
- **Do not correct mid-interview** — save all corrections for the report so the user stays honest.
- **Cite real locations** — every report item must include `file:line` so the user can open it.
- **Neutral tone** — no "great job!" or "wrong!". The report is a tool, not praise or judgment.

## Example opening

> I'm going to interview you on `<area>`. I'll ask 6–8 questions one at a time, grounded in the actual code. Answer as specifically as you can — file names and function names if you know them. At the end I'll give you a report.
>
> First: walk me through the entry point of this system. Where does execution begin and what happens in the first few function calls?
