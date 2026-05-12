---
url: https://vitalik.eth.limo/general/2026/04/02/secure_llms.html
category: "Security"
tags:
  - llm-security
  - self-sovereign
  - local-ai
  - privacy
  - vitalik-buterin
date_saved: "2026-04-07"
---

# My Self-Sovereign / Local / Private / Secure LLM Setup — Vitalik Buterin

**Source:** [https://vitalik.eth.limo/general/2026/04/02/secure_llms.html](https://vitalik.eth.limo/general/2026/04/02/secure_llms.html)
**Category:** Security

## Summary
Article de Vitalik Buterin (avril 2026) présentant son setup LLM 100% local et souverain. Il argumente que la dépendance aux services IA centralisés est un risque structurel et applique la logique crypto de self-custody au monde de l'IA : si les LLM médient la vie numérique, le contrôle local compte autant que la self-custody de l'argent.

## Why It's Interesting
Vision rare d'un leader crypto qui applique les principes d'Ethereum (décentralisation, contrôle utilisateur, trustlessness vérifiable) à l'IA quotidienne, avec un setup technique concret et fonctionnel.

## Key Takeaways
- **Hardware** : laptop avec Nvidia RTX 5090 (24 GB), ~90 tokens/sec — suffisant pour un usage fluide au quotidien
- **Modèle** : Qwen3.5:35B en local via llama-server (open-source)
- **Sandboxing** : chaque activité LLM/agent tourne dans un sandbox bubblewrap, rooté dans un répertoire spécifique avec accès whitelisté aux fichiers
- **Pattern 2-of-2 multisig** : l'IA rédige (email, transaction), Vitalik valide — jamais d'action autonome non supervisée
- **2026 = année pour inverser une décennie de recul vers les services centralisés**

## Tags

#llm-security, #self-sovereign, #local-ai, #privacy, #vitalik-buterin
