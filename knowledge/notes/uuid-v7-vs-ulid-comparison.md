---
created: 2026-04-07T16:00+09:00
type: reference
source: inbox/meetings/2026-04-03-one-on-one-alex.md
tags: [api-design, decision]
mentions: [projects/api-v2, people/alex-chen]
related:
  - knowledge/projects/api-v2.md
  - knowledge/notes/api-error-handling-patterns-2026.md
---

# UUID v7 vs ULID -- Identifier Choice for API v2

Decision record for the new entity identifier format in API v2. After a week of evaluation led by Alex Chen, UUID v7 was adopted on April 7. This note captures the comparison and the reasoning.

## The Requirements

The new entity model needs identifiers that are:

1. Globally unique without coordination
2. Roughly sortable by creation time (B-tree locality for the primary key)
3. Well supported by the surrounding ecosystem (PostgreSQL, client SDKs, logging tooling)
4. Stable against clock skew and ID generation across multiple services
5. Reasonably compact in logs, URLs, and error messages

## Candidate Formats

Both UUID v7 and ULID satisfy the core requirements: 128-bit values, timestamp-prefixed, monotonic within a millisecond. The differences are in specification maturity, encoding, and ecosystem support.

## Where UUID v7 Wins

- **Standardization**: UUID v7 is defined in RFC 9562, superseding RFC 4122. ULID has a well-known README-style spec but no RFC. For a long-lived public API, the RFC matters for client teams building their own implementations.
- **Ecosystem support**: PostgreSQL 17 ships native `uuidv7()` generation. Standard libraries in Go, Rust, Python, and TypeScript have or are adding first-class support. ULID support exists but is fragmented across community libraries.
- **B-tree locality**: The 48-bit millisecond timestamp prefix gives the same database locality benefit ULID offers. Inserts are append-heavy at the B-tree level, which helps write performance on large tables.
- **Tooling compatibility**: Existing logging, tracing, and error reporting tools already parse UUID format. No custom formatters needed for ULID's Crockford base32 encoding.
- **Compatibility with existing UUID columns**: v1 already uses UUID v4 in some tables. UUID v7 shares the same type and storage, which simplifies the migration path.

## Where ULID Would Have Won

- **String form**: ULID's 26-character Crockford base32 is shorter than UUID's 36-character hyphenated hex. Easier to paste in URLs and less visually noisy in logs.
- **Readability**: Base32 avoids ambiguous characters (0/O, 1/I). Useful for manual debugging and support tickets.
- **Monotonicity guarantee**: ULID's spec mandates monotonic generation within the same millisecond. UUID v7 leaves this as an implementation detail -- the library must take care.

## Trade-offs Acknowledged

The longer string form of UUID v7 is the main cost. For URL path parameters this adds noise, but the team decided that ecosystem maturity outweighs ergonomics. The monotonicity concern is addressed by pinning to a specific library (in Go, `google/uuid` v1.6+) that handles intra-millisecond ordering correctly.

## Decision

**UUID v7 adopted on April 7, 2026.** Primary reasons: RFC-backed specification, PostgreSQL 17 native support, and the alignment with existing UUID columns in the v1 schema. Alex's comparison document is the canonical reference for the decision.

All new entity tables in API v2 will use `uuidv7()` as the default for primary keys. The migration plan for existing v1 entities keeps their current v4 IDs; only new entities get v7.
