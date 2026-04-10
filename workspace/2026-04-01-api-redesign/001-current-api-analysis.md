---
created: 2026-04-01T16:00+09:00
topic: 2026-04-01-api-redesign
type: analysis
tags: [api-design, review]
mentions: [projects/api-v2, people/alex-chen]
---

# Current API v1 Analysis

Analysis of the existing v1 API to identify pain points, inconsistencies, and improvement opportunities for the v2 redesign.

## Overview

The v1 API has been in production for 18 months, serving 15 active clients. While functional, several design decisions made early on have become friction points as usage has scaled.

## Pain Points

### 1. Unstructured Error Responses

Errors are returned as free-text strings with HTTP status codes. Clients have built fragile parsers around specific error message formats. Any wording change risks breaking client error handling.

**Impact**: High. Every client team has complained about this. Error handling accounts for approximately 30% of support tickets.

**v2 Solution**: Typed error responses with machine-readable error codes and versioned error schemas. See [typed error patterns](../../knowledge/notes/api-error-handling-patterns-2026.md).

### 2. Inconsistent Resource Naming

Some endpoints use plural nouns (`/users`), others use singular (`/team`). Some use camelCase in response fields, others use snake_case. This inconsistency increases onboarding time for new client developers.

**Impact**: Medium. Causes confusion but clients adapt after initial integration.

**v2 Solution**: Strict naming convention -- plural nouns for collections, snake_case for all fields, consistent across every endpoint.

### 3. Missing Pagination

List endpoints return all results without pagination. This works with small datasets but causes timeouts and memory issues as clients scale. Adding pagination to v1 would be a breaking change.

**Impact**: High for clients with large datasets. Two clients have hit the 10,000-record wall.

**v2 Solution**: Cursor-based pagination by default on all list endpoints. Include `total_count` in response metadata.

### 4. Authentication Quirks

The token refresh flow has a race condition: simultaneous requests with an expired token both attempt to refresh, potentially invalidating each other's results. Alex identified this during a recent code review and proposed a promise-based lock solution.

**Impact**: Medium. Causes intermittent auth failures under high concurrency.

**v2 Solution**: Implement promise-based refresh lock. Document the refresh flow clearly in API docs.

### 5. No Batch Operations

Clients frequently need to create or update multiple resources in a single request. The current API requires individual requests, leading to N+1 call patterns and poor performance.

**Impact**: High for clients with bulk operations.

**v2 Solution**: Batch endpoint with partial failure support (per-item error reporting). Jordan Kim is drafting the schema.

## Migration Complexity Assessment

| Area | Complexity | Client Impact | Priority |
|------|-----------|---------------|----------|
| Error responses | Medium | High | P0 |
| Resource naming | Low | Medium | P1 |
| Pagination | Medium | High | P0 |
| Auth refresh | Low | Medium | P2 |
| Batch operations | High | High | P1 |

## Recommendation

Pursue incremental endpoint migration. Start with a high-traffic, low-complexity endpoint (likely `/users`) as a proof of concept. This validates the v2 patterns before committing to the full migration.

The database schema changes for the new entity model (UUID v7 identifiers) are the biggest technical risk. Alex is evaluating this and the decision should gate the migration timeline.
