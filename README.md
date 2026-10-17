# Santosha Pilates — project index

**This repo:** navigation only ([github.com/tompearson/Santosha](https://github.com/tompearson/Santosha)). Clone the two application repos **next to this README** (see [Setup](#setup)).

Each subproject has its own `README.md`, remotes, and deploy targets.

## Table of contents

1. [Setup](#setup)
2. [Repositories](#repositories)
3. [How they connect](#how-they-connect)
4. [Quick start (local demo)](#quick-start-local-demo)
5. [Where to read more](#where-to-read-more)

---

## Setup

Typical layout on your machine:

```text
~/Projects/Santosha/
├── README.md                 ← this repo
├── documentation-helper/     ← git clone
└── Iframe/                   ← git clone
```

```bash
mkdir -p ~/Projects/Santosha && cd ~/Projects/Santosha
git clone https://github.com/tompearson/Santosha.git .
git clone -b site/santoshapilates https://github.com/tompearson/documentation-helper.git
git clone https://github.com/tompearson/Iframe.git
```

If you already have the subfolders, clone only this index: `git clone https://github.com/tompearson/Santosha.git ~/Projects/Santosha-meta` or init from an existing folder (see repo history).

---

## Repositories

| Folder | GitHub | Role |
|--------|--------|------|
| [`documentation-helper/`](documentation-helper/) | [tompearson/documentation-helper](https://github.com/tompearson/documentation-helper) (branch `site/santoshapilates`) | Tavily ingest, Gemini RAG, Chroma/Pinecone, **Render API** (`POST /query`), optional Streamlit |
| [`Iframe/`](Iframe/) | [tompearson/Iframe](https://github.com/tompearson/Iframe) | Embeddable chat UI, local BFF, **Vercel** `/api/query` → Render |

Open the subfolder in Cursor when working on that codebase (each has its own `AGENTS.md`).

---

## How they connect

```text
Browser (iframe on Vercel or local /demo)
  → Iframe BFF (/api/query, secrets server-side)
    → documentation-helper RAG API on Render (or localhost:8000)
      → Pinecone + Gemini
```

- **Secrets** (`GEMINI_API_KEY`, `RAG_SERVICE_SECRET`) live in Render / Vercel / local `.env` — never in browser JS.
- **Content updates:** recrawl via `documentation-helper` — see [`documentation-helper/TAVILY_RECRAWL.md`](documentation-helper/TAVILY_RECRAWL.md).

---

## Quick start (local demo)

From **Iframe** (starts RAG + embed if needed):

```bash
cd ~/Projects/Santosha/Iframe
cp .env.example .env   # RAG_SERVICE_SECRET matches documentation-helper
./scripts/start-local-demo.sh --open
```

Demo URL: **http://127.0.0.1:8780/demo/**

---

## Where to read more

| Topic | Doc |
|-------|-----|
| Ingest, Pinecone, Render | [`documentation-helper/CHEATSHEET.md`](documentation-helper/CHEATSHEET.md) |
| HTTP API contract | [`documentation-helper/docs/planning/RAG_API_CONTRACT.md`](documentation-helper/docs/planning/RAG_API_CONTRACT.md) |
| Embed architecture & limits | [Iframe ARCHITECTURE_NOTES](https://github.com/tompearson/Iframe/blob/main/docs/ARCHITECTURE_NOTES.md) (local: `Iframe/docs/…`) |
| Vercel deploy | [Iframe VERCEL.md](https://github.com/tompearson/Iframe/blob/main/docs/VERCEL.md) |
