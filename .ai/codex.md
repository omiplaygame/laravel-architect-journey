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

## Future Lesson Update Contract

Future ChatGPT lesson specifications may be short. Codex should infer repository updates from the existing structure.

A typical future specification may include only:

```text
Lesson:
Milestone:
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
