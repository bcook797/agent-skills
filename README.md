# Agent Skills

A collection of installable skills for [Claude Code](https://github.com/anthropics/claude-code) that extend the agent with specialized architectural and engineering workflows.

## Skills

### `adr-builder`

Guides the creation of Architecture Decision Records (ADRs) following [Michael Nygard's format](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions). Covers the full workflow: gathering context, drafting, quality-checking, and writing the ADR file to the repository via [adr-tools](https://github.com/npryce/adr-tools).

**Use when you want to:** document an architecture decision, record a technical choice, or capture why a significant design or technology decision was made.

**Location:** `plugin/adr-builder/`

---

### `nfr-architect`

Guides teams through writing, validating, and operationalizing Non-Functional Requirements (NFRs). Translates requirements into SLOs and SLIs, and generates automatable fitness functions (performance tests, ArchUnit-style architecture tests, availability checks, recovery drills).

**Use when you want to:** define or review NFRs for a system, translate NFRs into SLOs/SLIs, or produce executable tests that validate a system against its non-functional requirements.

**Location:** `plugin/nfr-architect/`

---

### `tech-radar-blip`

Guides users through creating compelling, well-documented blip submissions for the [ThoughtWorks Tech Radar](https://www.thoughtworks.com/radar). Covers quadrant and ring selection, evidence gathering, and drafting a submission ready for editorial review.

**Use when you want to:** submit a blip to the Tech Radar, write a radar submission, or prepare a technology recommendation.

**Location:** `plugin/tech-radar-blip/`

---

## Installation

Skills are installed into your Claude Code project by copying the skill directory into your project's `.claude/skills/` folder (or equivalent plugin path configured for your setup).

```bash
# Example: install the adr-builder skill into your project
cp -r plugin/adr-builder /your-project/.claude/skills/
```

Refer to the [Claude Code documentation](https://docs.anthropic.com/en/docs/claude-code) for full details on skill installation and discovery.

## Structure

```
plugin/
├── adr-builder/
│   ├── SKILL.md                   # Skill definition and workflow
│   └── references/
│       └── adr-format.md          # Quality criteria and worked examples
├── nfr-architect/
│   ├── SKILL.md                   # Skill definition and workflow
│   └── references/
│       └── nfr-guide.md           # NFR patterns, templates, and code examples
└── tech-radar-blip/
    ├── SKILL.md                   # Skill definition and workflow
    └── references/
        └── submission-guide.md    # Ring/quadrant criteria and submission examples
```

Each skill contains a `SKILL.md` that defines the agent's behavior, and a `references/` directory with supporting documentation loaded into context during the workflow.
