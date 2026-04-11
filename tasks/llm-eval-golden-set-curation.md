---
created: 2026-04-08T15:00+09:00
type: task
status: open
source: workspace/2026-04-08-llm-eval-harness/001-metric-framework.md
tags: [llm]
due: 2026-04-22
related:
  - workspace/2026-04-08-llm-eval-harness/_workspace.md
---

# LLM Eval Golden Set Curation

## Goal

Curate 50 high-quality test cases for the LLM eval harness golden set. Each case needs an input, a reference output, a category tag, and a difficulty tag, reviewed by at least one engineer before inclusion.

## Background

The metric framework doc (001-metric-framework.md) settled on 50 cases as the target. The reasoning: 50 is enough to get a stable pairwise-judge signal without so much noise that small prompt changes disappear in the variance, but small enough that re-running the full set in CI stays under a 5-minute budget. Anything larger and engineers will start skipping the eval, which defeats the purpose.

Stratification target (rough):

- 20 code review suggestion cases
- 15 doc generation cases
- 10 PR summary cases
- 5 edge cases (adversarial or ambiguous inputs)

## Context

The cases should come from real production traffic (scrubbed for PII) rather than synthetic examples. Synthetic cases tend to be too clean and hide the messiness that real prompts have to deal with.

## History

- 2026-04-08: Task created after metric framework design was drafted
