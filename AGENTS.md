# AGENTS.md — rules for any AI working in this vault

These rules apply to any AI assistant (Claude Code, Cowork, Cursor, Aider, etc.) operating inside this Obsidian vault. Read this first, then `PROFIL.md` for context about Jerem.

## Vault organization (PARA)

Top-level folders are numbered for sort order. See `PARA-README.md` for the full convention. Summary:

- `0-Inbox/` — capture queue, nothing stays here >2 weeks
- `1-Projects/` — outcome + deadline (one folder per project)
- `2-Areas/` — ongoing responsibilities (no end date)
- `3-Resources/` — reference material, organized by topic (14 subfolders)
- `4-Archive/` — inactive items from the other three

### Placement decision tree (new note)

1. Is this driving a specific outcome by a specific date? → `1-Projects/<that project>/`
2. Is this maintaining an ongoing responsibility (health, career, finances)? → `2-Areas/<that area>/`
3. Is this reference / inspiration / something I might want later? → `3-Resources/<topic>/`
4. Unsure or batch-imported? → `0-Inbox/` and triage later

Never invent new top-level folders. If something genuinely doesn't fit, ask.

### Available 3-Resources/ topics

`AI & Agents`, `Architecture & Engineering`, `Claude & Skills`, `DevTools & Productivity`, `Frontend & Web Dev`, `Frontend Masters`, `Learning Resources`, `Open Source Projects`, `Product Management`, `Prompts & Context Engineering`, `Security`, `Shopping & Lifestyle`, `Testing & QA`, `UX & Design`.

When categorizing a new resource, pick from this list. If a note is borderline between two categories, prefer the more specific one. If genuinely no fit, ask before creating a new folder.

## Fiche format (canonical, for any captured veille note)

Every note saved from a URL must follow this format. It matches the ~700 existing notes in `3-Resources/`.

```markdown
---
url: <full URL>
category: "<one of the 14 topics above>"
tags:
  - <kebab-case-tag>
  - <kebab-case-tag>
  - <kebab-case-tag>
date_saved: "<YYYY-MM-DD>"
---

# <Title> - <Source/Author>

**Source:** [<host or author>](<URL>)
**Category:** <category>

## Summary
<2-4 sentences in your own words. Substantially original — do not paraphrase the source closely.>

## Why It's Interesting
<1-2 sentences. Why does this matter to Jerem specifically? Tie to his interests if relevant.>

## Key Takeaways
- <bullet 1>
- <bullet 2>
- <bullet 3>
<3-5 bullets, factual, in your own words>

## Tags

#tag1, #tag2, #tag3
```

### Filename convention

`<Title> - <Source>.md` works well (e.g., `How to Build an Agent - Ampcode.md`). Keep it readable in Obsidian's file list. Avoid leading articles ("The", "A"). No emojis in filenames.

### Tag conventions

- kebab-case in YAML, hashtag form at the bottom
- Reuse existing tags where possible (grep the relevant folder first)
- 3-6 tags per note, no more

## Copyright rules for summaries

Veille fiches summarize external sources. Strict rules:

- Summaries must be substantially original — no copy-paste, no lifted sentences
- One direct quote per fiche, max, in quotation marks, max ~15 words, only if it adds real value
- Never reproduce song lyrics, code snippets longer than 5 lines from copyrighted material, or large blocks of prose
- The summary should be much shorter than the source

## Communication style for Jerem

See `PROFIL.md` for full preferences. Defaults:

- Concise, direct, no fluff
- Plain prose over bullet lists in conversational replies (use bullets only when listing distinct items)
- No emojis unless he uses them first
- Mixed French/English is fine — match the language he's writing in
- He values being told *why* a choice was made, not just *what* the choice is
- Push back when his instruction is technically wrong or has a better alternative — he prefers honest correction over agreement

## When to ask vs. act

Ask before:
- Deleting or archiving anything
- Creating a new top-level folder (none beyond PARA)
- Pushing to git (commit is fine without asking, push requires confirmation if it's a public repo)
- Spending >5 web fetches on a single task
- Changing existing file structure or renaming many files

Don't ask:
- Creating new files in the right PARA folder
- Editing files Jerem just sent you to edit
- Running git status/diff/log
- Reading any file in the vault

## Working with the inbox

When Jerem says "process my inbox", "process veille", or similar:
- Run the `process-veille` skill (`.claude/skills/process-veille/SKILL.md`)
- See the skill for full behavior

## Things Jerem doesn't want

- Emojis in commit messages, notes, or chat
- Over-explanation after a task is done — link to the file, one or two lines, stop
- Markdown-heavy responses in conversation (headers, bold, nested bullets) — only when the structure earns it
- Sycophancy ("great question", "you're absolutely right")
- Auto-promoting borderline things to projects/areas
