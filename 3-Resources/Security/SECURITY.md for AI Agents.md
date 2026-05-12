---
url: https://www.sebiomo.com/writing/security-md-for-ai-agents
category: "Security"
tags:
  - AI-agents
  - security
  - documentation
  - best-practices
date_saved: "2026-03-11"
---

# SECURITY.md for AI Agents

**Source:** [https://www.sebiomo.com/writing/security-md-for-ai-agents](https://www.sebiomo.com/writing/security-md-for-ai-agents)
**Category:** Security

## Summary

Sebi Omo proposes SECURITY.md as a standardized file that acts as a 'system prompt for your codebase' — giving AI coding agents explicit security rules to follow. Just as CONTRIBUTING.md guides human contributors, SECURITY.md would guide AI agents on security boundaries. The template covers: no secrets in code, mandatory Row Level Security (RLS) for database access, server-only write operations, input validation patterns, and authentication boundaries.

## Why It's Interesting

Shifts security left to the point of code generation — AI agents follow security rules before writing a single line, rather than relying on post-hoc review

## Key Takeaways

- SECURITY.md serves as a directive file that AI coding agents (Copilot, Claude, Cursor) read before generating code
- Key rules in the template: never hardcode secrets, always use environment variables, enforce RLS on all database tables
- Server-only writes: all mutations must go through server-side endpoints, never direct client-to-database writes
- The file format is designed to be both human-readable documentation and machine-parseable instructions
- Proactive security: instead of catching vulnerabilities in review, prevent them at generation time

## Tags

#AI-agents, #security, #documentation, #best-practices
