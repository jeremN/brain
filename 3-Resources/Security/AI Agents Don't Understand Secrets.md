---
url: https://dev.to/0x711/ai-agents-dont-understand-secrets-thats-your-problem-43n4
category: "Security"
tags:
  - secrets-management
  - LLM-security
  - AI-agents
  - MCP
date_saved: "2026-03-20"
---

# AI Agents Don't Understand Secrets

**Source:** [https://dev.to/0x711/ai-agents-dont-understand-secrets-thats-your-problem-43n4](https://dev.to/0x711/ai-agents-dont-understand-secrets-thats-your-problem-43n4)
**Category:** Security

## Summary

Article sur DEV Community argumentant que les agents IA n'ont aucune notion de confidentialité — ils traitent un token API comme n'importe quelle autre string. Le problème n'est pas l'IA mais l'architecture autour : si un secret est dans le contexte, l'agent peut le répéter, le logger, ou l'envoyer dans une requête. Propose des patterns concrets pour isoler les secrets du contexte de l'agent. Mentionne aussi le scanning des configurations MCP qui a permis de détecter des credentials hardcodés.

## Why It's Interesting

Remet les pendules à l'heure : le problème n'est pas un bug de l'IA mais un défaut d'architecture — les secrets ne devraient jamais être dans le contexte

## Key Takeaways

- Un LLM ne distingue pas un secret d'une string quelconque — c'est un problème d'architecture, pas d'IA
- Si un secret est dans le context window, il PEUT et il VA fuiter tôt ou tard
- Scanner les configurations MCP (via TruffleHog ou règles custom) détecte les credentials hardcodés que les devs ont oubliés
- L'attaque SANDWORM_MODE (fév 2026) a installé des MCP servers malveillants dans Claude Code, Cursor, Windsurf via 19 packages npm
- Solution : ne jamais mettre de secrets dans le contexte, utiliser des références résolues au runtime

## Tags

#secrets-management, #LLM-security, #AI-agents, #MCP
