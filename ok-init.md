# OK_Init — Skill de Inicialización de Proyecto con Gobernanza

**Nombre:** ok-init
**Trigger:** "iniciar proyecto", "OK_Init", "crear gobernanza", "ok init"
**Descripción:** Genera el sistema de gobernanza de 5 archivos MD para un proyecto .NET con Arquitectura Hexagonal, incluyendo enlaces Obsidian bidireccionales y protocolo de sesión KISS.

---

## Instrucciones para la IA

Cuando el usuario active esta skill, debes:

### Paso 1: Recopilar datos (9 preguntas)

Haz las siguientes preguntas **una por una** o **en grupo** (máximo 3 por mensaje para no abrumar):

| # | Pregunta | Variable | Mock Data (ejemplo) |
|---|----------|----------|---------------------|
| 1 | ¿Cuál es el nombre del proyecto? | `{{PROJECT_NAME}}` | NexusPlatform |
| 2 | ¿Cuál es el nombre del módulo central/Kernel? | `{{KERNEL_NAME}}` | NexusCore |
| 3 | ¿Cuáles son los módulos de negocio? (separados por coma) | `{{MODULES}}` | NexusCRM, NexusInventario, NexusRRHH |
| 4 | ¿Qué stack tecnológico usas? | `{{STACK}}` | .NET 10 + C# 14 |
| 5 | ¿Qué base de datos usas? | `{{DATABASE}}` | SQL Server 2022 |
| 6 | ¿Qué framework de testing usas? | `{{TEST_FRAMEWORK}}` | xUnit + Moq |
| 7 | ¿Qué framework UI/CSS usas? | `{{UI_FRAMEWORK}}` | Tailwind CDN |
| 8 | ¿Qué estrategia de autenticación usas? | `{{AUTH_STRATEGY}}` | Cookie Authentication con PasswordHasher |
| 9 | ¿Cuáles son los esquemas de DB por módulo? | `{{DB_SCHEMAS}}` | nexus_core, nexus_crm, nexus_inv, nexus_rrhh |

### Paso 2: Generar los 5 archivos

Genera los siguientes archivos en el directorio de trabajo actual:

---

#### Archivo 1: `Proyecto {{PROJECT_NAME}}.md`

```markdown
# Mapa del Proyecto: Ecosistema {{PROJECT_NAME}}

> **Contexto técnico:** Ver [[stack]]
> **Reglas de desarrollo:** Ver [[agent]]
> **Estado de avance:** Ver [[progress]]

---

## Filosofía de Desarrollo

* **Gobernanza desde el Kernel:** Ningún módulo decide de forma autónoma si está disponible o no. Todo se valida consultando la infraestructura de `{{KERNEL_NAME}}`.
* **Simplicidad Extrema (KISS):** Evita la sobre-ingeniería. El encendido/apagado se gestiona mediante un filtro global que lee un flag del módulo en base de datos o caché.
* **Gestión de Errores:** No uses excepciones para controlar validaciones de negocio. Utiliza exclusivamente la estructura `Result` o `Result<T>` nativa de .NET 10 para notificar fallos lógicos desde la capa de aplicación.
* **Autenticación:** {{AUTH_STRATEGY}}. `AuthorizeFilter` global.
* **Control de Acceso:** Filtro guardián que verifica:
  1. ¿El módulo está activo en DB?
  2. ¿El rol del usuario autenticado tiene acceso al módulo?

---

## Arquitectura y Componentes

### Capa Core e Infraestructura Central
* **Control Global:** `{{PROJECT_NAME}}.{{KERNEL_NAME}}.Core` — Contenedor del patrón `Result`, entidades de seguridad.
* **Base de Datos Principal:** `{{PROJECT_NAME}}.{{KERNEL_NAME}}.Infrastructure` — `DbContext` central, repositorios, filtro guardián, `AuthService`.
* **Panel de Control:** `{{PROJECT_NAME}}.{{KERNEL_NAME}}.AdminUI` — Razor Class Library con panel administrativo, controladores, vistas.

### Módulos de Negocio
{{#each MODULES}}
* **{{this}}:**
  * Domain → Entidades 3NF y puertos de salida (interfaces)
  * Application → Casos de uso con patrón `Result`
  * Infrastructure → `DbContext` del módulo y repositorios EF Core
  * Presentation → Controlador con atributo de módulo y vistas Razor
{{/each}}

### Calidad y Pruebas
* **Pruebas:** `tests/{{PROJECT_NAME}}.Tests` — Espejo de pruebas unitarias en **{{TEST_FRAMEWORK}}**.

---

## Hoja de Ruta de Construcción

1. 🟩 **Fase 0:** Infraestructura de Testing
2. 🟩 **Fase 1:** El Exoesqueleto ({{KERNEL_NAME}})
{{#each MODULES}}
{{@index_plus_two}}. 🟨 **Fase {{@index_plus_two}}:** Módulo {{this}}
{{/each}}
{{@last_plus_one}}. ⬛ **Fase Final:** Ensamblaje en `{{PROJECT_NAME}}.WebHost`
```

---

#### Archivo 2: `agent.md`

```markdown
# Agent Instructions: Generación de Código Ecosistema {{PROJECT_NAME}}

> **Mapa del proyecto:** Ver [[Proyecto {{PROJECT_NAME}}]]
> **Stack tecnológico:** Ver [[stack]]
> **Estado actual:** Ver [[progress]]
> **Historial de fases:** Ver [[history]]

---

## 1. Filosofía de Desarrollo Obligatoria

* **Gobernanza desde el Kernel:** Ningún módulo decide de forma autónoma si está disponible o no. Todo se valida consultando la infraestructura de `{{KERNEL_NAME}}`.
* **Simplicidad Extrema (KISS):** Evita la sobre-ingeniería. El encendido/apagado se gestiona mediante un filtro global que lee un flag del módulo en base de datos o caché.
* **Gestión de Errores:** No uses excepciones para controlar validaciones de negocio. Utiliza exclusivamente la estructura `Result` o `Result<T>` nativa de .NET 10 para notificar fallos lógicos desde la capa de aplicación.
* **Autenticación:** {{AUTH_STRATEGY}}. `AuthorizeFilter` global.
* **Control de Acceso:** Filtro guardián que verifica:
  1. ¿El módulo está activo en DB?
  2. ¿El rol del usuario autenticado tiene acceso al módulo?

---

## 2. Directrices de Testing

* Cada proyecto en `src/` tiene su espejo de tests en `tests/` con el sufijo `.Tests`.
* Framework: **{{TEST_FRAMEWORK}}**.
* Nombre de métodos: `[Método]_[Escenario]_[ResultadoEsperado]`.
* Todo el código nuevo DEBE tener tests unitarios antes de darse por completado.

---

## 3. Hoja de Ruta para la Generación de Código

### Paso 1: Infraestructura Base de {{KERNEL_NAME}}
1. **Core:** Crear la clase contenedora del patrón `Result`.
2. **Infrastructure:** Crear el `{{KERNEL_NAME}}DbContext` asignado al esquema correspondiente.
3. **Filtro Guardián:** Crear un `ActionFilterAttribute` global llamado `ModuleGatekeeperFilter`.
4. **AdminUI:** Crear el proyecto Razor Class Library con el panel administrativo.
5. **Menú Dinámico:** ViewComponent de navegación que oculta/muestra módulos según su estado.

{{#each MODULES}}
### Paso {{@index_plus_two}}: Desarrollo del Módulo {{this}}
1. **Domain:** Generar las entidades en 3NF. Definir interfaz del repositorio.
2. **Application:** Escribir casos de uso con patrón `Result`. Un caso de uso por clase con método `EjecutarAsync`.
3. **Infrastructure:** Configurar el `{{this}}DbContext` apuntando al esquema correspondiente. Implementar repositorio concreto.
4. **Presentation:** Desarrollar controlador con atributo de módulo y vistas Razor.
{{/each}}

### Paso Final: Ensamblaje en {{PROJECT_NAME}}.WebHost
1. Configurar `Program.cs` para inyectar los `DbContext` de cada esquema.
2. Instalar el filtro guardián de forma global en el pipeline de MVC.
3. Generar la barra de navegación del Layout principal.

---

## 4. Decisiones y Reglas Acordadas

> [!note] Historial de sesiones
> Este espacio se llena automáticamente durante el desarrollo.
> Las decisiones acordadas se registran aquí para referencia futura.

* Pendiente de primera sesión de desarrollo.
```

---

#### Archivo 3: `stack.md`

```markdown
# Tech Stack & Architectural Blueprint: {{PROJECT_NAME}} Monolith

> **Mapa del proyecto:** Ver [[Proyecto {{PROJECT_NAME}}]]
> **Reglas de código:** Ver [[agent]]
> **Estado actual:** Ver [[progress]]

---

## 1. Tecnologías Centrales

* **Runtime:** {{STACK}}
* **Enfoque:** Monolito Modular Limpio (Modular Monolith) bajo el principio KISS.
* **Estilo Arquitectónico:** Arquitectura Hexagonal (Ports & Adapters) por cada módulo de negocio.
* **Framework Web:** ASP.NET Core MVC con Vistas Razor integradas.
* **Acceso a Datos:** Entity Framework Core 10 (Code-First) con un `DbContext` por módulo.
* **Base de Datos:** {{DATABASE}} en un único contenedor con aislamiento por esquemas.
* **Autenticación:** {{AUTH_STRATEGY}}.
* **Testing:** {{TEST_FRAMEWORK}}.
* **UI:** {{UI_FRAMEWORK}}.

---

## 2. Gobernanza Central: {{KERNEL_NAME}}

Toda la lógica transversal y el control del sistema se gestionan centralizadamente:

* **Control de Estado:** Una tabla centralizada define si un módulo está activo o inactivo.
* **Control de Acceso (Roles):** Cada rol tiene acceso a módulos específicos.
* **Gobernanza de Acceso (KISS):** Un `ActionFilter` global intercepta los controladores.
* **UI Dinámica:** La barra de navegación principal consulta al Kernel.
* **Control de Errores:** Patrón `Result<T>` para flujos lógicos erróneos.

---

## 3. Modelo de Datos de Base de Datos (3NF Estricta)

> [!important] Esquemas por módulo
> Cada módulo tiene su propio esquema en la misma base de datos para facilitar el aislamiento.

### Esquema: `{{DB_SCHEMAS[0]}}` (Gobernanza de {{KERNEL_NAME}})
* **Modulos:** `Id` (PK), `CodigoModulo` (Unique), `Nombre`, `EstaActivo` (Bit)
* **Usuarios:** `Id` (PK), `Username` (Unique), `PasswordHash`, `NombreCompleto`, `Email`, `EstaActivo`
* **Roles:** `Id` (PK), `Nombre` (Unique), `EstaActivo`
* **UsuarioRoles:** `UsuarioId` (FK), `RolId` (FK) — PK compuesta
* **RolModulos:** `RolId` (FK), `CodigoModulo` (FK) — PK compuesta

{{#each MODULES}}
### Esquema: `{{DB_SCHEMAS[@index_plus_one]}}` (Módulo {{this}})
* Entidades principales del módulo en 3NF
* Configuración Fluent API en `DbContext`
* `DeleteBehavior.Restrict` en todas las FKs de negocio
{{/each}}

> [!tip] Diseño de datos
> Organiza las entidades en esquemas independientes para facilitar el aislamiento. Usa `virtual` para lazy loading en navegaciones.

---

## 4. Estructura de la Solución

```text
{{PROJECT_NAME}}/
├── src/
│   ├── BuildingBlocks/
│   │   └── {{KERNEL_NAME}}/
│   │       ├── {{PROJECT_NAME}}.{{KERNEL_NAME}}.Core/
│   │       │   └── Result.cs
│   │       ├── {{PROJECT_NAME}}.{{KERNEL_NAME}}.Infrastructure/
│   │       │   ├── {{KERNEL_NAME}}DbContext.cs
│   │       │   ├── ModuleGatekeeperFilter.cs
│   │       │   └── AuthService.cs
│   │       └── {{PROJECT_NAME}}.{{KERNEL_NAME}}.AdminUI/
│   │           ├── KernelController.cs
│   │           ├── MenuModulosViewComponent.cs
│   │           └── Areas/Kernel/Views/
│   ├── Modules/
{{#each MODULES}}
│   │   ├── {{this}}/
│   │   │   ├── {{PROJECT_NAME}}.{{this}}.Domain/
│   │   │   │   ├── Entities/
│   │   │   │   └── Interfaces/
│   │   │   ├── {{PROJECT_NAME}}.{{this}}.Application/
│   │   │   │   └── UseCases/
│   │   │   ├── {{PROJECT_NAME}}.{{this}}.Infrastructure/
│   │   │   │   ├── {{this}}DbContext.cs
│   │   │   │   └── Repositories/
│   │   │   └── {{PROJECT_NAME}}.{{this}}.Presentation/
│   │   │       ├── Controllers/
│   │   │       └── Views/
{{/each}}
│   └── Web/
│       └── {{PROJECT_NAME}}.WebHost/
│           ├── Program.cs
│           ├── DbInitializer.cs
│           └── Views/Shared/_Layout.cshtml
```

---

## 5. Estructura de Tests

```text
{{PROJECT_NAME}}/
└── tests/
    └── BuildingBlocks/
        └── {{KERNEL_NAME}}/
            ├── {{PROJECT_NAME}}.{{KERNEL_NAME}}.Core.Tests/
            │   └── ResultTests.cs
            ├── {{PROJECT_NAME}}.{{KERNEL_NAME}}.Infrastructure.Tests/
            │   └── {{KERNEL_NAME}}DbContextTests.cs
            └── {{PROJECT_NAME}}.{{KERNEL_NAME}}.AdminUI.Tests/
                └── KernelControllerTests.cs
```

---

## 6. Estándar de Interfaz Visual (Look & Feel)

> [!note] Estándar UI
> Toda la interfaz web debe heredar un diseño moderno, minimalista y de alta gama.

* **Engine UI (KISS):** {{UI_FRAMEWORK}} cargado vía CDN oficial en el Layout principal.
* **Línea Gráfica Corporativa:**
  * Fondo Base: Blanco puro (`bg-white`) para áreas de trabajo y gris ultra claro (`bg-slate-50`) para fondos.
  * Color Primario: Indigo Tecnológico (`bg-indigo-600` / `text-indigo-600`).
  * Paleta de Estados: Verde Esmeralda (`bg-emerald-100 text-emerald-800`), Ámbar (`bg-amber-100 text-amber-800`), Rojo (`bg-rose-100 text-rose-800`).
* **Componentes UI:**
  * Layout SaaS Dashboard: Barra lateral izquierda fija (`w-64 bg-slate-900 text-slate-300`).
  * Tablas: `rounded-xl border border-slate-200 shadow-sm overflow-hidden`.
  * Formularios: Inputs con `rounded-lg border-slate-300 shadow-sm focus:border-indigo-500`.
  * Botones: Transiciones suaves, primarios en Indigo sólido.

---

## 7. Docker

```yaml
version: '3.8'

services:
  sqlserver:
    image: mcr.microsoft.com/mssql/server:2022-latest
    container_name: {{PROJECT_NAME_LC}}_sqlserver
    environment:
      - ACCEPT_EULA=Y
      - MSSQL_SA_PASSWORD=YourSecurePassword123!
    ports:
      - "1433:1433"
    volumes:
      - {{PROJECT_NAME_LC}}_data:/var/opt/mssql

  webhost:
    image: {{PROJECT_NAME_LC}}-webhost:latest
    build:
      context: .
      dockerfile: src/Web/{{PROJECT_NAME}}.WebHost/Dockerfile
    ports:
      - "8080:80"
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ConnectionStrings__DefaultConnection=Server=sqlserver;Database={{PROJECT_NAME}}Db;User Id=sa;Password=YourSecurePassword123!;TrustServerCertificate=True;
    depends_on:
      - sqlserver

volumes:
  {{PROJECT_NAME_LC}}_data:
```

---

## 8. Roles y Permisos

| Rol | Módulos a los que accede |
|-----|--------------------------|
| `AdminGeneral` | Todos los módulos |
{{#each MODULES}}
| `Admin{{this}}` | Solo {{this}}
{{/each}}

Usuario semilla: `admin@novatech.cl` / `Nexus2026!` con rol `AdminGeneral`.
```

---

#### Archivo 4: `progress.md`

```markdown
# {{PROJECT_NAME}} - Bitácora de Progreso e Iteración (KISS)

> **Historial de fases:** Ver [[history]]
> **Stack tecnológico:** Ver [[stack]]
> **Reglas de código:** Ver [[agent]]
> **Mapa del proyecto:** Ver [[Proyecto {{PROJECT_NAME}}]]

---

## 1. Estado General del Proyecto

* **Proyecto:** {{PROJECT_NAME}} ({{STACK}} + Arquitectura Hexagonal)
* **Fase Actual:** Pendiente de inicio
* **Último hito completado:** Ninguno

---

## 2. Checklist de Implementación

### Fase 0: Infraestructura de Testing
- [ ] **0.1.** Crear proyecto de tests con {{TEST_FRAMEWORK}}
- [ ] **0.2.** Tests del patrón `Result`
- [ ] **0.3.** Tests de infraestructura

### Fase 1: El Exoesqueleto ({{KERNEL_NAME}})
- [ ] **1.1.** Crear clase `Result<T>`
- [ ] **1.2.** Crear `{{KERNEL_NAME}}DbContext` (Esquema: `{{DB_SCHEMAS[0]}}`)
- [ ] **1.3.** Crear `ModuleGatekeeperFilter`
- [ ] **1.4.** Crear panel administrativo (AdminUI)
- [ ] **1.5.** Crear menú dinámico (ViewComponent)

{{#each MODULES}}
### Fase {{@index_plus_two}}: Módulo {{this}}
- [ ] **{{@index_plus_two}}.1.** Dominio (3NF): Entidades e interfaz de repositorio
- [ ] **{{@index_plus_two}}.2.** Aplicación: Casos de uso con patrón `Result`
- [ ] **{{@index_plus_two}}.3.** Infraestructura: `{{this}}DbContext` (Esquema: `{{DB_SCHEMAS[@index_plus_one]}}`) y repositorios
- [ ] **{{@index_plus_two}}.4.** Presentación: Controlador y vistas Razor
{{/each}}

### Fase Final: Orquestación ({{PROJECT_NAME}}.WebHost)
- [ ] **F.1.** Configurar `Program.cs` con inyección de dependencias
- [ ] **F.2.** Configurar Docker y migraciones automáticas
- [ ] **F.3.** Pruebas de integración del contenedor

---

## 3. Bitácora de Iteraciones Recientes

* Pendiente de primera iteración.

---

## 4. Próximo Paso Inmediato

> [!tip] Primer paso
> Inicia con la **Fase 0: Infraestructura de Testing**.
> Crea el proyecto de tests y validaciones del patrón `Result`.
```

---

#### Archivo 5: `history.md`

```markdown
# {{PROJECT_NAME}} — Historial de Fases Completadas

> **Estado actual:** Ver [[progress]]
> **Stack tecnológico:** Ver [[stack]]
> **Reglas de código:** Ver [[agent]]

---

> [!note] Archivo acumulativo
> Este archivo registra todas las fases completadas con fecha y hora.
> Se actualiza automáticamente cuando se completa una fase en [[progress]].
> El historial detallado de cada sesión queda registrado en las secciones de `agent.md`.

---

*No hay fases completadas aún.*
```

---

### Paso 3: Crear `.opencode.json`

Si no existe, crear `opencode.json` en el directorio actual:

```json
{
  "$schema": "https://opencode.ai",
  "mcp": {
    "microsoft-docs": {
      "type": "remote",
      "url": "https://learn.microsoft.com/api/mcp",
      "enabled": true
    }
  }
}
```

### Paso 4: Preguntar sobre copia global

Preguntar al usuario:
> ¿Quieres copiar la skill a `~/.config/opencode/skills/` para que esté disponible en todos los proyectos?

Si responde sí, copiar `ok-init.md` a `%USERPROFILE%\.config\opencode\skills\ok-init.md`.

### Paso 5: Confirmar

Mostrar resumen de archivos creados:
```
{{PROJECT_NAME}}/
├── Proyecto {{PROJECT_NAME}}.md
├── agent.md
├── stack.md
├── progress.md
├── history.md
└── opencode.json (opcional)
```

Preguntar si quiere abrir Obsidian para verificar los links.

---

## Protocolo de Sesión (KISS)

Después de generar los archivos, instruye al usuario sobre el ciclo de trabajo:

### Al INICIO de cada sesión:
1. Leer `progress.md` → fase actual y próximo paso inmediato
2. Leer `agent.md` → reglas vigentes y decisiones acordadas
3. Leer `stack.md` → decisiones técnicas definitivas
4. Usar MCP Context7 si necesitas documentación actualizada

### Durante la sesión:
- Generar código **solo para el paso actual** del `progress.md`
- Si hay ambigüedad, **preguntar al usuario** antes de escribir código
- Dependencies menores: crear stub con `// TODO: Implementar en el paso correspondiente`
- Formato de entrega: ruta completa del archivo como encabezado

### Al FINAL de cada sesión (tras aprobación del usuario):
1. **`progress.md`** → Marcar tarea como completada + establecer próximo paso
2. **`history.md`** → Mover fase completada con fecha (DD/MM/YYYY HH:MM America/Santiago)
3. **`stack.md`** → Documentar decisiones técnicas nuevas (mostrar solo líneas a agregar)
4. **`agent.md`** → Documentar reglas/decisiones nuevas (mostrar solo líneas a agregar)

> [!warning] Regla de oro
> Los archivos **solo se actualizan DESPUÉS de que el usuario apruebe el trabajo**.
> Nunca actualices archivos sin aprobación explícita.

---

## Notas de Implementación

### Reemplazos dinámicos

| Variable | Descripción | Ejemplo |
|----------|-------------|---------|
| `{{PROJECT_NAME}}` | Nombre del proyecto | NexusPlatform |
| `{{PROJECT_NAME_LC}}` | Nombre en minúsculas | nexusplatform |
| `{{KERNEL_NAME}}` | Nombre del módulo central | NexusCore |
| `{{MODULES}}` | Lista de módulos | NexusCRM, NexusInventario, NexusRRHH |
| `{{STACK}}` | Stack tecnológico | .NET 10 + C# 14 |
| `{{DATABASE}}` | Base de datos | SQL Server 2022 |
| `{{TEST_FRAMEWORK}}` | Framework de testing | xUnit + Moq |
| `{{UI_FRAMEWORK}}` | Framework UI | Tailwind CDN |
| `{{AUTH_STRATEGY}}` | Estrategia de auth | Cookie Authentication con PasswordHasher |
| `{{DB_SCHEMAS}}` | Esquemas por módulo | nexus_core, nexus_crm, nexus_inv, nexus_rrhh |

### Formato de fechas

Usar formato: `DD/MM/YYYY HH:MM` con zona horaria `America/Santiago`.

### Obsidian Features

- **Wiki-links:** `[[archivo]]` entre los 5 archivos MD
- **Callouts:** `> [!note]`, `> [!tip]`, `> [!warning]`, `> [!important]`
- **Tags:** `#fase/0`, `#fase/1`, `#modulo/{{MODULE_NAME}}`
- **Graph view:** Se genera automáticamente por los wiki-links

### Validación

Antes de confirmar la creación:
1. Verificar que no existen archivos con el mismo nombre (preguntar si sobreescribir)
2. Validar que las rutas son accesibles
3. Confirmar que Obsidian puede leer los archivos (formato UTF-8)
