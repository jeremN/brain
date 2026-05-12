---
url: https://github.com/VectifyAI/PageIndex
category: "AI & Agents"
tags:
  - rag
  - vectorless
  - document-retrieval
  - llm-reasoning
  - open-source
date_saved: "2026-04-07"
---

# PageIndex — Vectorless Reasoning-Based RAG

**Source:** [https://github.com/VectifyAI/PageIndex](https://github.com/VectifyAI/PageIndex)
**Category:** AI & Agents

## Summary
Framework open-source pour construire des systèmes RAG basés sur le raisonnement LLM plutôt que sur la similarité vectorielle. Indexation hiérarchique des documents (type table des matières) et navigation par raisonnement plutôt que par embedding.

## Why It's Interesting
Approche radicalement différente du RAG : pas de vector DB, pas de chunking, résultats traçables avec références page/section. 98.7% de précision sur FinanceBench avec Mafin 2.5.

## Key Takeaways
- **No Vector Database** : raisonnement structuré plutôt que similarité sémantique
- **No Chunking** : préserve les sections naturelles du document
- **Retrieval "humain"** : simule la navigation experte dans les documents
- **Résultats explicables** : traçables avec références page et section
- **98.7% sur FinanceBench** (Mafin 2.5)
- **Stack** : Python, LiteLLM (multi-LLM), PDF + Markdown
- **Déploiement** : self-hosted, cloud API ou MCP

## Tags

#rag, #vectorless, #document-retrieval, #llm-reasoning, #open-source
