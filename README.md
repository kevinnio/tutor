# TuTor

**English** · [Español](README-es.md)

An **agent skill** that turns your coding assistant into a learning tutor. TuTor plans work in small steps, explains every command, verifies your progress with read-only checks, and refuses to complete the assignment for you.

Use it when you want to **learn by doing**—homework, tutorials, walkthroughs, or any time you say “don’t do it for me.”

## Objective

TuTor helps **teachers and students** use AI as a coach, not a cheat sheet. Today’s coding assistants often hand over finished answers and skip the thinking, debugging, and trial-and-error where real learning happens. This skill redirects that power toward **learning by doing**: the learner writes the code and runs the commands; the agent plans steps, explains tools, and verifies progress—without doing the work for them. The aim is deeper understanding in class, in study groups, and on personal projects—not faster copy-paste.


## What it does

| Behavior | Detail |
|----------|--------|
| **Step-by-step** | Outlines a numbered plan up front; gives **one** action per turn (no full solution). |
| **Verification** | After you say you’re done, the agent checks with read-only tools (`Read`, `ls`, `cat`, `git status`, `git diff`, tests you ran). |
| **Tool gate** | The agent may inspect your work but must **not** create, edit, or run commands that build your deliverable for you. |
| **Command teaching** | Every shell command includes a breakdown: goal, pieces, and what success looks like. |

Optional deep dives live in [`references/`](references/):

- [`references/example-flow.md`](references/example-flow.md) — greenfield app walkthrough
- [`references/tool-gate.md`](references/tool-gate.md) — which shell commands are allowed

## Install

**Repository:** [github.com/kevinnio/tutor](https://github.com/kevinnio/tutor)

TuTor ships as `SKILL.md` plus `references/`. The frontmatter `name` must be `tutor`.

### Install with Skills CLI (recommended)

Use the [Skills CLI](https://github.com/vercel-labs/skills) ([skills.sh](https://skills.sh)) to install into your coding agents. It detects which tools you have and writes to the correct skill directories.

**User-wide (all projects)** — recommended for your first install:

```bash
npx skills add kevinnio/tutor -g -y
```

**This project only** (share with a class or team via git):

```bash
npx skills add kevinnio/tutor -y
```

**Pick specific agents** (skip the interactive agent list):

```bash
npx skills add kevinnio/tutor -g -a cursor -a claude-code -a opencode -y
```

| Flag | Meaning |
|------|---------|
| `-g`, `--global` | User-wide install (`~/…/skills/`) |
| (no `-g`) | Install into the current project only |
| `-a`, `--agent` | Target agent(s), e.g. `cursor`, `claude-code`, `opencode`, `codex`, `windsurf` |
| `-y`, `--yes` | Non-interactive |

Preview before installing: `npx skills add kevinnio/tutor --list`. Full agent list: [vercel-labs/skills](https://github.com/vercel-labs/skills#supported-agents).

After installing, start a **new agent session**.

### Per-agent usage

- **Cursor** — Ask to learn something (e.g. “Teach me to build a todo CLI—don’t write it for me”) or use `/tutor` if available. [Cursor Agent Skills](https://cursor.com/docs/context/skills).
- **Claude Code** — Run `/tutor` or describe a learning goal. [Claude Code skills](https://code.claude.com/docs/en/skills).
- **OpenCode** — Describe a learning goal; OpenCode loads skills on demand. [OpenCode Agent Skills](https://opencode.ai/docs/skills/).

### Manual install (git clone)

If you prefer not to use the CLI, clone the repo into a skill folder. Paths vary by agent; common examples:

| Agent | User-wide | This project |
|-------|-----------|--------------|
| [Cursor](https://cursor.com) | `~/.cursor/skills/tutor` | `.cursor/skills/tutor` or `.agents/skills/tutor` |
| [Claude Code](https://code.claude.com) | `~/.claude/skills/tutor` | `.claude/skills/tutor` |
| [OpenCode](https://opencode.ai) | `~/.config/opencode/skills/tutor` | `.opencode/skills/tutor` |

```bash
REPO=https://github.com/kevinnio/tutor.git
TARGET=~/.cursor/skills/tutor   # user-wide example

mkdir -p "$(dirname "$TARGET")"
git clone "$REPO" "$TARGET"
```

Do not install under `~/.cursor/skills-cursor/` (reserved by Cursor). If `TARGET` already exists, remove it or use [Upgrade](#upgrade) instead of cloning again.

### Symlink for local development

```bash
SKILL_ROOT="$HOME/code/tutor"
TARGET="$HOME/.cursor/skills/tutor"

mkdir -p "$TARGET"
ln -sf "$SKILL_ROOT/SKILL.md" "$TARGET/SKILL.md"
ln -sf "$SKILL_ROOT/references" "$TARGET/references"
```

### Install TuTor via TuTor

As a fun exercise, have TuTor install itself with you—you run every command; it coaches and checks your work. You’ll learn how skills are installed on disk, and once it’s set up you can always ask TuTor for help on real tasks.

Paste this into a coding agent that can read URLs (new chat).

```text
TuTor, install yourself with me—I run every command; you tutor.

Read and follow for this entire session:

- SKILL.md: https://raw.githubusercontent.com/kevinnio/tutor/master/SKILL.md
- README Install section: https://raw.githubusercontent.com/kevinnio/tutor/master/README.md

Follow TuTor’s rules: one step at a time, command breakdowns, verify after I say "done".
Never install for me (no writing skill files, no git clone, no npx on my behalf)—refuse "just do it."

First, ask me which coding agent I use (e.g. Cursor, Claude Code, OpenCode, Codex, Windsurf)
and whether I want a user-wide install or project-local. Use the correct skills path for that agent.

Goal: `tutor` skill installed on my machine.
Teach `npx skills add kevinnio/tutor -g -y` unless I want manual git clone.

After I answer, give a short numbered plan for my agent and scope, then Step 1 and stop.
```

## Upgrade

### Skills CLI (recommended)

```bash
npx skills update tutor -g -y    # user-wide
npx skills update tutor -y       # this project only
```

List installed copies: `npx skills list | grep tutor`

### Manual git clone

```bash
cd ~/.cursor/skills/tutor   # your install path
git pull origin master
```

For a **git submodule**: `git submodule update --remote path/to/tutor`

### Check your installed version

```bash
grep '^  version:' ~/.cursor/skills/tutor/SKILL.md
# path varies — use npx skills list to find installs
```

Compare with `metadata.version` in [SKILL.md](SKILL.md) on GitHub.

### After upgrading

1. **Restart or start a new agent session** so the agent reloads the skill.
2. **Confirm the version** with the `grep` command above.
3. If behavior is unchanged, run `npx skills list` and update every scope (user-wide vs project) where `tutor` is installed.

## How to use

1. **Install** the skill (see above) and open your project in the agent.
2. **Say what you want to learn**, and that the agent should tutor—not implement—for you.

Example prompts:

```text
I want to learn how to add tests to this repo. Walk me through it step by step; don't edit files for me.

Enséñame a crear un API REST con Express. No hagas el código por mí.

Help me fix this failing test, but only give hints and commands—I run everything myself.
```

3. **Do each step** the tutor assigns, then reply when done (e.g. “done”, “listo”).
4. The tutor **verifies** before moving on. If something is wrong, it tells you what’s missing—no skipping ahead.
5. If the agent tries to do your work, remind it: *“Follow the tutor skill—verify only, I run the commands.”*

## Repository layout

```text
tutor/
├── SKILL.md              # Main skill (required)
├── references/           # Optional deep reference for the agent
│   ├── example-flow.md
│   └── tool-gate.md
├── README.md
├── README-es.md
├── AGENTS.md             # Instructions for agents editing this repo
├── AUTHORS.md            # Contributors (names and GitHub handles)
└── LICENSE               # MIT
```

Version is declared in `SKILL.md` frontmatter (`metadata.version`, currently **0.4**).

## Contribute

We welcome feedback and collaboration.

- **Bug reports and feature requests** — [open a GitHub issue](https://github.com/kevinnio/tutor/issues/new). Describe what you expected, what happened, and which agent you use.
- **Code and docs** — send a [pull request](https://github.com/kevinnio/tutor/compare). Great fits: clearer teaching patterns, tool-gate edge cases, README translations, and install notes for new agents.

### Pull request workflow

1. **Fork** the repository and create a branch (`git checkout -b fix/tool-gate-example`).
2. **Change** `SKILL.md` and/or files under `references/`. Keep the skill focused; avoid bloating the main file—put long examples in `references/`.
3. **Test** by installing your branch into one agent (Cursor, Claude Code, or OpenCode) and running through a short learning task.
4. **Open a pull request** with:
   - What behavior changed and why
   - Which agent(s) you tested
   - Any breaking change to install paths or skill name (`tutor` must match the folder name for OpenCode)

Please do not commit secrets, personal paths, or generated `node_modules` / playground artifacts.

## Contributors

<a href="https://github.com/kevinnio/tutor/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=kevinnio/tutor&columns=3" alt="Contributors" />
</a>

Avatars are generated from [GitHub contributors](https://github.com/kevinnio/tutor/graphs/contributors) via [contrib.rocks](https://contrib.rocks) (contributors-img). Names and handles: [AUTHORS.md](AUTHORS.md).

## Donations

If TuTor helps you learn, you can buy me a coffee via PayPal:

**[paypal.me/kevindperezm](https://paypal.me/kevindperezm?locale.x=es_XC&country.x=MX)**

Donations are optional and not required to use or contribute to this project.

## License

[MIT](LICENSE) — Copyright (c) 2026 Kevin Perez
