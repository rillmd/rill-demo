---
created: 2026-03-15T10:00+09:00
type: org
id: techcorp
name: TechCorp
tags: [api-design]
---

Mid-size B2B SaaS company (approximately 300 employees) building a developer tooling platform. Primary product is a public API that external developers integrate into their own products and workflows.

## Key Facts

- Main product: developer tooling platform exposed primarily through a public API
- Approximately 300 employees, engineering organization around 60-70 people
- Remote-first engineering culture with team members spread across North America, Europe, and APAC
- Strong code review practice -- all changes require at least one review, non-trivial changes often get two
- Currently 15 active API clients on v1, ranging from small integrations to large-volume production consumers
- API v2 redesign is the current major engineering initiative, led by this team
- Engineering uses trunk-based development with short-lived feature branches
- OpenAPI is treated as contract-of-record for public endpoints; breaking changes require version bumps
- Uses GitHub for source control, Linear for project tracking, and a Slack-based async-first communication culture
- No dedicated QA team -- engineers own quality end-to-end, backed by integration tests and staged rollouts

## See Also

- [API v2 Redesign](../projects/api-v2.md) -- Current major initiative
