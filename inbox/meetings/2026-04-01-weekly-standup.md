---
created: 2026-04-01T10:00+09:00
source-type: meeting
original-source: "Google Meet Notes"
participants: [Alex Chen, Jordan Kim]
---

Weekly standup -- April 1

Alex:
- Finished the auth middleware refactor, ready for review
- Working on the database migration scripts for the new entity model
- Blocker: need decision on whether to use UUID v7 or ULID for new entity IDs

Jordan:
- Completed the client SDK TypeScript types update
- Starting integration tests for the batch endpoint
- Question: should batch endpoint errors be all-or-nothing or partial?

Me:
- Reviewed the error handling patterns doc, have some ideas for typed errors
- Will start on the context window management spike
- Want to discuss standup format -- async might work better for us

Discussion:
- UUID v7 vs ULID: leaning toward UUID v7 for broader ecosystem support. Alex will do a quick comparison doc
- Batch errors: consensus on partial failure with per-item error reporting. Jordan will draft the schema
- Standup format: agreed to try async for 2 weeks as an experiment

Action items:
- Alex: UUID comparison doc by Wednesday
- Jordan: Batch error schema draft by Thursday
- Me: Async standup proposal by Friday
