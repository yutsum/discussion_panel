# Brainstorms

Store brainstorming outputs, decision memos, and experiment logs in this directory.

Suggested convention:

- one subdirectory per topic
- `shared-brief.md` for externally researched common context
- `agent-notes/` for per-participant private scratchpads
- `transcript.md` for cumulative multi-turn discussion
- `notes.md` for raw brainstorming notes or condensed summaries
- `decision-memo.md` for synthesized output
- `experiments.md` for follow-up tests

Suggested transcript structure:

- topic
- transcript file owner path
- optional shared brief file path
- optional scratchpad directory path
- optional appended shared-brief updates when facts are refreshed mid-session
- append-only round blocks in chronological order
- persona statements inside each round
- optional user response inside the same round block
- optional summary or memo links created later

Suggested scratchpad structure:

- one file per participant, for example `agent-notes/steve-jobs.md`
- append-only short updates by round
- current stance
- open questions
- candidate next move
- relevant facts from the shared brief
