---
url: "https://github.com/gsd-build/get-shit-done"
category: "AI & Agents"
tags:
  - meta-prompting
  - context-engineering
  - Claude-Code
  - AI-dev-workflow
  - multi-agent
  - CLI
date_saved: "2026-03-23"
---

# Get Shit Done (GSD) - Meta-Prompting for AI Dev

**Source:** [github.com/gsd-build/get-shit-done](https://github.com/gsd-build/get-shit-done)
**Category:** AI & Agents

## Summary

GSD est un système de meta-prompting et de context engineering conçu pour fiabiliser le développement assisté par IA (Claude Code, Cursor, Gemini CLI, Codex, etc.). Il résout le problème du "context rot" — la dégradation de qualité quand la fenêtre de contexte se remplit — en maintenant l'utilisation sous 40% et en allouant un contexte frais de 200k tokens par tâche atomique. Le workflow est structuré en phases (discuss → plan → execute → verify → ship) avec orchestration multi-agents et commits Git atomiques.

## Why It's Interesting

Approche systématique pour transformer le coding avec LLM d'un processus ad-hoc en un pipeline reproductible. Les principes sous-jacents (contexte frais, tâches atomiques, vérification systématique) sont applicables même sans adopter l'outil.

## Key Takeaways

- Garde la fenêtre de contexte sous 40% pour éviter le "context rot"
- Orchestration multi-agents : researcher → planner → executor → verifier
- Commits Git atomiques par tâche pour un historique propre
- Compatible avec Claude Code, OpenCode, Gemini CLI, Codex, Copilot, Cursor, Antigravity
- Quick mode disponible pour les tâches ad-hoc sans pipeline complet

## Tags
#meta-prompting, #context-engineering, #Claude-Code, #AI-dev-workflow, #multi-agent, #CLI
