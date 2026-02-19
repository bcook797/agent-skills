---
name: adr-builder
description: "Guides the creation of Architecture Decision Records (ADRs) following Michael Nygard's format, ensuring each record captures sufficient context, a clear decision, and its consequences. After validating rigor, writes the ADR to the repository using adr-tools CLI commands. This skill should be used when a user wants to document an architecture decision, record a technical choice, create an ADR, or capture why a significant design or technology decision was made. Covers the full workflow: gathering context, drafting, quality-checking, and writing the ADR file via adr-tools."
---

# ADR Builder

## Overview

Produce well-structured Architecture Decision Records that give future developers enough context to understand not just *what* was decided, but *why*. After the ADR passes a quality check, write it to the repo using [adr-tools](https://github.com/npryce/adr-tools).

For detailed quality criteria and section-by-section guidance, load `references/adr-format.md` before drafting or reviewing any ADR.

## Workflow

### Step 1: Gather decision context

Ask the user for any information missing from their initial request. The minimum needed to draft a complete ADR:

- What was decided? (the title / one-line summary)
- What forces or constraints drove the decision? (context)
- What alternatives were considered and why were they rejected?
- What are the trade-offs, costs, or side-effects? (consequences)
- Is this a new decision, or does it supersede an existing ADR? (if superseding, which ADR number?)

Avoid over-asking: if the user has provided enough detail to fill all five sections, proceed directly to drafting.

### Step 2: Draft the ADR

Produce the five-section ADR in Markdown. Load `references/adr-format.md` for the exact format, quality bar, and worked example to guide writing.

### Step 3: Quality check

Before writing anything to disk, verify the draft meets all criteria in the **Quality Checklist** in `references/adr-format.md`. If any section is thin or missing, revise or ask the user for the missing information. Do not proceed to Step 4 until the draft passes.

### Step 4: Check adr-tools initialization

Check whether adr-tools has already been initialized in the repo:

```bash
# Look for an existing ADR directory (default: doc/adr)
ls doc/adr 2>/dev/null || echo "not initialized"
```

If not initialized, run:

```bash
adr init
```

This creates `doc/adr/` and records ADR 0001 (the decision to use ADRs).

### Step 5: Write the ADR to the repo

**New decision:**

```bash
adr new "<Title of the Decision>"
```

adr-tools creates a numbered Markdown file (e.g. `doc/adr/0002-title-of-the-decision.md`) and opens it in `$EDITOR`. Since Claude is writing the content directly, skip the interactive editor and instead overwrite the generated file with the drafted ADR content.

**Decision that supersedes an existing ADR:**

```bash
adr new -s <ADR-number> "<Title of the Decision>"
```

This automatically updates the old ADR's status to "Superseded by ADR-NNNN" and links the two records.

After writing, read the file back and confirm the content was saved correctly, then show the user the file path.

### Step 6: Confirm and summarize

Report to the user:
- The path of the written ADR file
- A one-sentence summary of the decision
- Any superseded ADR that was updated (if applicable)
