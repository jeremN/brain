---
url: https://www.doppler.com/blog/advanced-llm-security
category: "Security"
tags:
  - secrets-management
  - LLM-security
  - doppler
  - secret-rotation
date_saved: "2026-03-20"
---

# Advanced LLM Security - Preventing Secret Leakage

**Source:** [https://www.doppler.com/blog/advanced-llm-security](https://www.doppler.com/blog/advanced-llm-security)
**Category:** Security

## Summary

Article de Doppler sur la prévention des fuites de secrets dans les workflows LLM. Couvre les vecteurs de fuite (prompts, logs, training data, RAG), les bonnes pratiques de gestion de secrets (vault, rotation, least-privilege), et l'importance de scanner les sources de données (Confluence, Jira, Slack) avant de les utiliser en RAG. Recommande de traiter les logs des systèmes IA comme infrastructure sensible.

## Why It's Interesting

Vue d'ensemble complète par Doppler (leader du secret management) — couvre autant les fuites directes que les fuites indirectes via RAG et training data

## Key Takeaways

- Les secrets fuient via 4 vecteurs : prompts, logs système, données d'entraînement, et sources RAG
- Scanner et nettoyer toutes les knowledge bases (Confluence, Jira, Slack) avant de les utiliser en RAG
- Utiliser un vault (HashiCorp Vault, AWS Secrets Manager) avec rotation automatique des secrets
- Traiter les logs des systèmes IA comme infrastructure sensible — ne jamais logger de secrets
- Appliquer le principe de moindre privilège : un agent qui lit une API ne devrait jamais avoir accès en écriture

## Tags

#secrets-management, #LLM-security, #doppler, #secret-rotation
