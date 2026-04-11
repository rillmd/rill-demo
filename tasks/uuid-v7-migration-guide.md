---
created: 2026-04-07T16:00+09:00
type: task
status: open
source: inbox/meetings/2026-04-03-one-on-one-alex.md
tags: [api-design]
mentions: [projects/api-v2, people/alex-chen]
due: 2026-04-20
---

# UUID v7 Migration Guide

## Goal

Write migration guide for UUID v7 adoption — covering schema changes, backfill strategy, and client SDK updates. The guide needs to be concrete enough that Jordan can execute the client SDK changes without blocking on me.

## Background

Alex's UUID v7 vs ULID comparison (delivered April 3) landed on UUID v7 as the winner. On April 7 the team officially adopted it for the v2 API, and Alex started the schema migration work. The migration guide is the next deliverable — it needs to document the decision, the schema changes, and the path forward for every team that touches IDs.

Key points the guide must cover:

- Why UUID v7 over ULID (time-ordered, standardized, better index locality)
- Schema changes: `id` columns become `uuid` type, existing v1 records stay on the legacy scheme during parallel operation
- Backfill strategy: v1 records are not rewritten; new records get v7 IDs
- Client SDK changes: Jordan needs to update type definitions and the ID generation helper

## Context

This doc is a prerequisite for the April 15 API v2 review meeting. If it's not ready by then, the meeting stalls.

## History

- 2026-04-07: Task created after UUID v7 officially adopted
- 2026-04-03: Alex delivered the comparison doc in our 1-on-1
