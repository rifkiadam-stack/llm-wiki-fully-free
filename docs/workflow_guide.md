# Workflow Guide — LLM Wiki Feedback System

> This guide explains the **3 feedback loops** that make your LLM Wiki grow and become more valuable over time. Each loop has a **trigger** (when to do it), **step-by-step instructions**, and **example prompts** for Gemini.

---

## Overview: Why 3 Loops?

```
┌──────────────────────────────────────────────────────────────┐
│                      YOUR LLM WIKI                           │
│  ┌─────────┐    ┌──────────┐    ┌────────────────┐          │
│  │ Sources  │───→│ Wiki     │───→│ Synthesis &    │          │
│  │ (raw/)   │    │ Pages    │    │ Connections    │          │
│  └─────────┘    └──────────┘    └────────────────┘          │
│       ▲              ▲                  ▲                    │
│       │              │                  │                    │
│   LOOP 1         LOOP 3            LOOP 2                   │
│   Ingest         Maintain          Query→Save               │
│   (new input)    (upkeep)          (ask→store)               │
└──────────────────────────────────────────────────────────────┘
```

| Loop | Function | Analogy |
|------|----------|---------|
| **Loop 1** | Add new knowledge from external sources | Reading a new book and taking notes |
| **Loop 2** | Turn Gemini's answers/insights into permanent knowledge | Writing discussion insights into a notebook |
| **Loop 3** | Inspect and maintain wiki health | Organizing and tidying up a bookshelf |

---

## Loop 1: INGEST — Adding New Sources

### 🎯 When to Run Loop 1?

Run Loop 1 when:
1. **You've just read an interesting article/paper** and want the knowledge preserved
2. **You've found an important document** (PDF, report, meeting notes)
3. **You've watched/listened to a podcast or video** and have a transcript or notes
4. **You've gained an insight from a conversation** you want to document
5. **A Deep Research result from Loop 3** yields new findings that need ingesting

### 📋 User Steps

```
STEP 1: Save source to raw/sources/
├── Web article  → Copy-paste content → create .md in raw/sources/
├── PDF/DOCX     → Drag & drop to raw/sources/ via Google Drive
├── Notes        → Create a new .md file in raw/sources/ via Obsidian
└── Web clip     → Use Obsidian Web Clipper → save to raw/sources/

STEP 2: Ask Gemini to process
├── Open Gemini Gem (Wiki Maintainer)
├── Attach the file from Google Drive
└── Use the ingest prompt (see below)

STEP 3: Save output to wiki
├── Copy the Source Summary markdown from Gemini
├── Create new file in Obsidian: wiki/sources/[source-name].md
│   (Type only the name WITHOUT .md — Obsidian adds it automatically)
├── Update wiki/index.md (append the new entry)
└── Update wiki/log.md (append the log entry)

STEP 4: Execute Suggested Updates (optional but recommended)
├── Gemini may suggest updating purpose.md, overview.md, or other pages
├── Open those files in Obsidian and add the suggested content
└── For long suggestions, ask Gemini to write the exact paragraph to paste

STEP 5: Update wiki_snapshot.rar
├── Right-click the wiki/ folder → Compress to RAR
├── Replace the old wiki_snapshot.rar
└── The Gem now has access to your latest knowledge
```

### 💬 Example Gemini Prompts for Ingest

**Standard Ingest Prompt:**
```
Process the attached file according to WORKFLOW 1 in GEMINI.md.

1. Create a SOURCE SUMMARY in markdown with YAML frontmatter:
   - type, title, created, sources, tags, related
   - Key takeaways (3-5 bullet points)
   - Detailed summary
   - Connections to existing wiki pages (check index.md)

2. Provide SUGGESTED UPDATES for existing wiki pages
   (specify which page and what snippet to add)

3. Provide entries for index.md and log.md
```

### ✅ Checklist After Ingest
- [ ] Source summary file created in `wiki/sources/`
- [ ] New entry appended to `wiki/index.md`
- [ ] Log entry appended to `wiki/log.md`
- [ ] (Optional) Related pages updated based on Gemini's suggestions
- [ ] (Optional) Concept/Entity page created if Gemini recommended it
- [ ] `wiki_snapshot.rar` updated

---

## Loop 2: QUERY → SAVE — Preserving Valuable Answers

### 🎯 When to Run Loop 2?

Run Loop 2 when:
1. **You get an exceptionally good answer** from Gemini — deep analysis, unique insight
2. **You ask Gemini to compare** several concepts/entities in the wiki
3. **You discover a new connection** between two previously unrelated topics
4. **Gemini synthesizes information from multiple sources** into one coherent narrative
5. **An answer fundamentally changes your understanding** of a topic

### ❌ When NOT to Run Loop 2?

- Gemini's answer only repeats what's already in the wiki (no new insight)
- The question is a simple fact lookup ("When was X published?")
- The answer is too specific/contextual to that moment only

### 📋 User Steps

```
STEP 1: Evaluate Gemini's answer
├── Does this answer contain new insight?
├── Would someone else (or future you) benefit from this?
└── If YES → proceed to Step 2

STEP 2: Ask Gemini to reformat as a wiki page
├── Use the save-to-wiki prompt (see below)
└── Gemini will output markdown with proper frontmatter

STEP 3: Save to wiki
├── Copy output → create new file in:
│   ├── wiki/synthesis/    → for cross-source analysis
│   ├── wiki/queries/      → for valuable Q&A answers
│   └── wiki/comparisons/  → for side-by-side comparisons
├── Update wiki/index.md (append new entry)
└── Update wiki/log.md (append log entry)
```

### 💬 Example Gemini Prompts for Save-to-Wiki

**Save Answer Prompt:**
```
That answer was very valuable. Please reformat it as a standalone wiki page
with this format:

---
type: synthesis (or query/comparison)
title: "[descriptive title]"
created: [today's date]
sources: [list of wiki pages referenced]
tags: [relevant tags]
related: [related wiki pages in [[wikilink]] format]
---

# [Title]

[content rewritten to stand alone as a wiki page,
not as a chat answer]

Also provide entries for index.md and log.md.
```

### ✅ Checklist After Save
- [ ] New file exists in the appropriate wiki folder
- [ ] Frontmatter is complete (type, title, sources, tags, related)
- [ ] Entry appended to `wiki/index.md`
- [ ] Log entry appended to `wiki/log.md`
- [ ] (Optional) Check if other pages should link to this new page

---

## Loop 3: LINT & MAINTAIN — Wiki Maintenance

### 🎯 When to Run Loop 3?

Run Loop 3 when:
1. **Weekly** (schedule it regularly, e.g., every Friday afternoon)
2. **After a large batch ingest** (e.g., after ingesting 5+ sources at once)
3. **When the wiki feels "messy"** — hard to find things
4. **Every 20-30 new sources** ingested
5. **When you want to know** what's missing from your wiki

### 📋 User Steps

Loop 3 consists of **3 sub-operations**. You don't have to do all of them at once:

#### Sub-operation 3A: Health Check (Diagnosis)

```
STEP 1: Ask Gemini to audit the wiki
├── Attach index.md and a few key wiki pages
└── Use the lint prompt (see below)

STEP 2: Read Gemini's report
├── Gemini will provide a list of findings
├── Categorize: Critical / Nice-to-have / Ignore
└── No copy-paste here — this is just a report

STEP 3: Prioritize and act
├── Critical items → execute immediately (usually becomes a Loop 1 or manual edit)
├── Nice-to-have → add to your to-do list for later
└── Ignore → skip
```

**💬 Lint Prompt:**
```
Read wiki/index.md and wiki/overview.md.

Perform a health check on my wiki:

1. ORPHAN PAGES: Pages not linked from any other page
2. MISSING PAGES: Concepts/entities mentioned (via [[wikilink]])
   but don't have their own page yet
3. CONTRADICTIONS: Conflicting information between pages
4. SHALLOW AREAS: Topics with only 1 source, need more depth
5. STALE CONTENT: Pages that may be outdated

Format output as a checklist with priority (🔴 Critical / 🟡 Nice-to-have).
```

#### Sub-operation 3B: Update Existing Pages (Maintenance)

```
STEP 1: From the health check results, identify pages needing updates
STEP 2: Ask Gemini to generate the snippet/paragraph to add
STEP 3: Open the file in Obsidian → paste the snippet in the right section
STEP 4: Update log.md with a maintenance note
```

**💬 Update Page Prompt:**
```
Based on the health check, the page [[productivity]] needs info about [topic].

Read the file wiki/concepts/productivity.md.

Generate ONLY the new snippet/paragraph to add,
along with instructions on which section to insert it.
DO NOT output the entire file — just the new content.

Format:
📍 Insert at: [section/heading]
📝 Content:
[markdown snippet]
```

#### Sub-operation 3C: Evolve Schema (Monthly)

```
STEP 1: Review purpose.md — are the wiki's goals still relevant?
STEP 2: Review schema.md — is the structure still effective?
STEP 3: Discuss with Gemini if changes are needed
STEP 4: Update files if necessary
```

**💬 Evolve Prompt:**
```
Read purpose.md and schema.md.

Based on the current state of the wiki (see index.md):
1. Is the purpose still relevant? Does anything need revision?
2. Are there new page categories/types that should be added to the schema?
3. Are there any rules that should be changed based on experience so far?
```

### ✅ Checklist After Lint
- [ ] Health check report read and prioritized
- [ ] Critical items addressed
- [ ] Updated pages verified in Obsidian
- [ ] Log entry appended to `wiki/log.md`
- [ ] (Monthly) purpose.md and schema.md reviewed

---

## Quick Reference: Which Loop to Use?

```
Just read an interesting article?
  └── YES → LOOP 1 (Ingest)

Just got a great answer from Gemini?
  └── YES → Does it contain new insight?
        └── YES → LOOP 2 (Query → Save)
        └── NO  → No need to save

A week since the last lint?
  └── YES → LOOP 3A (Health Check)

A month since the last review?
  └── YES → LOOP 3C (Evolve Schema)

Just finished a large batch ingest?
  └── YES → LOOP 3A (Health Check) + LOOP 3B (Update)
```

---

## Important Tips

1. **Don't be a perfectionist.** The wiki doesn't have to be perfect. A slightly messy wiki that you actively use is better than a perfectly organized one that you avoid filling.

2. **log.md is Gemini's memory.** Always update the log — this is what allows Gemini to know what was done in previous sessions.

3. **index.md is the map.** Keep the index accurate — Gemini navigates the wiki through this file.

4. **Start small.** Ingest 2-3 sources first, get comfortable with the workflow, then scale up.

5. **Loop 2 is most often skipped.** Build the reflex: after getting a great answer → immediately ask "is this worth saving?"
