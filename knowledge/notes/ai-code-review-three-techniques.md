---
created: 2026-03-28T20:00+09:00
type: reference
source: inbox/web-clips/2026-03-28-ai-code-review-patterns.md
tags: [code-review, llm]
mentions: [projects/api-v2]
related:
  - knowledge/notes/llm-context-window-strategies.md
---

# Three Techniques for Effective AI Code Review

Summary of key patterns from an engineering blog post on AI-assisted code review. These techniques move AI review beyond simple linting into meaningful behavioral and architectural analysis.

## 1. Semantic Diff Review

Feed the AI the complete before-and-after of each changed function or module, not just the line diff. Prompt: "Given the previous implementation and the proposed change, identify any behavioral differences not covered by the existing test suite." This transforms the AI from a style checker into a behavioral regression detector.

The critical factor is providing enough context. Line-level diffs lose the semantic intent; function-level diffs preserve it. This connects to the broader context window management challenge -- you need enough context for the AI to reason about behavior, but not so much that it loses focus.

## 2. Architecture Conformance Checking

Define architecture rules as natural language constraints and check each PR against them. Examples: "Controllers should not directly access the database layer" or "All public API responses must go through a serializer."

This is where AI review genuinely outperforms traditional tooling. Architecture rules are hard to encode as AST-level lint rules but trivial for an LLM to evaluate. Teams report catching 40% more architecture violations. For the API v2 project, we could define our layering rules and have the AI enforce them automatically.

## 3. Test Gap Analysis

For each changed code path, ask the AI to enumerate test scenarios that should exist. Compare against actual test files in the PR. The difference is the test gap. This works best combined with semantic diff review, which gives the AI the context needed to suggest meaningful (not boilerplate) test cases.

## Practical Considerations

- Cost: $0.10-0.50 per PR review depending on diff size
- Latency: 30-90 seconds, run asynchronously
- Position as "additional reviewer," not replacement
- Start with high-confidence patterns, tune thresholds iteratively
