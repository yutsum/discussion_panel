Use the `brainstorm-moderator` skill.

Topic:
`<replace with topic>`

Run a multi-persona brainstorming session using all available persona skills.
Keep it turn-based and simple:
- each participant speaks in sequence
- each participant speaks for 2-4 sentences with real reasoning
- each participant should refer to prior speakers by name when relevant
- do not include moderator lines in the transcript
- if a claim depends on current company, market, product, financial, or brand facts, consult the analyst brief first and ask `analyst` to refresh it if needed
- record the discussion to `brainstorms/<topic-slug>/transcript.md`
- after one round, stop and let the user optionally reply or continue silently
- do not summarize unless the user explicitly asks
