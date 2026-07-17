# CLAUDE.md

## okk_init - Project Governance

This project uses okk_init governance system with 5 files:
- `Proyecto [NAME].md` - Architectural map
- `agent.md` - Coding rules + decisions
- `stack.md` - Tech stack + data model
- `progress.md` - Current state + next step
- `history.md` - Completed phases

## Commands

Use these slash commands:
- `/okk init` - Initialize or continue project
- `/okk sigamos` - Continue from where we left off
- `/okk sync` - Audit MD vs code
- `/okk status` - Show dashboard
- `/okk commit` - Commit changes

## Rules

- Files are only updated AFTER user approval
- Never delete entries from MD (mark as "Not built")
- Use Obsidian wiki-links between files
- Date format: DD/MM/YYYY HH:MM (America/Santiago)

## Architecture

- **Core:** [ProjectName].Application — Use cases and services
- **Data:** [ProjectName].Infrastructure — Database and repositories
- **Presentation:** [ProjectName].Web — Controllers and views
- **Tests:** tests/[ProjectName].Tests — Unit tests

## Coding Standards

- Use Result pattern for error handling (no exceptions)
- Every new feature MUST have unit tests
- Method naming: [Method]_[Scenario]_[ExpectedResult]
- KISS principle: simplicity first
