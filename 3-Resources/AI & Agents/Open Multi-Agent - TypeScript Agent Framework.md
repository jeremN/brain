---
url: https://github.com/JackChen-me/open-multi-agent
category: "AI & Agents"
tags:
  - multi-agent
  - typescript
  - orchestration
  - agent-framework
  - open-source
date_saved: "2026-04-07"
---

# Open Multi-Agent — TypeScript Multi-Agent Framework

**Source:** [https://github.com/JackChen-me/open-multi-agent](https://github.com/JackChen-me/open-multi-agent)
**Category:** AI & Agents

## Summary
Framework TypeScript pour orchestrer plusieurs agents IA sur des tâches complexes. Décomposition automatique des objectifs en sous-tâches coordonnées avec résolution de dépendances et exécution parallèle.

## Why It's Interesting
Framework léger (3 dépendances, 33 fichiers source) et TypeScript-natif — pas de Python ni de services externes. Support multi-modèle (Claude, GPT, Gemma 4, Grok, Ollama).

## Key Takeaways
- **3 modes d'exécution** : `runAgent()` (single), `runTeam()` (auto-orchestrated), `runTasks()` (pipeline explicite)
- **Exécution parallèle** des tâches indépendantes
- **Structured output** : validation Zod avec retry automatique
- **Human-in-the-loop** : gates d'approbation entre les batches
- **Loop detection** : détecte les agents qui se répètent
- **Observabilité** : trace callbacks pour visibilité complète
- **MIT License**, Node.js 18+, TypeScript 5.6+

## Tags

#multi-agent, #typescript, #orchestration, #agent-framework, #open-source
