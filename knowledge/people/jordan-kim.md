---
created: 2026-03-22T10:00+09:00
type: person
id: jordan-kim
name: Jordan Kim
aliases: [J. Kim, Kim]
role: Software Engineer
relationship: team
tags: [api-design]
---

Teammate on the API v2 redesign. Owns the client SDK and integration testing surface. Strong advocate for schema-first design and rigorous contract testing.

## Key Facts

- Owns the TypeScript client SDK and the type generation pipeline that derives client types from OpenAPI specs
- Based in a European/APAC timezone -- roughly 14 hours offset from the rest of the team, which is the main motivation for the async standup experiment
- Detail-oriented reviewer: consistently catches edge cases around null handling, pagination boundaries, and error shape mismatches
- Drafted the batch endpoint error schema, including the per-item error envelope
- Proposed the partial failure pattern for batch endpoints (return 207-style mixed results) over the all-or-nothing approach -- accepted by the team
- Maintains the integration test suite that runs client SDK against a staging server on every PR
- Known for thorough test coverage -- writes contract tests before implementation when possible
- Schema-first advocate: pushes for OpenAPI spec to be the source of truth, with both server handlers and client types generated from it
- Has a running list of v1 SDK ergonomic issues to avoid repeating in v2
