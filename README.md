# TuTor

**English** · [Español](README-es.md)

A multilingual **agent skill** that turns your coding assistant into a learning tutor. TuTor plans work in small steps, explains every command, verifies your progress with read-only checks, and refuses to complete the assignment for you.

Use it when you want to **learn by doing**—homework, tutorials, walkthroughs, or any time you say “don’t do it for me.”

## What it does

| Behavior | Detail |
|----------|--------|
| **Step-by-step** | Outlines a numbered plan up front; gives **one** action per turn (no full solution). |
| **Verification** | After you say you’re done, the agent checks with read-only tools (`Read`, `ls`, `cat`, `git status`, `git diff`, tests you ran). |
| **Tool gate** | The agent may inspect your work but must **not** create, edit, or run commands that build your deliverable for you. |
| **Command teaching** | Every shell command includes a breakdown: goal, pieces, and what success looks like. |
| **Multilingual** | Explanations match your language; commands, paths, and code stay literal. |

Optional deep dives live in [`references/`](references/):

- [`references/example-flow.md`](references/example-flow.md) — greenfield app walkthrough
- [`references/tool-gate.md`](references/tool-gate.md) — which shell commands are allowed

## Install

TuTor is a folder with `SKILL.md` plus `references/`. The skill name in frontmatter is `tutor`, so the install directory should be named **`tutor`**.

### Quick install (any agent)

Clone this repository into a `tutor` skill folder (personal = all projects, project = repo-only):

```bash
# Personal (recommended)
git clone https://github.com/kevin-perez/tutor.git ~/.cursor/skills/tutor
```

For a **project-local** copy, use the same path under your repo (examples below).

After installing, start a new agent session so skills are picked up.

### [Cursor](https://cursor.com)

| Scope | Path |
|-------|------|
| Personal | `~/.cursor/skills/tutor/` |
| Project | `.cursor/skills/tutor/` |

```bash
git clone https://github.com/kevin-perez/tutor.git ~/.cursor/skills/tutor
```

In chat, ask to learn something (e.g. “Teach me to build a todo CLI—don’t write it for me”) or invoke the skill if your Cursor version exposes `/tutor`.

Docs: [Cursor Agent Skills](https://cursor.com/docs/context/skills).

### [Claude Code](https://code.claude.com)

| Scope | Path |
|-------|------|
| Personal | `~/.claude/skills/tutor/` |
| Project | `.claude/skills/tutor/` |

```bash
git clone https://github.com/kevin-perez/tutor.git ~/.claude/skills/tutor
```

Run `/tutor` or describe a learning goal; Claude loads the skill when relevant.

Docs: [Claude Code skills](https://code.claude.com/docs/en/skills).

### [OpenCode](https://opencode.ai)

| Scope | Path |
|-------|------|
| Project | `.opencode/skills/tutor/` |
| Global | `~/.config/opencode/skills/tutor/` |

OpenCode also discovers `.claude/skills/tutor/` and `.agents/skills/tutor/`.

```bash
# From your project root
mkdir -p .opencode/skills
git clone https://github.com/kevin-perez/tutor.git .opencode/skills/tutor
```

Restart OpenCode after adding a new skills directory.

Docs: [OpenCode Agent Skills](https://opencode.ai/docs/skills/).

### Other agents (`.agents/skills`)

Many tools that follow the [Agent Skills](https://agentskills.io) layout look under:

| Scope | Path |
|-------|------|
| Personal | `~/.agents/skills/tutor/` |
| Project | `.agents/skills/tutor/` |

```bash
git clone https://github.com/kevin-perez/tutor.git ~/.agents/skills/tutor
```

### Symlink instead of clone

If you already cloned TuTor elsewhere:

```bash
SKILL_ROOT="$HOME/code/tutor"   # your clone
TARGET="$HOME/.cursor/skills/tutor"

mkdir -p "$TARGET"
ln -sf "$SKILL_ROOT/SKILL.md" "$TARGET/SKILL.md"
ln -sf "$SKILL_ROOT/references" "$TARGET/references"
```

## Upgrade

TuTor ships as plain files (`SKILL.md` + `references/`). How you upgrade depends on how you installed it.

### Check your installed version

```bash
grep '^  version:' ~/.cursor/skills/tutor/SKILL.md
# or whichever path you use (see Install)
```

Compare with the latest `metadata.version` in [SKILL.md](SKILL.md) on GitHub.

### Git clone install

If the skill folder is a clone of this repo (recommended), pull the latest release:

```bash
# Personal Cursor example — use your actual path
cd ~/.cursor/skills/tutor
git pull origin master
```

Repeat for every path where you installed a clone (e.g. `~/.claude/skills/tutor`, `.opencode/skills/tutor`).

**Project-local** installs: from your repo root:

```bash
cd .cursor/skills/tutor   # or .claude/skills/tutor, etc.
git pull origin master
```

If you track TuTor as a **git submodule**, update from the parent repo:

```bash
git submodule update --remote .cursor/skills/tutor
```

### Symlink install

Pull once in your main clone; symlinks pick up changes automatically:

```bash
cd ~/code/tutor          # your SKILL_ROOT
git pull origin master
```

No need to re-run `ln -sf` unless you moved the clone or broke the links.

### Re-install (no git history)

If you copied files without git, replace the folder:

```bash
rm -rf ~/.cursor/skills/tutor
git clone https://github.com/kevin-perez/tutor.git ~/.cursor/skills/tutor
```

### After upgrading

1. **Restart or start a new agent session** so the tool reloads skill content (required for OpenCode when the skills directory was new; good practice everywhere).
2. **Confirm the version** with the `grep` command above.
3. If behavior still looks old, check you upgraded the path your agent actually uses (personal vs project vs symlink target).

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
└── AGENTS.md             # Instructions for agents editing this repo
```

Version is declared in `SKILL.md` frontmatter (`metadata.version`, currently **0.4**).

## Contribute

Contributions are welcome—especially clearer teaching patterns, edge cases for the tool gate, and translations of example flows.

1. **Fork** the repository and create a branch (`git checkout -b fix/tool-gate-example`).
2. **Change** `SKILL.md` and/or files under `references/`. Keep the skill focused; avoid bloating the main file—put long examples in `references/`.
3. **Test** by installing your branch into one agent (Cursor, Claude Code, or OpenCode) and running through a short learning task.
4. **Open a pull request** with:
   - What behavior changed and why
   - Which agent(s) you tested
   - Any breaking change to install paths or skill name (`tutor` must match the folder name for OpenCode)

Please do not commit secrets, personal paths, or generated `node_modules` / playground artifacts.

## Credits

- **Author:** [kevin-perez](https://github.com/kevin-perez) (`metadata.author` in `SKILL.md`)
- **TuTor** — learning-by-doing tutor skill for AI coding agents
- Built on the shared **Agent Skills** pattern (`SKILL.md` + optional `references/`), compatible with Cursor, Claude Code, OpenCode, and `.agents/skills` layouts

## Donations

If TuTor helps you learn, you can buy me a coffee via PayPal:

**[paypal.me/kevindperezm](https://paypal.me/kevindperezm?locale.x=es_XC&country.x=MX)**

Donations are optional and not required to use or contribute to this project.

## License

No license file is included yet. If you plan to redistribute or fork commercially, open an issue or contact the author for clarification.
