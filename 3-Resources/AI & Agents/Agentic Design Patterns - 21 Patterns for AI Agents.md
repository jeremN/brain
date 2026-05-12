---
url: "https://github.com/Mathews-Tom/Agentic-Design-Patterns"
category: "AI & Agents"
tags:
  - design-patterns
  - AI-agents
  - multi-agent
  - MCP
  - RAG
  - prompt-chaining
  - reference
date_saved: "2026-03-24"
---

# Agentic Design Patterns - 21 Patterns for AI Agents

**Source:** [github.com/Mathews-Tom/Agentic-Design-Patterns](https://github.com/Mathews-Tom/Agentic-Design-Patterns)
**Category:** AI & Agents

## Summary

Repo open-source contenant le texte complet du livre "Agentic Design Patterns" (424 pages) par Antonio Gulli et Mauro Sauco. Couvre 21 design patterns pour construire des systèmes IA autonomes, organisés en 4 sections : patterns fondamentaux (prompt chaining, routing, parallelization, reflection, tool use, planning, multi-agent collaboration), systèmes avancés (memory management, learning/adaptation, MCP, goal monitoring), production (exception handling, human-in-the-loop, RAG), et architectures multi-agents (communication inter-agents, resource optimization, reasoning, safety guardrails, evaluation).

## Why It's Interesting

Référence complète et structurée qui comble le fossé entre théorie et implémentation pour les agents IA. Les 21 patterns couvrent tout le spectre, du prompt chaining basique jusqu'aux guardrails de sécurité et l'évaluation en production. Utile aussi bien comme guide de conception que comme checklist pour auditer un système existant.

## Key Takeaways

- 21 design patterns couvrant 4 niveaux : fondamental, avancé, production, multi-agent
- Patterns fondamentaux : prompt chaining, routing, parallelization, reflection, tool use, planning
- Patterns avancés : memory management, MCP (Model Context Protocol), goal monitoring
- Patterns production : exception handling, human-in-the-loop, RAG
- Patterns multi-agents : communication inter-agents, resource optimization, safety guardrails
- Appendice avec overview des frameworks agentiques et guide pour coding agents
- MIT License, 1.6k stars, livre complet en Markdown

## Deep Dive : Memory Management (Chapitre 8)

Stratégie à deux couches :

**Short-term memory** (mémoire de travail) — vit dans la fenêtre de contexte du LLM (messages récents, résultats d'outils, réflexions). Éphémère, limitée par la taille du contexte. Structurée via des préfixes de scope (`user:`, `app:`, `temp:`) pour organiser les données de session.

**Long-term memory** (mémoire persistante) — utilise des vector databases pour du semantic search au-delà d'une seule conversation. Permet à un agent de se souvenir de préférences, d'apprendre de ses erreurs, et de construire une base de connaissances cumulée.

Deux implémentations comparées :
- **Google ADK** : Session → State → Memory comme trois couches distinctes
- **LangChain/LangGraph** : thread-scoped short-term via checkpointers + namespace-based long-term partagée entre threads

Bonnes pratiques : types sérialisables simples, pas de nesting profond, ne jamais modifier le state directement — toujours passer par des event deltas (modifications incrémentielles émises comme événements) pour garantir le logging et la persistance.

## Deep Dive : Safety Guardrails (Chapitre 18)

Défense en profondeur sur 4 niveaux :

**1. Input** — Modération du contenu entrant (APIs de content moderation), validation de schéma via Pydantic, sanitization des entrées malveillantes. Premier mur contre les prompts toxiques et injections.

**2. Output** — Filtres post-génération analysant les réponses avant envoi. Détection de toxicité, biais, fuite d'informations sensibles. Validation de format pour conformité structurelle.

**3. Behavioral** — Contraintes au niveau du prompt et de la config agent (rôles, objectifs, backstories). Restrictions sur les outils accessibles, rate limiting, gestion de la fenêtre de contexte.

**4. Operational** — Human-in-the-loop pour décisions critiques, logging/observabilité pour audit, gestion des erreurs avec dégradation gracieuse, détection d'anomalies.

Implémentation concrète : CrewAI avec un modèle rapide (Gemini Flash) comme guardrail de pré-screening devant le modèle principal. Les policies couvrent l'instruction subversion (prompt injection), le contenu interdit, le hors-sujet, et la protection d'informations propriétaires.

### Détail des 4 couches

**Input — Validation & Sanitization**
Un Policy Enforcer Agent dédié (Gemini Flash, temperature 0.0) intercepte chaque input. Validation Pydantic via un modèle `PolicyEvaluation` (3 champs : `compliance_status`, `evaluation_summary`, `triggered_policies`). L'agent a `allow_delegation=False` et un traitement séquentiel strict. Cible : jailbreaking, prompt injection, inputs mal formés.

**Output — Filtrage post-génération**
Validation à 3 niveaux après génération : type checking (`isinstance`), vérification du statut de conformité ("compliant"/"non-compliant"), validation de la liste des policies déclenchées. Le `output_pydantic` de CrewAI force une structure de sortie stricte — réponse rejetée si elle ne match pas le schéma. Cible : contenu toxique/biaisé, hallucinations, fuite d'infos sensibles, discours haineux.

**Behavioral — Contraintes prompt-level**
Un `SAFETY_GUARDRAIL_PROMPT` définit 4 directives de policy couvrant 7+ catégories : jailbreaking, hate speech, activités dangereuses, contenu explicite, langage abusif, discussions hors-sujet, plagiat académique, exposition d'infos propriétaires. Inclut des exemples concrets (permis vs interdit), un processus d'évaluation en 3 étapes, et une résolution default-to-compliant en cas d'ambiguïté. Chaque agent configuré avec rôle/goal/backstory explicites.

**Operational — Monitoring & Observabilité**
Logging à chaque étape critique (entrée/sortie des fonctions, output brut avant validation, résultats guardrails PASSED/FAILED). Try-except autour de `crew.kickoff()` avec préservation du contexte d'erreur. Vérification d'attributs avec `hasattr()`. Cible : violations silencieuses, failures sans trace, anomalies non détectées, épuisement des quotas API.

### Architecture intégrée

Les 4 couches fonctionnent en séquence : Input filtre → Behavioral guide → Output valide → Operational surveille. Point clé : utiliser un modèle léger (Gemini Flash) comme guardrail plutôt que le modèle principal — sécurité ajoutée sans exploser coûts ni latence.

### Exemple d'implémentation

L'implémentation du livre utilise CrewAI avec les composants suivants :
- Un modèle Pydantic `PolicyEvaluation` avec 3 champs typés (`compliance_status: str`, `evaluation_summary: str`, `triggered_policies: List[str]`)
- Une fonction `validate_policy_evaluation()` qui accepte le output LLM brut, extrait le JSON (nettoyage markdown inclus), valide contre le schéma Pydantic, et retourne un tuple `(bool, résultat_ou_erreur)`
- Un `policy_enforcer_agent` configuré avec rôle "AI Content Policy Enforcer", Gemini Flash à temp 0.0, verbose=False
- Une `evaluate_input_task` avec le paramètre `guardrail` pointant vers la fonction de validation
- Un `Crew` en mode `Process.sequential`

Le chapitre inclut 8 cas de test : requêtes conformes ("Explain quantum entanglement"), tentatives de jailbreak, demandes d'infos concurrentielles, langage abusif, plagiat académique, et discussions politiques. Chaque test affiche ✅ ou ❌ selon le résultat du guardrail.

> Pour le code complet, voir directement le [Chapitre 18 sur GitHub](https://github.com/Mathews-Tom/Agentic-Design-Patterns/blob/main/04-Part_Four/Chapter_18-Guardrails_Safety_Patterns-1Gpc5af_okze1kprRLohP6-81e1KwL6HggjeLvxQyIuk.md).

## Deep Dive : Reflection Pattern (Chapitre 4)

Le pattern Producer-Critic : au lieu qu'un agent review son propre travail (biais cognitif), on sépare en deux rôles distincts.

**Le cycle en 4 étapes :**
1. **Execution** — Le Producer génère un output initial (code, texte, plan)
2. **Evaluation** — Le Critic (persona type "senior engineer" ou "fact-checker") analyse l'output contre des critères (accuracy, cohérence, complétude)
3. **Refinement** — Le Producer produit une version améliorée basée sur le feedback
4. **Iteration** — Le cycle recommence jusqu'à "CODE_IS_PERFECT" ou limite d'itérations atteinte

**Pourquoi séparer Producer et Critic :**
Un agent qui review son propre travail souffre de biais cognitif — il tend à valider ce qu'il a produit. Le Critic, avec un persona dédié et une perspective fraîche, identifie des problèmes que le Producer ne voit pas.

**Le prompt du Critic (LangChain) :**

> "You are a senior software engineer and an expert in Python. Your role is to perform a meticulous code review. Critically evaluate the provided Python code based on the original task requirements. Look for bugs, style issues, missing edge cases, and areas for improvement. If the code is perfect and meets all requirements, respond with 'CODE_IS_PERFECT'. Otherwise, provide a bulleted list of your critiques."

**Implémentation LangChain — boucle itérative :**
Boucle procédurale avec gestion d'historique de messages. À chaque itération :
1. Si i==0 : le Producer génère le code initial
2. Sinon : le Producer reçoit "Please refine the code using the critiques provided" + historique
3. Le Critic évalue avec un `reflector_prompt` séparé (persona distinct)
4. Si "CODE_IS_PERFECT" dans la critique → break
5. Sinon le feedback est appendé à l'historique pour le cycle suivant

Config : temperature 0.1 (déterminisme), GPT-4o recommandé, max 3 itérations par défaut. L'historique des messages est maintenu entre itérations mais les listes Producer et Critic sont séparées pour éviter la contamination de biais.

**Implémentation Google ADK — composition déclarative :**
Un `SequentialAgent` avec deux sous-agents : `DraftWriter` (output_key="draft_text") et `FactChecker` (output structuré : status "ACCURATE"/"INACCURATE" + reasoning via output_key="review_output"). Différence clé : ADK est conçu pour un cycle unique — pour boucler il faut wrapper dans un `LoopAgent`.

**Critères d'évaluation du Critic :**
- Accuracy factuelle (vérification contre la source)
- Complétude (présence de tous les éléments requis)
- Qualité du code (style, efficacité, robustesse)
- Gestion des edge cases (ex: factorielle de zéro, nombres négatifs)
- Error handling (validation d'input, exceptions)
- Adhérence aux specifications originales
- Cohérence logique et flow

**Stopping conditions :**
1. Phrase-based : "CODE_IS_PERFECT" dans la réponse du Critic
2. Limite d'itérations : max 3 (configurable)

**Exemple concret du livre :** génération d'une fonction factorielle Python. Le Producer crée une version initiale → le Critic vérifie la gestion des nombres négatifs, le cas zéro, la présence d'un docstring, l'efficacité → le Producer corrige → le Critic valide "CODE_IS_PERFECT".

**Cas d'usage :** code development (écriture → tests → correction), content generation (drafts itératifs), summarization (vérification contre la source), strategic planning (test de faisabilité), conversational systems (maintien de cohérence).

**Connexion avec les autres patterns :**
- **Memory** : la Reflection est amplifiée quand l'agent garde un historique — il peut évaluer son output contre les interactions passées et le feedback utilisateur, pas juste en isolation
- **Goal Setting** : l'objectif sert de benchmark ultime pour l'auto-évaluation du Critic

**Trade-offs :**
- ✅ Qualité significativement supérieure, meilleure adhérence aux requirements complexes
- ❌ Latence accrue (multiple appels LLM, 3+ calls par document), coûts plus élevés, risque d'expansion de la fenêtre de contexte
- ❌ Risk de throttling API sur des boucles intensives
- Règle : utiliser la Reflection quand la qualité du résultat est plus importante que la vitesse et le coût

**Anti-patterns à éviter :**
- Un seul agent qui fait production + critique (biais cognitif)
- Critic avec un persona trop similaire au Producer (réduit l'objectivité)
- Boucle infinie sans stopping condition adéquate
- Pas de variation dans l'approche de critique (peut se stabiliser sur un output sous-optimal)

> Pour le code complet, voir le [Chapitre 4 sur GitHub](https://github.com/Mathews-Tom/Agentic-Design-Patterns/blob/main/01-Part_One/Chapter_4-Reflection-1HXXJOQIMWowtLw4WMiSR360caDAlZPtl5dPPgvq9IT4.md).

## Deep Dive : Coding Agents (Appendice G)

Framework pour le développement assisté par IA avec le humain comme tech lead.

**3 principes fondateurs :**
1. **Human-Led Orchestration** — Le développeur reste le décisionnaire final, les agents sont l'équipe
2. **Context Primacy** — La qualité des outputs dépend entièrement de la qualité du briefing/contexte fourni
3. **Direct Model Access** — Utiliser les frontier LLMs (Gemini 2.5 Pro, Claude Opus 4) directement plutôt que des plateformes intermédiaires

**5 agents spécialisés avec leurs prompts :**

**Scaffolder** (L'Implémenteur) — Prompt : "You are a senior software engineer. Based on requirements in 01_BRIEF.md and existing patterns in 02_CODE/, implement the feature..." Reçoit le codebase complet pour reproduire les patterns existants, les specs en markdown, et les contraintes architecturales.

**Test Engineer** (Le Quality Guard) — Prompt : "You are a quality assurance engineer. For the code provided in 02_CODE/, write a full suite of unit tests using [Testing Framework]. Cover all edge cases and adhere to project's testing philosophy." Identifie les gaps de tests, couvre les edge cases, respecte les conventions du projet.

**Documenter** (Le Scribe) — Prompt : "You are a technical writer. Generate markdown documentation for API endpoints defined in provided code. Include request/response examples and explain each parameter." Produit docs API, docstrings, exemples request/response.

**Optimizer** (Le Refactoring Partner) — Prompt : "Analyze the provided code for performance bottlenecks or areas that could be refactored for clarity. Propose specific changes with explanations for why they are improvements." Focus : performance, clarté, maintenabilité.

**Process Agent** (Le Code Supervisor) — Le plus intéressant, fonctionne en 2 phases :
- Phase 1 (Critique) : analyse statique-like identifiant bugs, violations de style, failles logiques
- Phase 2 (Reflection) : analyse sa propre critique, priorise les issues critiques, élimine les suggestions à faible impact, produit un résumé actionnable
- Prompt : "You are a principal engineer conducting code review. First, perform detailed critique of changes. Second, reflect on your critique to provide concise, prioritized summary of most important feedback."
- La phase de réflection est un starter de conversation, pas un rapport final

**Architecture de contexte — le répertoire `task-context/` :**
- `01_BRIEF.md` : specs et requirements de la tâche
- `02_CODE/` : fichiers source pertinents
- Docs et références additionnelles
- Point crucial : le dev curate manuellement le contexte (pas de retrieval automatique) pour garantir que chaque agent reçoit exactement les infos nécessaires, sans bruit

**Prompt library versionnée dans Git :**
Répertoire `/prompts` avec un fichier markdown par agent : `scaffolder.md`, `tester.md`, `documenter.md`, `optimizer.md`, `reviewer.md`. Prompts traités comme du code — historique de versions, collaboration d'équipe, amélioration itérative.

**Intégration Git hooks :**
Un pre-commit hook invoque automatiquement le Process Agent sur les fichiers stagés. Le critique + réflection est retourné directement dans le terminal avant finalisation du commit. Assurance qualité intégrée dans le flow de dev sans friction.

**Setup recommandé :**
1. Deux clés API frontier (primaire + secondaire pour comparaison et redundance)
2. Orchestrateur de contexte local avec `context.toml` en racine du projet (spécifie fichiers/répertoires/URLs à compiler dans le payload)
3. Prompt library versionnée dans `/prompts`
4. Git hooks pour QA automatisée

**Workflow complet :**
Developer brief → Scaffolder code → Test Engineer tests → Documenter docs → Optimizer suggestions → Process Agent review (critique + reflection) → Developer gate (validation finale) → Git commit (avec hook QA)

Chaque output d'agent alimente le contexte de l'agent suivant.

**Concept de "Vibe Coding" :**
Technique de démarrage rapide pour le prototypage — permet de surmonter le syndrome de la page blanche en générant des drafts initiaux rapidement, que l'on raffine ensuite avec les agents spécialisés.

**Principes de leadership d'équipe IA :**
- Maintenir l'ownership architecturale — le dev définit le "quoi" et le "pourquoi", les agents accélèrent le "comment"
- Maîtriser l'art du brief — le brief fonctionne comme un onboarding document pour un nouveau team member
- Être le quality gate ultime — les outputs agents sont des propositions, pas des commandes
- Engager un dialogue itératif — raffiner les outputs imparfaits plutôt que les jeter

**Anti-patterns à éviter :**
- Retrieval de contexte automatique black-box
- Acceptation aveugle des outputs agents sans review
- Utilisation de modèles faibles ou plateformes intermédiaires qui tronquent le contexte
- Prompts monologues au lieu de refinement conversationnel
- Séparer le développement de prompts du version control

**Chiffre clé :** chez les grandes tech companies, plus de 30% du nouveau code est assisté ou généré par des modèles IA (Gemini), changeant fondamentalement la vélocité de développement.

> Pour le détail complet, voir l'[Appendice G sur GitHub](https://github.com/Mathews-Tom/Agentic-Design-Patterns/blob/main/05-Appendix/Appendix_G-Coding_Agents-1tVyhgwrD4fu_D_pHUrwhNxoguRG3tLc1KObXFxrxE_s.md).

## Autres chapitres notables

**Inter-Agent Communication / A2A (Chapitre 15)** — Protocole ouvert (Google, soutenu par Atlassian, MongoDB, Salesforce, SAP, Microsoft) pour la communication entre agents de frameworks différents. Les agents se découvrent via des "Agent Cards" (fiches d'identité avec capabilities, endpoints, auth). 4 modes de communication : synchrone, streaming SSE, polling async, webhooks. Distinction clé : A2A = communication inter-agents, MCP = accès outils/données. Complémentaires.

**Reasoning Techniques (Chapitre 17)** — 6 techniques comparées : Chain-of-Thought (étape par étape), Tree-of-Thought (exploration multi-chemins avec backtracking), Self-Correction (boucle d'auto-évaluation), PALMs (génération + exécution de code pour calculs), ReAct (raisonnement + action via outils), Chain/Graph of Debates (débat multi-modèles). Principe unificateur : Scaling Inference Law — investir plus de compute à l'inférence améliore la qualité des décisions.

**MCP (Chapitre 10)** — Au-delà de la spec : un bon serveur MCP nécessite des APIs avec features déterministes (filtrage, tri) pour aider l'agent non-déterministe. Exemples avec ADK + MCPToolset et FastMCP (Python, décorateurs, génération automatique de schémas).

**Exception Handling & Recovery (Chapitre 12)** — Pattern à 3 agents séquentiels : handler principal → fallback handler (infos dégradées) → response agent. Couvre state rollback, self-correction par replanning, escalation humaine. Applications : chatbots, trading, domotique, scraping, robotique.

## Tags
#design-patterns, #AI-agents, #multi-agent, #MCP, #RAG, #prompt-chaining, #reference
