---
url: "https://github.com/epicweb-dev/epicflare"
category: "Architecture & Engineering"
tags:
  - Cloudflare-Workers
  - MCP
  - Remix
  - testing
  - harness-engineering
  - AGENTS.md
  - TypeScript
  - best-practices
  - reference
date_saved: "2026-03-24"
---

# EpicFlare - Full-Stack Reference for Agents on Cloudflare

**Source:** [github.com/epicweb-dev/epicflare](https://github.com/epicweb-dev/epicflare)
**Category:** Architecture & Engineering

## Summary

Starter et référence d'Epic Web (Kent C. Dodds) pour construire des applications full-stack sur Cloudflare Workers avec support MCP natif. Stack : Remix 3, Bun, Cloudflare D1/KV/Durable Objects, Playwright, esbuild. Au-delà du code, le repo est surtout une mine d'or de documentation sur les best practices pour coder avec des agents IA : testing principles, MCP server patterns, harness engineering, code style, mock servers, E2E testing, et architecture.

## Why It's Interesting

Le code est secondaire — la vraie valeur c'est le `/docs` qui constitue un guide complet et opinionated pour structurer un codebase agent-friendly. Le concept de "harness engineering" (transformer chaque correction répétée en enforcement mécanique) et les MCP server patterns sont applicables à n'importe quel projet, pas juste Cloudflare.

## Key Takeaways

- AGENTS.md minimaliste qui pointe vers des docs focalisées — pas un monolithe
- Concept de harness engineering : "if reviewers repeat the same comment twice, encode it"
- 11 patterns MCP server exhaustifs (naming, annotations, schemas, errors, pagination)
- Testing principles pour codebases IA : tests plats, inline, offline, factory pattern
- Mock API servers isolés avec D1 par service et dashboard debug
- Code style TypeScript strict et opinionated
- PR preview deployments automatisés avec cleanup

---

## Deep Dive : MCP Server Patterns

> [docs/mcp-server-patterns.md](https://github.com/epicweb-dev/epicflare/blob/main/docs/mcp-server-patterns.md)

11 patterns pour construire des serveurs MCP propres :

**1. Server Instructions** — Onboarding pour l'IA. Focus sur les workflows et conventions cross-outils, pas la duplication de la doc tool-level. Sections : quick start, comportements par défaut, stratégies de chainage d'outils, patterns d'usage courants.

**2. Tool Descriptions** — Complémentent les schemas sans les dupliquer. Inclure : ce que l'outil fait (1-2 phrases), comportement et gotchas, sémantique de l'output, stratégies de recovery d'erreur, exemples concrets. Ne pas re-lister les paramètres déjà dans le inputSchema.

**3. Tool Annotations** — 4 annotations obligatoires sur chaque outil :
- `readOnlyHint` : pour les GET/LIST
- `destructiveHint` : pour les DELETE et actions irréversibles
- `idempotentHint` : même input → même résultat
- `openWorldHint` : accès à des APIs/ressources externes

**4. Schema Patterns** — Schemas riches avec descriptions claires, valeurs par défaut, valeurs valides (enums), formats attendus. `inputSchema` pour les arguments, `outputSchema` pour documenter la structure de `structuredContent`.

**5. Response Formatting** — Double réponse : texte lisible humain en markdown (emojis pour le statut : ✓, ⚠️, 🟢, 🔴, infos contextuelles, next steps) + données machine-friendly en `structuredContent`.

**6. Tool Modules** — Un outil par fichier. Description, annotations, schemas et handler colocalisés. Un petit module registry (`register-tools`) qui importe et enregistre chaque outil. Bénéfices : diffs plus petits, meilleure sync docs/code.

**7. Tool Naming** — snake_case avec préfixes sémantiques : `list_*` (multiple items), `get_*` (single by ID), `create_*` (création), `update_*` (modification), `delete_*` (suppression), `browse_*` (navigation/exploration), `search_*` (requête avec filtres).

**8. Error Handling** — Messages d'erreur actionnables : ce qui a échoué + suggestion de fix + référence aux outils liés + valeurs valides. Ex : "Feed 'X' not found. Next: Use list_feeds to see available feeds."

**9. Pagination** — Structure cohérente : `hasMore` (boolean), `nextCursor` (string|undefined), `itemsReturned` (number), `limit` (number). Documenter la sémantique de chainage pour passer `nextCursor` aux pages suivantes.

**10. Resource Patterns** — Données read-only avec URIs clairs (`media://feeds`, `media://feeds/{id}`), MIME types appropriés, descriptions expliquant la structure des données.

**11. Prompt Patterns** — Starters de conversation orientés tâche qui guident des workflows multi-étapes, fournissent du contexte sur les outils, incluent des next steps concrets, et supportent des paramètres optionnels.

**Anti-patterns** : dupliquer la doc des arguments entre descriptions et schemas, mentionner les noms de champs du protocole dans les descriptions, pointer vers les schemas sans les rendre auto-suffisants, annotations manquantes, erreurs vagues.

---

## Deep Dive : Harness Engineering

> [docs/agents/harness-engineering.md](https://github.com/epicweb-dev/epicflare/blob/main/docs/agents/harness-engineering.md)

Méthodologie pour construire des systèmes durables et agent-friendly. Transforme les corrections ponctuelles en améliorations permanentes.

**4 piliers :**
1. **Collaboration humain-agent** — Les humains dirigent, les agents exécutent
2. **Attention = monnaie rare** — Automatiser les guidances répétitives
3. **Vérité locale au repo** — La documentation vit où elle est utilisée, pas dans des wikis externes
4. **Enforcement > exhortation** — Petites règles mécaniques > longues instructions fragiles

**Boucle d'amélioration continue (5 étapes) :**
Define (intent + critères) → Implement → Evaluate (`bun run validate`) → Capture (docs, tests, automation) → Promote (transformer en enforcement mécanique)

**Escalade : de la leçon à la loi**
Quand une erreur se répète, monter dans l'échelle d'enforcement :
- Level 1 : Documentation (clarifier dans `docs/agents`)
- Level 2 : Testing (ajouter une couverture pour le failure mode)
- Level 3 : Linting/Structure (règle statique)
- Level 4 : Automation (encoder dans des scripts)

**Règle de décision : "If reviewers repeat the same comment twice, encode it."**

**Qualité lisible** — Les signaux de qualité doivent être : déterministes (scripts toujours identiques), exécutables localement (pas de surprises CI), explicites (messages d'erreur avec hints de remédiation), contextualisés (liens docs près du code).

**Maintenance hebdomadaire :** supprimer les guidances obsolètes, resserrer les instructions floues, identifier un défaut récurrent et proposer un guardrail, convertir la dette tech en items trackables. "Continuous small cleanups are cheaper than periodic large rewrites."

---

## Deep Dive : Testing Principles

> [docs/agents/testing-principles.md](https://github.com/epicweb-dev/epicflare/blob/main/docs/agents/testing-principles.md) + [docs/agents/end-to-end-testing.md](https://github.com/epicweb-dev/epicflare/blob/main/docs/agents/end-to-end-testing.md)

**Unit/Server tests :**
- Tests plats avec `test(...)` au top-level, pas de `describe` imbriqués
- Setup inline dans chaque test, pas de `beforeEach`/`afterEach`
- Ne pas tester ce que TypeScript garantit déjà
- Factory pattern pour le setup (fonctions → objets prêts, pas de globals)
- Tests offline par défaut : mocks locaux, fixtures, pas de dépendance internet
- Cleanup via `Symbol.dispose` / `Symbol.asyncDispose` avec try-catch (le cleanup ne fail jamais)
- Run explicite : `bun test ./server ./mock-servers`

**E2E tests (Playwright) :**
- Focus sur les parcours utilisateur complets, pas les détails d'implémentation
- Locators stables : `getByRole` > `getByLabel` > `getByText` > CSS (dernier recours)
- Assertions sur les outcomes visibles, pas de timeouts arbitraires
- Technique "window marker" pour vérifier la navigation client-side sans full reload
- Données de test réalistes mais évidemment locales (pas de secrets réels)
- `bun run test:e2e` auto-copie `.env.example` si manquant

---

## Deep Dive : Code Style TypeScript

> [docs/agents/code-style.md](https://github.com/epicweb-dev/epicflare/blob/main/docs/agents/code-style.md)

- `Array<T>` et `ReadonlyArray<T>` au lieu de `T[]` (évite les pièges de précédence dans les unions)
- Function declarations pour les fonctions réutilisables, arrow functions uniquement pour callbacks/event handlers
- Named exports par défaut, default exports uniquement si le framework l'exige
- Imports via `#...` (package.json imports) plutôt que `../..`, `./...` pour le même répertoire uniquement
- `type` pour shapes et unions, `interface` uniquement pour declaration merging ou extension points publics
- Inline types dans les paramètres de fonctions sauf si la réutilisation justifie un type nommé
- `null` = "pas de valeur explicite", `undefined` = "optionnel/omis" — jamais mélanger dans une même API
- `satisfies` pour les exports d'objets matchant des contrats framework

---

## Deep Dive : Mock API Servers

> [docs/agents/mock-api-servers.md](https://github.com/epicweb-dev/epicflare/blob/main/docs/agents/mock-api-servers.md)

Pattern pour émuler des APIs tierces avec des Workers Cloudflare isolés :
- Chaque mock dans `mock-servers/<service>/` avec son propre Worker, sa D1 dédiée, ses migrations
- Dashboard `GET /__mocks` pour discovery et debug des endpoints
- Déployé automatiquement en PR preview (`<app>-pr-<number>-mock-<service>`)
- CI crée les instances D1, génère les configs, applique les migrations, déploie avec auth partagée
- Les commentaires de PR incluent les liens dashboard authentifiés

---

## Deep Dive : Architecture

> [docs/architecture/](https://github.com/epicweb-dev/epicflare/tree/main/docs/architecture)

**Request lifecycle** — Toutes les requêtes entrent par `worker/index.ts` wrappé par `OAuthProvider`. Hiérarchie de routing : OAuth → Browser metadata → OAuth resource metadata → MCP endpoint (auth bearer) → Chat Agent (cookie session) → Static Assets → App Server (Remix).

**AGENTS.md** — Intentionnellement bref, une seule règle dure ("Use Bun, not npm"), le reste pointe vers les docs focalisées. Philosophie : index navigable, pas encyclopédie.

**MCP Apps** — Architecture modulaire : `mcp/index.ts` (server init), `mcp/register-tools.ts` (aggregator), `mcp/tools/*.ts` (un fichier par outil), `mcp/apps/*.ts` (UI entry points). Communication host via bridge MCP Apps standard ou `window.parent.postMessage()`. Sécurité : sandbox isolation, CSP, CORS, pas de secrets dans les UI payloads.

**OXLint custom plugins** — Framework pour règles de lint locales via `createOnce` API d'OXLint. Config séparée : `.oxlintrc.json` minimal → extends `tools/oxlint/oxlint-rules.json`. Plugins Oxlint-only, pas de compat ESLint.

## Tags
#Cloudflare-Workers, #MCP, #Remix, #testing, #harness-engineering, #AGENTS.md, #TypeScript, #best-practices, #reference
