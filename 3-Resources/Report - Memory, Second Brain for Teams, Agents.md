---
date_created: "2026-04-17"
type: "report"
tags:
  - memory
  - second-brain
  - agents
  - teams
  - synthesis
---

# Report: Memory, Second Brain for Teams, Agents

Synthesis of everything in the vault touching three intertwined themes — **agent memory**, **second brains (individual + team/company)**, and **agent architecture / frameworks**. Includes what was already present plus the 103 new entries added on 2026-04-17.

## 1. Agent Memory — the state of the art in this vault

### 1.1 Tools and libraries

Persistent memory layered on top of coding agents is the fastest-moving sub-theme.

- **MCP Memory Server** (official, `modelcontextprotocol/servers/src/memory`) — the canonical reference implementation. Baseline every other tool below either extends or competes with.
- **MCP Sequential Thinking Server** — complementary reasoning primitive; not "memory" strictly, but stores a structured thought trace the agent can reuse across turns.
- **ByteRover CLI** — REPL with 24 built-in tools, cloud-synced memory tree, 96.1% LoCoMo accuracy, 20+ LLM providers, 22+ agents. The closest thing to "team-distributable agent memory" you have.
- **Claude Mem** (`thedotmack/claude-mem`) — persistent memory specifically for Claude Code sessions.
- **OpenKB** (VectifyAI) — OSS knowledge base with vector search, designed to be queryable by agents over large corpuses.
- **OpenMementos** (Microsoft, HuggingFace dataset) — dataset. Likely fuel for training memory-retrieval models rather than infrastructure.
- **Hivemind** (Activeloop) — distributed intelligence coordination across multiple agents (more about multi-agent state than memory per se).
- **gbrain** (Garry Tan) — small-but-interesting brain-style repo.

### 1.2 Theory and patterns

- **Agentic Design Patterns — Chapter 8: Memory Management** (Gulli/Sauco book). The definitive reference piece in your vault. Two-layer model:
  - *Short-term*: in LLM context window, scoped via `user:`/`app:`/`temp:` prefixes, transient.
  - *Long-term*: vector DB, semantic search across conversations, preferences, error-learning.
  - Framework comparison: Google ADK (Session → State → Memory) vs LangChain/LangGraph (thread-scoped checkpointers + namespace long-term).
  - Rule: never mutate state directly — always through event deltas, to preserve logging and persistence.
- **Opensource Persistent & Portable Memory for Coding Agents** (Kevin Nguyen, X). Direct OSS response to the Anthropic agent-memory architecture leak.
- **Harness Engineering — Martin Fowler** + **Harness Design for Long-Running Apps — Anthropic**. Two complementary pieces on how to structure the *envelope* around a long-running agent, including where memory lives, how state is rehydrated, and how recovery works.
- **Knowledge Priming — Martin Fowler** (already in Prompts & Context Engineering) — the companion idea at the prompt layer: inject the right historical context before generation.

### 1.3 Why this matters for you (frontend + agents)

Everything you're building with Claude Code leans on *some* memory story. The practical options in your vault:
1. File-based memory via Obsidian Mind / claude-obsidian templates (lightweight, local, works now).
2. ByteRover CLI if you want cross-tool, cross-teammate shareable memory with cloud sync.
3. MCP Memory Server as the lowest-level building block you plug anything else into.

## 2. Second Brain — Individual and Team/Company

### 2.1 Individual second brain

- **Obsidian Mind** (breferrari). Vault template built for engineers using Claude Code as thinking partner. 15 slash commands (`/standup`, `/dump`, `/wrap-up`), 9 subagents, auto-injects North Star + active projects + recent changes at session start. Motto: *"folders group by purpose; links group by meaning."* The most Jerem-shaped tool in here.
- **claude-obsidian** (AgriciDaniel). Companion/adjacent repo — Claude + Obsidian integration.
- **Obsidian Skills by Kepano**. Obsidian-native skill bundle.
- **Build AI Second Brain with Claude Code and Obsidian** (MindStudio blog). Tutorial-grade walkthrough of the pattern.
- **Ars Contexta**. Claude skill tagged `#second-brain`, focused on context engineering.
- **Iwoszapar Blog**. Broader thinking around personal AI and knowledge systems.

### 2.2 Team / company second brain

This is the thinner shelf in your vault, but it grew meaningfully today.

- **Company Second Brain - AI Agent Teams** (iwoszapar). Explicit write-up on the pattern: company-wide second brain *powered by* AI agent teams (not just a shared knowledge base).
- **Enterprise Second Brain** (agentgill). OSS implementation of enterprise-scale second brain with AI agents. Architecture + tooling starting point.
- **ByteRover CLI**. Re-listed here because of its "distribute to team members" sync story — one of the few tools that treats agent memory as a team asset, not a personal one.
- **Encoding Team Standards for AI — Martin Fowler**. How to codify team conventions so agents respect them. The "what should the team brain *know*" side of the problem, distinct from "where should it live".
- **AGENTS.md Reference** + **Don't Delete AGENTS.md — d4m1n**. The convention file itself — the smallest possible shared-brain artifact any repo can carry.
- **Index Knowledge Skill**. Meta-tool for building navigable knowledge layers.

### 2.3 Gaps

Two gaps are clear even after today's additions:
- Not much on **shared team vector stores** or governance around who writes to the team brain.
- Nothing deeply comparative between "team second brain" tools — would be worth looking at Glean, Dust, Notion AI, or similar if you want product-grade references.

## 3. Agents — architecture, frameworks, coding agents

### 3.1 Frameworks and runtimes

- **LangChain DeepAgents**. Modern successor to LangChain's agent story — sophisticated reasoning + planning.
- **Hermes Agent** (Nous Research) + **Hermes Agent Profiles**. Open-source advanced agent framework and its persona/profile system.
- **Open Multi-Agent** (TS). TypeScript agent framework.
- **Agent Lightning** (Microsoft). Another framework entry.
- **Agent Orchestrator by Composio** + **Open Sourcing Agent Orchestrator**. Multi-agent orchestration.
- **Agentation** — platform-level dev environment.
- **arc-kit** (tractorjuice). Framework-adjacent kit.
- **Jules Google CLI**. Google's Jules agent CLI reference.
- **Conductor Build — Core Todos**. Todo/task primitive for agent systems.
- **Code Agent Orchestra — Addy Osmani**. Orchestration perspective from a practitioner.
- **How to Build an Agent — Ampcode**. Foundational primer.

### 3.2 Design patterns (the reference shelf)

- **Agentic Design Patterns — 21 Patterns** (Gulli/Sauco). The anchor. 4 tiers: fundamentals (prompt chaining, routing, parallelization, reflection, tool use, planning), advanced (memory, learning, MCP, goal monitoring), production (exception handling, HITL, RAG), multi-agent (A2A communication, resource optimization, safety guardrails, evaluation). Appendix G ("Coding Agents") has the Scaffolder/Tester/Documenter/Optimizer/Process-Agent composition.
- **Agentic Engineering Patterns — Simon Willison**.
- **Agent Harness Glossary**. Vocabulary reference.
- **The Cross-Agent Development Method**. Generate with model A, review with model B — formalized pair-programming across different LLMs.
- **Universal Planning Framework**.
- **Agent Flywheel**. Self-improvement loops.

### 3.3 Coding agents (what you actually use / could use)

- **Claude Code** — full shelf in `Claude & Skills/`: Best practices, hooks, Anatomy of `.claude`, CLAUDE.md guides, Game Studios post, Boris Cherny tips, 50-tips guide, etc.
- **Devin** — Use Cases Gallery + Data Analyst doc.
- **Fiberplane "How We Use Claude Code and Agents"** — engineering team practice write-up.
- **Trailofbits Claude Code Devcontainer** — sandboxed Claude Code environment. Good security pairing.
- **Vibe Kanban**. Kanban tailored to AI coding agents.
- **Ampcode — How to Build an Agent**. First-principles guide.
- **agent-teams-claude-code.md** (existing). Team-level Claude Code patterns.
- **Claude Code Projects Index — Daniel Rosehill**. Rosehill's project registry.

### 3.4 Research and autonomous agents

Strong cluster here after today's additions:
- **Autoresearch — Shopify Engineering** — production write-up from Shopify.
- **Research-Driven Agents — SkyPilot** — patterns for agents that actually do research.
- **autoresearch — Karpathy** + **pi-autoresearch — davebcn87** + **Deer Flow (ByteDance)** + **OpenViking (ByteDance)** + **Three Man Team** — open-source research-agent repos.
- **NanoCorp — Autonomous AI Companies**. The most ambitious frame: companies-as-agents.

### 3.5 Safety, evaluation, quality

- **Agentic Design Patterns Ch. 18** (4-layer defense-in-depth guardrails — already summarized in that note).
- **Testing Principles for AI Agent Codebases**.
- **Testing Agent Skills Guide — Philipp Schmid**.
- **SECURITY.md for AI Agents**.
- **AI Agents Don't Understand Secrets** + **Preventing AI Agents from Leaking Your Secrets** + **From .env to Leakage**.
- **Zero Alignment — Maggie Appleton**. Meta piece on alignment posture.
- **Who Is Responsible for AI-Generated Code**.
- **LLM Analytics Clustering — PostHog**. Observability for LLM apps in production.
- **How TanStack Tests AI Across 7 Providers**. Multi-provider eval methodology.

### 3.6 Agent tooling at the browser / devtools layer

- **Agent React DevTools — Callstack**. Gives AI agents access to React internals. Very much your wheelhouse.
- **Architecture Diagram Generator — Cocoon AI**. Auto-diagramming as an agent skill.
- **Code Review Graph** + **tirth8205 code-review-graph**. Graph-structured review.
- **IA en Entreprise - 10 Questions** (FR) + **REX IA Générative chez Fairplayer** (FR). French-language enterprise + retrospective pieces.

## 4. Where the three themes meet

The convergence point is: **a well-designed coding agent is a second brain on wheels, and the quality of its memory is the quality of the brain**.

- Memory infrastructure (MCP Memory / ByteRover / Claude Mem) → *the substrate*.
- Obsidian-based second brains → *the human-readable layer where the agent reads/writes*.
- Team brains (Enterprise Second Brain, Company Second Brain article, AGENTS.md, Encoding Team Standards) → *shared substrate across humans and agents*.
- Agent frameworks + design patterns → *the thing that actually reads/writes the substrate intelligently*.

If you want the shortest useful reading list out of this:
1. Agentic Design Patterns Ch. 8 (memory) and Appendix G (coding agents)
2. Obsidian Mind (repo)
3. Company Second Brain - AI Agent Teams (iwoszapar)
4. ByteRover CLI
5. Harness Engineering (Fowler) + Harness Design for Long-Running Apps (Anthropic)
6. Encoding Team Standards for AI (Fowler)

Those six cover: theory, individual tool, team pattern, distribution runtime, the envelope around long-running agents, and the team-standards side.
