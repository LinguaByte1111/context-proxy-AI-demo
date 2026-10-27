# ContextProxy AI 🚀

> *Production-grade AI context interception middleware designed to eliminate LLM token bloat, synchronize local codebases in real time, and deliver verifiable telemetry.*

---

## 🧭 Overview

When writing code with LLMs (like Claude or GPT-4), developers face a persistent dilemma: dumping entire codebases into a chat window destroys context windows and racks up unnecessary token costs, while traditional RAG systems suffer from stale indexes and rigid, manual synchronization.

**ContextProxy AI** solves this by acting as an invisible, drop-in replacement for the OpenAI Chat Completions API. It sits silently between your IDE/tools and your LLM provider, intelligently intercepting queries, matching developer intent, extracting precise code chunks from a vector store, caching redundant prompts, and streaming real-time telemetry back to a live dashboard.

---

## 🏗️ System Architecture

ContextProxy AI uses a decoupled, high-performance architecture optimized for low-latency code interception and reliable background synchronization:

```
[IDE/Client] ──> [FastAPI Proxy (Render)] ──> [Qdrant DB]
                      │
                      ├──> [LRU Cache]
                      └──> [X-Token-Savings Header]

[Watchdog Daemon] ──HTTPS + Token──> [/v1/sync/code]
```


### Core Components
1. **The Interception Gateway (FastAPI):** Mimics the standard `/v1/chat/completions` schema, allowing zero-config routing from tools like Cursor or Continue.
2. **Real-Time Sync Daemon (`watchdog`):** A lightweight background service monitoring local file changes and securely streaming updates to the cloud backend.
3. **Intent-Driven Compression & LRU Caching:** Automatically classifies developer intent (`DEBUG`, `REFACTOR`, `ARCH`) and caches responses to slash redundant vector lookups.
4. **Verifiable Telemetry Dashboard (Next.js / Vercel):** Captures real-time token savings and cache hit rates via custom HTTP response headers (`X-Cache-Hit`, `X-Token-Savings`).

---

## ⚡ Key Architectural Features

* **Drop-In OpenAI Compatibility:** Implements standard request/response models so it integrates seamlessly into existing developer tooling without modifying client logic.
* **Instant Local-to-Cloud Sync:** Uses Python's `watchdog` library coupled with pre-shared secret key authentication (`SYNC_SECRET_KEY`) to keep the cloud vector index perpetually in sync with your local workspace.
* **Custom HTTP Telemetry Injection:** Bypasses black-box UI rendering by passing live performance metrics directly through custom response headers, read natively by the frontend dashboard.
* **Resilient Fallback & Error Handling:** Gracefully falls back to localized safety code snippets if vector search latency spikes, ensuring uninterrupted developer flow.

---

## 🧰 Tech Stack

* **Backend & API:** Python 3.12, FastAPI, Uvicorn, Pydantic, Tiktoken
* **Vector Database & Storage:** Qdrant Vector DB
* **Sync Daemon:** Python `watchdog` file-system event listener
* **Frontend & Dashboard:** Next.js 14, TypeScript, Tailwind CSS
* **Deployment & Infrastructure:** Render (Backend), Vercel (Frontend)

---

## 🌐 Live Demo & Links

* **Frontend Dashboard (Vercel):** [View Live Dashboard](https://context-proxy-frontend.vercel.app)
