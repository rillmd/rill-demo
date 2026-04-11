---
created: 2026-04-04T10:30+09:00
source-type: web-clip
url: https://engineering.example.com/blog/llm-as-judge-for-code-review
title: "LLM-as-Judge for Code Review Quality: What Actually Works"
---

# LLM-as-Judge for Code Review Quality: What Actually Works

As teams ship more code with AI assistance, the question of how to evaluate review quality has moved from academic curiosity to production concern. We've spent the last six months running LLM-as-judge evaluations against our internal code review corpus, and the results surprised us.

## Three approaches we compared

**Pairwise comparison.** The classic setup: give the judge two reviews of the same diff and ask which one is better. This works well when you have a reference baseline to compare against, but it doesn't scale. Every new review requires pairing it against something, and the choice of pair meaningfully shifts the result.

**Rubric-based scoring.** Define criteria up front (e.g., correctness, clarity, actionability, tone) and have the judge score each one on a 1-5 scale. Reproducible and interpretable, but the rubric becomes the bottleneck. We went through four iterations before the scores correlated with senior engineer judgment.

**Reference-free evaluation.** Hand the judge a diff and a review, no baseline, no comparison. Ask it to identify issues with the review itself: missed bugs, hallucinated concerns, redundant nits. This is the approach we were most skeptical of going in.

## The surprise

Reference-free works better than you'd think for code reviews, because the criteria are well-defined. "Did this review miss a null pointer dereference on line 47?" is a question the judge can answer from the diff alone. Unlike open-ended writing evaluation, code review has ground truth embedded in the artifact.

Our reference-free judge agreed with human reviewers 78% of the time on "is this review adequate," compared to 71% for rubric-based and 82% for pairwise (with a fixed baseline). The gap closes further when you factor in how much cheaper reference-free is to run at scale.

## Practical recommendations

- Start with reference-free. You can always add rubric or pairwise later if you need finer-grained signal.
- Anchor the judge with a short "what good looks like" section in the prompt. Two or three examples beats a 500-word rubric.
- Measure judge-human agreement on a small gold set every time you change the prompt. Drift is real.
- Track cost per evaluation from day one. We burned $400 in the first week before noticing we were re-running the full review on every retry.

The bottom line: if you're building an eval harness for code-adjacent tasks, don't default to pairwise. The "no reference needed" path is underrated.
