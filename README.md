# OK_Init — Skill de Inicialización de Proyecto

Skill para [opencode](https://opencode.ai) que genera un sistema de gobernanza completo para proyectos .NET con Arquitectura Hexagonal.

## Qué genera

La skill crea **5 archivos** al iniciar un nuevo proyecto:

| Archivo | Propósito |
|---------|-----------|
| `Proyecto {{NAME}}.md` | Mapa arquitectónico del proyecto |
| `agent.md` | Reglas de código + historial de decisiones |
| `stack.md` | Stack técnico + modelo de datos + estándar UI |
| `progress.md` | Estado actual + próximo paso |
| `history.md` | Memoria acumulativa de fases completadas |

## Características

- **Obsidian-ready:** Todos los archivos MD se enlazan entre sí con `[[wiki-links]]`
- **Callouts:** Incluye `> [!note]`, `> [!tip]`, `> [!warning]`, `> [!important]`
- **Graph view:** Obsidian genera automáticamente el grafo de relaciones
- **Mock data realista:** Ejemplo con NexusPlatform, NexusCore, NexusCRM, etc.
- **Protocolo KISS:** Incluye instrucciones de sesión (inicio → código → aprobación → actualización)
- **9 preguntas personalizables:** Proyecto, Kernel, módulos, stack, DB, testing, UI, auth, esquemas

## Instalación

### Opción 1: Local (solo este proyecto)

Copia `ok-init.md` a `.opencode/skills/` en tu workspace:

```
tu-proyecto/
└── .opencode/
    └── skills/
        └── ok-init.md
```

### Opción 2: Global (todos los proyectos)

Copia `ok-init.md` a `~/.config/opencode/skills/`:

```
~/.config/opencode/skills/
└── ok-init.md
```

## Uso

En opencode, escribe cualquiera de estos comandos:

```
OK_Init
ok init
iniciar proyecto
nuevo proyecto
empezar proyecto
setup proyecto
```

La skill detecta automáticamente si ya existen archivos de gobernanza:

```
OK_Init
    │
    ├── ¿Existen archivos?
    │   ├── SÍ → ¿Qué quieres hacer?
    │   │   ├── Continuar → Leer estado → Empezar
    │   │   ├── Regenerar → Preguntas → Sobreescribir
    │   │   └── Nuevo proyecto → Preguntas → Generar en subdir
    │   └── NO → ¿Qué quieres hacer?
    │       ├── Preguntar todo → 9 preguntas → Generar
    │       └── Usar contexto → Resumen de conversación → Generar
    │
    └── Protocolo de Sesión
```

### Caso de uso: "Usar contexto"

Si ya estás chateando con la IA sobre tu proyecto y luego invocas `OK_Init`:

1. La IA detecta que no hay archivos de gobernanza
2. Pregunta si quieres usar el contexto de la conversación
3. Resume las decisiones que identificó (proyecto, kernel, módulos, stack, etc.)
4. Pides confirmación o ajustes
5. Genera los 5 archivos con esos datos

## Ejemplo de salida

### Graph view en Obsidian

```
┌──────────────────┐
│  Proyecto        │
│  NexusPlatform   │
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

### Mock Data (ejemplo)

1. Nombre del proyecto → `NexusPlatform`
2. Nombre del Kernel → `NexusCore`
3. Módulos de negocio → `NexusCRM, NexusInventario, NexusRRHH`
4. Stack tecnológico → `.NET 10 + C# 14`
5. Base de datos → `SQL Server 2022`
6. Framework testing → `xUnit + Moq`
7. Framework UI → `Tailwind CDN`
8. Estrategia auth → `Cookie Authentication con PasswordHasher`
9. Esquemas DB → `nexus_core, nexus_crm, nexus_inv, nexus_rrhh`

## Protocolo de Sesión

La skill incluye un protocolo de trabajo KISS:

### Al INICIO:
- Leer `progress.md` → fase actual
- Leer `agent.md` → reglas vigentes
- Leer `stack.md` → decisiones técnicas

### Al FINAL (tras aprobación):
1. `progress.md` → marcar completado + próximo paso
2. `history.md` → mover fase con fecha
3. `stack.md` → documentar decisiones nuevas
4. `agent.md` → documentar reglas nuevas

> **Regla de oro:** Los archivos solo se actualizan DESPUÉS de aprobación del usuario.

## Estructura de archivos generados

```
tu-proyecto/
├── Proyecto [TU_PROYECTO].md
├── agent.md
├── stack.md
├── progress.md
├── history.md
└── opencode.json (si no existía)
```

> El nombre del primer archivo se personaliza con el nombre de tu proyecto.

## Requisitos

- [opencode](https://opencode.ai) instalado
- Obsidian (opcional, para ver los graph views)

## Licencia

MIT
