---
created: 2026-04-10T16:00+09:00
source-type: meeting
original-source: "Google Meet Notes"
participants: [Alex Chen, Jordan Kim]
---

Weekly wrap-up (synchronous) -- April 10

Context: first full week of async standups. Agreed to do one synchronous wrap-up to review the experiment before deciding whether to continue.

What worked:

Alex:
- Written updates are noticeably more detailed than verbal ones
- Less context switching -- could stay in a flow state until mid-morning most days
- Easier to reference other people's updates later when working on related code
- Code review turnaround felt faster

Jordan:
- Same on context switching, huge win
- Appreciated being able to write updates at the end of the day vs beginning -- better recall of what actually happened
- Integration test work benefited most, since it requires long uninterrupted blocks

Me:
- Agreed on focus. Got more done on the context window spike than I expected
- Liked having written updates to skim before diving into reviews

What didn't work:

- Slower blocker discovery. Jordan hit a client SDK issue Tuesday morning that wasn't surfaced until Wednesday's async thread, when 10 minutes of live discussion would have caught it immediately
- Harder to notice when someone is stuck vs just quiet
- No natural moment for quick "hey while we're all here" side conversations

Discussion:
- The blocker discovery issue is the main concern. Async is great for status, bad for "I'm stuck right now"
- Alex suggested a dedicated async blocker channel with a ping expectation (response within 2 hours during work hours). I'm not sure a channel solves it
- Jordan proposed a short synchronous meeting once a week specifically for blockers and cross-cutting coordination
- I pushed back on adding a meeting back, but agreed that 15 minutes on Mondays is a fair tradeoff

Decision:
- Continue async standups as the default
- Add a 15-minute synchronous "blocker bash" every Monday morning to catch anything that needs real-time discussion
- Revisit in 2 weeks

Action items:
- Me: update the async standup doc to reflect the blocker bash addition
- Alex: set up the recurring Monday 15-min meeting
- Jordan: propose a format for the blocker bash (agenda-less vs round-robin vs something else)
- All: write the first Monday blocker bash notes as a real test of the format
