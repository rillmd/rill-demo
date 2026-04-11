---
created: 2026-04-07T10:00+09:00
type: task
status: open
source: inbox/journal/2026-04-01-143000.md
tags: [api-design]
mentions: [projects/api-v2]
due: 2026-04-17
related:
  - workspace/2026-04-01-api-redesign/_workspace.md
  - knowledge/notes/api-error-handling-patterns-2026.md
---

# Typed Error Schema Finalization

## Goal

Finalize the typed error response schema spec for v2 API. Output is a single spec doc that the backend and client SDK teams can implement against without further clarification.

## Background

The typed error insight note (`knowledge/notes/api-error-handling-patterns-2026.md`) documented the three dominant patterns for structured API errors and the tradeoffs between them. Alex's input during the April 3 1-on-1 pushed us toward a discriminated-union approach with stable machine-readable `code` values and human-readable `message` fields kept deliberately separate.

The v1 API's unstructured error strings were the number one pain point in the pre-redesign analysis (April 1 workspace). Fixing this is non-negotiable for v2.

## Context

Open questions still to resolve before the spec is final:

- Are error codes namespaced (e.g., `auth.token_expired`) or flat (`TOKEN_EXPIRED`)?
- How do we version the error schema itself when we add new codes?
- Should validation errors include a `field` pointer or a JSON Pointer path?

These need to be nailed down before the April 17 deadline so the backend team can start implementation the following week.

## History

- 2026-04-07: Task created as part of the week's planning
- 2026-04-01: Typed error design initiated during v1 API analysis
