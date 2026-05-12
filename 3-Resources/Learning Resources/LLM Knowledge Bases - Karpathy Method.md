---
url: https://academy.dair.ai/blog/llm-knowledge-bases-karpathy
category: "Learning Resources"
tags:
  - knowledge-base
  - karpathy
  - llm
  - markdown-wiki
  - personal-knowledge
date_saved: "2026-04-07"
---

# LLM Knowledge Bases — The Karpathy Method

**Source:** [https://academy.dair.ai/blog/llm-knowledge-bases-karpathy](https://academy.dair.ai/blog/llm-knowledge-bases-karpathy)
**Category:** Learning Resources

## Summary
Breakdown visuel de l'approche d'Andrej Karpathy pour construire des bases de connaissances personnelles avec des LLM. Pipeline en 4 phases (ingest, compile, query, maintain) avec un wiki Markdown structuré compilé incrémentalement par le LLM.

## Why It's Interesting
Alternative radicale au RAG classique : à l'échelle d'une knowledge base personnelle (~100 articles), les fichiers d'index + la context window du LLM suffisent, sans vector DB.

## Key Takeaways
- **LLM comme compilateur** : lit les sources brutes et produit un wiki structuré et interlinké
- **4 phases** : ingest → compile → query → maintain
- **Pas de vector DB nécessaire** à l'échelle personnelle (~100 articles)
- **Feedback loop** : les explorations sont réintégrées dans le wiki
- **Direction future** : fine-tuning d'un LLM sur le wiki pour que le modèle "connaisse" les données dans ses poids
- **Événement gratuit** DAIR.AI le 29 avril pour approfondir

## Tags

#knowledge-base, #karpathy, #llm, #markdown-wiki, #personal-knowledge
