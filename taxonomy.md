---
created: 2026-03-15T10:00+09:00
type: taxonomy
---

# Tag Taxonomy

## Rules

- Specify in `tags` frontmatter field as inline array: `tags: [tag1, tag2]`
- Maximum 3 per file
- Lowercase kebab-case
- **Tags are for topic discrimination only**. Entity references (projects/people/orgs) go in the `mentions` field. Do not include entity IDs in tags
- Select from the approved tags below. If no matching tag exists, create a new one and add it to this file
- Before creating a new tag, check existing tags and aliases for synonyms
- Granularity is at the theme level (do not include specific tool versions or dates)
- **Refer to the tag description (desc)** and select tags matching the definition. Guessing by name alone is prohibited
- When adding a new tag, record today's date (YYYY-MM-DD) in the "Added" column

## Topic Tags

| Tag       | Description                                                          | Aliases | Added |
|-----------|----------------------------------------------------------------------|---------|-------|
| research  | Investigation, literature review, and comparative analysis notes     |         | --    |
| decision  | Decision records, trade-off analysis, and rationale                  |         | --    |
| review    | Retrospectives, critiques, and evaluation summaries                  |         | --    |
| learning  | Learning notes, study records, and skill acquisition                 |         | --    |
| health    | Health, fitness, nutrition, and body management                      |         | --    |
| api-design| REST/GraphQL/gRPC API design patterns and conventions                |         | 2026-04-01 |
| llm       | Large language model usage, prompting, context management            |         | 2026-04-01 |
| remote-work | Remote and async collaboration patterns                            |         | 2026-04-01 |
| code-review | Code review processes, tooling, and best practices                 |         | 2026-03-28 |

## Deprecated Tags

| Old Tag | Successor | Reason |
|---------|-----------|--------|
