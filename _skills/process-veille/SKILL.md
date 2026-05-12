---
name: process-veille
description: Drain 0-Inbox/urls-to-process.md by fetching each URL, generating a structured fiche in Jerem's canonical format, and filing it under the right 3-Resources/<category>/ folder. Use this skill whenever Jerem says "process my inbox", "process veille", "drain the queue", or similar.
---

# process-veille

Drains the URL capture queue at `0-Inbox/urls-to-process.md`, producing one fiche per URL in the appropriate `3-Resources/<category>/` folder.

## Read first

Before processing anything, read these files for context:
1. `AGENTS.md` — fiche format spec, category list, copyright rules
2. `PROFIL.md` — who Jerem is, so "Why It's Interesting" can be tied to his interests

## Procedure

### 1. Parse the queue

Read `0-Inbox/urls-to-process.md`. Extract URLs from the "Queue" section (lines that start with `http://` or `https://`). Lines may have an optional hint after the URL — capture it.

If queue is empty, report "Queue is empty, nothing to process." and stop.

### 2. For each URL

For each URL in order:

#### a. Fetch the page

Use `WebFetch` or equivalent. If the fetch fails, note the URL as failed and continue to the next.

#### b. Determine category

Available categories (from `3-Resources/`):
- AI & Agents
- Architecture & Engineering
- Claude & Skills
- DevTools & Productivity
- Frontend & Web Dev
- Frontend Masters
- Learning Resources
- Open Source Projects
- Product Management
- Prompts & Context Engineering
- Security
- Shopping & Lifestyle
- Testing & QA
- UX & Design

Routing rules:
- If the URL has a hint in the queue, use it as the strongest signal
- Otherwise infer from the page content + URL host
- If borderline (could fit 2 categories), pick the more specific one
- If genuinely no fit, **stop and ask Jerem** which category to use (don't invent new folders)

#### c. Check for duplicates

Before writing, check if a fiche with this URL already exists. Quick way: grep `3-Resources/<chosen-category>/` for the URL in YAML frontmatter. If found, skip this URL and report it as "already captured".

#### d. Generate the fiche

Use this exact format (matches the ~700 existing notes):

```markdown
---
url: <full URL>
category: "<chosen category>"
tags:
  - <kebab-case-tag>
  - <kebab-case-tag>
  - <kebab-case-tag>
date_saved: "<today, YYYY-MM-DD>"
---

# <Title> - <Source/Author>

**Source:** [<host or author>](<URL>)
**Category:** <category>

## Summary
<2-4 sentences in your own words. Substantially original — no copy-paste from the source.>

## Why It's Interesting
<1-2 sentences. Connect to Jerem's interests where relevant (see PROFIL.md).>

## Key Takeaways
- <bullet 1, factual, your own words>
- <bullet 2>
- <bullet 3>
<3-5 bullets>

## Tags

#tag1, #tag2, #tag3
```

#### e. Title and filename rules

- Filename: `<Title> - <Source>.md` (e.g., `How to Build an Agent - Ampcode.md`)
- Strip leading articles ("The", "A") from titles when natural
- No emojis in filenames or titles
- Keep titles under ~80 chars
- The Source is the publisher/blog/author — short form

#### f. Tag rules

- 3-6 tags, kebab-case in YAML
- Hashtag form repeated in the bottom Tags section
- **Reuse existing tags where possible** — before inventing a tag, grep the chosen category folder for similar tags
- Tags should describe *what the note is about*, not generic ("article", "blog" are bad; "agent-architecture", "vitest", "css-container-queries" are good)

#### g. Copyright / originality rules

This is non-negotiable (also in `AGENTS.md`):

- Summaries must be substantially original. No paraphrasing close to the source.
- One direct quote per fiche maximum, in quotation marks, <15 words, only if it's pivotal
- Never reproduce code blocks longer than 5 lines from the source
- Summary should be significantly shorter than the source — capture the essence, not the content

#### h. Write the file

Write to `3-Resources/<category>/<filename>.md`. Do not overwrite existing files (de-dupe should have caught this; if a clash sneaks through, append a short disambiguator like ` (2)` and flag in the final report).

### 3. Drain the queue

After processing all URLs successfully, rewrite `0-Inbox/urls-to-process.md` with the queue section emptied (keep the header and instructions).

URLs that **failed** (couldn't fetch, no category match, dupe) should stay in the queue with a comment marker explaining why, so Jerem can triage:

```
https://example.com/article  # FAILED: fetch returned 403
https://example.com/other    # SKIPPED: already captured at 3-Resources/Security/Other - Example.md
```

### 4. Report

Print a concise summary:

```
Processed N URLs:
  [ok]   <count> filed
  [skip] <count> skipped (dupes)
  [fail] <count> failed (see queue for details)

Filed:
  - 3-Resources/<category>/<filename>.md
  - 3-Resources/<category>/<filename>.md
```

No emojis anywhere — neither in fiches nor in the report. Jerem dislikes them.

## Don'ts

- Don't ask Jerem questions mid-processing if you can avoid it. Batch all questions and ask at the start or end.
- Don't write to `3-Resources/` if the URL is clearly about a project Jerem is actively working on (e.g., the URL is about Godot puzzle mechanics and `1-Projects/Le Tombeau/` exists) — in that case, ask whether to file in the project folder instead.
- Don't change existing files.
- Don't `git add` or commit. Jerem does that.

## Edge cases

- **Paywalled article:** Note the title + URL in the summary, mark that full content wasn't accessible, still file it (it's a reference for later).
- **YouTube / video:** Extract title and description, summarize from those.
- **GitHub repo:** Fetch the README, summarize the project, tag with the language/domain.
- **Tweet/X post:** If accessible, summarize. If not, note the author and topic from URL.
- **PDF / non-HTML:** Try to fetch and read; if you can't parse, note the URL + title in a stub fiche and flag for manual review.

## Invoking this skill

Three ways to use this skill:

1. **From this Cowork chat:** Say "process my inbox" or "drain the veille queue" — I'll read this file and execute.
2. **From Claude Code (terminal):** `cd ~/Documents/Second-Brain && claude` then "process veille".
3. **As a global Claude Code skill:** Symlink this folder into `~/.claude/skills/`:

   ```
   ln -s ~/Documents/Second-Brain/_skills/process-veille ~/.claude/skills/process-veille
   ```

   Then any Claude Code session anywhere will know about it.
