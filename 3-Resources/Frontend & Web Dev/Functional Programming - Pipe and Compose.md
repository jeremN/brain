---
url: "https://dev.to/joelbonetr/js-functional-concepts-pipe-and-compose-1mho"
category: "Frontend & Web Dev"
original_section: "JS / TS"
source: "chaos.md"
tags:
  - javascript
  - functional-programming
  - pipe
  - compose
date_saved: "2026-03-11"
---

# Functional Programming - Pipe and Compose

**Source:** [https://dev.to/joelbonetr/js-functional-concepts-pipe-and-compose-1mho](https://dev.to/joelbonetr/js-functional-concepts-pipe-and-compose-1mho)
**Category:** Frontend & Web Dev
**Original Section:** JS / TS

## Summary

Clear explanation of pipe and compose functional programming patterns in JavaScript. Pipe applies functions left-to-right, compose right-to-left. Shows how to implement both from scratch and use them for clean, testable data transformation chains.

## Why It's Interesting

Core FP building blocks that make complex data transformations readable and testable

## Key Takeaways

- Pipe: applies functions left-to-right — pipe(f, g, h)(x) = h(g(f(x)))
- Compose: applies functions right-to-left — compose(f, g, h)(x) = f(g(h(x)))
- Both enable point-free style: describing transformations as function chains
- Each function in the chain should be pure with one input and one output
- Dramatically improves readability for complex data transformations

## Tags

#javascript, #functional-programming, #pipe, #compose
