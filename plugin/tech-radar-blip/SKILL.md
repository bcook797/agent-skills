---
name: tech-radar-blip
description: Guide for writing high-quality blip submissions for the ThoughtWorks Tech Radar. This skill should be used when someone asks for help submitting a blip to the Tech Radar, writing a radar submission, or preparing a technology recommendation for the ThoughtWorks radar.
---

# Tech Radar Blip Submission

## Overview

This skill guides users through creating compelling, well-documented blip submissions for the public ThoughtWorks Tech Radar. High-quality submissions provide sufficient detail and context to defend the blip during editorial discussions.

## Submission Workflow

### Step 1: Gather Core Information

To begin a blip submission, collect the following essential details from the user:

1. **Proposed Blip Name** - The technology, tool, technique, or platform name
2. **What is it?** - Brief description for those unfamiliar with the blip
3. **Why blip this now?** - Current relevance and timing rationale
4. **What value does it provide?** - Concrete benefits and outcomes

If the user is unsure about any of these, help them articulate the answers through discussion.

### Step 2: Determine Placement

Help the user identify the appropriate quadrant and ring:

**Quadrant** (what type of blip):
- **Tools** - Software tools used in development or operations
- **Techniques** - Approaches, processes, or ways of working
- **Languages & Frameworks** - Programming languages or development frameworks
- **Platforms** - Infrastructure, cloud services, or foundational systems

**Ring** (maturity/recommendation level):
- **Adopt** - Proven, mature, industry should embrace
- **Trial** - Worth pursuing, used at least once in production
- **Assess** - Worth exploring, interesting to watch
- **Caution** - Proceed with care, industry should avoid (formerly "Hold")

Ring selection criteria are detailed in `references/submission-guide.md`.

### Step 3: Gather Supporting Evidence

Strong submissions require concrete evidence:

1. **Production usage** - Has this been used in production? (Critical for Trial ring)
2. **Client context** - At which client(s) was this used?
3. **Recommending group** - Which team, community, or service line recommends this?
4. **Vertical relevance** - Is it specific to a market or industry?
5. **Reference URLs** - Links to official sites, GitHub, blog posts, documentation

### Step 4: Check for Existing Blips

If updating an existing blip's status (e.g., moving from Assess to Trial):
- Obtain the URL to the existing blip on the radar
- Explain what has changed to warrant the status update

### Step 5: Draft the Submission

Compile all gathered information into a structured submission. The final output should include:

```
BLIP SUBMISSION
===============

Proposed Blip Name: [Name]

Quadrant: [Tools | Techniques | Languages & Frameworks | Platforms]
Ring: [Adopt | Trial | Assess | Caution]

Description:
[2-3 sentences explaining what this blip is for those unfamiliar with it]

Why Blip This Now?
[Explain current relevance, timing, and why this matters today]

Value Proposition:
[Concrete benefits and outcomes from using this]

Production Experience:
- Used in production: [Yes/No]
- Client(s): [Client names or anonymized references]
- Recommending team/community: [Team or service line name]

Vertical/Market Relevance: [Specify if applicable, or "General"]

Reference URLs:
- [URL 1]
- [URL 2]

Existing Blip URL: [If updating status, otherwise N/A]
```

## Quality Checklist

Before finalizing, verify the submission addresses these criteria:

- [ ] Clear explanation of what the blip is
- [ ] Compelling "why now" rationale with current relevance
- [ ] Concrete value proposition with specific benefits
- [ ] Appropriate quadrant selection with justification
- [ ] Ring selection supported by evidence (especially production use for Trial)
- [ ] At least 2-3 reference URLs for credibility
- [ ] Sufficient detail to defend during editorial discussions

## Common Pitfalls to Avoid

- **Vague descriptions** - Avoid generic statements; be specific about capabilities
- **Missing "why now"** - Every blip needs a timely reason for inclusion
- **Unsupported ring claims** - Trial requires production use; Adopt requires proven maturity
- **No evidence** - Submissions without client context or references are difficult to defend
- **Wrong quadrant** - Ensure the blip fits the quadrant definition

## Resources

Detailed ring criteria, quadrant definitions, and examples of strong submissions are available in `references/submission-guide.md`.
