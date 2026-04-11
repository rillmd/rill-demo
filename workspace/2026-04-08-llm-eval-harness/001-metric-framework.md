---
created: 2026-04-08T14:00+09:00
topic: 2026-04-08-llm-eval-harness
type: research
tags: [llm]
---

# Metric Framework for LLM Eval Harness

## Motivation

We are shipping more LLM-driven features (code review suggestions, auto-generated changelogs, PR summaries) but have no principled way to detect regressions when we swap models, change prompts, or upgrade dependencies. Manual spot-checks are inconsistent and don't scale beyond a handful of prompts.

The harness should let any engineer answer three questions in under a minute:

1. Did this prompt change make the output better or worse?
2. How much does it cost per successful completion?
3. Is latency still within budget?

Anything fancier than that is a stretch goal.

## Core Metrics

Four dimensions, all reported per eval run:

### 1. Quality (LLM-as-judge pairwise comparison)

- Use a stronger model as judge comparing candidate output vs. reference output
- Pairwise win rate is more stable than absolute scoring (per anecdotal evidence and the Zheng et al. MT-Bench findings)
- Judge prompt is versioned in the repo — judge drift is a real risk
- Position bias mitigation: randomize order, run each pair twice

### 2. Latency

- p50, p95, p99 across the golden set
- Measured end-to-end (network + model + any post-processing)
- Separate cold vs. warm measurements — prompt caching can hide real regressions

### 3. Cost per successful completion

- Not just tokens in + tokens out
- Divide by success rate (judged quality pass threshold) so a cheap-but-wrong model doesn't look attractive
- Report absolute USD, not tokens, so it's legible in reviews

### 4. Hallucination rate

- Only meaningful for eval types with factual claims (e.g., code review citing line numbers, doc generation referencing APIs)
- Automated check: parse claims, verify against source
- Threshold: <2% on the golden set for anything we ship

## Scoring Rubric

Quality judge uses a 1-5 scale with explicit anchors:

- 5: Better than reference, substantively
- 4: Equal to reference
- 3: Slightly worse but acceptable
- 2: Noticeably worse, would need revision
- 1: Wrong or harmful

Pass threshold for CI: median score >= 4, no score of 1.

## Golden Set Design

- Target size: 50 cases (see `tasks/llm-eval-golden-set-curation.md`)
- Stratified across eval types (code review, doc gen, PR summary, etc.)
- Each case has: input, reference output, category tag, difficulty tag
- Reference outputs reviewed by at least one engineer before inclusion
- Refresh quarterly to prevent overfitting

Why 50 and not 500: we need to be able to re-run the full set in under 5 minutes of CI time. 50 cases gives enough statistical signal for the pairwise judge while staying cheap.

## CI Integration Sketch

```
on: [pull_request]  # only when prompts or model config change
  run eval harness against golden set
  compare to main branch baseline
  fail if median quality drops by >0.5 or cost increases >20%
  post summary as PR comment
```

Baseline is recomputed nightly on main so we're comparing against recent numbers, not stale ones.

## Open Questions

- How do we handle non-deterministic outputs? Run each case N times and average? Or fix temperature=0 and accept the coverage loss?
- Who owns the golden set long-term? Rotating ownership or a single curator?
- Do we version the judge model separately from the candidate model?
- Hallucination detection for code review is fuzzy — may need a separate harness just for that
