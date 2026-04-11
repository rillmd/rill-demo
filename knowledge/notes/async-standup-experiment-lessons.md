---
created: 2026-04-10T17:00+09:00
type: insight
source: inbox/journal/2026-04-06-211500.md
tags: [remote-work]
mentions: [people/alex-chen, people/jordan-kim]
related:
  - knowledge/notes/remote-standup-async-format.md
---

# Async Standup Experiment -- One Week In

Reflections after the first week of the async standup trial (April 6-10). The experiment was motivated by the 14-hour timezone spread between Jordan and the rest of the team, which made synchronous standups impossible for everyone to attend live.

## What Worked

- **Deeper written updates.** When the format shifted from "two sentences in a video call" to "a paragraph in a thread," the quality of the updates went up noticeably. Jordan's write-ups in particular became more detailed, including code snippet links and specific questions rather than generic status.
- **Less context switching.** No one had to drop what they were doing at a fixed time. Updates were posted and read during natural transition points, which preserved focus blocks.
- **Written record for async teammates.** Team members coming online later in the day could skim the thread and catch up in 3-4 minutes. This was a real improvement over asking "what did I miss?" in DMs.
- **Design issue caught by re-reading.** Alex caught a subtle edge case in the pagination design because he had time to re-read Jordan's update and think about it, rather than respond live. He left a comment that turned into a 30-minute async discussion with a clear resolution.

## What Didn't Work

- **Slower blocker discovery.** The biggest downside. When someone was stuck, it could take 8-12 hours for another teammate to notice and respond. For a team shipping daily, this was too slow.
- **Less social connection.** The synchronous standup, however inefficient, was the only regular face-to-face moment. Without it, the team felt more transactional. Two people mentioned this independently.
- **Updates drifted toward status reports.** Despite the "blockers and decisions" format, some updates slipped back into "here's what I did yesterday" by mid-week.

## Hybrid Solution

Keep the async daily updates (they work) and add a short synchronous session for the blocker discovery problem:

- **Async daily**: Structured update posted by local morning. Format stays: blockers, decisions needed, one highlight.
- **Monday blocker bash (15 min, sync)**: One scheduled time each week, chosen to be workable (if imperfect) for all three timezones. Agenda is strictly unresolved blockers and cross-cutting decisions -- no status recap.

The weekly sync gives the team a predictable point to unblock each other and preserves a minimum level of face-to-face contact. The async daily still carries most of the load.

## Observable Signals to Watch

- Time-to-response on posted blockers (target: same business day in at least one other timezone)
- Jordan's update detail level (qualitative, but it's a useful canary)
- Whether the Monday session stays focused on blockers or drifts into general discussion

## Next Step

Continue the hybrid model for another two weeks. Revisit on April 24 with the team. If the blocker-response latency doesn't improve with the weekly sync, the next thing to try is a rotating "on-call for questions" role rather than adding more meetings.
