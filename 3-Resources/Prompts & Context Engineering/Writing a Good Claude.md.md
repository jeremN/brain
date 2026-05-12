---
url: https://www.humanlayer.dev/blog/writing-a-good-claude-md
category: "Prompts & Context Engineering"
tags:
  - claude-md
  - context
  - prompts
  - documentation
date_saved: "2026-03-11"
---

# Writing a Good Claude.md

**Source:** [https://www.humanlayer.dev/blog/writing-a-good-claude-md](https://www.humanlayer.dev/blog/writing-a-good-claude-md)
**Category:** Prompts & Context Engineering

## Summary

Comprehensive guide by HumanLayer on crafting effective CLAUDE.md files for AI-assisted coding. LLMs are stateless — CLAUDE.md is the only file auto-loaded in every Claude Code session, making it the highest-leverage configuration point. The article warns that Claude Code's system prompt tells the model to ignore CLAUDE.md content it deems irrelevant, so bloated files get ignored. Recommends keeping it under 60 lines, focused on universal instructions only.

## Why It's Interesting

Reveals that Claude Code's system prompt contains a directive telling Claude to ignore CLAUDE.md if it seems irrelevant — making conciseness critical

## Key Takeaways

- CLAUDE.md should cover three things: WHAT (tech stack, project structure), WHY (purpose of the project), and HOW (build/test commands, conventions)
- Frontier LLMs can reliably follow ~150-200 instructions; Claude Code's system prompt already uses ~50, leaving limited budget for your CLAUDE.md
- Use Progressive Disclosure: keep task-specific docs in separate files (e.g. agent_docs/running_tests.md) and point Claude to them rather than inlining everything
- Never use /init or auto-generate your CLAUDE.md — it's too high-leverage to leave to automation
- Don't use Claude as a linter — use deterministic tools like Biome, and set up Stop hooks instead

## Tags

#claude-md, #context, #prompts, #documentation
