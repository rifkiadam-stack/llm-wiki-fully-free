---
type: meta
title: "Wiki Schema"
created: 2026-05-03
updated: 2026-05-03
---

# Schema — Wiki Structure Rules

## Page Types

| Type | Folder | Description | Example |
|------|--------|-------------|---------|
| `source` | wiki/sources/ | Summary of a raw source document | `deep_work.md` |
| `entity` | wiki/entities/ | Person, organization, product, place | `cal_newport.md` |
| `concept` | wiki/concepts/ | Idea, method, theory, framework | `deliberate_practice.md` |
| `query` | wiki/queries/ | Saved answer from a Q&A session | `sleep_vs_productivity.md` |
| `synthesis` | wiki/synthesis/ | Cross-source analysis combining multiple sources | `learning_strategies_overview.md` |
| `comparison` | wiki/comparisons/ | Side-by-side comparison of concepts/entities | `value_vs_growth_investing.md` |

## Frontmatter Standard

Every wiki page MUST have this YAML frontmatter:

```yaml
---
type: source | entity | concept | query | synthesis | comparison
title: "Human-Readable Title"
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: ["filename1.md", "filename2.pdf"]
tags: [tag1, tag2, tag3]
related: ["[[page_name_1]]", "[[page_name_2]]"]
---
```

## File Naming Rules

- **Format**: lowercase_with_underscores
- **Extension**: always `.md`
- **Length**: keep concise but descriptive (2-4 words)
- **No dates in filenames** (dates go in frontmatter)
- **Examples**: `deep_work.md`, `intermittent_fasting.md`, `cal_newport.md`

## Wikilink Conventions

- Format: `[[page_name]]` (no .md extension, no folder path)
- Use display text when needed: `[[cal_newport|Cal Newport]]`
- Link liberally — more links = richer knowledge graph
- Every page should link to at least 1 other page (no orphans)

## Special Files

| File | Purpose | Update Frequency |
|------|---------|-----------------|
| `wiki/index.md` | Content catalog, Gemini's navigation map | Every ingest/save |
| `wiki/log.md` | Chronological operation record | Every operation |
| `wiki/overview.md` | Global wiki summary | After major changes |

## Log Entry Format

```markdown
## [YYYY-MM-DD] operation | Title
- Details of what was done
- Files created/updated
- Status
```

Operations: `ingest`, `query`, `save`, `lint`, `update`, `evolve`
