---
created: 2026-03-25T10:00+09:00
type: project
id: api-v2
name: API v2 Redesign
stage: active
relationship: own
tags: [api-design]
---

Major redesign of the public API, moving from the current v1 to a new v2 with improved error handling, entity model, and versioning strategy.

## Goals

- Replace unstructured error strings with typed, machine-readable error responses
- Implement new entity model with proper UUID-based identifiers
- Support incremental endpoint migration (no big-bang cutover)
- Maintain backward compatibility during the transition period

## Current Focus

Finalizing the error handling pattern and migration strategy. Database schema changes for the new entity model are the biggest technical blocker. Alex Chen is leading the migration script development.

## Watch

### Competitors

- No direct competitors tracked for this internal project

### Keywords

- "api versioning strategy"
- "typed error responses"
- "api migration patterns"

## Key Facts

- 15 active clients currently on v1
- Leaning toward incremental endpoint migration (option 3) over big-bang or parallel operation
- Path-based versioning (/v2/resources) preferred over content negotiation for clarity
- Error schemas will be versioned alongside endpoint versions
- Database schema changes for new entity model are the biggest blocker
- Team exploring UUID v7 for new entity IDs (broader ecosystem support than ULID)

## See Also

- [API Redesign Workspace](../../workspace/2026-04-01-api-redesign/_workspace.md) -- Active workspace
- [API Error Handling Patterns](../../knowledge/notes/api-error-handling-patterns-2026.md) -- Typed error design
