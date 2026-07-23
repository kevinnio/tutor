# AGENTS.md — TuTor repository

Instructions for AI coding agents working **on this repo** (not for end users learning with the TuTor skill).

## Project

**TuTor** is an [Agent Skills](https://agentskills.io)-style package: a tutor skill that teaches step-by-step without completing the user's task. It ships as `SKILL.md` plus optional `references/` for deeper agent-only docs.

| File / directory | Purpose |
|----------------|---------|
| `SKILL.md` | **Source of truth** for tutor behavior. YAML frontmatter (`name`, `description`, `metadata.version`) must stay valid for Cursor, Claude Code, OpenCode, and `.agents/skills`. |
| `references/` | Long examples and rules loaded on demand (e.g. `example-flow.md`, `tool-gate.md`). Keep `SKILL.md` lean; add detail here. |
| `README.md` | User-facing docs (English): install, upgrade, usage, contribute. |
| `README-es.md` | Same content as `README.md` in Spanish. Must stay aligned with the English README. |
| `AGENTS.md` | This file — repo conventions for agents. |
| `AUTHORS.md` | Contributor list — keep in sync with README Contributors section. |
| `LICENSE` | MIT — keep in sync with README license sections. |
| `playground/` | Local experiments (gitignored). Do not commit. |

**Skill identity:** folder name and frontmatter `name` must both be `tutor`.

**Current version:** see `metadata.version` in `SKILL.md` (bump when behavior or compatibility meaningfully changes).

**Tool-gate declaration:** `SKILL.md` frontmatter uses only Agent Skills standard fields — `compatibility` documents the read-only verification constraint; `allowed-tools` (experimental in the spec) pre-approves read-only verification tools so checks don't prompt where supported. There is no standard field to *deny* tools, so the tool gate is prose-enforced via `SKILL.md` + `references/tool-gate.md` (the agent must refuse forbidden commands). Do not add platform-specific frontmatter keys (`disallowed-tools`, `context`, `agent`, etc.) — keep the skill cross-platform.

## Editing conventions

1. **Behavior changes** → edit `SKILL.md` and/or `references/`. Prefer `references/` for long walkthroughs or tables.
2. **User-visible docs** → update `README.md` and `README-es.md` when relevant (see below).
3. **Minimal diffs** — match existing tone and structure; do not refactor unrelated files.
4. **No secrets** — never commit `.env`, tokens, or personal machine paths as examples.
5. **Do not commit** `playground/`, `.cursor/`, `.opencode/`, `.agents/` (see `.gitignore`).

## Keep README and README-es in sync

When you change something that affects how people **install, upgrade, use, or contribute** to TuTor, update **both** `README.md` and `README-es.md` in the same change.

| Update both READMEs when… | Examples |
|---------------------------|----------|
| Install or upgrade paths change | New agent platform, renamed directories |
| Version bump | `metadata.version` in `SKILL.md` — update the version line in both READMEs |
| New user-facing feature | New section in docs (e.g. upgrade flow) |
| Repository layout changes | New top-level files agents/users should know about |
| Contribution workflow changes | PR checklist, testing expectations |

| README sync not required when… | Examples |
|----------------------------------|----------|
| Only tutor runtime behavior in `SKILL.md` / `references/` | Wording of refusals, internal tool-gate edge cases |
| Typos or clarity inside skill text with no user doc impact | — |

**Spanish README rules:**

- Translate prose; keep commands, paths, flags, and code **literal** (same as TuTor teaches learners).
- Preserve the language switcher at the top: English README links to `README-es.md`; Spanish links to `README.md`.
- Keep section structure parallel so diffs between languages stay easy to review.

## Version bumps

When bumping `metadata.version` in `SKILL.md`:

1. Increment the version string in frontmatter.
2. Update the “currently **X.Y**” (or equivalent) line in **both** READMEs.
3. Update the pinned release tag in **both** READMEs’ “Install TuTor via TuTor” exercise (`v0.6` → `vX.Y`), and in the “Pin to a release tag” snippet if the example tag is referenced.
4. Create a git tag `vX.Y` on the resulting commit and push it (`git tag vX.Y && git push origin vX.Y`). Tags are the immutable pins users install from.
5. Mention the version in the commit message if the user asked for commits.

## Commits

Group related changes logically (e.g. skill + reference in one commit; README pair in another) unless the user asks for a single commit.

Do not create commits unless the user explicitly requests them.

## Testing changes

- Install the skill from this repo into one target path (e.g. `~/.cursor/skills/tutor` via clone or symlink).
- Run a short “teach me X, don’t do it for me” session and confirm verify-only behavior.
- For doc-only changes, skim both READMEs for broken links and mismatched version numbers.
- CI (`.github/workflows/security.yml`) runs on every PR: gitleaks gates on leaked secrets; SkillSpector uploads static findings to the Security tab as advisory. Treat a new HIGH/CRITICAL finding as a blocker even though the job stays green.

## Links

- English docs: [README.md](README.md)
- Spanish docs: [README-es.md](README-es.md)
- Skill entrypoint: [SKILL.md](SKILL.md)
