---
url: https://addyosmani.com/blog/code-agent-orchestra/
category: "AI & Agents"
tags:
  - ai-coding
  - agent-orchestration
  - multi-agent
  - subagents
  - agent-teams
  - quality-gates
  - agents-md
date_saved: "2026-03-27"
---

# The Code Agent Orchestra

**Source:** [https://addyosmani.com/blog/code-agent-orchestra/](https://addyosmani.com/blog/code-agent-orchestra/)
**Category:** AI & Agents

## Summary
Write-up du talk d'Addy Osmani à O'Reilly AI CodeCon (mars 2026). Couvre le passage du mode "conducteur" (un seul agent synchrone) au mode "orchestrateur" (équipe d'agents asynchrones). Présente trois patterns concrets de coordination multi-agent, les outils du marché en 2026, et les quality gates nécessaires pour garder la confiance dans le code produit.

## Why It's Interesting
Article de référence qui structure tout le paysage de l'orchestration d'agents de code en 2026 — des patterns simples (subagents) aux systèmes cloud async, avec un regard pragmatique sur les limites et la discipline nécessaire.

## Key Takeaways
- **3 patterns clés** : Subagents (Task tool, décomposition simple), Agent Teams (task list partagée + messaging peer-to-peer + file locking), Orchestration at scale (outils cloud async)
- **8 niveaux de maturité AI-coding** (framework de Steve Yegge) — l'orchestration commence au niveau 6
- **3 tiers d'outils** : in-process (Claude Code subagents/teams), local orchestrators (Conductor, Vibe Kanban, Claude Squad…), cloud async (Claude Code Web, Copilot Coding Agent, Jules, Codex Web)
- **Quality gates** : plan approval avant le code, hooks automatiques (tests/lint à chaque TaskCompleted), AGENTS.md pour l'apprentissage cumulatif
- **Ralph Loop** : pattern stateless-but-iterative (pick → implement → validate → commit → reset context) pour éviter l'overflow de contexte
- **AGENTS.md doit être écrit par des humains** — recherche ETH Zurich montre que les versions générées par LLM n'apportent rien et coûtent +20% en tokens
- **Le bottleneck n'est plus la génération mais la vérification** — le review humain reste indispensable
- **Sweet spot : 3-5 agents** — les coûts en tokens scalent linéairement
- **"Delegate the tasks, not the judgment"** — garder pour soi l'architecture, le choix de ce qu'on ne construit PAS, et le review avec le contexte global

## Tags

#ai-coding, #agent-orchestration, #multi-agent, #subagents, #agent-teams, #quality-gates, #agents-md
