---
url: https://github.com/garagon/nanostack
category: "Claude & Skills"
tags:
  - agent-framework
  - sprint-workflow
  - quality-gates
  - security-audit
  - zero-dependency
date_saved: "2026-04-07"
---

# Nanostack — Turn Your AI Agent Into an Engineering Team

**Source:** [https://github.com/garagon/nanostack](https://github.com/garagon/nanostack)
**Category:** Claude & Skills

## Summary
Framework transformant un agent IA en équipe d'ingénierie complète — challenge de scope, planning, review, QA, audit de sécurité et déploiement en un seul sprint. Zero dépendances, shell scripts + JSON.

## Why It's Interesting
Sprint complet en quelques minutes au lieu de semaines, avec 33 règles de garde bloquantes (pas de mass deletion, pas de force push) et mode autopilot après approbation du `/think`.

## Key Takeaways
- **Workflow sprint** : `/think` → `/nano` → build → `/review` → `/qa` → `/security` → `/ship`
- **`/think`** : challenge les requirements avant de coder, recommande la réduction de scope
- **`/guard`** : 33 règles bloquantes avec alternatives plus sûres
- **Knowledge persistence** : artefacts sauvés dans `.nanostack/`
- **Exécution parallèle** : review, QA et security simultanés via `/conductor`
- **Mode autopilot** : workflow complet après approbation initiale
- **Zero dépendances** : shell scripts, JSON, `jq`
- **Compatible** : Claude Code, Cursor, Codex, Gemini CLI, Cline
- **Apache 2.0**

## Tags

#agent-framework, #sprint-workflow, #quality-gates, #security-audit, #zero-dependency
