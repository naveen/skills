---
name: land
description: Finish a workspace cleanly — verify tests pass, generate the commit and PR body, push, open a PR against origin/main, and drop a one-line summary into .context so sibling agents see it landed. Use when the user types /land, says "land this", "wrap this up", "ship it", "open the PR", or otherwise signals the work in this worktree is done and ready to go up. The bookend to /recap.
---

# Land

Take a worktree from "work is done" to "PR is open against `origin/main` and the fleet knows." This is the closing move of a Conductor workspace — the bookend to `/recap`. Do the mechanical dance the user does by hand every time: verify, commit, push, PR, note it.

The discipline is the same as `clipboard`: one direction, one outcome. Read the diff, produce one PR. Don't refactor, don't keep coding, don't clean up unrelated things on the way out.

## The sequence

Run these in order. Each gate protects the next — don't push a failing build, don't PR an empty diff.

### 1. Confirm there's something to land

- `git status` and `git diff origin/main...HEAD --stat` (three dots — changes on this branch since it forked from main).
- If the working tree is clean **and** the branch is already even with `origin/main`, there's nothing to do — say so and stop.
- Never land from `main` (or the repo's default branch). If `HEAD` is on it, stop and tell the user to branch first. In a Conductor worktree you're normally on a task branch already.

### 2. Verify tests pass

- Detect the project's test command rather than assuming: look for `package.json` scripts (`test`), `Makefile` targets, `justfile`, `pyproject.toml`/`pytest`, `cargo test`, `go test ./...`, etc. Prefer a script the repo already defines over inventing one.
- Run it. Also run the linter/typecheck if there's an obvious one (`lint`, `typecheck`, `tsc --noEmit`) — cheap and catches what tests miss.
- **If tests fail:** stop. Report the failure plainly with the relevant output. Do not commit or push. Ask whether to fix or land anyway — landing red is the user's call to make explicitly, never the default.
- **If there's no test setup at all:** say so in one line and continue — absence of tests isn't a failure, but the user should know you didn't verify anything.

### 3. Generate the commit

- Stage the intended changes. Review what's staged — don't blindly `git add -A` over stray files, build artifacts, or `.env`. Flag anything surprising before committing.
- Write a real commit message from the *diff*, not from memory: a concise imperative subject line, a blank line, then a short body covering what changed and why when it isn't obvious. Match the repo's existing commit style (skim `git log`).
- End the message with the required trailer:

  ```
  Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>
  ```
- If there are already commits on the branch, prefer one clean additional commit over rewriting history. Don't squash or force-push unless the user asks.

### 4. Push

- Push the current branch to its remote, setting upstream if needed (`git push -u origin HEAD`).
- If the push is rejected because `origin/main`-derived history moved, don't force it — surface the conflict and let the user decide (this is the parallel-worktree footgun; a sibling agent may have landed first).

### 5. Open the PR against main

- Use `gh pr create --base main` (the target branch for these workspaces is `origin/main`).
- **Title:** the commit subject, or a tighter summary if the branch has several commits.
- **Body:** written from the full branch diff (`git diff origin/main...HEAD`) — what changed, why, and any test/verification notes. Tight and skimmable; no filler.
- End the PR body with:

  ```
  🤖 Generated with [Claude Code](https://claude.com/claude-code)
  ```
- If a PR for this branch already exists, update it (push already did the commits; refresh the body if the scope changed) rather than opening a duplicate. Report its URL.

### 6. Note it in .context

- Append a one-line, dated entry to `.context/landed.md` (create the file if absent) so sibling agents running in parallel worktrees see this shipped:

  ```
  - 2026-09-08 — <branch>: <one-line summary> → <PR url>
  ```
- Keep it to one line. `.context/` is Conductor's cross-agent scratch dir; this is a signal, not a changelog.

## What to report back

End with a tight status the user can act on — not a replay of every command:

- ✅/❌ tests (and lint) result
- the commit subject
- the PR URL
- the `.context/landed.md` line you added

One screen. If anything gated (tests failed, nothing to land, push rejected), lead with that.

## Guards

- **Never land red silently.** Failing tests stop the sequence; landing anyway is an explicit user decision.
- **Never force-push or rewrite shared history** unless the user asks. Parallel worktrees share the remote — a force-push can clobber a sibling's work.
- **Never touch the git stash stack.** It's shared across all worktrees. If you must set something aside, use a tagged WIP commit.
- **Stay in scope.** `land` closes out the work that's already here. It doesn't add features, fix unrelated lint, or reformat files the branch didn't touch. If you notice something, mention it — don't fix it on the way out.
- **`origin/main` is the base**, per the Conductor workspace target. Don't retarget the PR elsewhere unless the user says so.
