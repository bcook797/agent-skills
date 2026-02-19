---
name: nfr-architect
description: "Guides users through writing, validating, and operationalizing Non-Functional Requirements (NFRs), Service Level Objectives (SLOs), Service Level Indicators (SLIs), and fitness functions. This skill should be used when a user wants to define or review NFRs for a system, translate NFRs into SLOs/SLIs, or generate automatable fitness functions (performance tests, ArchUnit-style architecture tests, availability checks, recovery drills) that validate a system against its non-functional requirements."
---

# NFR Architect

## Overview

NFR Architect guides teams through the full lifecycle of non-functional requirements: writing
well-formed NFRs that satisfy the BINT criteria, translating them into SLOs and SLIs, and
producing concrete fitness functions that turn requirements into executable tests.

Reference file `references/nfr-guide.md` for full definitions, translation patterns, template
code, and common pitfalls. Load it into context at the start of any session.

---

## Workflow

### Step 1 — Elicit and Validate NFRs (BINT Check)

When a user presents or describes an NFR, evaluate it against all four BINT criteria:

- **Bounded** — Is it scoped to a specific service, time window, and load condition?
- **Independent** — Does it stand alone without depending on another NFR?
- **Negotiable** — Is there an explicit business driver (KPI, SLA, risk) behind it?
- **Testable** — Does it have a quantified, automatable acceptance criterion?

If any criterion fails, rewrite the NFR using the anatomy pattern from the reference:

```
[Subject] shall [metric verb] [quantified threshold]
  [during/within] [bounded context / time window]
  [under] [defined load or condition]
```

Ask clarifying questions to fill any gaps:
- What service or component does this apply to?
- What is the load condition or time window?
- What business outcome breaks if this is violated?
- What is the measurement source (logs, APM, synthetic probe)?

---

### Step 2 — Translate NFR → SLO + SLI

Once an NFR is valid, produce the corresponding SLO and SLI using the patterns in
`references/nfr-guide.md` (Performance, Availability, Resilience, Security / Structural).

For each NFR, output a table:

| Element | Value |
|---------|-------|
| **NFR** | Original statement |
| **SLO** | Internal target (stricter than SLA) |
| **SLI formula** | `(good events / total events) × 100` or equivalent |
| **Measurement source** | Tool, query, or probe |
| **Error budget** | `1 − SLO` headroom |
| **Measurement window** | Rolling 30-day / per-incident / etc. |

SLOs must always be equal to or stricter than the corresponding SLA. Flag any violation.

---

### Step 3 — Generate Fitness Functions

Produce automatable fitness function code that enforces the SLO.

Choose the right tool based on NFR category:

| Category | Recommended Tools |
|----------|------------------|
| Performance (latency/throughput) | k6, Gatling, Locust, JMeter |
| Availability | Synthetic probes, Datadog monitors, Pingdom |
| Architecture / structural | ArchUnit (Java), Dependency Cruiser (JS/TS), py-arch-rule (Python) |
| Resilience / recovery | Chaos engineering scripts, DR drill harnesses |
| Security | Snyk in CI, OWASP ZAP, Trivy |

For each fitness function, specify:
1. **File path** — e.g., `fitness-function/performance/checkout-slo.js`
2. **CI stage** — where it runs (PR check, nightly, quarterly drill)
3. **Failure action** — block deploy, alert on-call, fail PR
4. **Threshold values** — wired directly from the SLO

Use the code templates in `references/nfr-guide.md` as starting points. Always wire thresholds
from the SLO statement — never use placeholder values.

---

### Step 4 — Produce the NFR Document

Use the NFR Documentation Template from `references/nfr-guide.md` to produce a complete,
structured NFR record per requirement. Include:

- NFR statement
- Bounded context
- Acceptance criteria checklist
- SLO table
- SLI formula and measurement source
- Fitness function file reference
- Trade-off notes tied to the business driver

---

## Core Rules

- Never accept an unbounded NFR (e.g., "shall be fast"). Always quantify.
- SLOs must be stricter than (or equal to) any corresponding SLA.
- Every NFR must have at least one fitness function. "Testable" is the most important BINT criterion.
- Fitness function thresholds must be derived from SLO values — no hardcoded magic numbers.
- Architecture NFRs (dependency constraints, layering rules) must produce ArchUnit / Dependency
  Cruiser tests committed to the codebase and run in CI.

---

## Quick Reference

| Term | One-line definition |
|------|---------------------|
| **NFR** | How well the system performs a quality attribute |
| **BINT** | Bounded, Independent, Negotiable, Testable |
| **SLO** | Internal measurable target, stricter than SLA |
| **SLI** | Actual measured value tracked against SLO |
| **Error budget** | Allowable failure headroom = `1 − SLO` |
| **Fitness function** | Automated test that enforces an NFR/SLO |

For full examples, translation patterns, code templates, and common pitfalls, read
`references/nfr-guide.md`.
