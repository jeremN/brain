---
url: https://www.humanlayer.dev/blog/advanced-context-engineering
category: "Prompts & Context Engineering"
tags:
  - context-engineering
  - advanced
  - prompts
  - AI
date_saved: "2026-03-11"
---

# Advanced Context Engineering

**Source:** [https://www.humanlayer.dev/blog/advanced-context-engineering](https://www.humanlayer.dev/blog/advanced-context-engineering)
**Category:** Prompts & Context Engineering

## Summary

Deep technical guide by HumanLayer on 'Frequent Intentional Compaction' — a workflow for using AI coding agents effectively in large, complex codebases. The authors shipped 35k LOC to a 300k LOC Rust codebase (BAML) in 7 hours using a research->plan->implement workflow that keeps context window utilization at 40-60%. The key insight: context window contents are the ONLY lever you have to affect output quality.

## Why It's Interesting

Real case study: shipped 35k LOC of Rust (cancellation + WASM support) to BAML in 7 hours, features estimated at 3-5 days each for a senior engineer

## Key Takeaways

- Frequent Intentional Compaction: design your entire workflow around context management, keeping utilization at 40-60%
- Research->Plan->Implement: three-phase workflow where research produces structured understanding, plans produce precise steps, and implementation executes phase by phase
- Human review should focus on highest-leverage artifacts: a bad line of research leads to thousands of bad lines of code
- Subagents are about context control, not role-playing — they search/summarize in a fresh context without polluting the parent
- Specs become the new source of truth for team mental alignment — read 200 lines of a plan instead of 2000 lines of code

## Tags

#context-engineering, #advanced, #prompts, #AI
