---
url: https://steipete.me/posts/just-talk-to-it
category: "AI & Agents"
tags:
  - conversation
  - UI
  - interaction
  - natural-language
date_saved: "2026-03-11"
---

# Just Talk To It

**Source:** [https://steipete.me/posts/just-talk-to-it](https://steipete.me/posts/just-talk-to-it)
**Category:** AI & Agents

## Summary

Peter Steinberger's battle-tested guide to agentic engineering workflows. He describes running 3-8 parallel AI agents (using Codex CLI, Claude Code, Cursor) on a single codebase, with an ~800-line Agents.md file as the shared context document. The core philosophy: treat agents like junior developers — give them clear specs, constrain their blast radius, and review their work. He covers practical tactics like using git worktrees for parallel agent work, pre-commit hooks to catch mistakes, and 'blast radius' thinking to decide what's safe to delegate.

## Why It's Interesting

Real practitioner workflow from someone shipping production code daily with multiple parallel agents — not theory but a refined daily practice

## Key Takeaways

- Run 3-8 parallel agents using git worktrees so they don't conflict — each agent gets its own branch and working directory
- Agents.md (~800 lines) serves as the single source of truth: architecture decisions, coding conventions, common pitfalls, and explicit instructions
- 'Blast radius' thinking: delegate low-risk tasks (tests, docs, refactors) freely, but keep high-risk changes (auth, payments, data migrations) under tight human review
- Use pre-commit hooks and CI as automated guardrails — agents will run them and self-correct
- Treat agent output like a junior developer's PR: review the diff, not the process

## Tags

#conversation, #UI, #interaction, #natural-language
