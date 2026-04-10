---
created: 2026-04-01T20:00+09:00
type: insight
source: inbox/journal/2026-04-01-091500.md
tags: [llm, research]
related:
  - knowledge/notes/ai-code-review-three-techniques.md
---

# LLM Context Window Management Strategies

The naive approach of stuffing everything into the context window degrades model performance as earlier instructions get diluted. A structured approach to context management can significantly improve output quality without increasing costs.

## Tiered Context Architecture

The core idea is to treat the context window like a database with different storage tiers:

1. **Hot tier (always retained)**: System prompt, critical instructions, user preferences. These are pinned and never evicted. Typically 10-15% of the total window.

2. **Warm tier (recent, full fidelity)**: The last 3-5 conversation turns. Kept verbatim because recency correlates strongly with relevance.

3. **Cool tier (summarized)**: Turns 5-15 are compressed to key facts and decisions. A smaller model can handle this summarization step to save cost.

4. **Cold tier (evicted/externalized)**: Anything beyond the cool tier is either dropped or stored in an external retrieval system for on-demand access.

## Hybrid RAG Approach

The most promising architecture combines context window management with retrieval-augmented generation. RAG handles the "reference library" (documentation, code snippets, historical data) while the tiered context manages conversational state. This separation clarifies what the model needs to "remember" versus what it needs to "look up."

## Criticality Heuristic

Not all context is equally important. A rough priority ordering:

- User-stated preferences and constraints (highest)
- Recent tool outputs and their interpretations
- Conversation decisions and commitments
- General conversation history (lowest)

The challenge is building this heuristic dynamically -- what's "critical" changes as the conversation evolves. Static rules get you 80% of the way; the remaining 20% likely requires the model itself to participate in context curation.

## Practical Implications

For the API v2 project, this thinking applies directly to how we design our LLM-powered features. If we're building a code assistant, the context window strategy determines whether it can handle large codebases or only small snippets.
