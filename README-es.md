# TuTor

[English](README.md) · **Español**

Una **skill de agente** multilingüe que convierte tu asistente de código en un tutor de aprendizaje. TuTor planifica el trabajo en pasos pequeños, explica cada comando, verifica tu avance con comprobaciones de solo lectura y se niega a completar la tarea por ti.

Úsala cuando quieras **aprender haciendo**: tareas, tutoriales, guías paso a paso, o cuando digas «no lo hagas por mí».

## Objetivo

TuTor ayuda a **docentes y estudiantes** a usar la IA como tutor, no como atajo. Los asistentes de código suelen entregar respuestas terminadas y saltarse el razonamiento, la depuración y el ensayo y error donde ocurre el aprendizaje real. Esta skill redirige esa capacidad hacia el **aprender haciendo**: quien aprende escribe el código y ejecuta los comandos; el agente planifica pasos, explica herramientas y verifica el avance—sin hacer el trabajo por ellos. La meta es una comprensión más profunda en clase, en grupo y en proyectos personales, no copiar y pegar más rápido.

## Qué hace

| Comportamiento | Detalle |
|----------------|---------|
| **Paso a paso** | Presenta un plan numerado al inicio; da **una** acción por turno (sin la solución completa). |
| **Verificación** | Cuando dices que terminaste, el agente comprueba con herramientas de solo lectura (`Read`, `ls`, `cat`, `git status`, `git diff`, las pruebas que ejecutaste). |
| **Puerta de herramientas** | El agente puede inspeccionar tu trabajo pero **no** debe crear, editar ni ejecutar comandos que construyan tu entregable por ti. |
| **Enseñanza de comandos** | Cada comando de shell incluye un desglose: objetivo, piezas y cómo se ve el éxito. |
| **Multilingüe** | Las explicaciones siguen tu idioma; comandos, rutas y código se mantienen literales. |

Material de referencia opcional en [`references/`](references/):

- [`references/example-flow.md`](references/example-flow.md) — recorrido de una app desde cero
- [`references/tool-gate.md`](references/tool-gate.md) — qué comandos de shell están permitidos

## Instalación

**Repositorio:** [github.com/kevinnio/tutor](https://github.com/kevinnio/tutor)

TuTor se distribuye como `SKILL.md` más `references/`. El `name` del frontmatter debe ser `tutor`.

### Instalar con Skills CLI (recomendado)

Usa la [Skills CLI](https://github.com/vercel-labs/skills) ([skills.sh](https://skills.sh)) para instalar en tus agentes de código. Detecta las herramientas instaladas y escribe en los directorios correctos.

**Usuario (todos los proyectos)** — recomendado la primera vez:

```bash
npx skills add kevinnio/tutor -g -y
```

**Solo este proyecto** (comparte con clase o equipo vía git):

```bash
npx skills add kevinnio/tutor -y
```

**Agentes concretos** (sin el selector interactivo):

```bash
npx skills add kevinnio/tutor -g -a cursor -a claude-code -a opencode -y
```

| Flag | Significado |
|------|-------------|
| `-g`, `--global` | Instalación de usuario (`~/…/skills/`) |
| (sin `-g`) | Solo en el proyecto actual |
| `-a`, `--agent` | Agente(s), p. ej. `cursor`, `claude-code`, `opencode`, `codex`, `windsurf` |
| `-y`, `--yes` | Sin preguntas interactivas |

Vista previa: `npx skills add kevinnio/tutor --list`. Lista de agentes: [vercel-labs/skills](https://github.com/vercel-labs/skills#supported-agents).

Después de instalar, inicia una **nueva sesión** del agente.

### Uso por agente

- **Cursor** — Pide aprender algo (p. ej. «Enséñame a crear un CLI de tareas—no lo escribas por mí») o usa `/tutor` si está disponible. [Cursor Agent Skills](https://cursor.com/docs/context/skills).
- **Claude Code** — Ejecuta `/tutor` o describe un objetivo de aprendizaje. [Claude Code skills](https://code.claude.com/docs/en/skills).
- **OpenCode** — Describe un objetivo; OpenCode carga skills bajo demanda. [OpenCode Agent Skills](https://opencode.ai/docs/skills/).

### Instalación manual (git clone)

Si prefieres no usar la CLI, clona el repo en una carpeta de skill. Las rutas varían según el agente:

| Agente | Usuario (todos los proyectos) | Este proyecto |
|--------|-------------------------------|---------------|
| [Cursor](https://cursor.com) | `~/.cursor/skills/tutor` | `.cursor/skills/tutor` o `.agents/skills/tutor` |
| [Claude Code](https://code.claude.com) | `~/.claude/skills/tutor` | `.claude/skills/tutor` |
| [OpenCode](https://opencode.ai) | `~/.config/opencode/skills/tutor` | `.opencode/skills/tutor` |

```bash
REPO=https://github.com/kevinnio/tutor.git
TARGET=~/.cursor/skills/tutor   # ejemplo usuario

mkdir -p "$(dirname "$TARGET")"
git clone "$REPO" "$TARGET"
```

No instales en `~/.cursor/skills-cursor/` (reservado por Cursor). Si `TARGET` ya existe, bórralo o usa [Actualizar](#actualizar).

### Enlace simbólico para desarrollo local

```bash
SKILL_ROOT="$HOME/code/tutor"
TARGET="$HOME/.cursor/skills/tutor"

mkdir -p "$TARGET"
ln -sf "$SKILL_ROOT/SKILL.md" "$TARGET/SKILL.md"
ln -sf "$SKILL_ROOT/references" "$TARGET/references"
```

### Instalar TuTor con TuTor

Como ejercicio divertido, deja que TuTor se instale contigo—tú ejecutas cada comando; él guía y comprueba tu trabajo. Aprenderás cómo se instalan las skills en disco y, cuando esté listo, siempre podrás pedirle ayuda en tareas reales.

Pégalo en un agente que pueda leer URLs (chat nuevo).

```text
TuTor, instálate conmigo—yo ejecuto cada comando; tú tutoreas.

Lee y sigue durante toda la sesión:
- SKILL.md: https://raw.githubusercontent.com/kevinnio/tutor/master/SKILL.md
- README sección Instalación: https://raw.githubusercontent.com/kevinnio/tutor/master/README.md

Sigue las reglas de TuTor: un paso a la vez, desglose de comandos, verificar cuando diga "listo".
Nunca instales por mí (sin escribir archivos, sin git clone, sin npx en mi lugar)—rechaza "hazlo tú".

Primero, pregúntame qué agente de código uso (p. ej. Cursor, Claude Code, OpenCode, Codex, Windsurf)
y si quiero instalación de usuario o local al proyecto. Usa la ruta de skills correcta para ese agente.

Objetivo: skill `tutor` instalada en mi máquina.
Enseña `npx skills add kevinnio/tutor -g -y` salvo que quiera git clone manual.

Cuando responda, da un plan numerado corto para mi agente y ámbito, luego Paso 1 y detente.
```

## Actualizar

### Skills CLI (recomendado)

```bash
npx skills update tutor -g -y    # usuario (todos los proyectos)
npx skills update tutor -y       # solo este proyecto
```

Copias instaladas: `npx skills list | grep tutor`

### Clon git manual

```bash
cd ~/.cursor/skills/tutor   # tu ruta de instalación
git pull origin master
```

**Submódulo git:** `git submodule update --remote ruta/al/tutor`

### Comprobar la versión instalada

```bash
grep '^  version:' ~/.cursor/skills/tutor/SKILL.md
# la ruta varía — usa npx skills list para localizar instalaciones
```

Compárala con `metadata.version` en [SKILL.md](SKILL.md) en GitHub.

### Después de actualizar

1. **Reinicia o abre una sesión nueva** del agente.
2. **Confirma la versión** con el comando `grep` de arriba.
3. Si no cambia el comportamiento, ejecuta `npx skills list` y actualiza cada ámbito (usuario vs proyecto) donde esté `tutor`.

## Cómo usar

1. **Instala** la skill (arriba) y abre tu proyecto en el agente.
2. **Di qué quieres aprender** y que el agente debe tutorear, no implementar por ti.

Ejemplos de prompts:

```text
I want to learn how to add tests to this repo. Walk me through it step by step; don't edit files for me.

Enséñame a crear un API REST con Express. No hagas el código por mí.

Help me fix this failing test, but only give hints and commands—I run everything myself.
```

3. **Haz cada paso** que asigne el tutor y responde cuando termines (p. ej. «done», «listo»).
4. El tutor **verifica** antes de seguir. Si algo falla, indica qué falta—no avanza.
5. Si el agente intenta hacer tu trabajo, recuérdale: *«Sigue la skill tutor—solo verifica, yo ejecuto los comandos.»*

## Estructura del repositorio

```text
tutor/
├── SKILL.md              # Skill principal (obligatorio)
├── references/           # Referencia opcional para el agente
│   ├── example-flow.md
│   └── tool-gate.md
├── README.md
├── README-es.md
├── AGENTS.md             # Instrucciones para agentes que editan este repo
└── LICENSE               # MIT
```

La versión se declara en el frontmatter de `SKILL.md` (`metadata.version`, actualmente **0.4**).

## Contribuir

Las contribuciones son bienvenidas—sobre todo patrones de enseñanza más claros, casos límite de la puerta de herramientas y traducciones de flujos de ejemplo.

1. **Haz fork** del repositorio y crea una rama (`git checkout -b fix/tool-gate-example`).
2. **Modifica** `SKILL.md` y/o archivos en `references/`. Mantén la skill enfocada; evita hinchar el archivo principal—pon ejemplos largos en `references/`.
3. **Prueba** instalando tu rama en un agente (Cursor, Claude Code u OpenCode) y haciendo una tarea de aprendizaje corta.
4. **Abre un pull request** con:
   - Qué comportamiento cambió y por qué
   - En qué agente(s) probaste
   - Cualquier cambio incompatible en rutas de instalación o nombre de la skill (`tutor` debe coincidir con el nombre de la carpeta en OpenCode)

No subas secretos, rutas personales ni artefactos generados (`node_modules`, playground, etc.).

## Créditos

- **Autor:** [Kevin Perez](https://github.com/kevinnio) (`metadata.author` en `SKILL.md`)
- **TuTor** — skill de tutor aprender-haciendo para agentes de código con IA
- Basado en el patrón compartido **Agent Skills** (`SKILL.md` + `references/` opcional), compatible con Cursor, Claude Code, OpenCode y `.agents/skills`

## Donaciones

Si TuTor te ayuda a aprender, puedes invitarme un café por PayPal:

**[paypal.me/kevindperezm](https://paypal.me/kevindperezm?locale.x=es_XC&country.x=MX)**

Las donaciones son opcionales y no son necesarias para usar ni contribuir al proyecto.

## Licencia

[MIT](LICENSE) — Copyright (c) 2026 Kevin Perez
