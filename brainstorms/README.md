# Brainstorms

Store brainstorming outputs, decision memos, and experiment logs in this directory.

Suggested convention:

- one subdirectory per topic
- `analyst/brief.md` for externally researched common context
- `analyst/sources/` for user-provided files and durable source material
- `analyst/index.md` for the manifest of analyst materials
- `analyst/notes.md` for extracted analyst notes
- `analyst/qa-log.md` for append-only answered-question history
- `agent-notes/` for per-participant private scratchpads
- `transcript.md` for cumulative multi-turn discussion
- `notes.md` for raw brainstorming notes or condensed summaries
- `decision-memo.md` for synthesized output
- `experiments.md` for follow-up tests

Suggested transcript structure:

- topic
- transcript file owner path
- optional shared brief file path
- optional analyst workspace path
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
- relevant facts from the analyst brief

Suggested analyst workspace structure:

- `analyst/brief.md` as the shared fact base
- `analyst/sources/` for copied user files and durable source documents
- `analyst/index.md` with origin, date, and why each file matters
- `analyst/notes.md` for extracted evidence and working notes
- `analyst/qa-log.md` for append-only answered questions
