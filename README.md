# Discussion Panel Skills

This repository is a repo-local Codex skill pack for structured brainstorming.

It is designed around three parts:

- `VS Code / Codex` as the interaction UI
- `.codex/skills/` as the persona and orchestration skills
- `brainstorms/` as the place to store outputs

## Included skills

- `brainstorm-setup`: lets the user choose participants before discussion
- `brainstorm-research-brief`: gathers shared external context before discussion
- `brainstorm-moderator`: orchestrates the session
- `visionary-founder`: looks for asymmetric opportunity
- `steve-jobs`: pressures focus, taste, and end-to-end product quality
- `jensen-huang`: pressures platform leverage, ecosystems, and full-stack advantage
- `elon-musk`: pressures first-principles redesign, speed, and vertical integration
- `analyst`: maintains its own workspace, stores relevant source material, refreshes current facts, and updates the shared brief mid-discussion, but does not join ordinary debate turns unless research grounding is needed
- `contrarian`: breaks consensus assumptions
- `systems-architect`: pressures operational complexity
- `customer-psychologist`: focuses on motivation, trust, and adoption
- `strategist-sunzi`: looks for positioning and indirect advantage
- `synthesis-editor`: converts divergent output into a decision memo

## Repository layout

```text
.codex/
  skills/
    brainstorm-setup/
    brainstorm-research-brief/
    brainstorm-moderator/
    visionary-founder/
    steve-jobs/
    jensen-huang/
    elon-musk/
    analyst/
    contrarian/
    systems-architect/
    customer-psychologist/
    strategist-sunzi/
    synthesis-editor/
brainstorms/
prompts/
```

## How to use

### Claude Code (CLI)

Claude Code reads `.codex/skills/` automatically. Start a session in this directory:

```sh
cd /path/to/discussion_panel
claude
```

Then use the `/brainstorm` slash command:

```text
/brainstorm Should a B2B SaaS startup prioritize enterprise sales or self-serve growth?
```

You can name participants inline:

```text
/brainstorm [topic] with steve-jobs, jensen-huang, elon-musk, analyst
```

Or let the skill pick the default set (`visionary-founder`, `analyst`, `contrarian`, `systems-architect`, `customer-psychologist`, `strategist-sunzi`).

Each persona runs as a separate isolated sub-agent with its own context. The moderator coordinates turns, appends rounds to `brainstorms/<topic-slug>/transcript.md`, and stops after each round so you can reply, redirect, or continue silently.

To request synthesis after the discussion:

```text
/brainstorm synthesize the transcript at brainstorms/saas-growth-strategy/transcript.md
```

### Codex / VS Code with Codex enabled

1. Open this repository in Codex or VS Code with Codex enabled.
2. Start a chat in the repo.
3. Either:
   - use `brainstorm-setup` to choose participants first, or
   - use `brainstorm-research-brief` first when the discussion needs current external facts, or
   - use `brainstorm-moderator` directly and accept the default participant set.
4. Provide a concrete topic.
5. Let each participant speak for one round.
6. Continue the discussion or ask for a separate summary later.

Each non-analyst participant may keep a private scratchpad under `brainstorms/<topic-slug>/agent-notes/`. These files are for compact working notes and are not copied into the transcript.

`analyst` keeps a separate workspace under `brainstorms/<topic-slug>/analyst/`:

- `brief.md` for the shared fact base
- `sources/` for user-provided files and other durable research materials
- `index.md` for a manifest of stored materials
- `notes.md` for analyst-only extraction notes
- `qa-log.md` for append-only answered-question history

### Choose participants first

Use this prompt directly:

```text
Use the `brainstorm-setup` skill.

Topic:
「生成AIを活用した法人向け学習プラットフォームの戦略」

Let me choose which participants to include before the discussion starts.
After I choose, start the brainstorming session with only those participants.
Record the discussion to a transcript file.
```

You can also start from `prompts/select-participants.md`.

### Start with shared research

Use this prompt directly:

```text
Use the `brainstorm-research-brief` skill.

Topic:
「生成AIを活用した法人向け学習プラットフォームの戦略」

Search for current official and primary sources.
Create a shared brief for all participants.
Save it to `brainstorms/saas-growth-strategy/analyst/brief.md`.
Then continue into brainstorming using that shared context.
```

If you expect fact-checking or source updates during the debate, include `analyst` as a participant. `analyst` stays silent during ordinary rounds and joins only for brief refreshes, verification requests, user-file ingestion, or when participants need to ground company-specific claims instead of relying on memory.

If you want `analyst` to use a local document, give the file path or pasted content and store it in `brainstorms/<topic-slug>/analyst/sources/` before the relevant round.

### Minimum prompt

Use this prompt directly:

```text
Use the `brainstorm-moderator` skill.

Topic:
「AIを使った古典兵法・経営戦略の学習アプリ」

Run a multi-persona brainstorming session using all available persona skills.
Keep it turn-based and simple.
Each participant should speak in sequence for 2-4 sentences.
Each participant should refer to prior speakers by name when relevant.
Do not include moderator lines in the transcript.
If a claim depends on current company realities, market facts, product facts, financial status, or brand assets, consult the analyst brief first and ask `analyst` to refresh it if needed.
Record the discussion to `brainstorms/<topic-slug>/transcript.md`.
After one round, stop and let me optionally reply or continue silently.
Do not summarize unless I explicitly ask.
Append each new round at the end of the transcript file.
```

You can also start from `prompts/brainstorm.md`.

### Continue a prior session

When you want to continue from an earlier discussion, paste the existing transcript and use a continuation prompt.

```text
Use the `brainstorm-moderator` skill.

Continue this brainstorming session from the prior transcript.

Topic:
<topic>

Prior transcript:
<paste prior transcript or summary>

Continue with one more round, append it to the transcript, then stop.
```

You can also start from `prompts/continue-brainstorm.md`.

## Recommended prompting style

Use a narrow topic and explicit constraints.

Good:

- `B2B AI sales coaching for Japanese SMEs`
- `Consumer app for daily strategy learning with 3 months of runway`
- `Internal research tool with no paid acquisition`

Weak:

- `AI startup ideas`
- `Something with education`

Useful constraints to add:

- target user
- geography
- time horizon
- distribution channel
- budget
- technical constraints

## What the moderator should produce

The intended flow is:

1. clarify the target outcome
2. load the analyst brief when external facts matter
3. continue from prior transcript when available
4. update private per-agent scratchpads and the analyst workspace
5. collect separate persona turns
6. refresh the analyst brief mid-session when facts need updating, when user files are added, or when participants are leaning on unsafe memory
7. append the round to the transcript
8. let participants refer to prior speakers by name when relevant
9. stop and let the user optionally comment or choose the next move
10. synthesize only on request

When `analyst` is present, treat it as an on-demand research function rather than a standard debating voice. The default expectation is that participants should distrust their own memory on company-specific current claims and either cite the analyst brief or ask `analyst` to refresh it from the analyst workspace.

For a live round, the expected output shape is:

- `Topic`
- `Analyst workspace`
- `Shared brief file`
- `Scratchpad directory`
- `Transcript file`
- `Round N transcript`
- `User options`

For final synthesis, hand off to `synthesis-editor` only when the user explicitly asks.

## Output storage

Store each brainstorming session under its own subdirectory in `brainstorms/`.

Suggested files:

- `transcript.md` for cumulative discussion history
- `analyst/brief.md` for common researched context
- `analyst/sources/` for user-provided files and stored research material
- `analyst/index.md` for the analyst's material manifest
- `agent-notes/` for per-participant private scratchpads
- `notes.md` for optional raw notes
- `decision-memo.md` for optional synthesized output
- `experiments.md` for follow-up tests

Example:

```text
brainstorms/
  strategy-learning-app/
    analyst/
      brief.md
      index.md
      notes.md
      qa-log.md
      sources/
    agent-notes/
    transcript.md
    notes.md
    decision-memo.md
    experiments.md
```

## Design principle

These skills work best when personas are treated as roleplay characters with stable decision lenses.

Prefer:

- a recognizable speaking style
- a stable evaluative lens
- concrete judgment in each turn
- explicit reference to prior speakers when relevant

Avoid empty imitation, catchphrase-heavy parody, or personas with no clear decision lens.
