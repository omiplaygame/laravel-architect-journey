# Codex Repository Rules

This file is the permanent maintenance contract for `omiplaygame/laravel-architect-journey`.

The repository is a long-term learning system, not a normal code repository. Future ChatGPT lesson specifications are authoritative. When a future specification conflicts with assumptions in this repository, follow the specification while preserving history and documenting the change clearly.

## Core Rules

- Never remove learning history.
- Preserve numbering of lessons, exercises, and mini-projects.
- Always update `ROADMAP.md` after a lesson.
- Always update `PROGRESS.md` after a lesson.
- Every lesson creates lesson notes and practice.
- Keep Markdown formatting consistent.
- Prefer incremental changes.
- Never rewrite existing lessons unless explicitly requested.
- Keep commits focused.
- Preserve educational history.

## Standard Lesson Specification

Future lessons should use the `LAS-0000` format. `LAS` means Learning Architecture System.

The lesson specification is the source of truth for repository updates. Codex should treat it as authoritative and use engineering judgment only to fill reasonable gaps.

Recommended format:

```text
LAS-0007

Milestone:
HTTP Lifecycle

Lesson:
HTTP Kernel

Learning Objectives
- Understand the role of the HTTP Kernel.
- Trace request flow through middleware.
- Explain bootstrapping before controller execution.

Knowledge Acquired
- Laravel HTTP Kernel coordinates request handling.
- Global middleware runs before route middleware.
- Service providers are bootstrapped before request dispatch.

Practice
- Draw the request lifecycle from public/index.php to response.
- Identify global vs route middleware in a sample Laravel app.
- Explain where authentication middleware fits.

Senior Architect Focus
- Request boundaries.
- Middleware ordering.
- Framework lifecycle debugging.
- Production observability.

Status Changes
- HTTP Lifecycle: In Progress

Repository Operations
- Create: notes/lesson-007-http-kernel.md
- Create: practice/lesson-007-http-kernel.md
- Update: ROADMAP.md
- Update: PROGRESS.md

Commit
Add LAS-0007 HTTP Kernel lesson
```

### Required Fields

- `LAS-0000`
- `Milestone`
- `Lesson`
- `Learning Objectives`
- `Knowledge Acquired`
- `Practice`
- `Repository Operations`
- `Commit`

### Recommended Fields

- `Senior Architect Focus`
- `Status Changes`
- `Resources`
- `Mini-Project`
- `Cheatsheet`

### Interpretation Rules

- Use the LAS number as the stable lesson number.
- Convert `LAS-0007` to `lesson-007-*` filenames unless the specification says otherwise.
- Follow `Repository Operations` exactly when provided.
- If `Repository Operations` is incomplete, infer the minimum safe updates from this file.
- Use the `Commit` value as the commit message.
- Do not ask clarifying questions unless the specification is unsafe, destructive, or impossible to apply.

## Future Lesson Update Contract

Future ChatGPT lesson specifications may be short. Codex should infer repository updates from the existing structure.

A typical future specification may include only:

```text
LAS-0007
Milestone:
Lesson:
Topics:
Practice:
Commit:
```

Codex should then:

1. Create a numbered lesson note under `notes/`.
2. Create related practice under `practice/`.
3. Update the relevant milestone status in `ROADMAP.md`.
4. Update `PROGRESS.md`.
5. Preserve all previous lessons.
6. Commit only the files related to that lesson.

## Lesson Artifacts

Each lesson should produce:

- Notes with concepts, examples, trade-offs, and production concerns.
- Practice tasks that reinforce the lesson.
- A progress update.
- Roadmap status changes when appropriate.

Optional artifacts may include:

- Cheatsheets.
- Resource links.
- Mini-project steps.
- Architecture decision notes.

## Numbering Rules

- Use stable lesson numbers.
- Do not renumber completed lessons.
- If a lesson is inserted later, use a suffix such as `lesson-003a`.
- Keep filenames predictable and sortable.

Recommended naming:

```text
notes/lesson-001-topic-name.md
practice/lesson-001-topic-name.md
mini-projects/project-001-name/README.md
```

## Markdown Rules

- Use clear headings.
- Use tables when tracking status.
- Use code blocks for commands and examples.
- Prefer concise lists over long unstructured prose.
- Keep terminology consistent across files.

## Status Rules

Roadmap items use one of these statuses:

- Not Started
- In Progress
- Completed

`PROGRESS.md` should always reflect the current state after each lesson.

## Commit Rules

- Keep commits focused.
- Use clear commit messages.
- Do not mix unrelated lesson updates.
- Do not rewrite history unless the maintainer explicitly asks.

## Production Learning Standard

When adding educational content, include senior-level context where useful:

- Why the concept matters.
- Common mistakes.
- Trade-offs.
- Testing implications.
- Maintainability concerns.
- Scalability concerns.
- Security concerns.
- Laravel best practices.
