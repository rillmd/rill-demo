---
created: 2026-04-08T10:00+09:00
type: workspace
id: 2026-04-08-llm-eval-harness
name: LLM Eval Harness (Internal Tool)
status: active
origin: inbox/journal/2026-04-05-165000.md
tags: [llm]
---

# LLM Eval Harness (Internal Tool)

Small internal tool for evaluating AI-assisted features in the product (code review suggestions, auto-generated docs, etc.). Not client-facing, purely an internal quality and cost measurement system. The goal is to build confidence when we ship LLM-driven features by making regressions visible before customers see them.

## Completion Criteria

- [ ] Core metrics defined (quality, latency, cost, hallucination)
- [ ] Golden set of 50 test cases curated
- [ ] CI integration with threshold-based pass/fail
- [ ] Dashboard for tracking metric drift over time
- [ ] Documentation for adding new eval types

## Related Files (MOC)

- [001 - Metric Framework](001-metric-framework.md) — initial design of what to measure

## Session History

- 2026-04-08: Workspace created from the April 5 journal exploration. Started with metric framework design.
- 2026-04-09: First eval run on existing code review suggestions. Baseline established.

## Next Steps

- [ ] Curate golden set (target 50 cases)
- [ ] Implement quality scoring via LLM-as-judge
- [ ] Benchmark existing code review suggestions
