---
url: "https://github.com/epistates/turbovault"
category: "AI & Agents"
tags:
  - MCP
  - Obsidian
  - Rust
  - vault-intelligence
  - knowledge-management
  - search
date_saved: "2026-03-23"
---

# TurboVault - MCP Server for Markdown Vaults

**Source:** [github.com/epistates/turbovault](https://github.com/epistates/turbovault)
**Category:** AI & Agents

## Summary

TurboVault est un SDK Rust et serveur MCP qui transforme un vault Obsidian (ou tout vault Markdown) en système de connaissance intelligent. Il expose 44+ outils MCP couvrant les opérations fichier, l'analyse du graphe de liens, le health scoring du vault, la recherche full-text BM25 (sub-100ms), les templates et la gestion du cycle de vie. Supporte le multi-vault avec switching de contexte instantané et les opérations batch atomiques pour des modifications multi-fichiers sûres.

## Why It's Interesting

Donne à un agent IA (Claude, etc.) une interface structurée et performante pour interagir avec un vault Markdown — bien plus efficace que de naviguer manuellement avec grep et lecture de fichiers. L'analyse de graphe (orphelins, hubs, cycles) et la détection de liens cassés automatisent la maintenance du vault.

## Key Takeaways

- 44+ outils MCP : file ops, link analysis, vault health, full-text search, templates, batch ops
- Recherche BM25 full-text avec performances sub-100ms
- Analyse du graphe de liens : relations, hubs, orphelins, références circulaires
- Opérations batch atomiques pour modifications multi-fichiers sûres
- Architecture Rust modulaire (core, parser, graph, vault, tools, batch, export)

## Tags
#MCP, #Obsidian, #Rust, #vault-intelligence, #knowledge-management, #search
