# Okk_Init — Project Governance Skill

> **File-based "long-term memory" for AI agents.** Zero dependencies, works with any agent that can read files.

[![Obsidian](https://img.shields.io/badge/Obsidian-ready-7C3AED?style=flat-square&logo=obsidian)](https://obsidian.md)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg?style=flat-square)](LICENSE)
[![OpenCode](https://img.shields.io/badge/OpenCode-000000?style=flat-square)](https://opencode.ai)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-D97757?style=flat-square&logo=claudecode&logoColor=white)](https://claude.com)
[![Cursor](https://img.shields.io/badge/Cursor-000000?style=flat-square&logo=cursor&logoColor=white)](https://cursor.com)
[![Windsurf](https://img.shields.io/badge/Windsurf-00A6FF?style=flat-square&logo=windsurf&logoColor=white)](https://windsurf.com)
[![Cline](https://img.shields.io/badge/Cline-E8593C?style=flat-square&logo=cline&logoColor=white)](https://cline.bot)

---

## What is Okk_Init?

AI agents forget your project context quickly — leading to mistakes, repeated explanations, or starting from scratch. **Okk_Init** creates 5 interconnected markdown files that summarize your project perfectly. Each time you start a session, the agent reads these files: it understands everything instantly, uses the minimum data possible, and guides you step by step to finish tasks without going in circles.

### The 5 Files

| File | Purpose | Obsidian Link |
|------|---------|---------------|
| `Proyecto [NAME].md` | Architectural map of your project | Links to all other files |
| `agent.md` | Coding rules + decision history | `[[stack]]` `[[progress]]` |
| `stack.md` | Tech stack + data model + UI standard | `[[agent]]` `[[progress]]` |
| `progress.md` | Current state + next step | `[[history]]` `[[stack]]` |
| `history.md` | Accumulated memory of completed phases | `[[progress]]` |

### The 5 Commands

| Command | What it does |
|---------|--------------|
| `okk init` | **The starter:** Asks basic questions and sets up the entire structure in seconds |
| `okk sigamos` | **The reminder:** "Where did we leave off?" — reads files and tells you the next step |
| `okk sync` | **The auditor:** Compares real work against the plan and flags incomplete items |
| `okk status` | **The dashboard:** Shows a visual progress bar of where the project stands |
| `okk commit` | **The safe keeper:** Asks quick questions and archives both code and governance notes |

---

## Skills by Platform

All skills are organized in the `skills/` directory:

| Platform | Skill Location | Installation |
|----------|----------------|--------------|
| **OpenCode** | `skills/opencode/okk-init/SKILL.md` | Copy to `~/.config/opencode/skills/okk-init/` |
| **Claude Code** | `skills/claude-code/` | Copy `CLAUDE.md` + `okk-init/` to `.claude/skills/` |
| **Cursor** | `skills/cursor/.cursorrules` | Copy to project root |
| **Windsurf** | `skills/windsurf/.windsurfrules` | Copy to project root |
| **Cline** | `skills/cline/governance.md` | Copy to `.clinerules/` |

### Installation by Platform

#### OpenCode

```bash
# Global (all projects)
mkdir -p ~/.config/opencode/skills/okk-init
cp skills/opencode/okk-init/SKILL.md ~/.config/opencode/skills/okk-init/

# Local (single project)
mkdir -p .opencode/skills/okk-init
cp skills/opencode/okk-init/SKILL.md .opencode/skills/okk-init/
```

#### Claude Code

```bash
# Copy CLAUDE.md to project root
cp skills/claude-code/CLAUDE.md .

# Copy skill directory
mkdir -p .claude/skills/okk-init
cp skills/claude-code/okk-init/SKILL.md .claude/skills/okk-init/
```

#### Cursor / Windsurf

```bash
# Cursor
cp skills/cursor/.cursorrules .

# Windsurf
cp skills/windsurf/.windsurfrules .
```

#### Cline

```bash
mkdir -p .clinerules
cp skills/cline/governance.md .clinerules/
```

---

## The 6 Questions

When you run `okk init` for the first time, Okk_Init asks 6 questions to configure your project. Press **Enter** to accept the default.

| # | Question | Default | Why it matters |
|---|----------|---------|----------------|
| 1 | Project name | *(required)* | Names all 5 governance files |
| 2 | Tech stack | `.NET 10 + C# 14` | Sets runtime in `stack.md` |
| 3 | Database | `SQL Server 2022` | Configures data access layer |
| 4 | Testing framework | `xUnit + Moq` | Defines test project structure |
| 5 | UI framework | `Tailwind CDN` | Sets UI standard in `stack.md` |
| 6 | Auth strategy | `Cookie Authentication` | Configures security layer |

> [!tip] Customize or default
> All questions have sensible defaults. Just press Enter for each one and you'll have a working governance structure in 30 seconds.

---

## Quick Start

### 1. Install the skill for your platform (see above)

### 2. Initialize your project

```
okk init
```

### 3. Answer 6 questions (or press Enter for defaults)

### 4. Start coding!

### 5. Continue a session

```
okk sigamos
```

### 6. Audit your progress

```
okk sync
```

### 7. Commit your work

```
okk commit
```

---

## Complete Example

### First time (new project)

1. Open your AI agent in the project folder
2. Type: `okk init`
3. Answer 6 questions (or press Enter for defaults)
4. Files are generated!
5. Start coding with the AI

### During a session

1. Type: `okk sigamos` (to continue)
2. Work on the current step
3. When done: `okk sync` (to audit)
4. When ready: `okk commit` (to save)

### Flowchart

```
┌─────────────────────────────────────────────────────────────┐
│                    Okk_Init Workflow                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌───────────┐    ┌──────────┐    ┌──────────┐             │
│  │ okk init  │───▶│ 6 Q's    │───▶│ 5 files  │             │
│  └───────────┘    └──────────┘    └──────────┘             │
│       │                              │                      │
│       ▼                              ▼                      │
│  ┌───────────┐    ┌──────────┐    ┌──────────┐             │
│  │okk sigamos│───▶│  Read    │───▶│  Work    │             │
│  └───────────┘    │  state   │    │  on code │             │
│                  └──────────┘    └──────────┘             │
│                                     │                       │
│                                     ▼                       │
│                              ┌───────────┐                  │
│                              │ okk sync  │                  │
│                              └───────────┘                  │
│                                     │                       │
│                                     ▼                       │
│                              ┌───────────┐                  │
│                              │ okk commit│                  │
│                              └───────────┘                  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## Obsidian Integration

### Wiki-links

Each file starts with links to the other files:

```markdown
# Agent Instructions: MyProject

> **Map:** Ver [[Proyecto MyProject]]
> **Stack:** Ver [[stack]]
> **Status:** Ver [[progress]]
> **History:** Ver [[history]]
```

### Graph View

This creates an interactive graph in Obsidian:

```
┌──────────────────┐
│  Proyecto        │
│  MyProject       │
└────────┬─────────┘
         │
    ┌────┴────┐
    ▼         ▼
┌────────┐ ┌────────┐
│ stack  │ │ agent  │
│ (tech) │ │ (rules)│
└───┬────┘ └───┬────┘
    │          │
    ▼          ▼
┌────────┐ ┌────────┐
│progress│ │ history│
│ (state)│ │ (mem)  │
└────────┘ └────────┘
```

### Callouts

| Callout | Usage |
|---------|-------|
| `> [!note]` | General information |
| `> [!tip]` | Helpful tips and recommendations |
| `> [!warning]` | Important rules and constraints |
| `> [!important]` | Critical architectural decisions |

---

## Session Protocol

### Start of Session

1. Read `progress.md` → current phase and next step
2. Read `agent.md` → rules and decisions
3. Read `stack.md` → technical decisions

### During Session

- Generate code **only for the current step**
- If ambiguous, **ask before coding**
- Minor dependencies: create stub with `// TODO`

### End of Session (after user approval)

1. `progress.md` → mark completed + set next step
2. `history.md` → move phase with date (DD/MM/YYYY HH:MM)
3. `stack.md` → document new technical decisions
4. `agent.md` → document new rules/decisions

> **Golden rule:** Files are **only updated AFTER user approval**.

---

## Repository Structure

```
Okk_Init/
├── README.md                      # This file
├── CONVENTIONS.md                 # Aider conventions
├── REFERENCE.md                   # Complete reference
├── LICENSE                        # MIT
├── .gitignore
│
├── skills/                        # All skills organized
│   ├── opencode/
│   │   └── okk-init/
│   │       └── SKILL.md
│   ├── claude-code/
│   │   ├── okk-init/
│   │   │   └── SKILL.md
│   │   └── CLAUDE.md
│   ├── cursor/
│   │   └── .cursorrules
│   ├── windsurf/
│   │   └── .windsurfrules
│   └── cline/
│       └── governance.md
│
└── .opencode/                     # For local development
    └── skills/
        └── okk-init/
            └── SKILL.md
```

---

## Requirements

- One of: [OpenCode](https://opencode.ai), [Claude Code](https://claude.com), [Cursor](https://cursor.com), [Windsurf](https://windsurf.com), or [Cline](https://cline.bot)
- Obsidian (optional, for graph views)

---

## License

MIT

---

# Español

## Qué es Okk_Init?

Los agentes de IA olvidan el contexto de tu proyecto rápidamente — causando errores, explicaciones repetidas o empezando desde cero. **Okk_Init** crea 5 archivos markdown interconectados que resumen tu proyecto perfectamente. Cada vez que inicias una sesión, el agente lee estos archivos: entiende todo al instante, usa la mínima cantidad de datos y te guía paso a paso para terminar tareas sin dar vueltas.

### Los 5 Archivos

| Archivo | Propósito | Enlace Obsidian |
|---------|-----------|-----------------|
| `Proyecto [NOMBRE].md` | Mapa arquitectónico del proyecto | Enlaza a todos los demás |
| `agent.md` | Reglas de código + historial de decisiones | `[[stack]]` `[[progress]]` |
| `stack.md` | Stack técnico + modelo de datos + estándar UI | `[[agent]]` `[[progress]]` |
| `progress.md` | Estado actual + próximo paso | `[[history]]` `[[stack]]` |
| `history.md` | Memoria acumulativa de fases completadas | `[[progress]]` |

### Los 5 Comandos

| Comando | Qué hace |
|---------|----------|
| `okk init` | **El arranque:** Hace unas preguntas básicas y monta toda la estructura en un segundo |
| `okk sigamos` | **El recordatorio:** "¿En qué nos quedamos?" — Lee los archivos y te recuerda el siguiente paso |
| `okk sync` | **La auditoría:** Revisa el trabajo real y lo compara con el plan para avisar si algo quedó a medias |
| `okk status` | **El tablero:** Te muestra una barra de progreso visual de cómo va el proyecto hoy |
| `okk commit` | **El archivador seguro:** Te hace preguntas rápidas y archiva tanto los avances como las notas de control |

---

## Skills por Plataforma

Todas las skills están organizadas en el directorio `skills/`:

| Plataforma | Ubicación | Instalación |
|------------|-----------|-------------|
| **OpenCode** | `skills/opencode/okk-init/SKILL.md` | Copiar a `~/.config/opencode/skills/okk-init/` |
| **Claude Code** | `skills/claude-code/` | Copiar `CLAUDE.md` + `okk-init/` a `.claude/skills/` |
| **Cursor** | `skills/cursor/.cursorrules` | Copiar a la raíz del proyecto |
| **Windsurf** | `skills/windsurf/.windsurfrules` | Copiar a la raíz del proyecto |
| **Cline** | `skills/cline/governance.md` | Copiar a `.clinerules/` |

### Instalación por Plataforma

#### OpenCode

```bash
# Global (todos los proyectos)
mkdir -p ~/.config/opencode/skills/okk-init
cp skills/opencode/okk-init/SKILL.md ~/.config/opencode/skills/okk-init/

# Local (un solo proyecto)
mkdir -p .opencode/skills/okk-init
cp skills/opencode/okk-init/SKILL.md .opencode/skills/okk-init/
```

#### Claude Code

```bash
# Copiar CLAUDE.md a la raíz del proyecto
cp skills/claude-code/CLAUDE.md .

# Copiar directorio del skill
mkdir -p .claude/skills/okk-init
cp skills/claude-code/okk-init/SKILL.md .claude/skills/okk-init/
```

#### Cursor / Windsurf

```bash
# Cursor
cp skills/cursor/.cursorrules .

# Windsurf
cp skills/windsurf/.windsurfrules .
```

#### Cline

```bash
mkdir -p .clinerules
cp skills/cline/governance.md .clinerules/
```

---

## Las 6 Preguntas

Cuando ejecutas `okk init` por primera vez, Okk_Init hace 6 preguntas para configurar tu proyecto. Presiona **Enter** para aceptar el valor por defecto.

| # | Pregunta | Por defecto | Por qué importa |
|---|----------|-------------|------------------|
| 1 | Nombre del proyecto | *(requerido)* | Nombra los 5 archivos de gobernanza |
| 2 | Stack tecnológico | `.NET 10 + C# 14` | Define el runtime en `stack.md` |
| 3 | Base de datos | `SQL Server 2022` | Configura la capa de acceso a datos |
| 4 | Framework de testing | `xUnit + Moq` | Define la estructura del proyecto de tests |
| 5 | Framework de UI | `Tailwind CDN` | Establece el estándar UI en `stack.md` |
| 6 | Estrategia de auth | `Cookie Authentication` | Configura la capa de seguridad |

> [!tip] Personalizar o usar defectos
> Todas las preguntas tienen valores sensatos por defecto. Solo presiona Enter para cada una y tendrás una estructura de gobernanza funcionando en 30 segundos.

---

## Inicio Rápido

### 1. Instala la skill para tu plataforma (ver arriba)

### 2. Inicializa tu proyecto

```
okk init
```

### 3. Responde 6 preguntas (o Enter para valores por defecto)

### 4. ¡Empieza a codificar!

### 5. Continúa una sesión

```
okk sigamos
```

### 6. Audita tu progreso

```
okk sync
```

### 7. Guarda tu trabajo

```
okk commit
```

---

## Ejemplo Completo

### Primera vez (proyecto nuevo)

1. Abre tu agente de IA en la carpeta del proyecto
2. Escribe: `okk init`
3. Responde 6 preguntas (o Enter para valores por defecto)
4. ¡Se generan los archivos!
5. Empieza a codificar con la IA

### Durante una sesión

1. Escribe: `okk sigamos` (para continuar)
2. Trabaja en el paso actual
3. Cuando termines: `okk sync` (para auditar)
4. Cuando estés listo: `okk commit` (para guardar)

### Diagrama de Flujo

```
┌─────────────────────────────────────────────────────────────┐
│                  Flujo de Trabajo Okk_Init                  │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌───────────┐    ┌──────────┐    ┌──────────┐             │
│  │ okk init  │───▶│ 6 Preg.  │───▶│ 5 archivos│            │
│  └───────────┘    └──────────┘    └──────────┘             │
│       │                              │                      │
│       ▼                              ▼                      │
│  ┌───────────┐    ┌──────────┐    ┌──────────┐             │
│  │okk sigamos│───▶│  Leer    │───▶│ Trabajar │             │
│  └───────────┘    │  estado  │    │  código  │             │
│                  └──────────┘    └──────────┘             │
│                                     │                       │
│                                     ▼                       │
│                              ┌───────────┐                  │
│                              │ okk sync  │                  │
│                              └───────────┘                  │
│                                     │                       │
│                                     ▼                       │
│                              ┌───────────┐                  │
│                              │ okk commit│                  │
│                              └───────────┘                  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## Estructura del Repositorio

```
Okk_Init/
├── README.md                      # Este archivo
├── CONVENTIONS.md                 # Convenciones para Aider
├── REFERENCE.md                   # Referencia completa
├── LICENSE                        # MIT
├── .gitignore
│
├── skills/                        # Todas las skills organizadas
│   ├── opencode/
│   │   └── okk-init/
│   │       └── SKILL.md
│   ├── claude-code/
│   │   ├── okk-init/
│   │   │   └── SKILL.md
│   │   └── CLAUDE.md
│   ├── cursor/
│   │   └── .cursorrules
│   ├── windsurf/
│   │   └── .windsurfrules
│   └── cline/
│       └── governance.md
│
└── .opencode/                     # Para desarrollo local
    └── skills/
        └── okk-init/
            └── SKILL.md
```

---

## Requisitos

- Uno de: [OpenCode](https://opencode.ai), [Claude Code](https://claude.com), [Cursor](https://cursor.com), [Windsurf](https://windsurf.com), o [Cline](https://cline.bot)
- Obsidian (opcional, para graph views)

---

## Licencia

MIT
