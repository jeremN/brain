---
url: https://simonwillison.net/guides/agentic-engineering-patterns/hoard-things-you-know-how-to-do/
category: "Prompts & Context Engineering"
tags:
  - agentic-patterns
  - engineering
  - knowledge-caching
  - simon-willison
date_saved: "2026-03-11"
---

# Hoard Things You Know How To Do

**Source:** [https://simonwillison.net/guides/agentic-engineering-patterns/hoard-things-you-know-how-to-do/](https://simonwillison.net/guides/agentic-engineering-patterns/hoard-things-you-know-how-to-do/)
**Category:** Prompts & Context Engineering

## Summary

Simon Willison's guide on building a personal library of working code solutions and combining them with AI coding agents. The core insight: knowing something is theoretically possible isn't the same as having running code that proves it. When you combine two working examples as input for a coding agent, results are dramatically better than starting from scratch.

## Why It's Interesting

Built a browser-based OCR tool in minutes by combining Tesseract.js and PDF.js snippets — a pattern that works even better with modern coding agents

## Key Takeaways

- Hoard working code solutions — having seen it done yourself is more valuable than knowing it's theoretically possible
- Agents mean you only ever need to figure out a trick once — documented working code can solve similar problems forever
- Combining existing working examples is one of the most powerful prompting patterns
- Give agents access to your repos: 'clone my repo to /tmp and use it as input' produces better results than abstract descriptions
- Use curl instead of WebFetch when you need agents to see raw HTML, not summarized content

## Tags

#agentic-patterns, #engineering, #knowledge-caching, #simon-willison
