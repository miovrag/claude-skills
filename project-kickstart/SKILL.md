---
name: project-kickstart
description: >
  Use this skill whenever the user wants to start working on a development task, feature, or bug fix.
  Triggers include: "hajde da radimo na X", "kreni na task", "počni projekat", "setup za X", "treba mi kontekst za zadatak",
  "poveži se na repo", "pokupi karticu", or any phrase suggesting the user is about to begin work on a specific ticket or feature.
  This skill orchestrates the full project kickstart: asks which GitHub repo and which task source (BugHerd and/or Trello) to use,
  pulls repo structure and relevant files, loads the BugHerd task and/or Trello card context (title, description, checklist, comments),
  connects to the CustomGPT.ai MCP, and synthesizes a ready-to-work context summary.
  Always use this skill at the start of a work session — don't just answer inline.
---

# Project Kickstart Skill

Orchestrates the full setup needed before starting work on a development task:
pulls GitHub repo context, loads the Trello card, connects CustomGPT.ai MCP, and
produces a synthesized "ready to work" context summary.

---

## Step 1 — Gather inputs

Ask the user for the following (can be in one message):

1. **GitHub repo** — full name or URL (e.g. `customgpt-ai/customgpt-web`)
2. **Task reference** — from either or both sources:
   - **BugHerd** — task ID, or project name + task title
   - **Trello** — card URL or card name/ID
3. **Branch** (optional) — defaults to `main` if not specified

If the user references both a BugHerd task and a Trello card, load both and merge their context in the summary.
If the user already provided any of these in the conversation, use those values — don't re-ask.

---

## Step 2 — Load GitHub repo context

Use the GitHub MCP or GitHub API via web_fetch/web_search to:

1. Fetch the **repo tree** (top-level + `src/` or `app/` if present) to understand structure
2. Read key context files if they exist:
   - `README.md`
   - `CONTRIBUTING.md` or `docs/contributing.md`
   - Package manifest (`package.json`, `pyproject.toml`, `Cargo.toml`, etc.)
3. If the user mentioned a specific component or area, navigate to that directory and read relevant files

**Goal**: understand the stack, conventions, and relevant code area for the task.

---

## Step 3 — Load task context (BugHerd and/or Trello)

Load whichever source(s) the user referenced. If they named both, load both and merge.

### BugHerd

The BugHerd MCP is connected. Load its tools via `tool_search` with query `"bugherd task"`. Useful tools:
- `mcp__BugHerd__list_projects` — resolve a project name → project ID
- `mcp__BugHerd__list_project_tasks` / `mcp__BugHerd__list_all_tasks` — find the task when only a name/title is known
- `mcp__BugHerd__get_task_details` — full task once the ID is known

Extract: **title**, **description**, **status / column**, **assignee(s)**, **tags**, **due date**, and the **latest comments** (last 3–5, via the task details / comment data). If only a project name is given, list its tasks and ask the user which one.

### Trello

Use the Trello MCP (search via `tool_search` / `search_mcp_registry` with keywords `["trello", "cards"]`) or `web_fetch` the card URL directly.

Extract: **title**, **description** (full), **checklist items**, **latest comments** (last 3–5, most relevant), **labels / due date**.

### Fallbacks

If a referenced source's MCP is unavailable, ask the user to paste that task/card's content manually, and note the fallback in the summary.

---

## Step 4 — Connect CustomGPT.ai MCP

Load the CustomGPT.ai MCP tool via `tool_search` with query `"customgpt knowledge base"`.

Use it to query relevant context for the task. Good queries to run:
- The task/feature name from the Trello card title
- Key technical terms from the card description
- Component or module name if identifiable from the repo

Retrieve up to 3 relevant knowledge base results and include summaries in the final context.

---

## Step 5 — Synthesize context summary

Produce a structured **"Ready to Work" context block** in this format:

```
## 🚀 Project Kickstart — [Card Title]

### Task — [BugHerd #ID / Trello card]
[1–3 sentence summary of what needs to be done, based on the BugHerd task and/or Trello card]

### Repo: [repo-name] ([branch])
- Stack: [e.g. Next.js 14, TypeScript, Tailwind]
- Relevant area: [e.g. `src/components/ChatWidget/`]
- Key conventions: [e.g. uses Zustand for state, shadcn/ui components]

### Checklist
- [ ] Item 1
- [ ] Item 2
...

### CustomGPT.ai Knowledge Base
- [Relevant finding 1 — source]
- [Relevant finding 2 — source]

### Notes / Comments
[Relevant context from BugHerd and/or Trello comments or description extras]

---
Ready. Šta prvo radimo?
```

---

## Error handling

- **GitHub repo not accessible**: Ask user to confirm visibility (public/private) and whether they're authenticated
- **BugHerd task not found**: List the project's tasks so the user can pick, or ask them to paste the task content
- **Trello card not found**: Ask user to paste card content manually
- **No task source given**: Ask whether the task lives in BugHerd, Trello, or both
- **CustomGPT MCP unavailable**: Skip that step and note it in the summary
- **No branch specified**: Use `main`, note assumption in output

---

## Notes

- Keep the context summary **scannable** — use headers and bullets, not paragraphs
- Don't dump raw file contents into the summary — extract the relevant parts
- If the repo is large, focus on the directory most relevant to the task
- CustomGPT.ai MCP URL: `https://mcp.customgpt.ai/projects/1403/sse`
