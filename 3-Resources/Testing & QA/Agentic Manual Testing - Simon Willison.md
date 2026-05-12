---
url: https://simonwillison.net/guides/agentic-engineering-patterns/agentic-manual-testing/
category: "Testing & QA"
tags:
  - testing
  - agentic
  - simon-willison
  - patterns
  - manual
date_saved: "2026-03-11"
---

# Agentic Manual Testing - Simon Willison

**Source:** [https://simonwillison.net/guides/agentic-engineering-patterns/agentic-manual-testing/](https://simonwillison.net/guides/agentic-engineering-patterns/agentic-manual-testing/)
**Category:** Testing & QA

## Summary

Simon Willison's guide on getting coding agents to manually test their own code beyond unit tests. Covers python -c patterns, curl for API testing, Playwright for browser automation, and introduces Rodney (Chrome DevTools Protocol CLI for agents) and Showboat (documenting agent test sessions with captured commands and outputs).

## Why It's Interesting

Introduces Showboat pattern for agents to document testing, preventing agents from writing what they hoped happened

## Key Takeaways

- Never assume LLM-generated code works until it's been executed — automated tests are necessary but not sufficient
- Use 'python -c' for quick validation, curl for API testing, and Playwright for full browser automation
- Rodney: CLI wrapper around Chrome DevTools Protocol designed for agents to take screenshots and interact with pages
- Showboat: tool for agents to document their testing with captured commands and outputs — prevents 'cheating'
- Issues found through manual testing should be fixed with red/green TDD for permanent coverage
- Browser automation tests are less flaky when agents maintain them — agents handle constant HTML changes

## Tags

#testing, #agentic, #simon-willison, #patterns, #manual
