<div align="center">

# 🧠 Claude-Code-Mastery

### The Complete Architectural Reference for Claude AI & Claude Code

*A bilingual, engineering-grade knowledge base for developers who want to go beyond "prompting" and actually master the agentic stack.*

[![Made For](https://img.shields.io/badge/Made%20For-Developers-blueviolet?style=for-the-badge)](#)
[![Focus](https://img.shields.io/badge/Focus-Claude%20%7C%20Claude%20Code%20%7C%20MCP-orange?style=for-the-badge)](#)
[![Docs](https://img.shields.io/badge/Docs-Bilingual%20🇬🇧%20🇵🇰-green?style=for-the-badge)](#)
[![Status](https://img.shields.io/badge/Status-Actively%20Maintained-success?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](#)
[![PRs Welcome](https://img.shields.io/badge/PRs-Welcome-brightgreen?style=for-the-badge)](#)

</div>

---

## 🎯 Repository Mission

> **"Don't just use the tool. Understand the architecture behind it."**

`Claude-Code-Mastery` exists to give developers — especially in **Pakistan** and the broader South Asian tech community — a single, structured, no-fluff reference for understanding and operating **Claude AI** and **Claude Code** at an expert level.

This repository is built on three pillars:

| Pillar | What It Means |
|---|---|
| 🏗️ **Architecture First** | We explain *why* Claude Code works the way it does (LLM reasoning + terminal access + MCP), not just *how* to type commands. |
| 🌍 **Local Relevance** | Every guide includes a Pakistan-specific lens: USD pricing realities, and free local-model alternatives via Ollama. |
| 🗣️ **Bilingual Access** | Core technical content is written in **English** and **Roman Urdu**, so the depth of the material is never gatekept by language. |

---

## 🏛️ Architectural Overview

Claude Code is not a single feature — it's a layered system. This repo is organized around that same layered mental model:

```
┌────────────────────────────────────────────────────────────┐
│                     YOUR TERMINAL / IDE                    │
│        (where you type natural-language instructions)      │
└───────────────────────────┬──────────────────────────────────┘
                            │
┌───────────────────────────▼──────────────────────────────────┐
│                  CLAUDE CODE (Agent Layer)                  │
│  • Reads & plans across your codebase                       │
│  • Executes file edits, bash commands, git operations       │
│  • Asks permission before risky actions (Hooks system)      │
└───────────────────────────┬──────────────────────────────────┘
                            │
┌───────────────────────────▼──────────────────────────────────┐
│         MCP — Model Context Protocol (Connector Layer)      │
│  • Bridges Claude to external tools: GitHub, databases,     │
│    Slack, Google Drive, custom internal APIs                │
└───────────────────────────┬──────────────────────────────────┘
                            │
┌───────────────────────────▼──────────────────────────────────┐
│              THE REASONING ENGINE (Claude Model)             │
│  • Sonnet / Opus / Haiku family — the "brain" that plans,    │
│    reasons, and generates code & decisions                  │
└──────────────────────────────────────────────────────────────┘
```

Each layer maps directly to a section in our [Comprehensive Guide](docs/COMPREHENSIVE-GUIDE.md).

---

## 🧭 Navigation Table

| 📄 File | 📌 Purpose | 🌐 Language |
|---|---|---|
| [`README.md`](README.md) | You are here — mission, architecture, quick orientation | EN + Roman Urdu summary |
| [`docs/COMPREHENSIVE-GUIDE.md`](docs/COMPREHENSIVE-GUIDE.md) | Full technical master file: history, internals, pricing vs. Ollama, skill roadmap, hook-based security | EN + Roman Urdu (full) |
| [`visuals_and_structure.md`](visuals_and_structure.md) | Recommended repo folder layout, asset organization, and badge library | EN |
| `assets/` | Screenshots, diagrams, terminal recordings | — |
| `.github/` | Issue templates, contribution guidelines, community health files | — |

---

## 🇬🇧 Why This Matters for Pakistani Developers

Claude Code represents a structural shift in how software gets built: instead of writing every line manually, a developer **directs an agent** that can read an entire repository, plan multi-file changes, run tests, and self-correct. For Pakistani developers — many of whom work as freelancers on global platforms or in lean local startups — this translates into a real competitive advantage: a single developer can credibly deliver work that previously required a small team, competing on **global timelines without global headcount**. The cost barrier (USD-denominated pricing vs. PKR income) is real, but as this guide covers, there are practical paths around it — from disciplined use of the entry-level subscription tier to **fully free local workflows using Ollama** for learning, prototyping, and non-sensitive work.

## 🇵🇰 Pakistani Developers Ke Liye Iski Ahmiyat (Roman Urdu)

Claude Code asal mein software banane ka tareeqa hi badal deta hai: ab developer har line khud nahi likhta, balke ek **AI agent ko direct karta hai** jo poora codebase parh sakta hai, multiple files mein changes plan kar sakta hai, tests chala sakta hai, aur apni ghaltiyan khud theek kar sakta hai. Pakistani developers ke liye — jin mein bohat se freelancing platforms par kaam karte hain ya chhoti local startups mein hotay hain — iska matlab hai ek **real competitive advantage**: ab aik single developer wo kaam credibly deliver kar sakta hai jiske liye pehle poori team chahiye hoti thi, aur **global clients ke timelines ko global team ke bina** match kar sakta hai. Pricing ka masla (USD mein cost vs. PKR income) haqeeqi hai, lekin jaisa is guide mein cover kiya gaya hai, iske practical hal maujood hain — entry-level subscription ko disciplined tareeqe se use karne se le kar, **Ollama ke zariye bilkul free local workflow** tak jo learning, prototyping, aur non-sensitive kaam ke liye chal jata hai.

---

## 🚀 Quick Start

1. Read the [Architectural Overview](#-architectural-overview) above to build your mental model.
2. Jump into [`docs/COMPREHENSIVE-GUIDE.md`](docs/COMPREHENSIVE-GUIDE.md) for the full technical deep-dive.
3. Use [`visuals_and_structure.md`](visuals_and_structure.md) if you're organizing your own fork or documentation hub.
4. Star ⭐ the repo if this saved you research time — it helps other developers find it too.

---

<div align="center">

**Built with 🧠 + ☕ for the developer community.**

*Contributions, corrections, and translations welcome — see `.github/CONTRIBUTING.md`.*

</div>
