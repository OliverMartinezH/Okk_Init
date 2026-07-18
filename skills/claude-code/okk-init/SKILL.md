---
name: okk-init
description: Sistema de gobernanza para proyectos con Obsidian wiki-links y protocolo KISS. Usar cuando el usuario escriba "okk init", "okk sigamos", "okk sync", "okk status" o "okk commit".
version: 1.0.0
---

# Okk_Init — Project Governance Skill

## Trigger: "okk init"

If the user types "okk init":

1. Verify governance files exist in current directory
2. If they do NOT exist → Ask:
   - What's the project name? (required)
   - What tech stack? (default: .NET 10 + C# 14)
   - What database? (default: SQL Server 2022)
   - What testing framework? (default: xUnit + Moq)
   - What UI framework? (default: Tailwind CDN)
   - What auth strategy? (default: Cookie Authentication)
   - Auto-commit at end of session? (default: Yes)
3. Generate 5 governance files with Obsidian wiki-links
4. If files DO exist → Continue directly (like okk sigamos)

> [!tip] Session preferences
> Question 7 configures whether the agent will ask to commit changes at the end of each session (after updating governance files). This setting is stored in `agent.md` and can be changed later.

> [!warning] Privacy notice
> This mode reads the current conversation to extract project decisions only (project name, stack, database, testing, UI, auth). No sensitive data (API keys, passwords, tokens, personal information) should be extracted or stored. If the conversation contains sensitive information, ask the user to confirm before proceeding.

## Trigger: "okk sigamos"

If the user types "okk sigamos":

1. Verify governance files exist
2. Read `progress.md` → current phase and next step
3. Read `agent.md` → rules and decisions
4. Read `stack.md` → technical decisions
5. Show summary and ask: "What do you want to do now?"

## Trigger: "okk sync"

If the user types "okk sync":

1. Verify governance files exist
2. Read all 5 governance files
3. Scan actual code structure using **explicit glob patterns only** (e.g., `src/**/*.cs`, `tests/**/*.cs`)
4. **Exclude** these paths from scanning:
   - `.git/` — Version control internals
   - `node_modules/`, `bin/`, `obj/`, `dist/` — Build artifacts
   - `.env*`, `*.key`, `*.pem`, `*.p12`, `*.jks` — Secrets and certificates
   - `secrets/`, `credentials/` — Sensitive directories
   - `*.log`, `*.tmp` — Temporary files
5. Compare using sync rules:
   - Code exists, MD doesn't document → ADD to MD
   - MD documents, code exists → ✅ Synced
   - MD documents, code DOESN'T exist → MARK as "Not built"
   - MD documents, code was DELETED → MARK as "Removed"
6. Show sync report
7. Ask: "Do you want to update the MD files?"

> [!warning] Never delete entries
> When code doesn't exist, MARK as "Not built" instead of deleting from MD.
> The MD files represent the plan/vision, not just what's built.

## Trigger: "okk status"

If the user types "okk status":

1. Verify governance files exist
2. Read all 5 governance files
3. Show dashboard:
   - Progress bar (checked/unchecked tasks)
   - Current phase
   - Last session (from history.md)
   - Next step (first unchecked task)

## Trigger: "okk commit"

If the user types "okk commit":

1. Verify governance files exist
2. If they do NOT exist → inform and suggest `okk init`
3. If they DO exist → proceed with commit flow:

### Commit Flow

1. **`git fetch origin`** (silently update remote refs)
2. **Check if behind remote:**
   - Run `git rev-list --count master..origin/master` to get behind count
   - If **behind > 0** → Show warning:
     ```
     ⚠️ Estás [N] commits behind origin/master
     ¿Quieres hacer pull antes de commitear?
     1. Sí, hacer pull
     2. No, continuar sin pull
     ```
     - If user selects **Sí** → Run `git pull`
       - If **conflict** → STOP and warn:
         ```
         ❌ Hay conflictos de merge.
         Resuélvelos manualmente con 'git pull' y luego vuelve a ejecutar 'okk commit'.
         ```
       - If **success** → Continue to step 3
     - If user selects **No** → Continue to step 3
   - If **behind = 0** → Continue silently to step 3
3. Run `git status` and capture output
4. **Check `.gitignore` exists and review its contents**
5. Classify modified files:
   - Governance: `progress.md`, `agent.md`, `stack.md`, `history.md`, `Proyecto *.md`
   - Code: All other files
6. **Secret detection — scan for sensitive files:**

> [!warning] Security: Secret detection
> Before staging files, check if any match these patterns:
> - `.env*`, `.env.local`, `.env.production`
> - `*.key`, `*.pem`, `*.p12`, `*.jks`, `*.keystore`
> - `credentials.*`, `secrets.*`, `*secret*`
> - `appsettings.*.json` (if contains connection strings)
> - `docker-compose*.yml` (if contains passwords)
> - Any file matching: `password`, `token`, `api_key`, `secret`
>
> **If any match:** Show a WARNING to the user and ask for explicit confirmation before staging.

7. Show summary with sensitive file count
8. Ask: What to commit? (Governance / Code / Both / Cancel)
9. Ask: Commit message? (custom or auto-suggest)
10. Execute git add + commit **with specific files only**

> [!danger] Never use `git add .`
> Always stage files explicitly by name. Never use `git add .` or `git add -A` to avoid committing secrets, build artifacts, or unintended files.

11. Show result: `✅ Commit realizado: [hash]`

### Push Flow

12. Check if remote `origin` is configured:
    - Run `git remote get-url origin`
    - **If remote does NOT exist** → Ask: "No hay remote configurado. ¿Quieres crear el repo en GitHub?"
      - **Sí** → Check if `gh` is installed (`gh --version`)
        - If `gh` installed → Execute: `gh repo create {{PROJECT_NAME}} --private --source=. --remote=origin --push`
        - If `gh` NOT installed → Warn: "Necesitas instalar GitHub CLI o configurar el remote manualmente:\n`git remote add origin <URL>`"
      - **No** → End (local commit saved)
    - **If remote EXISTS** → Ask: "¿Quieres push a origin?"
      - **Sí** → Execute `git push`
      - **No** → End

### Example Interaction

```
USUARIO: okk commit

AI: 📝 Governance (2 archivos):
    - progress.md (modificado)
    - agent.md (modificado)

    💻 Código (3 archivos):
    - src/Services/PaymentService.cs (nuevo)
    - src/Controllers/WebhookController.cs (modificado)
    - tests/PaymentTests.cs (nuevo)

    ¿Qué quieres commitear?
    1. Governance
    2. Código
    3. Ambos
    4. No

USUARIO: 3

AI: ¿Mensaje de commit? (o Enter para sugerencia automática)

USUARIO: [Enter]

AI: git add src/Services/PaymentService.cs src/Controllers/WebhookController.cs tests/PaymentTests.cs progress.md agent.md history.md
    git commit -m "feat: payment service + governance update"
    ✅ Commit realizado: abc1234

    ¿Quieres push a origin?
    1. Sí, hacer push
    2. No, solo commit local

USUARIO: 1

AI: git push
    ✅ Push realizado a origin/master
```

## File Templates

### Proyecto [NAME].md

```markdown
# Mapa del Proyecto: {{PROJECT_NAME}}

> **Technical context:** Ver [[stack]]
> **Development rules:** Ver [[agent]]
> **Current status:** Ver [[progress]]

---

## Development Philosophy

* **Simplicity First (KISS):** Avoid over-engineering. Keep things simple and maintainable.
* **Error Handling:** Use a Result Pattern or similar (no exceptions for business validations).
* **Testing:** Every new feature MUST have unit tests before completion.

---

## Architecture

### Core

* **Business Logic:** `{{PROJECT_NAME}}.Application` — Use cases and services
* **Data Access:** `{{PROJECT_NAME}}.Infrastructure` — Database context and repositories
* **Presentation:** `{{PROJECT_NAME}}.Web` — API controllers or UI views

### Quality

* **Tests:** `tests/{{PROJECT_NAME}}.Tests` — Unit tests with **{{TEST_FRAMEWORK}}**

---

## Roadmap

1. Core infrastructure (Result pattern, base entities)
2. Data access layer
3. Business logic
4. Presentation layer
5. Testing
```

### agent.md

```markdown
# Agent Instructions: {{PROJECT_NAME}}

> **Project map:** Ver [[Proyecto {{PROJECT_NAME}}]]
> **Tech stack:** Ver [[stack]]
> **Current status:** Ver [[progress]]
> **History:** Ver [[history]]

---

## 1. Development Philosophy

* **Simplicity First (KISS):** Avoid over-engineering. Keep things simple and maintainable.
* **Error Handling:** Use a Result Pattern or similar (no exceptions for business validations).
* **Testing:** Every new feature MUST have unit tests before completion.

---

## 2. Testing Guidelines

* Framework: **{{TEST_FRAMEWORK}}**
* Method naming: `[Method]_[Scenario]_[ExpectedResult]`
* All new code MUST have unit tests before completion.

---

## 3. Roadmap for Code Generation

### Step 1: Core Infrastructure

1. Create the Result pattern class/container
2. Create base entities
3. Set up project structure

### Step 2: Data Access

1. Create database context
2. Configure entity mappings
3. Implement repositories

### Step 3: Business Logic

1. Create use cases / services
2. Implement business rules with Result pattern
3. Add validation logic

### Step 4: Presentation

1. Create controllers/views
2. Wire up dependency injection
3. Add routing

### Step 5: Testing

1. Write unit tests for each layer
2. Ensure all tests pass

---

## 4. Decisions and Agreed Rules

> [!note] Session history
> This space fills automatically during development.
> Agreed decisions are recorded here for future reference.

* Pending first development session.

---

## 5. Session Preferences

| Setting | Value |
|---------|-------|
| Auto-commit on session end | {{AUTO_COMMIT}} |
| Pull before commit | Yes |

> [!tip] Customize
> Change these values to adjust session behavior.
> `Auto-commit`: Ask to commit at end of session after updating governance files.
> `Pull before commit`: Check if behind remote before committing.
```

### stack.md

```markdown
# Tech Stack: {{PROJECT_NAME}}

> **Project map:** Ver [[Proyecto {{PROJECT_NAME}}]]
> **Rules:** Ver [[agent]]
> **Status:** Ver [[progress]]

---

## 1. Technologies

* **Runtime:** {{STACK}}
* **Approach:** Clean architecture under KISS principle
* **Data Access:** Code-First with migrations
* **Authentication:** {{AUTH_STRATEGY}}
* **Testing:** {{TEST_FRAMEWORK}}
* **UI:** {{UI_FRAMEWORK}}

---

## 2. Data Model

> [!important] Data organization
> Organize your data in separate modules or schemas for better isolation.

### Example Structure

| Entity | Key Fields | Relationships |
|--------|------------|---------------|
| Users | Id, Name, Email | → Roles |
| Roles | Id, Name | → Permissions |

> [!tip] Data design
> Use normalization (3NF) where appropriate. Keep entities focused and minimal.

---

## 3. Project Structure

```text
{{PROJECT_NAME}}/
├── src/
│   ├── {{PROJECT_NAME}}.Core/
│   │   └── Entities/
│   ├── {{PROJECT_NAME}}.Infrastructure/
│   │   ├── DbContext.cs
│   │   └── Repositories/
│   ├── {{PROJECT_NAME}}.Application/
│   │   └── Services/
│   └── {{PROJECT_NAME}}.Web/
│       ├── Controllers/
│       └── Views/
└── tests/
    └── {{PROJECT_NAME}}.Tests/
```

---

## 4. UI Standard

> [!note] Visual standard
> All web interfaces should have a modern, minimalist design.

* **Engine (KISS):** {{UI_FRAMEWORK}}
* **Color Palette:**
  * Primary: Indigo (`bg-indigo-600`)
  * Success: Emerald (`bg-emerald-100 text-emerald-800`)
  * Warning: Amber (`bg-amber-100 text-amber-800`)
  * Error: Rose (`bg-rose-100 text-rose-800`)

---

## 5. Roles and Permissions

| Role | Access |
|------|--------|
| `Admin` | Full access |
| `User` | Limited access |

> Customize this section based on your project's needs.
```

### progress.md

```markdown
# {{PROJECT_NAME}} — Progress Log

> **History:** Ver [[history]]
> **Stack:** Ver [[stack]]
> **Rules:** Ver [[agent]]
> **Project map:** Ver [[Proyecto {{PROJECT_NAME}}]]

---

## 1. Project Status

* **Project:** {{PROJECT_NAME}} ({{STACK}})
* **Current Phase:** Pending start
* **Last milestone:** None

---

## 2. Implementation Checklist

### Phase 0: Testing Infrastructure

- [ ] **0.1.** Create test project with {{TEST_FRAMEWORK}}
- [ ] **0.2.** Unit tests for Result pattern
- [ ] **0.3.** Integration tests for data access

### Phase 1: Core

- [ ] **1.1.** Create Result pattern class
- [ ] **1.2.** Create base entities
- [ ] **1.3.** Set up project structure

### Phase 2: Data Access

- [ ] **2.1.** Create database context
- [ ] **2.2.** Configure entity mappings
- [ ] **2.3.** Implement repositories

### Phase 3: Business Logic

- [ ] **3.1.** Create use cases / services
- [ ] **3.2.** Implement business rules
- [ ] **3.3.** Add validation

### Phase 4: Presentation

- [ ] **4.1.** Create controllers/views
- [ ] **4.2.** Wire up dependency injection
- [ ] **4.3.** Add routing

### Phase 5: Testing & Deployment

- [ ] **5.1.** Write unit tests for all layers
- [ ] **5.2.** Ensure all tests pass
- [ ] **5.3.** Deploy

---

## 3. Recent Iterations

* Pending first iteration.

---

## 4. Next Immediate Step

> [!tip] First step
> Start with **Phase 0: Testing Infrastructure**.
> Create the test project and validate the Result pattern.
```

### history.md

```markdown
# {{PROJECT_NAME}} — Completed Phases History

> **Current status:** Ver [[progress]]
> **Tech stack:** Ver [[stack]]
> **Rules:** Ver [[agent]]

---

> [!note] Accumulative file
> This file records all completed phases with date and time.
> It is automatically updated when a phase is completed in [[progress]].

---

*No phases completed yet.*
```

## Session Protocol

### At the START of each session:

1. Read `progress.md` → current phase and next step
2. Read `agent.md` → rules and decisions
3. Read `stack.md` → technical decisions

### During the session:

- Generate code **only for the current step** from `progress.md`
- If ambiguous, **ask the user** before writing code
- Minor dependencies: create stub with `// TODO: Implement in corresponding step`

### At the END of each session (after user approval):

1. **`progress.md`** → Mark task as completed + set next step
2. **`history.md`** → Move completed phase with date (DD/MM/YYYY HH:MM America/Santiago)
3. **`stack.md`** → Document new technical decisions (show only lines to add)
4. **`agent.md`** → Document new rules/decisions (show only lines to add)
5. **Auto-commit check** → Read `agent.md` section 5 (Session Preferences):
   - If `Auto-commit on session end` = **Yes** → Ask: "¿Quieres commitear los cambios?"
     - **Sí** → Execute `okk commit` flow
     - **No** → End session
   - If `Auto-commit on session end` = **No** → End session without asking

> [!warning] Golden rule
> Files are **only updated AFTER user approval**.
> Never update files without explicit approval.

## Date Format

Use format: `DD/MM/YYYY HH:MM` with timezone `America/Santiago`.

## Obsidian Features

- **Wiki-links:** `[[file]]` between the 5 MD files
- **Callouts:** `> [!note]`, `> [!tip]`, `> [!warning]`, `> [!important]`
- **Tags:** `#fase/0`, `#fase/1`
- **Graph view:** Generated automatically by wiki-links
