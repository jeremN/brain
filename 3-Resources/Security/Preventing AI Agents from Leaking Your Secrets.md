---
url: https://codenotary.com/blog/preventing-ai-agents-from-leaking-your-secrets
category: "Security"
tags:
  - secrets-management
  - LLM-security
  - defense-in-depth
  - AI-agents
date_saved: "2026-03-20"
---

# Preventing AI Agents from Leaking Your Secrets

**Source:** [https://codenotary.com/blog/preventing-ai-agents-from-leaking-your-secrets](https://codenotary.com/blog/preventing-ai-agents-from-leaking-your-secrets)
**Category:** Security

## Summary

Guide de Codenotary proposant une architecture defense-in-depth en 4 couches pour protéger les secrets des agents IA : contrôle d'accès (bloque les fichiers secrets), nettoyage du contexte (supprime les secrets avant envoi au LLM), traitement par l'agent, puis filtrage des outputs (masque les secrets dans les réponses). Chaque couche réduit le risque, ensemble elles rendent la fuite accidentelle quasi impossible.

## Why It's Interesting

Architecture concrète et actionable en 4 couches — pas juste des principes abstraits mais un modèle implémentable

## Key Takeaways

- Architecture 4 couches : Access Control → Context Cleaning → Agent/LLM → Output Filtering
- Le contrôle d'accès bloque l'accès aux fichiers contenant des secrets avant que l'agent ne les voie
- Le nettoyage de contexte retire les patterns de secrets (clés API, tokens) du prompt envoyé au LLM
- Le filtrage de sortie masque tout secret qui aurait pu passer les couches précédentes
- Approche probabiliste : chaque couche n'est pas parfaite, mais empilées elles offrent une protection robuste

## Tags

#secrets-management, #LLM-security, #defense-in-depth, #AI-agents
