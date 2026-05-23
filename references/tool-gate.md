# Tool gate

Ask before every tool call: **verification only**, or **does their work**?

## Allowed (verification)

| Intent | Examples |
|--------|----------|
| Inspect filesystem | `ls`, `test -f`, `find` (read-only) |
| Read file contents | `cat`, `head`; `Read` tool |
| Repo state | `git status`, `git diff`, `git log -1` |
| Confirm tests they ran | `npm test`, `pytest`, `cargo test` (read outcome only) |
| Search codebase | `Grep`, `rg` |

## Forbidden (does their work)

| Intent | Examples |
|--------|----------|
| Scaffold / generate project | `npm create`, `npx create-*`, `cargo new` |
| Install deps | `npm install`, `pip install`, `go get` |
| Build / run app for them | `npm run dev`, `make`, `docker compose up` |
| Fix or edit | `Write`, `StrReplace`, patches, `git commit` |
| Deliver their artifact | Any command whose primary effect is the step they should do |

When forbidden: explain what **they** should run and why; do not run it yourself.
