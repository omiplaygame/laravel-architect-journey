# Progress

This file tracks the current learning state and should be updated after every lesson.

## Current State

| Field | Value |
| --- | --- |
| Current lesson | Lesson 006: Eloquent Internals |
| Current milestone | Eloquent |
| Overall progress percentage | 6% |
| Last updated | 2026-07-17 |

## Completed Lessons

- Lesson 006: Eloquent Internals

## Knowledge Acquired

- Eloquent stores model data in `attributes`, not public column properties.
- `__get` reads from internal attributes and `__set` writes into internal attributes.
- `original` is the clean database snapshot used for change detection.
- Dirty checking compares current `attributes` against `original`.
- Hydration loads database rows into model state without marking them dirty.
- `save()` only updates changed fields for existing models.
- A second `save()` with no changes usually does not issue an `UPDATE`.
- Eloquent uses convention over configuration for default table names.
- Eloquent Builder wraps Query Builder while preserving model-level behavior.

## Senior Skills Acquired

- Reasoning about ORM state instead of treating models as plain PHP objects.
- Debugging persistence behavior through dirty checking and hydration.
- Explaining trade-offs between dynamic attributes and public properties.
- Separating model-level responsibilities from SQL query construction.
- Recognizing when Laravel conventions improve maintainability.

## Practice Completed

- Practice tasks created for Lesson 006: Eloquent Internals.

## Mini-Projects Completed

No mini-projects completed yet.

## Progress Update Rules

After each lesson, update:

- Current lesson.
- Completed lessons.
- Current milestone.
- Knowledge acquired.
- Senior skills acquired.
- Practice completed.
- Mini-projects completed when applicable.
- Overall progress percentage.
- Last updated.
