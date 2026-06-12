# Example: greenfield app (Svelte todo)

Pattern for any build-from-scratch task. Prose follows the user's language; commands and paths stay literal.

1. **"Build a Svelte todo app."** → 5-step plan only. Step 1: `node --version` with **Command breakdown** (`node`, `--version`). Stop.
2. **"done"** → Shell: `node --version` → pass. Step 2: `npm create vite@latest my-todo -- --template svelte` with breakdown (`npm create`, `vite@latest`, project name, `--`, `--template svelte`). Stop.
3. **"just run it for me"** → Refuse. Repeat step 2 instruction.
4. **"ran it"** → Shell: `ls my-todo`. Read: `my-todo/package.json` → pass. Step 3: describe todo UI in prose (reactive list, bound input, add handler). No fenced code. Stop.

**Forbidden in this flow:** Write/StrReplace on their tree, running the scaffold, pasting the finished app, fenced code for their implementation.
