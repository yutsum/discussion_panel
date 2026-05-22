---
name: brainstorm-setup
description: Starts a brainstorming session by asking the user which participants to include, optionally where to save the transcript, and then handing off to brainstorm-moderator with only the selected personas. Use when Codex should begin a brainstorming session with an explicit participant-selection step instead of automatically using all available personas.
---

# Role

Set up a brainstorming session before the discussion starts.

# Important limitation

This skill does not create a native modal dialog by itself. Treat "dialog" as a structured participant-selection step in chat.

- If the runtime supports short structured user-input prompts, use them.
- Otherwise, ask a concise plain-text selection question.

# Process

1. Confirm the topic in one sentence.
2. Present the available participants:
   - `visionary-founder`
   - `steve-jobs`
   - `jensen-huang`
   - `elon-musk`
   - `analyst`
   - `contrarian`
   - `systems-architect`
   - `customer-psychologist`
   - `strategist-sunzi`
3. Ask the user which participants to include.
4. Ask whether the session should start with a shared external research brief.
   - If yes, use `brainstorm-research-brief` first.
   - If no, proceed directly.
5. Explain that the session will also maintain private participant scratchpads in `brainstorms/<topic-slug>/agent-notes/`.
6. Optionally ask where to save the transcript. If the user does not care, default to `brainstorms/<topic-slug>/transcript.md`.
7. Restate the chosen participants, whether a shared brief will be used, the scratchpad directory, and the transcript path.
8. Hand off to `brainstorm-moderator` with the selected participant list and shared brief path if available.

# Rules

- Keep setup concise.
- Do not begin the debate before the user has chosen participants.
- Do not add unselected participants.
- Preserve the exact participant list in later rounds unless the user changes it.
- If the user gives a vague answer like "standard set," interpret it as the original core set: `visionary-founder`, `contrarian`, `systems-architect`, `customer-psychologist`, and `strategist-sunzi`.
- If the user asks for current-fact grounding, prefer creating a shared brief before the first debate round.
- If the user wants factual refreshes during the debate, recommend including `analyst` in the participant set.
- Explain that `analyst` is an on-demand research role, not a normal debating voice.

# Output

Use this structure:

## Topic

## Available participants

## Selected participants

## Shared brief

## Scratchpad directory

## Transcript file

## Next action
