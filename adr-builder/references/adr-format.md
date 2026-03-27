# ADR Format Reference

Based on Michael Nygard's format from [Documenting Architecture Decisions](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions).

## Template

```markdown
# <ADR-NUMBER>. <Title>

Date: <YYYY-MM-DD>

## Status

<Proposed | Accepted | Deprecated | Superseded by [ADR-NNNN](NNNN-title.md)>

## Context

<Describe the forces at play: technical, political, social, and project-local constraints.
Write in neutral, factual language. Do not express opinions or recommend solutions here.
Include relevant alternatives that were considered.>

## Decision

<State the chosen response to the forces described above.
Write in active voice: "We will …">

## Consequences

<Describe the resulting context after applying the decision.
List all consequences—positive, negative, and neutral.
Include any follow-up decisions or work that becomes necessary.>
```

## Section-by-Section Quality Criteria

### Title
- Short noun phrase (4–8 words ideal)
- Describes *what* was decided, not *why*
- Examples of good titles:
  - "Use PostgreSQL for primary data storage"
  - "Adopt trunk-based development workflow"
  - "Deploy services as Docker containers on Kubernetes"

### Status
- Must be one of: **Proposed**, **Accepted**, **Deprecated**, **Superseded**
- If superseded, include a link to the replacement ADR: `Superseded by [ADR-0009](0009-use-rust.md)`

### Context
A strong Context section answers:
- What problem or need prompted this decision?
- What technical, organizational, or external constraints exist?
- What alternatives were considered and what were their trade-offs?
- Why is a decision needed *now*?

Weak context (too thin): "We needed a database."
Strong context: "The service handles 10k writes/sec with complex relational queries. We evaluated SQLite (insufficient concurrency), MongoDB (schema flexibility not needed, weaker ACID guarantees), and PostgreSQL. The team has existing PostgreSQL expertise. Budget precludes a managed NewSQL solution."

### Decision
- One or more declarative sentences beginning with "We will …"
- States the *choice made*, not a description of the options
- May briefly name the key reason, but the rationale belongs in Context

Weak: "PostgreSQL was chosen."
Strong: "We will use PostgreSQL 15 as the primary relational data store, deployed via the managed RDS service on AWS."

### Consequences
Must cover all three categories:

**Positive:** What becomes easier or better?
**Negative:** What becomes harder, riskier, or more expensive?
**Neutral / follow-on:** What new decisions or work does this create?

Weak consequences: "This is a good choice for our team."
Strong consequences:
- "Positive: ACID compliance simplifies transaction logic in the billing module."
- "Negative: RDS adds ~$200/month to infrastructure cost. Requires DBA expertise for tuning."
- "Follow-on: ADR needed for connection pooling strategy (PgBouncer vs. application-level)."

## Quality Checklist

Before writing the ADR to disk, confirm all of the following:

- [ ] Title is a short noun phrase (not a question or sentence)
- [ ] Status is set to one of the four valid values
- [ ] Context explains the problem, constraints, and alternatives considered — not just background
- [ ] Context is written in neutral language (no advocacy for the chosen option)
- [ ] Decision uses active voice ("We will …")
- [ ] Decision states what was chosen, not just what was evaluated
- [ ] Consequences include at least one positive, one negative, and any follow-on decisions
- [ ] The ADR is self-contained: a future developer unfamiliar with the project can understand it
- [ ] Length is appropriate: 1–2 pages (roughly 200–500 words of prose)

## Worked Example

```markdown
# 0003. Use JWT for stateless API authentication

Date: 2024-03-12

## Status

Accepted

## Context

The API must support authentication for both a web SPA and third-party integrations.
Server-side sessions require sticky routing or a shared session store, complicating
horizontal scaling. We evaluated:

- **Cookie-based sessions with Redis**: Simple to revoke, but adds Redis as a required
  dependency and requires sticky sessions or a shared store across all API nodes.
- **Opaque tokens stored in the database**: Easy to revoke, but every request requires
  a database lookup, adding latency and a single point of failure.
- **JWT (signed, short-lived)**: Stateless, scales horizontally without shared state.
  Revocation requires short expiry windows or a denylist. The team has prior experience
  with JWT in two previous projects.

Third-party integrations require a machine-to-machine flow (client credentials), which
JWT handles natively via standard claims.

## Decision

We will use short-lived JWTs (15-minute expiry) signed with RS256 for all API
authentication. Refresh tokens stored in an httpOnly cookie will handle session
continuity. The Auth service will issue and validate tokens; all other services will
validate signatures using the public key from a shared JWKS endpoint.

## Consequences

- **Positive**: No shared session state required; API nodes are fully stateless and scale
  horizontally without coordination.
- **Positive**: Standard JWT claims support the client-credentials flow needed for
  third-party integrations without additional infrastructure.
- **Negative**: Token revocation before expiry requires maintaining a denylist (e.g.,
  Redis-backed blocklist), partially re-introducing shared state for logout/revoke flows.
- **Negative**: Debugging auth failures is harder than session-based auth; expired or
  mis-signed tokens produce opaque errors without proper tooling.
- **Follow-on**: A separate ADR is needed to decide the denylist implementation
  (in-memory vs. Redis vs. accept the 15-minute revocation window).
- **Follow-on**: Key rotation strategy for the RS256 signing key must be documented.
```
