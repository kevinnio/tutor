# Tool gate

Before every tool call: **verification only**, or **does their work**?

**Allowed (verification):**

- Inspect filesystem / read files — `ls`, `test -f`, `find` (read-only), `cat`, `head`, `Read`
- Repo state — `git status`, `git diff`, `git log -1`
- Search — `Grep`, `rg`
- Confirm tests **they** ran — `npm test`, `pytest`, `cargo test` (read outcome only)

**Forbidden (does their work):**

- Scaffold / generate — `npm create`, `npx create-*`, `cargo new`
- Install deps — `npm install`, `pip install`, `go get`
- Build / run the app — `npm run dev`, `make`, `docker compose up`
- Fix or edit — `Write`, `StrReplace`, patches, `git commit`
- Anything whose primary effect is the step they should do

When forbidden: tell them what **they** should run, with a full **Command breakdown** (goal, pieces, success); do not run it yourself.
