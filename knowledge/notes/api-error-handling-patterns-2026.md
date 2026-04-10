---
created: 2026-04-01T21:00+09:00
type: insight
source: inbox/journal/2026-04-01-143000.md
tags: [api-design, decision]
mentions: [projects/api-v2, people/alex-chen]
related:
  - knowledge/projects/api-v2.md
---

# Typed Error Responses -- A Pattern for API v2

Moving from unstructured error strings to typed, machine-readable error responses. This is a key improvement for the v2 API migration.

## The Problem with String Errors

Most APIs return errors as human-readable strings with HTTP status codes. Clients end up parsing these strings to determine error types -- a fragile approach that breaks whenever the error message wording changes. Localization becomes impossible without a lookup table that maps English strings to localized versions.

## Discriminated Union Error Pattern

Errors should be structured data, just like success responses:

```json
{
  "error": "VALIDATION_FAILED",
  "field": "email",
  "constraint": "format",
  "detail": "Expected RFC 5322 format"
}
```

The `error` field acts as a discriminant -- clients pattern-match on it to determine handling. The remaining fields provide context specific to that error type. Each error type has a defined schema, versioned alongside the API.

## Key Design Principles

1. **Error schemas are part of the API contract**: Document them in OpenAPI specs. Breaking changes to error shapes require version bumps.
2. **Machine-readable first, human-readable second**: The `detail` field is for developer debugging, not end-user display. Client apps should map error codes to their own localized messages.
3. **Hierarchical error codes**: Use dot notation for specificity (`VALIDATION.FORMAT`, `VALIDATION.REQUIRED`, `AUTH.TOKEN_EXPIRED`). Clients can handle at whatever granularity they need.
4. **Include recovery hints**: Where possible, include an `action` field suggesting what the client should do (`"action": "refresh_token"`).

## Versioning Strategy

Error types should be versioned alongside the endpoint version. When the API moves from v1 to v2, clients know that error shapes may change. Within a version, error types are additive only -- new error codes can be added but existing ones cannot be removed or changed.

Alex Chen's "typed errors" framing captures this well: errors deserve the same type discipline we apply to request and response bodies.
