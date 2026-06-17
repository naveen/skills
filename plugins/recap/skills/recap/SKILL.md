---
name: recap
description: Summarize where a project stands — what it is, what landed recently, what's in flight, and what's next. Use when the user types /recap, asks "where do things stand", "catch me up", "what's the status here", or returns to a project after time away and needs to reload context. Pulls from git, design/roadmap docs, and open PRs; produces a scannable brief, not a commit log dump.
---

# Recap

Reload the state of a project into a short, scannable brief so the user (or a fresh agent) can pick up where things left off. The point is synthesis — answer "where are we and what's next", not "list every commit".

## What to gather

Pull from whatever the repo actually has; skip silently what it doesn't. In rough priority:

1. **Git reality** — current branch, divergence from `origin/main` (`git log --oneline origin/main..HEAD`), and working-tree status (staged/unstaged/untracked). This is ground truth for what's in flight *right now*.
2. **Recent history** — last ~10 commits (`git log --oneline -10`) to see the arc of recent work. Read the messages, don't just count them.
3. **Intent docs** — design/roadmap/planning files: `DESIGN.md`, `ROADMAP*`, `README`, `TODO*`, and anything under `.context/` (Conductor's cross-agent scratch dir). These say what's *meant* to happen next; git says what already did.
4. **Open PRs** — if `gh` is available, `gh pr list` (and the current branch's PR if any) for review state and what's awaiting merge.

Run the independent reads in parallel. Don't read entire large files — pull the roadmap/next-steps sections and skim the rest.

## What to produce

A tight brief, organized by these beats (drop any that are empty — don't pad):

- **What this is** — one line on the project's purpose, only if it isn't obvious or the user seems to be returning cold. Skip for a project you've clearly been working in this session.
- **Recently landed** — 2–5 bullets of what's been done, synthesized from commits (group related ones; don't transcribe). Past tense, outcome-focused.
- **In flight** — what's on the current branch but not merged, plus uncommitted working-tree changes. Be concrete about what's half-done.
- **What's next** — pulled from roadmap/design docs and any obvious gaps. This is the most valuable section; lead with it if the user asked "what should I do next".
- **Watch out for** — blockers, failing checks, open PR review comments, TODO/FIXME clusters, or anything mid-refactor. Only if real.

Keep it under ~15 lines for a normal project. Use `file_path:line` references so anything is one click away. No preamble, no "here's your recap" — just the brief.

## Scope

- **Default target:** the repo in the current working directory.
- If the user names another path or a Conductor workspace, recap that instead.
- `/recap <topic>` (e.g. `/recap the proactivity work`) — narrow to that thread: filter commits/docs/PRs to it rather than summarizing the whole project.

## Guards

- Read-only. This skill observes and reports — it never commits, pushes, edits, or changes branches. (The user runs git themselves.)
- Don't invent next-steps the docs and code don't support. If "what's next" is genuinely unclear, say so and point at where the answer would live (e.g. "no roadmap file — next steps aren't written down anywhere I can see").
- Don't dump raw `git log` or full diffs. If the user wants the raw log, they'll run git. The value here is the synthesis.
