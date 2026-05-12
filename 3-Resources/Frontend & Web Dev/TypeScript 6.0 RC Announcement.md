---
url: https://devblogs.microsoft.com/typescript/announcing-typescript-6-0-rc/
category: "Frontend & Web Dev"
tags:
  - typescript
  - release
  - javascript
  - microsoft
date_saved: "2026-03-11"
---

# TypeScript 6.0 RC Announcement

**Source:** [https://devblogs.microsoft.com/typescript/announcing-typescript-6-0-rc/](https://devblogs.microsoft.com/typescript/announcing-typescript-6-0-rc/)
**Category:** Frontend & Web Dev

## Summary

Microsoft's announcement of TypeScript 6.0 RC — the last release based on the JavaScript codebase before TypeScript 7.0 (written in Go). This is a major transition release with significant breaking changes and deprecations designed to align with TS 7.0. Key new features: less context-sensitivity on this-less functions, subpath imports starting with #/, es2025 target, Temporal API types, Map upsert methods (getOrInsert/getOrInsertComputed), and RegExp.escape. Major deprecations: target es5, baseUrl, moduleResolution node/classic, outFile, and module amd/umd/systemjs.

## Why It's Interesting

The most consequential TypeScript release in years — every project will need to adjust tsconfig defaults before TS 7.0 lands

## Key Takeaways

- TypeScript 7.0 will be rewritten in Go with parallel type checking — TS 6.0 is the bridge release to prepare for migration
- strict is now true by default, module defaults to esnext, target defaults to current-year ES (es2025)
- types now defaults to [] — you must explicitly list @types packages (e.g. 'types': ['node', 'jest']), can improve build time 20-50%
- rootDir now defaults to '.' (tsconfig directory) — if your src is nested, explicitly set 'rootDir': './src'
- Deprecated: target es5, baseUrl (use full paths in paths entries instead), moduleResolution node10/classic, outFile
- New: Temporal API types, Map.getOrInsert/getOrInsertComputed (upsert), RegExp.escape, subpath imports with #/
- --stableTypeOrdering flag helps diagnose type ordering differences between 6.0 and 7.0 (up to 25% slowdown though)

## Tags

#typescript, #release, #javascript, #microsoft
