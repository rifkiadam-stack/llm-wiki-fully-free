# LLM Wiki: Personal Knowledge Base (Free & Portable)

> Build a compounding Second Brain using **Gemini Web UI + Google Drive + Obsidian** — no API keys, no coding, no server. Works from any device with a browser.

## 🧬 Inspiration

This project is an implementation of [Andrej Karpathy's LLM Wiki Pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f), which proposes using an LLM as a **wiki maintainer** that incrementally builds a persistent, interlinked knowledge base — instead of the traditional RAG approach that re-derives knowledge from scratch every query.

The original concept was further explored through [nashsu's llm-wiki repository](https://github.com/nichnarmada/llm-wiki), which implements a desktop application with Tauri + API integration. **This repo takes a different approach**: achieving similar results using only free, widely-available tools — no API, no custom app, no local server.

## 💡 What Makes This Setup Different?

### The Problem with Chat & RAG
When you interact with an LLM in a typical chat or RAG setup, the knowledge is **ephemeral**. You ask a question, the LLM reads some chunks, gives an answer, and then it's gone. The next time you ask a complex question, the AI has to re-derive all the connections from scratch. The knowledge doesn't accumulate.

### The LLM Wiki Solution
Instead of retrieving from raw documents at query time, the LLM incrementally builds and maintains a **persistent wiki** — a structured, interlinked collection of Markdown files where every new piece of knowledge is woven into the existing fabric.

## 🏗️ Architecture

```
┌──────────────────────────────────────────────────────────────┐
│                     Google Drive (Cloud)                     │
│  ┌─────────────┐  ┌──────────┐  ┌────────────────────────┐  │
│  │ raw/sources/ │  │ wiki/    │  │ wiki_snapshot.rar      │  │
│  │ (PDFs, docs) │  │ (.md)    │  │ (daily snapshot)       │  │
│  └──────┬───────┘  └────┬─────┘  └───────────┬────────────┘  │
│         │               │                    │               │
└─────────┼───────────────┼────────────────────┼───────────────┘
          │               │                    │
    ┌─────▼─────┐   ┌─────▼──────┐   ┌────────▼────────┐
    │  Gemini   │   │  Obsidian  │   │  Gemini Gem     │
    │  Web UI   │   │  (Viewer)  │   │  (Custom Agent) │
    │  (Ingest) │   │  Graph View│   │  (Deep Query)   │
    └───────────┘   └────────────┘   └─────────────────┘
```

| Tool | Role | How |
|------|------|-----|
| **Google Drive** | Storage & Sync | Auto-syncs local ↔ cloud. Gemini reads files directly from Drive |
| **Gemini Web UI** | Brain / Maintainer | Reads sources, generates wiki pages, suggests cross-references |
| **Gemini Gem** | Persistent Agent | Pre-configured with GEMINI.md + wiki_snapshot.rar for instant queries |
| **Obsidian** | Viewer & Editor | Visualizes `[[wikilinks]]` as a knowledge graph, paste Gemini output here |
| **wiki_snapshot.rar** | Full Knowledge Snapshot | Daily RAR of the entire `wiki/` folder, attached to Gem for deep queries |

## ✅ Advantages

- **100% Free** — No API keys, no subscriptions. Uses free Gemini Web UI + free Google Drive + free Obsidian
- **Fully Portable** — Query your wiki from any device (phone, tablet, work laptop) via Gemini Web UI. No local setup needed beyond the first time
- **Cross-Device Sync** — Google Drive handles sync automatically. Open Obsidian on any computer with Drive installed, and your wiki is there
- **No Coding Required** — The entire workflow is copy-paste between Gemini and Obsidian. No Python scripts, no terminal commands
- **Future-Proof** — All data is stored as plain Markdown files. If you ever want to migrate to a different LLM or tool, your knowledge is portable

## ⚠️ Limitations

- **Semi-Automated (Human-in-the-Loop)** — You must manually copy Gemini's output and paste it into Obsidian. Gemini cannot write files to your Drive directly
- **Manual File Selection** — Gemini Web UI cannot browse your Drive folders autonomously. You must attach files (like `index.md`) manually when prompting
- **RAR Snapshot Grows Over Time** — As your wiki accumulates hundreds of pages, `wiki_snapshot.rar` will grow larger. This may eventually approach Gemini's file upload limits
- **RAR Must Be Manually Updated** — After adding new wiki pages, you need to re-create `wiki_snapshot.rar` (right-click `wiki/` folder → Compress to RAR) to keep the Gem's knowledge current

## 📁 Directory Structure

```text
My_LLM_WIKI/
├── GEMINI.md              ← System prompt / operating instructions for Gemini
├── purpose.md             ← Goals, scope, and key questions of the wiki
├── schema.md              ← Rules for file naming, frontmatter, and page types
├── README.md              ← This file
├── wiki_snapshot.rar      ← Daily RAR snapshot of wiki/ (attached to Gem)
├── raw/
│   ├── sources/           ← Raw, immutable source documents (PDFs, articles)
│   └── assets/            ← Images and media
├── wiki/
│   ├── index.md           ← Content catalog (Gemini's navigation map)
│   ├── log.md             ← Chronological append-only operation history
│   ├── overview.md        ← Global summary of current wiki state
│   ├── sources/           ← LLM-generated summaries of raw documents
│   ├── entities/          ← Pages for people, organizations, places
│   ├── concepts/          ← Pages for ideas, methods, theories
│   ├── queries/           ← Saved valuable Q&A answers
│   ├── synthesis/         ← Cross-source analysis
│   └── comparisons/       ← Side-by-side comparisons
└── .obsidian/             ← Pre-configured settings for optimal graph viewing
```

## 🔄 How It Works: The 3 Loops

### Loop 1: Ingest (Add New Knowledge)
1. Save a new document (PDF, article, notes) to `raw/sources/`
2. Open Gemini Gem → attach the new file → prompt: *"Process this file"*
3. Gemini generates: **Source Summary** + **Suggested Updates** + **Index Entry** + **Log Entry**
4. Copy-paste each output to the correct file in Obsidian
5. (Optional) If Gemini suggests creating a Concept/Entity page, approve it
6. Re-create `wiki_snapshot.rar` to update the Gem's knowledge

### Loop 2: Query → Save (Preserve Insights)
1. Ask Gemini a question via the Gem
2. If the answer contains a valuable new insight, ask Gemini to format it as a wiki page
3. Save it into `wiki/queries/`, `wiki/synthesis/`, or `wiki/comparisons/`
4. Update `index.md` and `log.md`

### Loop 3: Lint & Maintain (Keep It Healthy)
1. Periodically ask Gemini to perform a health check on `index.md`
2. Gemini reports: orphan pages, missing pages, contradictions, shallow areas
3. Execute the recommended fixes to keep the knowledge graph connected

## 🚀 Getting Started

### Step 1: Setup Folders
1. Clone or copy this template to your Google Drive
2. Install [Google Drive for Desktop](https://www.google.com/drive/download/) to sync files locally
3. Open the synced folder as a **Vault** in [Obsidian](https://obsidian.md/)

### Step 2: Setup Gemini Gem
1. Go to [gemini.google.com](https://gemini.google.com/) → Gems → **+ New Gem**
2. Name: `Wiki Maintainer`
3. Instructions:
   ```
   You are the LLM Wiki maintainer.
   Your operating instructions are in the attached GEMINI.md.
   Your scope and goals are in purpose.md.
   The content catalog is in index.md.
   The full knowledge base is in wiki_snapshot.rar.
   When processing a new source, follow WORKFLOW 1 in GEMINI.md.
   When answering questions, search wiki_snapshot.rar contents only.
   NEVER use .md extension in wikilinks. Use [[page_name]], NOT [[page_name.md]].
   ```
4. Under **Knowledge**, attach these 4 files from your Drive:
   - `GEMINI.md`
   - `purpose.md`
   - `wiki/index.md`
   - `wiki_snapshot.rar`
5. Save the Gem

### Step 3: Your First Ingest
1. Drop any PDF or article into `raw/sources/`
2. Open your Gem → attach the file → type: *"Process this new source file"*
3. Copy the outputs to the appropriate files in Obsidian
4. Watch your knowledge graph grow in Obsidian's Graph View! 🎉

## 📖 Detailed Workflow Guide

For a comprehensive step-by-step guide on each feedback loop (including example prompts and checklists), see **[docs/workflow_guide.md](docs/workflow_guide.md)**.

## 🙏 Credits

- [Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — Original LLM Wiki concept and pattern
- [nashsu (Nich Narmada)](https://github.com/nichnarmada/llm-wiki) — Desktop implementation that inspired the architecture exploration
- Built with ❤️ using [Google Gemini](https://gemini.google.com/), [Google Drive](https://drive.google.com/), and [Obsidian](https://obsidian.md/)
