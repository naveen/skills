---
name: clipboard
description: Copy content you just generated to the macOS clipboard via pbcopy, formatted for clean pasting. Use when the user types /clipboard, asks to "copy that", "pbcopy this", or wants the last commit message / blurb / snippet on their clipboard ready to paste. Strips markdown and leading indentation by default so chat apps and git commits paste cleanly.
---

# Clipboard

Pipe content the user just asked for to `pbcopy` so they can paste it without using the mouse. The point is to avoid mouse-highlighting, which drags in leading whitespace from code blocks and renders markdown asterisks literally.

## What to copy

- **Default target:** the most recent content *you generated* in this conversation (the blurb, message, snippet, or commit message just discussed) — not the user's prompt, not surrounding explanation.
- **Targeted:** if the user names something (`/clipboard the bash command above`, `/clipboard the second option`), copy that specific thing.
- If it's genuinely ambiguous which artifact they mean, ask one short question. Otherwise just copy and report what you copied.

## Formatting modes

Pick based on the content and any flag the user gives:

### Default — chat / plain-text paste
For messages, blurbs, announcements pasted into Slack/iMessage/email:
- Strip markdown emphasis: remove `**bold**`, `*italic*`, `` `code` `` backticks, heading `#` markers.
- Keep the text and line breaks readable; keep list content but drop heavy markdown syntax.
- No leading indentation.

### Commit message — `/clipboard commit` or when the content is a git commit message
Copy exactly what should go into `git commit` — nothing else:
- **Dedent every line** so there is zero leading whitespace (this is the whole reason the user reaches for this — code-block indentation otherwise comes along).
- No surrounding prose, no code fences, no markdown.
- Preserve the subject line, the blank line after it, and the body as-is.
- Do not add a trailing `Co-Authored-By` line unless it was already part of the message you wrote.

### Raw — `/clipboard raw`
Copy the content verbatim *with* markdown intact (for pasting into docs, GitHub, Notion). Still strip code-block leading indentation if the content was shown inside a fence.

## How to copy

Use a quoted heredoc so nothing is shell-expanded:

```bash
pbcopy <<'EOF'
<the exact content>
EOF
```

`pbcopy` is macOS-only and takes no arguments — content comes via stdin. This skill assumes macOS (the user is on darwin).

## After copying

Confirm in one line: what you copied and which mode (e.g. "Copied the commit message to your clipboard, dedented and ready to paste."). Don't re-print the full content unless the user asks.

## Scope guard

This skill is one-directional: take what was just generated → format → copy. Don't read the existing clipboard, don't build a general clipboard manager. If the user wants something transformed, generate it normally first, then this copies it.
