# 🗂️ Visuals & Repository Structure Guide

This file defines the recommended folder layout and the badge library used across `Claude-Code-Mastery`, so the repository looks consistent, premium, and easy for contributors to navigate.

---

## 📁 Recommended Folder Structure

```
Claude-Code-Mastery/
│
├── README.md                      # Elite landing page (mission, architecture, navigation)
├── LICENSE                        # MIT (or your chosen license)
├── visuals_and_structure.md       # This file
│
├── docs/
│   ├── COMPREHENSIVE-GUIDE.md     # Master technical file (EN + Roman Urdu)
│   ├── architecture/              # Optional: deeper architecture diagrams as standalone docs
│   ├── pricing/                   # Optional: living pricing comparison notes (updated periodically)
│   └── security/                  # Optional: hook recipes, settings.json examples
│
├── assets/
│   ├── screenshots/                # Terminal & UI screenshots
│   ├── diagrams/                   # Architecture / flow diagrams (SVG/PNG)
│   ├── logos/                      # Repo logo, social-preview banner
│   └── recordings/                 # Optional: short terminal session GIFs/MP4s
│
├── examples/
│   ├── hooks/                      # Example PreToolUse / PostToolUse scripts
│   ├── claude-md-templates/        # Sample CLAUDE.md files for different stacks
│   └── mcp-servers/                # Minimal example MCP server configs
│
└── .github/
    ├── ISSUE_TEMPLATE/
    │   ├── bug_report.md
    │   └── feature_request.md
    ├── CONTRIBUTING.md             # How to propose edits/translations
    ├── CODE_OF_CONDUCT.md          # Community behavior guidelines
    ├── PULL_REQUEST_TEMPLATE.md
    └── workflows/
        └── markdown-lint.yml       # Optional CI: lint docs on every PR
```

**Why this layout works:**
- `docs/` keeps long-form technical content separate from the landing page, so the README stays scannable.
- `assets/` centralizes all binary/media files — never commit screenshots directly into `docs/`.
- `examples/` gives contributors copy-paste starting points (hooks, MCP configs, CLAUDE.md) without cluttering the guide itself.
- `.github/` signals a maintained, community-ready project to anyone browsing GitHub.

---

## 🏆 Premium Badge Library

Badges are generated via [Shields.io](https://shields.io). Use the `for-the-badge` style for a bold landing-page look, or the default flat style inside `docs/` for a more understated technical tone.

### Status & Quality

```markdown
[![Build Status](https://img.shields.io/badge/Build-Passing-brightgreen?style=for-the-badge)](#)
[![Maintained](https://img.shields.io/badge/Maintained-Yes-success?style=for-the-badge)](#)
[![Docs Coverage](https://img.shields.io/badge/Docs-Complete-blue?style=for-the-badge)](#)
```

### Versioning & License

```markdown
[![Version](https://img.shields.io/badge/Version-1.0.0-informational?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)](#)
[![Last Updated](https://img.shields.io/badge/Updated-2026-lightgrey?style=for-the-badge)](#)
```

### Community & Contribution

```markdown
[![PRs Welcome](https://img.shields.io/badge/PRs-Welcome-brightgreen?style=for-the-badge)](#)
[![Contributors](https://img.shields.io/badge/Contributors-Open-orange?style=for-the-badge)](#)
[![Stars](https://img.shields.io/github/stars/your-username/Claude-Code-Mastery?style=for-the-badge)](#)
```

### Topic / Tech Tags

```markdown
[![Claude AI](https://img.shields.io/badge/Claude-AI-CC785C?style=for-the-badge)](#)
[![Claude Code](https://img.shields.io/badge/Claude-Code-D97757?style=for-the-badge)](#)
[![MCP](https://img.shields.io/badge/Model%20Context%20Protocol-MCP-9B59B6?style=for-the-badge)](#)
[![Bilingual](https://img.shields.io/badge/Bilingual-EN%20%7C%20Roman%20Urdu-2ECC71?style=for-the-badge)](#)
```

> 💡 **Tip:** Once this repo has a real GitHub remote, swap any `(#)` placeholder links for the actual GitHub Actions workflow badge URL, license file link, or live star-count badge (`https://img.shields.io/github/stars/<user>/<repo>`) so badges become live and clickable rather than decorative.

---

## 🖼️ Suggested Visual Assets to Create

| Asset | Purpose | Suggested Location |
|---|---|---|
| Repo banner (1280×640px) | Social preview when the repo link is shared | `assets/logos/banner.png` |
| Architecture diagram (the 4-layer stack) | Visual companion to README's architecture section | `assets/diagrams/architecture.svg` |
| Terminal screenshot of a Claude Code session | Shows real usage on the README or guide | `assets/screenshots/session-example.png` |
| Hooks flow diagram (PreToolUse → action → PostToolUse) | Visual aid for the security section | `assets/diagrams/hooks-flow.svg` |
| Pricing comparison chart (Cloud vs. Ollama) | Supports the pricing section visually | `assets/diagrams/pricing-comparison.png` |

Keep all diagrams in **SVG** where possible — they stay sharp at any zoom level and render cleanly inside GitHub's markdown viewer.
