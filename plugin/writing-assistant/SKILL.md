---
name: writing-assistant
description: Provides structured writing feedback for Articles, Conference Talk Submissions, and Short Posts. Evaluates style (active voice, conciseness, American English), clarity, narrative coherence, and formatting. This skill should be used when a user asks for feedback on their writing, wants to improve a draft, or needs a review of an article, conference talk proposal, or short post.
---

# Writing Assistant

Provide actionable writing feedback tailored to the content type. Load `references/criteria.md` for detailed per-type criteria and the full style checklist before providing feedback.

## Workflow

### Step 1: Identify the Content Type

If the user does not specify, determine the type from context:

- **Article** — long-form piece (blog post, technical article, essay), typically 500+ words
- **Conference Talk Submission** — a proposal or abstract submitted to a CFP, including title, abstract, and optional outline
- **Short Post** — brief content for social media, a newsletter blurb, or a LinkedIn/X post, typically under 300 words

If the type is ambiguous, ask the user to confirm before proceeding.

### Step 2: Read and Internalize the Content

Read the full piece before commenting. Understand:

- What is the central argument or takeaway?
- Who is the target audience?
- What is the narrative arc?

### Step 3: Deliver Structured Feedback

Structure feedback using the following sections, applying the criteria from `references/criteria.md` for the identified content type:

#### Narrative & Point
State in one sentence what the piece is arguing or communicating. Flag if this is unclear, buried, or missing entirely.

#### Style
Flag specific sentences or phrases that violate the style rules. Always quote the offending text and suggest a revision. Cover:
- Passive voice → rewrite in active voice
- Filler words and hedging language → cut or replace
- Non-American English spellings or idioms → correct
- Wordiness → tighten

#### Clarity & Flow
Comment on whether ideas connect logically. Flag:
- Abrupt transitions
- Undefined jargon
- Paragraphs that lose focus
- Sections that repeat without adding new information

#### Formatting
Flag formatting issues specific to the content type (see `references/criteria.md`). Check:
- Markdown used appropriately
- External links included where relevant and helpful
- Headers and structure match the content type's conventions

#### Strengths
Note two or three things that work well. Specific praise reinforces good patterns.

#### Priority Fixes
List the top three changes the author should make first, ranked by impact.

## Output Format

Deliver feedback in markdown. Use headers matching the sections above. Quote specific passages when pointing out issues. Offer concrete rewrites, not just descriptions of problems.

Example pattern for a style issue:
> **Passive voice:** "The decision was made by the team" → "The team decided"

Keep feedback direct and specific. Avoid vague praise or criticism.
