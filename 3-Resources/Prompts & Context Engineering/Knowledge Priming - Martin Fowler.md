---
url: https://martinfowler.com/articles/reduce-friction-ai/knowledge-priming.html
category: "Prompts & Context Engineering"
tags:
  - knowledge-priming
  - context
  - martin-fowler
  - AI-development
date_saved: "2026-03-11"
---

# Knowledge Priming - Martin Fowler

**Source:** [https://martinfowler.com/articles/reduce-friction-ai/knowledge-priming.html](https://martinfowler.com/articles/reduce-friction-ai/knowledge-priming.html)
**Category:** Prompts & Context Engineering

## Summary

Martin Fowler (Thoughtworks) article proposing Knowledge Priming — treating project context as versioned infrastructure rather than ad-hoc copy-pasting. Essentially manual RAG: filling the AI's context window with project-specific tokens that override generic training data. Proposes a 7-section priming document: architecture overview, tech stack with versions, curated knowledge sources, project structure, naming conventions, code examples, and anti-patterns.

## Why It's Interesting

Proposes 7 curated sections mirroring human onboarding, with a practical complete priming doc under 50 lines

## Key Takeaways

- AI defaults to 'the average of the internet' without context — treat AI like new hires who need onboarding
- Knowledge hierarchy: Training Data (lowest) -> Conversation Context (medium) -> Priming Documents (highest priority)
- A good priming doc should be under 3 pages / ~50 lines — it's a cheat sheet, not comprehensive documentation
- Treat priming as infrastructure (version-controlled files in repo) not habit (manual copy-paste)
- Include anti-patterns ('Do NOT use') alongside patterns to prevent common AI mistakes
- Curation matters more than volume: focused context shifts transformer attention weights

## Tags

#knowledge-priming, #context, #martin-fowler, #AI-development
