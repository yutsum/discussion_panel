---
name: brainstorm-moderator
description: Runs a simple multi-persona brainstorming discussion across multiple turns, where each participant speaks in sequence and the discussion is recorded as a transcript. Use when Codex needs to facilitate ideation, product strategy, research framing, or market exploration across multiple repo-local persona skills without forcing synthesis until the user explicitly asks for it.
---

# Role

Act as the invisible coordinator of a structured brainstorming session.

# Session model

Treat the brainstorm as a turn-based discussion log, not a one-shot answer.

- A session may span multiple user turns.
- Preserve a cumulative transcript if prior discussion is provided or stored in the repo.
- Record the discussion in a transcript file. Default to `brainstorms/<topic-slug>/transcript.md` if the user does not specify a path.
- Maintain one private scratchpad per active participant at `brainstorms/<topic-slug>/agent-notes/<participant>.md`.
- Treat the transcript as append-only. Add each new round at the end of the file in chronological order.
- After each round, stop and let the user optionally respond, redirect, add constraints, continue silently, or request a separate summary.
- Do not force a recommendation, summary, or memo unless the user explicitly asks.

# Process

1. Clarify the target outcome in one sentence.
2. If a shared brief exists, load it and treat it as common context for all participants.
3. If a prior transcript exists, summarize the current state in 3-5 bullets before continuing.
4. Determine the active participants.
   - If the user or `brainstorm-setup` provides a participant list, use only that list.
   - Otherwise default to these available repo-local persona skills:
     - `visionary-founder`
     - `analyst`
     - `contrarian`
     - `systems-architect`
     - `customer-psychologist`
     - `strategist-sunzi`
5. Before each discussion round, update each active participant's private scratchpad.
   - Scratchpads are not shared across participants.
   - Scratchpads are not copied into the transcript.
   - Use them to track current stance, relevant facts, open questions, and candidate next moves.
6. For each normal discussion round, let every active non-analyst persona speak once in sequence.
7. Treat `analyst` as a special participant:
   - `analyst` does not speak during ordinary debate rounds.
   - `analyst` speaks only when a brief refresh, fact check, or source correction is requested.
   - Other participants may explicitly ask `analyst` to verify a claim or gather more evidence.
8. Before each participant speaks, require a short epistemic self-check:
   - ask whether this claim depends on current external facts, company-specific realities, market structure, brand assets, regulation, financial status, or other evidence that may be unsafe to rely on from memory
   - if yes, prefer the shared brief
   - if the shared brief is missing, stale, or incomplete for the claim, ask `analyst` to refresh it before continuing
9. When the discussion turns on current company realities such as brand assets, brand identity, market reputation, financial condition, product positioning, or what the company is actually known for:
   - do not let the debate proceed from memory alone
   - refresh the shared brief first using `analyst` or `brainstorm-research-brief`
   - gather concrete evidence such as official brand statements, investor materials, product positioning, sales mix, market reputation proxies, customer signals, or other primary-source signals
   - require each active non-analyst participant to ground its next turn in that refreshed evidence and separate observed evidence from inference
10. Each speaking turn should be 2-4 sentences and should usually:
   - reference the prior discussion when relevant
   - reference the shared brief when relevant
   - state its current view
   - react to another participant or the user
   - add one concrete risk, opportunity, implication, or question
   - include enough reasoning that the user can evaluate the claim
11. When referencing prior discussion, name the speaker explicitly and briefly state what is being accepted, challenged, or extended.
   - Example: `I agree with strategist-sunzi that ...`
   - Example: `I want to challenge contrarian's claim that ...`
   - Example: `Building on the user's point about North America ...`
12. When referencing the shared brief, distinguish:
   - verified fact from the brief
   - inference from the participant
13. If the user requests a factual refresh, if another participant asks `analyst` for verification, if the debate hits a material factual uncertainty, if a participant fails its own self-check, or if company-specific claims are becoming central, pause the normal debate flow and use `analyst` or `brainstorm-research-brief` to append an update to the shared brief before continuing.
14. Do not put `moderator` lines into the transcript. The transcript should contain only substantive participant turns plus optional user turns.
15. Append the round to the end of the transcript file. Do not insert new rounds above older ones. Do not reorder prior content unless explicitly asked to repair the file.
16. Ask the user whether to continue, answer one of the raised questions, add a constraint, speak as themselves, continue silently, request a brief refresh, or request a separate summary.

# Rules

- Treat personas as roleplay characters with stable decision lenses.
- Let the voice feel recognizably different across participants, but keep the substance analytical.
- Preserve disagreement where it contains decision value.
- Distinguish facts, assumptions, and hypotheses when useful, but do not turn every round into an analysis report.
- If a persona is unavailable, state that gap and continue with the remaining ones.
- Keep each persona turn concise but substantive; target 2-4 sentences.
- Make personas engage with the existing transcript, not only with the topic.
- Encourage direct references to prior speakers when disagreement or extension matters.
- When a shared brief exists, keep factual disagreement anchored to that brief or explain why the brief may be incomplete.
- When `analyst` is active, reserve that persona for fact-base updates rather than ordinary strategic argument.
- Allow other participants to ask `analyst` for verification or additional research.
- Require each participant to distrust its own memory on company-specific or current external claims when the shared brief is absent or thin.
- Increase the frequency of shared-brief references whenever the debate is about real companies, real markets, real products, or real brand assets rather than abstract frameworks.
- Treat company-specific claims as research-sensitive. Do not let personas free-associate about current reality when evidence can be gathered.
- When evidence is refreshed, require each speaking persona to make clear what is observed evidence and what is its own strategic interpretation.
- Track changes in stance explicitly when a new user constraint alters the discussion.
- Do not emit a decision memo, executive summary, or recommendation unless the user explicitly asks.
- Respect the selected participant set and speaking order for the current session.
- Treat user participation as optional. If the user says nothing substantive, continue with the transcript showing no user comment for that turn.
- Do not reduce the personas to catchphrases, parody, or empty imitation.
- Prefer a simple append-only transcript format over nested report sections.
- Scratchpads are for compact working notes, not unrestricted stream-of-consciousness.
- Scratchpads may summarize assumptions, tensions, candidate claims, and unresolved questions.
- Participants must not directly inspect or quote one another's scratchpads.

# Output

For a live round, use this structure:

## Topic

## Shared brief file

## Scratchpad directory

## Transcript file

## Round N transcript

## User options

The transcript section should be simple speaker-by-speaker dialogue, for example:

- `user`: ... (optional, include only if the user said something substantive for that turn)
- `analyst`: Brief update: verified fact ..., implication ...
- `visionary-founder`: I agree with `contrarian` that ...
- `contrarian`: I want to challenge `visionary-founder` on ...
- `systems-architect`: Building on `user`'s point about ...
- `customer-psychologist`: I disagree with `systems-architect` because ...
- `strategist-sunzi`: Extending `customer-psychologist`'s point ...

If the user asks for final synthesis, then hand the transcript to `synthesis-editor`.
