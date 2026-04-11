---
created: 2026-04-03T15:00+09:00
source-type: meeting
original-source: "Google Meet Notes"
participants: [Alex Chen]
---

1-on-1 with Alex -- April 3

Agenda:
- UUID v7 comparison update
- Database migration timeline
- Team health check

UUID v7 comparison:
- Alex walked through the draft doc
- v7 wins on ecosystem support, timestamp-ordering, and Postgres index locality
- ULID wins on string representation (marginally more readable)
- Benchmarks on our workload: v7 ~18% faster inserts on the hot table, negligible difference on reads
- Decision: locking in UUID v7

Database migration concerns:
- Alex is worried about the entity ID migration window
- Current estimate is 2 hours, but Alex thinks it's closer to 6-8 once you factor in index rebuilds and the foreign key constraint chain
- Suggestion: break into two phases -- add v7 columns alongside existing IDs first, backfill, then swap in a second maintenance window
- I agreed this is safer. Alex will update the migration plan.

Team observation from Alex:
- Code review quality has gotten noticeably better in the last 3-4 weeks
- Theory: the async standup trial (even though it's only a few days in) is giving people more heads-down time to actually read diffs carefully
- Worth noting in the async standup retrospective

The "3am thinking" heuristic:
- Alex shared how he evaluates risk on deploys: "If I'd be anxious about this at 3am, it's not ready"
- Different from the usual checklist approach -- it's a gut-check on whether you understand the failure modes
- I want to steal this

Action items:
- Alex: finalize UUID v7 doc by EOD today, share for review
- Me: review UUID v7 doc tonight, reply with any pushback tomorrow morning
- Alex: rework migration plan into two-phase approach, target draft by Monday
- Me: bring up review quality observation in async standup retro
