---
created: 2026-04-09T20:00+09:00
type: insight
source: inbox/journal/2026-04-05-165000.md
tags: [llm]
mentions: [projects/api-v2]
related:
  - knowledge/notes/llm-context-window-strategies.md
---

# LLM Eval Harness -- Design Principles

Design principles for an internal eval harness to measure the quality of AI-assisted features. Currently focused on two features shipping with API v2: AI-generated code review suggestions and auto-generated SDK documentation. The harness is a side project but increasingly feels like it needs to be first-class infrastructure.

## Why an Eval Harness

Without quantitative measurement, it is impossible to tell whether a prompt change, a model upgrade, or a retrieval tweak is an improvement or a regression. The team has already hit this twice: once when a "minor" prompt edit silently degraded code review quality, and once when swapping the base model for a cheaper variant broke doc generation in subtle ways no one noticed for a week. Both times the feedback loop was "users complained eventually." That is not sustainable.

## Core Metrics

Four metrics cover the quality/cost/reliability surface:

- **Quality (LLM-as-judge, pairwise)**: For subjective tasks like "is this code review comment useful?" a reference answer is often impossible. Pairwise comparison -- "is output A better than output B?" -- is more reliable than absolute scoring. Use a stronger judge model than the one being evaluated.
- **Latency (p50, p95, p99)**: Tail latency is what users feel. p99 matters more than average. Track separately per feature.
- **Cost per successful completion**: Divide total cost by successful output count, not by request count. A cheaper model that fails more often is not actually cheaper.
- **Hallucination rate**: For doc generation specifically, track claims that are not grounded in the source code. This one is hardest to automate -- current approach uses a secondary check that extracts claims and verifies each against the source.

## Key Design Decisions

- **Reference-free evaluation for subjective tasks.** Maintaining gold answers for "good code review" is a losing battle. Pairwise LLM-as-judge scales better and is more honest about what "correct" means for these tasks.
- **Golden set for regression tests.** For narrow tasks with known-good outputs (specific doc strings, specific error handling suggestions), maintain a small golden set. These catch regressions fast and give high-signal CI failures.
- **CI integration from day one.** An eval harness that only runs manually before releases will not prevent regressions. It must run on every PR that touches prompts, models, or retrieval code. The friction has to be near-zero.
- **Store raw outputs, not just scores.** Scores change when the judge model or rubric changes. Raw inputs and outputs let us re-score historical runs without re-running inference.

## The Core Trade-off: Coverage vs. Cost

Running the full eval suite on every PR is too expensive -- LLM-as-judge evals on a few hundred test cases can cost more than the feature's production traffic. The compromise:

- **PR-level**: Small smoke suite (~20 cases), cheap model, under $0.50 per run. Catches gross regressions.
- **Nightly**: Full suite (~300 cases), full judge model. Catches subtle quality drift.
- **Release-gated**: Full suite plus human spot checks on a random sample.

This tiering keeps daily iteration fast without giving up coverage on the things that matter before shipping.

## Status

v0 of the harness runs locally. Pairwise LLM-as-judge works, latency and cost tracking work, the golden set has about 30 entries covering the code review feature. CI integration is the next milestone -- aiming for PR-level smoke evals by end of next week.
