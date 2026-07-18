# Okk_Init — Project Governance Skill

**Name:** ok-init
**Triggers:** `okk init`, `okk sigamos`, `okk sync`, `okk status`, `okk commit`
**Description:** Generates a 5-file governance system for any project with Obsidian wiki-links and a KISS workflow protocol.

---

## Instructions for the AI

When the user activates this skill, you must:

### Step 0: Detect if project already exists

Before asking questions, check if governance files exist in the current directory:

1. Search for `Proyecto *.md` in the current directory
2. Search for `agent.md`, `stack.md`, `progress.md`, `history.md`

#### If files do NOT exist → Ask:

> [!note] New project
> No governance files found in this directory.

| Option | Description |
|--------|-------------|
| **Ask all** | I'll ask 6 questions to configure everything |
| **Use context** | Generate files based on what we've discussed |

**If "Ask all":**
→ Go to Step 1 (6 questions)

**If "Use context":**
1. Read the entire current conversation
2. Extract decisions made (project name, stack, database, testing, UI, auth)
3. Show summary to user:

> [!tip] Captured decisions
> Here's what I identified from our conversation:
> - **Project:** [name]
> - **Stack:** [technology]
> - **Database:** [database]
> - **Testing:** [framework]
> - **UI:** [framework]
> - **Auth:** [strategy]

4. Ask: "Is this correct? Anything missing?"
5. If data is missing → ask only those questions
6. Generate the 5 files
7. Go to **Session Protocol**

#### If files DO exist → Continue directly:

1. Read `progress.md` → current phase and next step
2. Read `agent.md` → rules and decisions
3. Read `stack.md` → technical decisions
4. Show summary of current state
5. Ask: "What do you want to do now?"
6. Go to **Session Protocol**

---

### Step 1: Collect data (6 questions)

Ask these questions **one by one** or **in groups** (max 3 per message to avoid overwhelm):

| # | Question | Variable | Default |
|---|----------|----------|---------|
| 1 | What's the project name? | `{{PROJECT_NAME}}` | *(required)* |
| 2 | What tech stack? (Enter for default) | `{{STACK}}` | .NET 10 + C# 14 |
| 3 | What database? (Enter for default) | `{{DATABASE}}` | SQL Server 2022 |
| 4 | What testing framework? (Enter for default) | `{{TEST_FRAMEWORK}}` | xUnit + Moq |
| 5 | What UI framework? (Enter for default) | `{{UI_FRAMEWORK}}` | Tailwind CDN |
| 6 | What auth strategy? (Enter for default) | `{{AUTH_STRATEGY}}` | Cookie Authentication |

If user presses Enter without typing, use the default value.

---

### Step 2: Generate the 5 files

Generate the following files in the current working directory:

---

#### File 1: `Proyecto {{PROJECT_NAME}}.md`

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

---

#### File 2: `agent.md`

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
```

---

#### File 3: `stack.md`

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

---

#### File 4: `progress.md`

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

---

#### File 5: `history.md`

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

---

### Step 3: Create `opencode.json`

If it doesn't exist, create `opencode.json` in the current directory:

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

### Step 4: Confirm

Show summary of generated files:

```
{{PROJECT_NAME}}/
├── Proyecto {{PROJECT_NAME}}.md
├── agent.md
├── stack.md
├── progress.md
└── history.md
```

Ask if they want to open Obsidian to verify the links.

---

## Session Protocol (KISS)

After generating files, instruct the user on the workflow:

### At the START of each session:
1. Read `progress.md` → current phase and next step
2. Read `agent.md` → rules and decisions
3. Read `stack.md` → technical decisions
4. Use MCP Context7 if you need documentation

### During the session:
- Generate code **only for the current step** from `progress.md`
- If ambiguous, **ask the user** before writing code
- Minor dependencies: create stub with `// TODO: Implement in corresponding step`
- Delivery format: full file path as header

### At the END of each session (after user approval):
1. **`progress.md`** → Mark task as completed + set next step
2. **`history.md`** → Move completed phase with date (DD/MM/YYYY HH:MM America/Santiago)
3. **`stack.md`** → Document new technical decisions (show only lines to add)
4. **`agent.md`** → Document new rules/decisions (show only lines to add)

> [!warning] Golden rule
> Files are **only updated AFTER user approval**.
> Never update files without explicit approval.

---

## Trigger: "okk sigamos"

If the user types "okk sigamos":

1. Verify governance files exist
2. If they do NOT exist → inform and suggest `okk init`
3. If they DO exist → skip menu and go directly to **Session Protocol**
4. Read `progress.md`, `agent.md`, `stack.md`
5. Show summary of current state
6. Ask: "What do you want to do now?"

---

## Trigger: "okk sync"

If the user types "okk sync":

1. Verify governance files exist
2. If they do NOT exist → inform and suggest `okk init`
3. If they DO exist → proceed with sync audit:

### Sync Rules

| Situation | Action |
|-----------|--------|
| Code exists, MD doesn't document | ADD to MD |
| MD documents, code exists | ✅ Synced (no change) |
| MD documents, code DOESN'T exist | MARK as "Not built" (do NOT delete) |
| MD documents, code was DELETED | MARK as "Removed" (do NOT delete) |

### Sync Flow

1. Read all 5 governance files
2. Scan actual code structure (glob patterns)
3. Compare using sync rules
4. Show report:

```
📊 Sync Report - {{PROJECT_NAME}}

✅ Synced (12):
- Phase 1: Core (complete)
- Entity: Client
- Endpoint: GET /api/clients

➕ New in code (3):
- src/Services/PaymentService.cs
- src/Controllers/WebhookController.cs

⏸️ Pending - Not built (2):
- progress.md: "3.2 Add validation" → code doesn't exist
- agent.md: "Payment module" → not implemented

❌ Removed from code (1):
- stack.md: "LegacyService" → removed
```

5. Ask: "Do you want to update the MD files?"
6. If yes → update MD (keep entries, change status)
7. If no → show report only

> [!warning] Never delete entries
> When code doesn't exist, MARK as "Not built" instead of deleting from MD.
> The MD files represent the plan/vision, not just what's built.

---

## Trigger: "okk status"

If the user types "okk status":

1. Verify governance files exist
2. If they do NOT exist → inform and suggest `okk init`
3. If they DO exist → read all 5 MD files and show dashboard:

### Dashboard Format

```
📊 {{PROJECT_NAME}} - Estado del Arte

Progreso: ████████░░░░░░░░░░░░ 40% (10/25)
Fase actual: 2 - Business Logic
Última sesión: 16/07/2026 - Phase 1 completed
Próximo paso: 2.1 Create domain entities
```

### What to show

| Field | Source |
|-------|--------|
| Progress bar | `progress.md` (count checked/unchecked) |
| Current phase | `progress.md` (current phase section) |
| Last session | `history.md` (most recent entry) |
| Next step | `progress.md` (first unchecked task) |

---

## Trigger: "okk commit"

If the user types "okk commit":

1. Verify governance files exist
2. If they do NOT exist → inform and suggest `okk init`
3. If they DO exist → proceed with commit flow:

### Commit Flow

1. Run `git status` and capture output
2. Classify modified files:
   - **Governance:** `progress.md`, `agent.md`, `stack.md`, `history.md`, `Proyecto *.md`
   - **Code:** All other files (src/, tests/, etc.)
3. Show summary:

```
📝 Governance (3 archivos):
- progress.md (modificado)
- agent.md (modificado)
- history.md (modificado)

💻 Código (5 archivos):
- src/Services/PaymentService.cs (nuevo)
- src/Controllers/WebhookController.cs (modificado)
- tests/PaymentTests.cs (nuevo)
```

4. Ask: **¿Qué quieres commitear?**

| Option | Description |
|--------|-------------|
| **1. Governance** | Only MD files |
| **2. Código** | Only source files |
| **3. Ambos** | Governance + code |
| **4. No** | Cancel |

5. If user selects 1, 2, or 3 → ask: **¿Mensaje de commit?**
   - Option A: User types custom message
   - Option B: Press Enter for auto-suggested message based on modified files

6. Execute:
   - `git add [files]`
   - `git commit -m "[message]"`

7. Show result: `✅ Commit realizado: [hash]`

### Auto-Suggested Messages

| Scenario | Message |
|----------|---------|
| Only governance | `docs: update governance files` |
| Only code | `feat: [based on files modified]` |
| Both | `feat: [code summary] + docs: governance update` |
| Custom | User provides the message |

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

AI: [git add .]
    [git commit -m "feat: payment service + governance update"]
    ✅ Commit realizado: abc1234
```

---

## Implementation Notes

### Dynamic Replacements

| Variable | Description | Example |
|----------|-------------|---------|
| `{{PROJECT_NAME}}` | Project name | NexusPlatform |
| `{{STACK}}` | Tech stack | .NET 10 + C# 14 |
| `{{DATABASE}}` | Database | SQL Server 2022 |
| `{{TEST_FRAMEWORK}}` | Testing framework | xUnit + Moq |
| `{{UI_FRAMEWORK}}` | UI framework | Tailwind CDN |
| `{{AUTH_STRATEGY}}` | Auth strategy | Cookie Authentication |

### Date Format

Use format: `DD/MM/YYYY HH:MM` with timezone `America/Santiago`.

### Obsidian Features

- **Wiki-links:** `[[file]]` between the 5 MD files
- **Callouts:** `> [!note]`, `> [!tip]`, `> [!warning]`, `> [!important]`
- **Tags:** `#fase/0`, `#fase/1`
- **Graph view:** Generated automatically by wiki-links

### Validation

Before confirming creation:
1. Verify no files with the same name exist (ask if overwriting)
2. Validate that paths are accessible
3. Confirm Obsidian can read the files (UTF-8 format)
