# PARA — How this vault is organized

Based on [Tiago Forte's PARA method](https://fortelabs.com/blog/para/). The principle: organize by **actionability**, not by topic.

## The four folders

### `1-Projects/`
Short-term efforts with a **specific outcome** and an **end date**. Each project gets its own subfolder.

- "Outcome + deadline" is the test. "Learn Rust" is not a project. "Ship `cli-tool` v0.1 by July 15" is a project.
- When a project completes (or is abandoned), move its folder to `4-Archive/`.

Currently empty — populate as projects arise.

### `2-Areas/`
**Ongoing responsibilities** you maintain indefinitely. No end date. The standard is "this should always be in good shape," not "this should reach a goal."

- `Fitness & Health/` — personal health, ongoing
- `Career & Hiring/` — career management, job-hunting material

### `3-Resources/`
Topics of **ongoing interest** for reference and inspiration. Most of your veille lives here.

- `AI & Agents/`
- `Architecture & Engineering/`
- `Claude & Skills/`
- `DevTools & Productivity/`
- `Frontend & Web Dev/`
- `Frontend Masters/`
- `Learning Resources/`
- `Open Source Projects/`
- `Product Management/`
- `Prompts & Context Engineering/`
- `Security/`
- `Shopping & Lifestyle/`
- `Testing & QA/`
- `UX & Design/`

### `4-Archive/`
Inactive items from the other three buckets. Don't delete — archive. You'll thank yourself later when you need to dig something up.

## Workflow

1. **New note?** Ask: is this driving a specific outcome by a date? → `1-Projects/<that project>/`. Otherwise: is this an ongoing responsibility I maintain? → `2-Areas/<area>/`. Otherwise: it's reference → `3-Resources/<topic>/`.
2. **Project ends.** Move the whole subfolder to `4-Archive/`.
3. **Area no longer relevant.** Same — move to `4-Archive/`.
4. **Resource becomes a project** (you decide to act on it). Move/copy into the relevant project folder.

## The numbering

The `1-`, `2-`, `3-`, `4-` prefixes are intentional: Finder, Obsidian, and most file browsers sort lexically, so the prefixes keep the folders in actionable-first order (most actionable at top).
