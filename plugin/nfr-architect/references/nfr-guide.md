# NFR Architect Reference Guide

## Core Concepts

### Non-Functional Requirements (NFRs)

NFRs define *how well* a system performs, not *what* it does. They govern qualities like
performance, availability, security, and resilience.

Every valid NFR must satisfy the **BINT** criteria:

| Criterion | Definition | Anti-pattern |
|-----------|-----------|--------------|
| **Bounded** | Scoped to a specific context, time window, or subsystem | "The system shall be fast" |
| **Independent** | Testable without relying on other NFRs | "Availability shall be high when latency is low" |
| **Negotiable** | Backed by a business driver or KPI that can be traded off | "Latency shall be 0ms" |
| **Testable** | Has a concrete, automatable acceptance criterion | "Performance shall be acceptable" |

**Mandatory NFR anatomy:**

```
[Subject] shall [metric verb] [quantified threshold]
  [during/within] [bounded context / time window]
  [under] [defined load or condition]
```

Example: "The checkout service **shall** respond within **200ms** at the **99th percentile**
during **peak shopping hours (6pm–10pm ET)** under a load of **5,000 concurrent users**."

---

## NFR Categories

### Performance
- Throughput (TPS, RPS, events/sec)
- Latency (p50, p95, p99, p999)
- Batch processing time

### Availability & Reliability
- Uptime percentage (99.9%, 99.95%, 99.99%)
- Error rate ceiling (< 0.1% 5xx responses)
- MTTR / MTTF

### Resilience & Recovery
- RTO (Recovery Time Objective) – how fast must the system recover
- RPO (Recovery Point Objective) – how much data loss is acceptable
- Circuit breaker thresholds

### Scalability
- Horizontal scaling trigger points
- Max response time under N× normal load

### Security
- Authentication/authorization enforcement (ArchUnit-style)
- Data encryption at rest / in transit
- Secret rotation windows

### Architecture / Structural
- Dependency constraints (e.g., "domain layer shall not import infrastructure layer")
- Package coupling limits (ArchUnit)
- Module size boundaries

---

## SLO and SLI Definitions

### SLO (Service Level Objective)
An **internal** target for a measurable system metric. SLOs are stricter than SLAs and
drive engineering design decisions. They must be monitored continuously.

**SLO anatomy:**
```
[Service/component] shall [metric] [threshold]
  during [time window]
  measured over [rolling window / calendar period]
```

### SLI (Service Level Indicator)
The **actual measurement** that tracks compliance with an SLO.

```
SLI = (good events / total events) × 100
```

| Concept | Definition | Example |
|---------|-----------|---------|
| **SLO** | Internal target | "99.95% of requests succeed" |
| **SLI** | Measured value | "99.97% of requests succeeded last 30 days" |
| **SLA** | External commitment | "99.9% uptime or credit issued" |
| **Error Budget** | SLO − SLA headroom | "0.05% allowed failures" |

---

## NFR → SLO Translation Patterns

### Pattern 1: Performance (Throughput)

**NFR:**
> An application must process 1,000,000 transactions within 20 minutes.

**Decomposition:**
- 1,000,000 ÷ 1,200s = ~833 TPS raw → 800 TPS with safety margin
- "Within 20 minutes" → bounded to a peak processing window

**SLO:**
> The application shall process transactions at a minimum average rate of **800 TPS**
> during peak processing windows between **11pm and 3am ET**,
> measured over a rolling **7-day window**.

**SLI:**
> Average TPS observed in the 11pm–3am ET window over the last 7 days.

---

### Pattern 2: Performance (Latency)

**NFR:**
> API responses must be fast for end users.

**SLO:**
> The payments API shall respond within **200ms** at the **p99 percentile**
> for all authenticated POST /payments requests,
> measured over a rolling **30-day window**.

**SLI:**
> p99 latency for POST /payments, sampled every 60 seconds via synthetic probes.

---

### Pattern 3: Availability

**NFR:**
> The platform must be highly available.

**SLO:**
> The platform shall maintain **≥ 99.95% availability** (≤ 21.6 min/month downtime)
> for all customer-facing endpoints,
> measured over a rolling **30-day calendar window**.

**SLI:**
> `(successful_requests / total_requests) × 100` where success = HTTP 2xx/3xx,
> sampled every 1 minute from edge load balancers.

---

### Pattern 4: Resilience / Recovery

**NFR:**
> Platinum Resiliency shall be incorporated into the overall architecture design
> before development begins.

**SLOs:**
> - RTO (Recovery Time Objective) shall not exceed **15 minutes** following any
>   declared service outage.
> - RPO (Recovery Point Objective) shall not exceed **15 minutes** of data loss
>   following any declared service outage.

**SLIs:**
> - Time from outage declaration to full service restoration (measured per incident).
> - Age of most recent recoverable backup at time of failure (measured per incident).

---

### Pattern 5: Security / Structural (ArchUnit-style)

**NFR:**
> No service in the payments domain shall have a direct runtime dependency on
> the reporting domain.

**SLO (Architecture Gate):**
> Zero violations of the payments→reporting dependency constraint shall exist
> in the compiled artifact at release time.

**Fitness Function (ArchUnit):**
```java
@Test
void payments_domain_shall_not_depend_on_reporting() {
    noClasses()
        .that().resideInAPackage("..payments..")
        .should().dependOnClassesThat()
        .resideInAPackage("..reporting..")
        .check(importedClasses);
}
```

---

## Fitness Functions

Fitness functions are automated tests that continuously validate an architecture or
system characteristic against a defined threshold. They make NFRs executable.

### Categories

| Category | Tool / Framework | Validates |
|----------|-----------------|-----------|
| Performance | k6, Gatling, JMeter, Locust | Throughput, latency SLOs |
| Availability | Synthetic monitors (Datadog, PagerDuty), Chaos engineering | Uptime SLOs |
| Architecture | ArchUnit (Java), Dependency Cruiser (JS/TS), py-arch-rule (Python) | Structural NFRs |
| Data integrity | Custom assertions in CI | RPO validation |
| Security | Snyk, OWASP ZAP, Trivy in CI | Security NFRs |
| Scalability | Load tests at N× baseline | Elasticity NFRs |

---

### Performance Fitness Function Templates

#### k6 — Throughput + Latency Gate

```javascript
// fitness-function/performance/checkout-slo.js
import http from 'k6/http';
import { check, sleep } from 'k6';

export const options = {
  // NFR: 5,000 concurrent users during peak
  vus: 5000,
  duration: '10m',
  thresholds: {
    // SLO: p99 latency ≤ 200ms
    http_req_duration: ['p(99)<200'],
    // SLO: error rate < 0.1%
    http_req_failed: ['rate<0.001'],
    // SLO: throughput ≥ 800 TPS
    http_reqs: ['rate>800'],
  },
};

export default function () {
  const res = http.post('https://api.example.com/checkout', JSON.stringify({
    items: [{ id: 'prod-1', qty: 1 }],
  }), { headers: { 'Content-Type': 'application/json' } });

  check(res, { 'status 200': (r) => r.status === 200 });
  sleep(0.1);
}
```

#### Gatling — Throughput Ramp

```scala
// src/test/scala/simulations/ThroughputSLO.scala
class ThroughputSLOSimulation extends Simulation {

  val httpProtocol = http.baseUrl("https://api.example.com")

  val scn = scenario("Throughput SLO")
    .exec(http("POST /transactions")
      .post("/transactions")
      .body(StringBody("""{"amount": 100}"""))
      .check(status.is(200)))

  setUp(
    scn.inject(
      // Ramp to 800 TPS over 2 minutes, sustain 10 minutes
      rampUsersPerSec(0) to 800 during (2 minutes),
      constantUsersPerSec(800) during (10 minutes)
    )
  ).assertions(
    // NFR: 1,000,000 transactions in 20 minutes
    global.requestsPerSec.gte(800),
    global.responseTime.percentile3.lte(200),  // p99 ≤ 200ms
    global.failedRequests.percent.lte(0.1)
  )
}
```

---

### Architecture Fitness Functions

#### ArchUnit (Java) — Dependency Constraint

```java
// src/test/java/architecture/DomainBoundaryTest.java
@AnalyzeClasses(packages = "com.example")
class DomainBoundaryTest {

    @ArchTest
    static final ArchRule domain_layer_isolation =
        noClasses()
            .that().resideInAPackage("..domain..")
            .should().dependOnClassesThat()
            .resideInAPackage("..infrastructure..")
            .because("Domain layer must not depend on infrastructure (hexagonal architecture)");

    @ArchTest
    static final ArchRule payments_reporting_isolation =
        noClasses()
            .that().resideInAPackage("..payments..")
            .should().dependOnClassesThat()
            .resideInAPackage("..reporting..")
            .because("NFR: payments domain shall have zero runtime dependencies on reporting domain");

    @ArchTest
    static final ArchRule no_cyclic_dependencies =
        slices().matching("com.example.(*)..").should().beFreeOfCycles();
}
```

#### Dependency Cruiser (TypeScript/JS) — Module Boundaries

```json
// .dependency-cruiser.json
{
  "forbidden": [
    {
      "name": "no-cross-domain-imports",
      "comment": "NFR: Domain modules must not import from sibling domains",
      "severity": "error",
      "from": { "path": "^src/payments" },
      "to":   { "path": "^src/reporting" }
    },
    {
      "name": "no-circular-deps",
      "comment": "NFR: Zero circular dependencies in module graph",
      "severity": "error",
      "from": {},
      "to":   { "circular": true }
    }
  ]
}
```

#### py-arch-rule (Python) — Package Constraints

```python
# tests/architecture/test_domain_boundaries.py
from arch_rule import ArchRule, ImportRule

def test_domain_does_not_import_infrastructure():
    """NFR: Domain layer shall have zero imports from infrastructure layer."""
    rule = (
        ArchRule
        .classes_that().reside_in_package("app.domain")
        .should_not().import_modules_that().reside_in_package("app.infrastructure")
    )
    rule.check()
```

---

### Availability Fitness Function

```python
# fitness-function/availability/uptime_check.py
"""
SLO: 99.95% availability over rolling 30-day window.
SLI: (successful_probes / total_probes) × 100
Run every 60s via CI scheduled job or synthetic monitor.
"""
import requests
import sys
from datetime import datetime

ENDPOINTS = [
    "https://api.example.com/health",
    "https://api.example.com/payments/health",
]
SLO_THRESHOLD = 99.95  # percent

def check_availability(results_db):
    failures = []
    for url in ENDPOINTS:
        try:
            r = requests.get(url, timeout=5)
            success = r.status_code < 400
        except Exception:
            success = False

        results_db.record(url, success, datetime.utcnow())

    sli = results_db.rolling_success_rate(days=30)
    if sli < SLO_THRESHOLD:
        print(f"SLO BREACH: availability={sli:.4f}% < {SLO_THRESHOLD}%")
        sys.exit(1)
    print(f"SLO OK: availability={sli:.4f}%")
```

---

### Recovery Fitness Function (Chaos / DR Drill)

```python
# fitness-function/resilience/rto_rpo_validation.py
"""
SLO: RTO ≤ 15 min, RPO ≤ 15 min.
Run as part of quarterly chaos/DR drills.
"""
import time

RTO_SECONDS = 15 * 60   # 15 minutes
RPO_SECONDS = 15 * 60   # 15 minutes

def validate_rto(outage_start: float, recovery_end: float):
    actual_rto = recovery_end - outage_start
    assert actual_rto <= RTO_SECONDS, (
        f"RTO SLO BREACH: recovered in {actual_rto/60:.1f}m, limit={RTO_SECONDS/60:.0f}m"
    )
    print(f"RTO OK: {actual_rto/60:.1f} min")

def validate_rpo(last_backup_ts: float, outage_ts: float):
    data_loss = outage_ts - last_backup_ts
    assert data_loss <= RPO_SECONDS, (
        f"RPO SLO BREACH: data loss window={data_loss/60:.1f}m, limit={RPO_SECONDS/60:.0f}m"
    )
    print(f"RPO OK: {data_loss/60:.1f} min")
```

---

## NFR Documentation Template

```markdown
### NFR-[ID]: [Short title]

**Category:** [Performance | Availability | Resilience | Security | Architecture | Scalability]
**Business Driver:** [KPI or risk that motivates this requirement]
**Priority:** [P1 Critical | P2 High | P3 Medium]

#### Statement
[Subject] shall [metric verb] [quantified threshold]
during [bounded context / time window]
under [defined condition or load].

#### Bounded Context
- Service / component: [name]
- Time window: [e.g., peak hours 6pm–10pm ET]
- Load conditions: [e.g., 5,000 concurrent users]

#### Acceptance Criteria
- [ ] [Measurable criterion 1]
- [ ] [Measurable criterion 2]

#### SLO Translation
| Metric | Threshold | Window |
|--------|-----------|--------|
| [metric] | [value] | [rolling 30d / calendar month / per incident] |

#### SLI Definition
`SLI = [formula]`
Measured via: [tool / probe / query]

#### Fitness Function
File: `fitness-function/[category]/[nfr-id]-[short-name].[ext]`
Runs in: [CI pipeline stage | scheduled job | chaos drill]
Failure action: [block deploy | alert on-call | fail PR check]

#### Trade-off Notes
[What was negotiated and why — links this NFR to its business driver]
```

---

## Common Pitfalls

| Pitfall | Example | Fix |
|---------|---------|-----|
| Unbounded NFR | "System shall be fast" | Add percentile, threshold, time window, and load condition |
| Coupled NFRs | "Latency shall be low when load is below SLO" | Separate into independent latency NFR and load NFR |
| Untestable NFR | "System shall feel responsive" | Quantify: "p95 latency ≤ 300ms" |
| SLO looser than SLA | SLA=99.9%, SLO=99.5% | SLO must always be ≥ SLA |
| Missing error budget | SLO defined, no budget tracked | Define error budget = 1 - SLO, monitor burn rate |
| Architecture NFR without ArchUnit | "No circular deps" in a doc | Add ArchUnit / Dependency Cruiser test in CI |
