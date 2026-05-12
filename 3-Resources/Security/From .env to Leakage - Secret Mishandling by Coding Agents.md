---
url: https://www.knostic.ai/blog/claude-cursor-env-file-secret-leakage
category: "Security"
tags:
  - secrets-management
  - LLM-security
  - dotenv
  - claude-code
  - cursor
date_saved: "2026-03-20"
---

# From .env to Leakage - Secret Mishandling by Coding Agents

**Source:** [https://www.knostic.ai/blog/claude-cursor-env-file-secret-leakage](https://www.knostic.ai/blog/claude-cursor-env-file-secret-leakage)
**Category:** Security

## Summary

Analyse par Knostic du comportement des assistants IA (Claude Code, Cursor) face aux fichiers `.env`. Démontre que ces outils chargent automatiquement les secrets sans prévenir l'utilisateur, créant un risque de fuite vers les logs, le contexte LLM, ou les outputs générés. Documente les vecteurs de fuite concrets et les patterns de mishandling.

## Why It's Interesting

Preuve concrète que le problème de fuite de secrets n'est pas théorique — les outils qu'on utilise au quotidien (Claude Code, Cursor) ont des failles documentées

## Key Takeaways

- Claude Code charge automatiquement les fichiers `.env` dans son contexte sans avertissement explicite
- Les secrets peuvent fuiter via les logs, les suggestions de code, ou les outputs de l'agent
- Le problème touche tous les principaux assistants IA de coding (Claude Code, Cursor, Windsurf)
- Recommandation : ne jamais laisser de vrais secrets dans les `.env` accessibles aux agents

## Tags

#secrets-management, #LLM-security, #dotenv, #claude-code, #cursor
