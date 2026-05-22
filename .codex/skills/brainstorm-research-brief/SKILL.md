---
name: brainstorm-research-brief
description: Gathers externally researched shared context for a brainstorming session, writes a concise research brief for all personas to use, and distinguishes verified facts from inference and open questions. Use when the discussion depends on current facts, official documents, market context, financial status, regulations, or any other information that benefits from web research before the debate begins.
---

# Role

Prepare a shared research brief before a brainstorming session.

# Purpose

Personas should not each reinvent the factual setup independently.

Use this skill to create one common fact base that every participant can reference during the discussion.

# Process

1. Clarify the topic and what needs verification.
2. Search externally when the topic depends on current, changing, or source-sensitive facts.
3. Prefer official and primary sources whenever possible.
4. If an analyst workspace already exists, inspect it first so you do not duplicate stored material.
   - Canonical workspace: `brainstorms/<topic-slug>/analyst/`
   - Canonical brief: `brainstorms/<topic-slug>/analyst/brief.md`
   - If only legacy `brainstorms/<topic-slug>/shared-brief.md` exists, treat it as seed material and migrate or mirror its useful content.
5. Produce a concise shared brief with:
   - verified facts
   - source links
   - date context
   - explicit inferences
   - key unknowns
6. Save the brief to `brainstorms/<topic-slug>/analyst/brief.md` unless the user specifies another path.
7. If a brief already exists, append a dated `Update` section at the end instead of rewriting the whole file unless repair is explicitly requested.
8. Update `brainstorms/<topic-slug>/analyst/index.md` with any newly added source artifacts or authoritative links worth preserving for later analyst questions.
9. Hand the brief to `brainstorm-setup` or `brainstorm-moderator` as common context for all participants.

# Rules

- Prefer official company sites, filings, investor materials, primary documentation, and original releases.
- If a source is current-sensitive, include the concrete publication or reporting date.
- Separate facts from your own inference.
- Keep the brief short enough that it can be reused across multiple rounds.
- Include only the facts that materially change the quality of the discussion.
- If the evidence is mixed or incomplete, state that clearly instead of smoothing it over.
- Prefer append-only updates when refreshing an existing brief during an ongoing session.
- Treat the analyst workspace as the durable store for research materials across rounds.

# Output

Use this structure:

## Topic

## Shared brief file

## Verified facts

## Inferences

## Open questions

## Sources
