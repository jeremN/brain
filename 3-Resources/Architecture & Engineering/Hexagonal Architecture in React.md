---
url: "https://alexkondov.com/hexagonal-inspired-architecture-in-react/"
category: "Architecture & Engineering"
original_section: "React"
source: "chaos.md"
tags:
  - hexagonal-architecture
  - react
  - design-patterns
  - frontend
date_saved: "2026-03-11"
---

# Hexagonal Architecture in React

**Source:** [https://alexkondov.com/hexagonal-inspired-architecture-in-react/](https://alexkondov.com/hexagonal-inspired-architecture-in-react/)
**Category:** Architecture & Engineering
**Original Section:** React

## Summary

Alex Kondov applies hexagonal (ports and adapters) architecture to React applications. Separates business logic from framework concerns with clear boundaries between domain, application, and infrastructure layers. Shows how to make React apps testable by isolating side effects behind interfaces.

## Why It's Interesting

Applies a mature backend architecture pattern to frontend, producing more testable React apps

## Key Takeaways

- Separate domain logic from React components — business rules should be framework-agnostic
- Use ports (interfaces) and adapters (implementations) to decouple data fetching from UI
- Application layer orchestrates use cases without knowing about React or HTTP
- Makes testing trivial: swap real adapters for test doubles at the boundary
- Prevents the common antipattern of mixing business logic into components

## Tags

#hexagonal-architecture, #react, #design-patterns, #frontend
