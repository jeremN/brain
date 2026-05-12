---
url: "https://github.com/epicweb-dev/epicflare/blob/main/docs/agents/testing-principles.md"
category: "Testing & QA"
tags:
  - testing
  - AI-agents
  - TypeScript
  - best-practices
  - factory-pattern
  - Symbol-dispose
date_saved: "2026-03-24"
---

# Testing Principles for AI Agent Codebases

**Source:** [github.com/epicweb-dev/epicflare - testing-principles.md](https://github.com/epicweb-dev/epicflare/blob/main/docs/agents/testing-principles.md)
**Category:** Testing & QA

## Summary

Document d'EpicWeb (Kent C. Dodds) définissant les principes de testing pour un codebase servant des agents IA. Philosophie pragmatique : tests minimalistes, plats, explicites, sans abstraction inutile. Chaque test est auto-contenu et lisible d'un coup d'œil — crucial pour que des agents IA puissent comprendre, générer et itérer sur des tests rapidement.

## Why It's Interesting

Les principes produisent des tests qu'un agent IA peut lire, comprendre et reproduire sans naviguer dans du setup partagé ou des hiérarchies de describe. Applicable directement au pattern Test Engineer du livre Agentic Design Patterns. Approche opinionated mais pragmatique qui simplifie autant le travail humain que celui des agents.

## Key Takeaways

- Tests plats : `test(...)` au top-level, pas de `describe` imbriqués
- Setup inline dans chaque test, pas de `beforeEach`/`afterEach` ni d'état partagé
- Ne pas tester ce que TypeScript garantit déjà (pas de tests redondants sur les types)
- Factory pattern pour le setup (fonctions retournant des objets prêts, pas de globals)
- Tests offline par défaut : mocks locaux et fixtures, pas de dépendance internet/services tiers
- Cleanup via `Symbol.dispose` / `Symbol.asyncDispose` avec try-catch (le cleanup ne doit jamais faire échouer un test)
- Unit tests rapides pour la logique serveur, E2E réservés aux parcours utilisateur
- Run explicite : `bun test ./server ./mock-servers` pour éviter les conflits de discovery Playwright

## Principes détaillés

**Structural :** organisation plate, setup explicite inline, pas de config cachée ni d'état partagé entre tests.

**Coverage :** faire confiance au type system TypeScript, ne pas tester ce qui est déjà garanti par les types. Utiliser `using` + `Symbol.dispose` uniquement quand il y a un vrai cleanup à faire (fichiers, serveurs), pas pour des objets simples.

**Factory pattern :** créer des helpers qui retournent des objets prêts à l'emploi (factory functions), pas des singletons globaux. Chaque test construit son propre état.

**Offline & isolation :** écrire les tests pour qu'ils tournent offline — fakes locaux, mock servers, fixtures. Pas de dépendance à des services tiers.

**Performance :** unit tests rapides pour la logique métier, E2E uniquement pour les parcours utilisateur complets. Scoper le runner explicitement pour éviter les découvertes accidentelles de specs.

## Tags
#testing, #AI-agents, #TypeScript, #best-practices, #factory-pattern, #Symbol-dispose
