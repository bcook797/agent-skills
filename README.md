# Agent Skills

A collection of installable skills that extend AI agents with specialized architectural and engineering workflows. Works with Claude Code, OpenCode, Cursor, Copilot, Cline, Windsurf, Gemini, and [many more](https://skills.sh).

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

Clone this repo, then use the `skills` CLI to install into your project. The CLI detects your agents and installs to the right place automatically.

```bash
git clone https://github.com/bcook797/agent-skills
cd agent-skills
npx skills add .
```

To install globally (available in all projects) instead of just the current project:

```bash
npx skills add . --global
```

To install for a specific agent only:

```bash
npx skills add . --agent claude-code
npx skills add . --agent opencode
```

See [skills.sh/docs](https://skills.sh/docs) for full CLI documentation.

---

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
