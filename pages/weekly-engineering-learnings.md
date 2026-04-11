---
created: 2026-04-10T16:00+09:00
type: page
id: weekly-engineering-learnings
name: Engineering Learnings — Week of April 6
description: Weekly synthesis of technical insights and decisions
updated: 2026-04-10T17:00+09:00
tags: [learning]
sources:
  - knowledge/notes/uuid-v7-vs-ulid-comparison.md
  - knowledge/notes/async-standup-experiment-lessons.md
  - knowledge/notes/llm-eval-harness-design-principles.md
---

# Engineering Learnings — Week of April 6-10

A week where three separate threads — API design, team process, and AI tooling — each landed a concrete result. Capturing the learnings while they're still fresh.

## API v2: UUID v7 Decision

After a week of back-and-forth on ULID vs UUID v7, we officially adopted UUID v7 for the v2 API on Monday. Alex's comparison doc from the previous Friday did most of the heavy lifting — the clinching factors were standardization (RFC 9562), better index locality in Postgres, and the fact that every language we care about already has a library.

The lesson worth keeping: "standardized" beat "slightly better" in this decision. ULID has some genuine ergonomic wins (shorter, Crockford base32, no hyphens), but none of them matter as much as being able to tell a new engineer "use the standard library" and have them move on.

Schema migration work started Tuesday. The backfill strategy landed on leaving v1 records untouched — new records get v7 IDs, v1 records keep their legacy scheme during parallel operation. This avoids a multi-day backfill job and keeps the migration reversible.

## Team Process: Async Standups Work

The one-week async standup trial ended Friday with unanimous agreement to keep the format. The short version of why it worked:

- Written updates forced clarity. "Yesterday I worked on X" became "Yesterday I finished X, unblocking Y, and hit a snag on Z" because you had time to think about what you'd actually done.
- The optional sync fell back to a live call exactly twice during the week. Both times it was clearly the right call, which is a good sign the escalation rule is calibrated.
- Recovered roughly 60 minutes per person per week. Not earthshaking, but the bigger win was the quality of the morning — no one started the day in a meeting they didn't need.

The honest concern: we lose the casual "how's it going" texture that live standups had. Mitigation so far is a Friday social-ish check-in, which is fine but feels contrived. TBD whether it sticks.

## LLM Evaluation: Internal Harness v0

The weekend side project (LLM eval harness) became a real working tool this week. The metric framework doc landed Wednesday, the first baseline eval ran Thursday on existing code review suggestions.

Design principles that came out of the work:

- Four metrics, not twelve. Quality, latency, cost, hallucination. Anything more and engineers stop looking at the dashboard.
- LLM-as-judge using pairwise comparison, not absolute scoring. More stable, and maps better to the question we actually want to answer ("is this better than what we had?").
- CI integration with pass/fail thresholds from day one. A dashboard no one checks is worthless; a CI check that fails loudly is impossible to ignore.
- Golden set small enough to re-run in 5 minutes. The moment an eval takes 20 minutes it silently gets skipped.

Baseline from Thursday's run: code review suggestions currently score a median 3.8 on the 1-5 quality rubric. That's roughly "acceptable but noticeably worse than reference." Useful to know — and a concrete target for the next round of prompt iteration.

## Connection Points

The three threads are more related than they look.

UUID v7 and typed errors are both about making the v2 API legible — to humans, to machines, to tooling. The async standup experiment freed up enough morning hours that the LLM harness side project could actually become a weekday thing instead of a weekend-only thing. And the LLM harness gives us a way to measure whether the AI-assisted tooling we're building (including code review for the API v2 migration itself) is actually helping or getting in the way.

The shared pattern: in all three cases, the winning move was to pick the boring, measurable option and ship it. Standardized IDs, written updates, four metrics. Nothing clever.

## Next Week

- Typed error schema finalized by April 17 (task open)
- UUID v7 migration guide drafted by April 20 (task open)
- LLM harness golden set curated by April 22 (task open)
- First full API v2 review meeting April 15
