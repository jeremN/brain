---
url: "https://tkdodo.eu/blog/hooks-dependencies-and-stale-closures"
category: "Frontend & Web Dev"
original_section: "React / Hooks"
source: "chaos.md"
tags:
  - react
  - hooks
  - closures
  - debugging
date_saved: "2026-03-11"
---

# Hooks Dependencies and Stale Closures

**Source:** [https://tkdodo.eu/blog/hooks-dependencies-and-stale-closures](https://tkdodo.eu/blog/hooks-dependencies-and-stale-closures)
**Category:** Frontend & Web Dev
**Original Section:** React / Hooks

## Summary

TkDodo explains React's trickiest gotcha: how hooks dependencies interact with JavaScript closures to create stale data bugs. When a callback captures a variable but the dependency array doesn't include it, the callback sees outdated values. Shows the right mental model for hook dependencies.

## Why It's Interesting

Addresses the single most common source of subtle bugs in React hooks code

## Key Takeaways

- Every render creates a new closure — functions capture values from the render they were created in
- Stale closures happen when dependency arrays are incomplete — the callback sees old values
- useCallback doesn't prevent stale closures — it just prevents re-creating the function
- Fix: include all used values in dependency array, or use refs for non-reactive values
- ESLint exhaustive-deps rule is your friend — don't suppress without understanding why

## Tags

#react, #hooks, #closures, #debugging
