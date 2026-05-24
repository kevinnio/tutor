# TuTor

[English](README.md) · **Español**

Una **skill de agente** multilingüe que convierte tu asistente de código en un tutor de aprendizaje. TuTor planifica el trabajo en pasos pequeños, explica cada comando, verifica tu avance con comprobaciones de solo lectura y se niega a completar la tarea por ti.

Úsala cuando quieras **aprender haciendo**: tareas, tutoriales, guías paso a paso, o cuando digas «no lo hagas por mí».

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

TuTor es una carpeta con `SKILL.md` más `references/`. El nombre de la skill en el frontmatter es `tutor`, así que el directorio de instalación debe llamarse **`tutor`**.

### Instalación rápida (cualquier agente)

Clona este repositorio en una carpeta de skill `tutor` (personal = todos los proyectos, de proyecto = solo ese repo):

```bash
# Personal (recomendado)
git clone https://github.com/kevin-perez/tutor.git ~/.cursor/skills/tutor
```

Para una copia **local al proyecto**, usa la misma ruta dentro de tu repo (ejemplos abajo).

Después de instalar, inicia una **nueva sesión** del agente para que cargue las skills.

### [Cursor](https://cursor.com)

| Ámbito | Ruta |
|--------|------|
| Personal | `~/.cursor/skills/tutor/` |
| Proyecto | `.cursor/skills/tutor/` |

```bash
git clone https://github.com/kevin-perez/tutor.git ~/.cursor/skills/tutor
```

En el chat, pide aprender algo (p. ej. «Enséñame a crear un CLI de tareas—no lo escribas por mí») o invoca la skill si tu versión de Cursor expone `/tutor`.

Documentación: [Cursor Agent Skills](https://cursor.com/docs/context/skills).

### [Claude Code](https://code.claude.com)

| Ámbito | Ruta |
|--------|------|
| Personal | `~/.claude/skills/tutor/` |
| Proyecto | `.claude/skills/tutor/` |

```bash
git clone https://github.com/kevin-perez/tutor.git ~/.claude/skills/tutor
```

Ejecuta `/tutor` o describe un objetivo de aprendizaje; Claude carga la skill cuando es relevante.

Documentación: [Claude Code skills](https://code.claude.com/docs/en/skills).

### [OpenCode](https://opencode.ai)

| Ámbito | Ruta |
|--------|------|
| Proyecto | `.opencode/skills/tutor/` |
| Global | `~/.config/opencode/skills/tutor/` |

OpenCode también descubre `.claude/skills/tutor/` y `.agents/skills/tutor/`.

```bash
# Desde la raíz de tu proyecto
mkdir -p .opencode/skills
git clone https://github.com/kevin-perez/tutor.git .opencode/skills/tutor
```

Reinicia OpenCode después de añadir un directorio de skills nuevo.

Documentación: [OpenCode Agent Skills](https://opencode.ai/docs/skills/).

### Otros agentes (`.agents/skills`)

Muchas herramientas que siguen el esquema [Agent Skills](https://agentskills.io) buscan en:

| Ámbito | Ruta |
|--------|------|
| Personal | `~/.agents/skills/tutor/` |
| Proyecto | `.agents/skills/tutor/` |

```bash
git clone https://github.com/kevin-perez/tutor.git ~/.agents/skills/tutor
```

### Enlace simbólico en lugar de clonar

Si ya clonaste TuTor en otro sitio:

```bash
SKILL_ROOT="$HOME/code/tutor"   # tu clon
TARGET="$HOME/.cursor/skills/tutor"

mkdir -p "$TARGET"
ln -sf "$SKILL_ROOT/SKILL.md" "$TARGET/SKILL.md"
ln -sf "$SKILL_ROOT/references" "$TARGET/references"
```

## Actualizar

TuTor son archivos planos (`SKILL.md` + `references/`). Cómo actualizar depende de cómo lo instalaste.

### Comprobar la versión instalada

```bash
grep '^  version:' ~/.cursor/skills/tutor/SKILL.md
# o la ruta que uses (ver Instalación)
```

Compárala con la última `metadata.version` en [SKILL.md](SKILL.md) en GitHub.

### Instalación por clon git

Si la carpeta de la skill es un clon de este repo (recomendado), trae los últimos cambios:

```bash
# Ejemplo personal en Cursor — usa tu ruta real
cd ~/.cursor/skills/tutor
git pull origin master
```

Repite en cada ruta donde tengas un clon (p. ej. `~/.claude/skills/tutor`, `.opencode/skills/tutor`).

**Instalaciones locales al proyecto**, desde la raíz del repo:

```bash
cd .cursor/skills/tutor   # o .claude/skills/tutor, etc.
git pull origin master
```

Si TuTor es un **submódulo git**, actualiza desde el repo padre:

```bash
git submodule update --remote .cursor/skills/tutor
```

### Instalación por enlace simbólico

Haz `pull` una vez en tu clon principal; los enlaces recogen los cambios solos:

```bash
cd ~/code/tutor          # tu SKILL_ROOT
git pull origin master
```

No hace falta volver a ejecutar `ln -sf` salvo que hayas movido el clon o roto los enlaces.

### Reinstalar (sin historial git)

Si copiaste archivos sin git, sustituye la carpeta:

```bash
rm -rf ~/.cursor/skills/tutor
git clone https://github.com/kevin-perez/tutor.git ~/.cursor/skills/tutor
```

### Después de actualizar

1. **Reinicia o abre una sesión nueva** del agente para que recargue la skill (obligatorio en OpenCode si el directorio de skills era nuevo; buena práctica en todos).
2. **Confirma la versión** con el comando `grep` de arriba.
3. Si el comportamiento sigue siendo el antiguo, comprueba que actualizaste la ruta que tu agente usa de verdad (personal vs proyecto vs destino del enlace simbólico).

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

- **Autor:** [kevin-perez](https://github.com/kevin-perez) (`metadata.author` en `SKILL.md`)
- **TuTor** — skill de tutor aprender-haciendo para agentes de código con IA
- Basado en el patrón compartido **Agent Skills** (`SKILL.md` + `references/` opcional), compatible con Cursor, Claude Code, OpenCode y `.agents/skills`

## Donaciones

Si TuTor te ayuda a aprender, puedes invitarme un café por PayPal:

**[paypal.me/kevindperezm](https://paypal.me/kevindperezm?locale.x=es_XC&country.x=MX)**

Las donaciones son opcionales y no son necesarias para usar ni contribuir al proyecto.

## Licencia

[MIT](LICENSE) — Copyright (c) 2026 Kevin Perez
