---
tags:
  - prompt-engineering
  - llm
  - ai
  - prompting
  - chain-of-thought
  - few-shot
  - zero-shot
  - personas
  - delimiters
  - structured-output
source: Frontend Masters
instructor: Sabrina Goldfarb
course: Practical Prompt Engineering for Developers
repo: https://github.com/stevekinney/web-security
---

# Practical Prompt Engineering — Base de connaissances

Cours "Practical Prompt Engineering for Developers" — Sabrina Goldfarb / Frontend Masters
Source : https://sgoldfarb2.github.io/practical-prompt-engineering/

---

## 1. Introduction

### Qu'est-ce que le Prompt Engineering ?

Le prompt engineering est le processus d'ecriture d'instructions efficaces pour un modele, afin qu'il genere du contenu correspondant a nos besoins de maniere consistante. C'est un melange d'art et de science, car les reponses des LLMs sont non-deterministes.

Points cles :

- Ce n'est pas de la magie, c'est de la science
- C'est l'outil le plus accessible pour ameliorer les resultats d'un LLM
- Ce n'est pas un remplacement de notre esprit critique — c'est un amplificateur
- Avec le meme modele, on peut obtenir des resultats significativement meilleurs grace aux techniques de prompting
- Pour les applications IA, un meilleur prompt = moins de couts API

### Comment fonctionnent les LLMs

**Les bases** : les LLMs sont des machines de prediction de patterns qui generent un token a la fois. Il n'y a pas de "planification" — le modele "reflechit" au fur et a mesure qu'il ecrit. Comparable a l'autocompletion du telephone, mais bien plus performant.

**Non-determinisme** : meme prompt, meme modele, meme moment → reponses differentes. C'est fondamental a comprendre car cela signifie qu'il y a de l'aleatoire dans les reponses, et on doit chercher a le minimiser.

**Architecture Transformer** (2017) : le papier "Attention Is All You Need" de Google a ete revolutionnaire. Avant les transformers, les modeles ne pouvaient suivre que quelques mots proches. Le mecanisme d'attention permet au modele de determiner ce qui compte dans un contexte beaucoup plus large. Aujourd'hui, les fenetres de contexte atteignent des millions de tokens.

**Entrainement** : les LLMs sont des reseaux de neurones qui ajustent leurs parametres via l'entrainement (pre-training + fine-tuning). Le fine-tuning transforme le modele de base en assistant conversationnel. L'intelligence entiere du modele tient dans ses milliards de parametres et quelques centaines de lignes de code d'inference.

### Temperature, Top P, Tokens et Context Windows

**Temperature** (0 a 2) : controle la creativite/aleatoire du modele.

| Valeur | Comportement |
|--------|-------------|
| 0 | Toujours le mot le plus probable (deterministe) |
| ~0.5 | Peu creatif, fiable (apps medicales) |
| ~1.4 | Creatif (ecriture, brainstorming) |
| 2 | Completement aleatoire et incoherent |

> Non disponible dans les interfaces chat (ChatGPT, Claude), uniquement via API.

**Top P** : filtre les tokens par probabilite cumulee. Si Top P = 0.9, seuls les tokens representant les 90% les plus probables sont consideres. Fonctionne en complement de la temperature pour affiner le controle de la creativite.

**Tokens** : l'unite de base pour le modele (≈ 0.75 mots). "JavaScript" peut etre 1 token, "javascript" peut en etre 2.

**Context Window** : nombre maximum de tokens que le LLM peut "retenir". Les LLMs n'ont pas de memoire — a chaque message, tout l'historique de la conversation est renvoye au modele. Points critiques :

- Quand la limite est atteinte, les messages les plus anciens disparaissent silencieusement (FIFO)
- Le modele ne previent pas quand du contexte est perdu
- Meme avec de grandes fenetres, ce concept reste crucial

---

## 2. Techniques de Prompting de Base

### Standard Prompt

La forme la plus basique : poser des questions directes ou donner des instructions directes, sans formatage special ni technique particuliere.

Principe fondamental : question vague → reponse vague. Question precise → reponse precise. Le modele travaille avec ce qu'on lui donne, exactement comme un collegue humain.

### Zero-Shot

Demander au modele de realiser une tache **sans fournir d'exemple**. On s'appuie entierement sur les donnees d'entrainement du modele.

**Quand l'utiliser** : pour les taches simples que le modele peut accomplir sans exemple — ecriture de code basique, relecture, classification de sentiment, tout ce que le modele a vu en abondance dans ses donnees d'entrainement.

```
Classify the customer rating into neutral, negative or positive.

Text: The product was okay. It worked, but wasn't the easiest to understand how to use.
Sentiment:
```

### One-Shot

Fournir **un seul exemple** pour guider le modele. Exploite la capacite du modele a generaliser a partir d'un minimum de donnees.

**Quand l'utiliser** : pour les patterns simples, les transformations directes, ou quand on manque de temps pour preparer plusieurs exemples.

Bonnes pratiques :

- Choisir son exemple avec soin — il definit le pattern
- Utiliser un exemple representatif du cas majoritaire, pas un edge case
- Inclure tous les elements attendus dans l'output
- Combiner avec des instructions explicites si necessaire
- Ne pas surcharger l'exemple pour couvrir tous les cas

### Few-Shot

Fournir **deux exemples ou plus**. Le papier "Language Models are Few-Shot Learners" a prouve que les exemples few-shot ameliorent drastiquement les performances, et que cette amelioration s'accentue avec la taille du modele.

**Quand l'utiliser** : patterns complexes avec plusieurs variations, classification multi-categories, standardisation de formats sur des inputs divers, taches specifiques a un domaine, quand la consistance est critique.

Bonnes pratiques :

- La diversite des exemples compte
- Inclure des edge cases et des cas d'echec
- Garder les exemples concis mais complets
- Tester avec differents nombres d'exemples dans plusieurs chats

### Context Placement (Placement du contexte)

Le papier "Lost in the Middle: How Language Models Use Long Contexts" a demontre un phenomene critique :

- Les LLMs performent le mieux quand l'information est au **debut** du contexte
- La performance est aussi correcte quand l'information est a la **fin**
- L'information **au milieu** peut etre "perdue" — parfois le modele performe **moins bien** qu'avec zero document

> Regle d'or : informations critiques au debut, informations importantes secondaires a la fin, le reste au milieu.

Implications pratiques :

- Demarrer de nouveaux chats quand c'est possible
- Garder le contexte aussi reduit que possible
- Positionner le contexte crucial en premier

---

## 3. Techniques de Prompting Avancees

### Structured Output (Sortie structuree)

Obtenir des formats consistants et utilisables des la premiere reponse. Crucial pour le code de production et les applications IA ou le format de la reponse doit etre predictible.

**Quand l'utiliser** : besoin d'un format specifique, generation d'items similaires (fonctions, tests, docs, data), extraction de donnees structurees depuis du contenu non-structure, ou tout simplement quand on en a marre de reformater les reponses.

Exemples de formats demandables : JSON, schemas specifiques, templates avec champs, specifications de fonctions avec types, schemas de validation.

```
Convert this error into JSON. Return ONLY the JSON, no explanation.

"Error 404 occurred at 3:45 PM when trying to access /users/profile endpoint"
```

### Chain of Thought (CoT)

Le papier "Large Language Models are Zero-Shot Reasoners" a montre que simplement ajouter **"let's think step by step"** a un prompt fait passer la precision sur les problemes multi-arithmetiques de **17.7% a 78.7%**.

Cette amelioration s'accentue avec la taille du modele. C'est versatile et independant de la tache — utilisable pour la generation de code, les problemes mathematiques, le raisonnement complexe, etc.

**Deux approches** :

1. **Zero-shot CoT** : ajouter simplement "Think through this step by step" a la fin du prompt
2. **CoT structure** : decomposer explicitement les etapes dans le prompt lui-meme

```
Can penguins fly?
Think through this step by step.
```

Pour des taches complexes, on peut pre-decomposer les etapes :

```
Let's build a complete export/import system step by step.

Step 1: First, analyze what data we need to export...
Step 2: Design the export JSON schema...
Step 3: Create the export function...
Step 4: Create the import function...
Step 5: Add error recovery...

Implement this complete system with all steps. Think step by step.
```

### Delimiters / XML

Les delimiteurs creent des frontieres claires entre les sections d'un prompt, aidant le modele a distinguer les differentes parties. Fonctionne comme la hierarchie visuelle dans un article : titres, sous-titres, corps de texte — chaque section a une importance differente.

**Points cles** :

- Le type de delimiteur n'a pas d'importance (XML, markdown, tirets, custom...) tant qu'on est consistent
- A utiliser des qu'un prompt est complexe (donc souvent)
- Les balises XML sont particulierement efficaces pour structurer des prompts longs

```xml
<research_area>
  <topic>Prompt Management Solutions</topic>
  <questions>
    - What tools currently exist for prompt library management?
  </questions>
</research_area>
```

### Personas

Donner un role au modele ne le rend pas "plus intelligent" — cela **oriente** le modele vers certaines parties de ses donnees d'entrainement. Dire "you are a database expert" pousse le modele vers tout ce qu'il a appris sur les index, la normalisation, l'optimisation de requetes, etc.

**Effets mesures des personas** :

- Selection du vocabulaire (termes techniques vs langage simple)
- Structure des reponses (systematique vs conversationnel)
- Comportement de verification d'erreurs (rigueur)
- Confiance dans les assertions

**Cas d'usage courants** :

| Contexte | Persona |
|----------|---------|
| Code review | "Senior engineer focused on security and performance" |
| Documentation | "Technical writer who prioritizes clarity for beginners" |
| Debugging | "Systematic debugger who checks assumptions" |
| Architecture | "Solutions architect who considers scalability" |

**Attention** : une persona trop longue ou detaillee peut rendre les reponses du modele plus rigides.

> Astuce : on peut changer de persona en cours de conversation pour obtenir differents points de vue sur le meme probleme.

---

## Resume — Cheat Sheet

| Technique | Quand l'utiliser | Effort |
|-----------|-----------------|--------|
| **Standard** | Taches simples et directes | Minimal |
| **Zero-Shot** | Taches que le modele connait bien sans exemple | Minimal |
| **One-Shot** | Patterns simples, transformations directes | Faible |
| **Few-Shot** | Patterns complexes, classification, consistance requise | Moyen |
| **Context Placement** | Longs contextes, conversations importantes | Faible |
| **Structured Output** | Formats specifiques, production code, pipelines | Moyen |
| **Chain of Thought** | Raisonnement complexe, maths, logique, debuggage | Faible |
| **Delimiters/XML** | Prompts complexes avec plusieurs sections | Moyen |
| **Personas** | Orienter l'expertise, le ton, la perspective | Faible |

### Papiers de reference cites

- "Attention Is All You Need" (2017, Google) — Architecture Transformer
- "Language Models are Few-Shot Learners" (2020, OpenAI) — GPT-3 et few-shot
- "Lost in the Middle: How Language Models Use Long Contexts" (2023) — Placement du contexte
- "Large Language Models are Zero-Shot Reasoners" (2022) — Chain of Thought zero-shot

### Principes generaux

1. Question precise → reponse precise (le modele travaille avec ce qu'on lui donne)
2. Les LLMs sont non-deterministes → il faut minimiser l'aleatoire
3. Le contexte au milieu peut etre "perdu" → structurer ses prompts
4. "Let's think step by step" est un game-changer universel
5. Les exemples (few-shot) ameliorent drastiquement la qualite
6. Structurer sa demande (delimiteurs, XML) aide le modele a distinguer les sections
7. Les personas orientent le modele vers les bonnes donnees d'entrainement
8. Demander un format de sortie specifique evite le reformatage
9. Ouvrir de nouveaux chats regulierement pour garder un contexte propre

---

*Genere a partir du cours Frontend Masters "Practical Prompt Engineering for Developers" de Sabrina Goldfarb*
*Site : https://sgoldfarb2.github.io/practical-prompt-engineering/*

## Why It's Interesting
Critical skill for effectively leveraging AI in development workflows.

## Key Takeaways
- Prompt structure and composition
- Techniques for better results
- Context and framing strategies
- Iterative refinement approaches

## Tags
