Use the `brainstorm-moderator` skill.

Topic:
`<replace with topic>`

Run a multi-persona brainstorming session using all available persona skills.
Keep it turn-based and simple:
- each participant speaks in sequence
- each participant speaks for 2-4 sentences with real reasoning
- each participant should refer to prior speakers by name when relevant
- each participant should be run in isolated context and should only see the transcript, shared brief if any, persona instructions, and its own scratchpad
- participants should use the shared brief only when needed and should not repeat stock phrases like `shared brief says` unless the evidence itself is the point
- within the same round, run participants sequentially and pass a round-in-progress transcript so later speakers can react to earlier speakers from that round
- across rounds, prefer reusing the same agent for the same participant while sending fresh explicit context each time
- do not include moderator lines in the transcript
- if a claim depends on current company, market, product, financial, or brand facts, or if a factual claim becomes a point of the discussion, consult the analyst brief first and ask `analyst` to research it if needed
- record the discussion to `brainstorms/<topic-slug>/transcript.md`
- after one round, stop and let the user optionally reply or continue silently
- do not summarize unless the user explicitly asks
