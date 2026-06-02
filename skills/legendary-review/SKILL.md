---
name: legendary-review
description: A multi-dimensional, adversarial code review that goes far beyond style nits. Reads the full diff and surrounding context, attacks it from five lenses (correctness, security, performance, maintainability, test coverage), then delivers a ranked, actionable report with file:line citations for every finding. Use when user says "review this", "legendary review", "/legendary-review", "review my PR", or "give me a real code review".
---

# Legendary Review

Go beyond surface-level nits. Read the code like an adversary, a compiler, a security auditor, and a future maintainer — all at once. Every finding must be grounded in the actual code with a clear location and a concrete fix.

## Workflow

### Phase 1 — Orientation

Before reviewing anything, establish scope:
- If a diff / PR / file is already provided, use it.
- Otherwise ask: **"What should I review?"** (a file, a directory, a git diff, a PR number).

Then read the target code **plus its surrounding context** — callers, tests, related modules. Do not review in isolation.

### Phase 2 — Five-Lens Attack

Silently analyse the code through all five lenses. Do not output anything yet — collect all findings first.

| Lens | What to look for |
|---|---|
| **Correctness** | Logic bugs, off-by-one errors, wrong assumptions, edge cases that crash or silently corrupt data, incorrect error handling |
| **Security** | Injection vectors, auth bypass, insecure defaults, secrets in code, unsafe deserialization, trust boundary violations |
| **Performance** | N+1 queries, unnecessary allocations, blocking calls in hot paths, missing indexes implied by the query patterns |
| **Maintainability** | Unclear naming, deeply nested logic, hidden coupling, missing/wrong abstractions, code that will be painful to change |
| **Test Coverage** | Missing tests for critical paths, tests that don't actually assert anything meaningful, untested error branches |

**Adversarial probing rules:**
- For every function, ask: *"What input breaks this?"*
- For every external call, ask: *"What if this fails, returns null, or returns unexpected data?"*
- For every assumption, ask: *"Is this actually guaranteed?"*
- For every new dependency or import, ask: *"Is this necessary? Is there a simpler built-in?"*

### Phase 3 — The Report

Output findings ranked by severity. Use this exact structure:

---

## Legendary Review Report

**Target:** `<file / PR / diff reviewed>`
**Lines reviewed:** `<N>`
**Finding summary:** 🔴 `<N>` critical · 🟠 `<N>` major · 🟡 `<N>` minor · 🔵 `<N>` suggestions

---

### 🔴 Critical — Must Fix

Issues that will cause bugs, data loss, security vulnerabilities, or crashes in production.

#### `<Finding title>`
- **Location:** `path/to/file.ts:line`
- **Lens:** Correctness / Security / Performance / Maintainability / Tests
- **Problem:** [Precise description of what is wrong and why it matters]
- **Proof:** [The specific line(s) that demonstrate the issue]
- **Fix:**
  ```
  // before
  <problematic code>

  // after
  <corrected code>
  ```

---

### 🟠 Major — Should Fix

Issues that degrade reliability, security posture, or future changeability significantly.

#### `<Finding title>`
*(same structure as Critical)*

---

### 🟡 Minor — Nice to Fix

Low-risk improvements: naming, small logic simplifications, missing edge-case guards.

#### `<Finding title>`
*(same structure as Critical)*

---

### 🔵 Suggestions — Optional

Architectural ideas, alternative approaches, or future-proofing thoughts. No obligation to act.

- **`<topic>`** (`path/to/file.ts:line`) — [suggestion and rationale]

---

### Verdict

> [2–3 sentence honest summary: is this code ready to merge? What is the single most important thing to address first?]

---

## Review quality rules

- **No finding without a location.** Every item must cite `file:line`. Vague observations ("this could be cleaner") are not findings.
- **No speculation.** Only report what you can prove from the code you have read.
- **Fixes must be concrete.** A finding without a suggested fix is incomplete.
- **Rank ruthlessly.** Do not bury a critical security issue under style nits.
- **Read the tests too.** A bug with no test coverage is more dangerous than a bug with a failing test.
- **Neutral, precise language.** No flattery, no hedging. Say exactly what is wrong and exactly how to fix it.

## Example opening

> I'll review `<target>` across five lenses: correctness, security, performance, maintainability, and test coverage.
> Reading the code and its context now — findings coming up.
