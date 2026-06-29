---
name: owasp-api
description: Audits API code against the OWASP API Security Top 10 (2023). Reads the actual route handlers, auth middleware, serializers, and config — not just the surface — then delivers a ranked vulnerability report with file:line citations and concrete fixes. Use when user says "/owasp-api", "check my API security", "audit my API", "OWASP check", or "find security vulnerabilities in my API".
---

# OWASP API Security Audit

Systematically check the codebase against every OWASP API Security Top 10 category. Ground every finding in real code. No guessing, no hypotheticals — only provable vulnerabilities with exact locations and fixes.

## Workflow

### Phase 1 — Reconnaissance

Before checking anything, map the API surface:
- Find all route definitions (REST endpoints, GraphQL resolvers, RPC handlers)
- Find auth/authz middleware and where it is (and isn't) applied
- Find input validation / serialization layers
- Find DB query construction and ORM usage
- Find rate limiting, logging, and error handling config

Use `Glob` and `Grep` aggressively. Do not skip this step — you cannot audit what you haven't read.

### Phase 2 — OWASP Top 10 Sweep

Check every category in order. For each one, read the relevant code sections and record every finding before producing output.

| # | Category | What to look for |
|---|---|---|
| **API1** | Broken Object Level Authorization | Endpoints that fetch objects by user-supplied ID without verifying the requesting user owns that object |
| **API2** | Broken Authentication | Missing/weak token validation, credentials in URLs, no expiry on tokens, JWT `alg: none`, insecure session management |
| **API3** | Broken Object Property Level Authorization | Endpoints that return or accept more fields than the caller should access (mass assignment, over-fetching) |
| **API4** | Unrestricted Resource Consumption | No rate limiting, no pagination limits, no request size caps, unbounded file uploads, expensive queries with no timeout |
| **API5** | Broken Function Level Authorization | Admin/privileged endpoints reachable by regular users, HTTP verb confusion (GET doing writes), missing role checks |
| **API6** | Unrestricted Access to Sensitive Business Flows | No abuse-prevention on critical flows (checkout, password reset, OTP), no CAPTCHA or step-up auth where needed |
| **API7** | Server Side Request Forgery (SSRF) | User-controlled URLs passed to HTTP clients, file fetchers, webhooks, or image processors without allowlist validation |
| **API8** | Security Misconfiguration | CORS wildcard, verbose error messages leaking stack traces, debug endpoints enabled, default credentials, missing security headers |
| **API9** | Improper Inventory Management | Undocumented endpoints, deprecated API versions still live, shadow APIs, endpoints with no auth that should have it |
| **API10** | Unsafe Consumption of APIs | Third-party API responses trusted and used without validation, no timeout/retry limits on outbound calls, deserialization of untrusted data |

### Phase 3 — The Report

---

## OWASP API Security Report

**Target:** `<project / files audited>`
**Standard:** OWASP API Security Top 10 (2023)
**Finding summary:** 🔴 `<N>` critical · 🟠 `<N>` high · 🟡 `<N>` medium · ✅ `<N>` passed

---

For each finding, use this structure:

#### [API`N`:Title] `<short title>`
- **Severity:** Critical / High / Medium / Low
- **Location:** `path/to/file.ts:line`
- **Problem:** [Precise description — what the vulnerability is, how it could be exploited]
- **Evidence:** [The specific code that proves it]
- **Fix:**
  ```
  // before
  <vulnerable code>

  // after
  <fixed code>
  ```

---

### Passed Checks ✅

List each OWASP category that was checked and found clean:
- **API`N` — `<Category>`** — [one sentence on what was checked and why it passed]

---

### Verdict

> [2–3 sentence summary: overall security posture, the single most critical issue to fix first, and whether this API is safe to expose publicly in its current state.]

---

## Audit quality rules

- **No finding without a location.** Every vulnerability must cite `file:line`. Vague warnings are not findings.
- **No speculation.** Only report what the code actually does, not what it might do.
- **Check every category.** Do not skip a category because it seems unlikely — note it as passed if clean.
- **Fixes must be concrete.** Show the actual corrected code, not just advice.
- **Auth is load-bearing.** API1, API2, and API5 findings are always at least High severity — do not downgrade them.
- **Report passed checks.** An audit that only lists failures gives no confidence. Explicitly confirm what is clean.

## Example opening

> Auditing `<target>` against OWASP API Security Top 10 (2023).
> Mapping API surface now — routes, auth middleware, validation layers.
