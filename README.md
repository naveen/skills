# skills

A small marketplace of [Claude Code](https://docs.claude.com/en/docs/claude-code) skills I use day to day. Install any of them in two commands.

## Install

In Claude Code, add this marketplace once:

```
/plugin marketplace add naveen/skills
```

Then install whichever skills you want:

```
/plugin install clipboard@skills
/plugin install recap@skills
```

That's it — the skills load on their trigger phrases (below) or when you type the matching `/` command. To update later: `/plugin marketplace update skills`.

## Skills

### 📋 clipboard

Copy content you just generated straight to the macOS clipboard via `pbcopy`, formatted for clean pasting — no mouse-highlighting, no stray indentation, no literal markdown asterisks.

- **Trigger:** `/clipboard`, "copy that", "pbcopy this", "put that on my clipboard"
- **Modes:** plain-text (default, strips markdown), `commit` (dedented, ready for `git commit`), `raw` (markdown intact)
- **Platform:** macOS only (`pbcopy`)

### 📌 recap

Reload the state of a project into a short, scannable brief — what landed, what's in flight, what's next — synthesized from git, design/roadmap docs, and open PRs. Not a commit-log dump.

- **Trigger:** `/recap`, "where do things stand", "catch me up", "what's the status here"
- **Scoped:** `/recap <topic>` narrows to one thread of work
- **Read-only:** observes and reports; never commits, pushes, or changes branches

## Manual install (without plugins)

```bash
git clone https://github.com/naveen/skills ~/.claude/naveen-skills
ln -s ~/.claude/naveen-skills/plugins/clipboard/skills/clipboard ~/.claude/skills/clipboard
ln -s ~/.claude/naveen-skills/plugins/recap/skills/recap ~/.claude/skills/recap
```

## Repo layout

```
skills/
├─ .claude-plugin/marketplace.json   # lists the plugins below
└─ plugins/
   ├─ clipboard/
   │  ├─ .claude-plugin/plugin.json
   │  └─ skills/clipboard/SKILL.md
   └─ recap/
      ├─ .claude-plugin/plugin.json
      └─ skills/recap/SKILL.md
```

## License

MIT
