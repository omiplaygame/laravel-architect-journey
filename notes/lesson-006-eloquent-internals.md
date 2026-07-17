# Lesson 006: Eloquent Internals

## Goal

Understand how Eloquent models store state, detect changes, hydrate database rows, and delegate query work while preserving a model-level API.

This lesson explains why Eloquent models do not behave like simple PHP objects with public properties for every database column. Eloquent is an active record ORM, so a model must track both current data and database state.

## Concepts

### Model State

An Eloquent model stores column values inside internal state, mainly the `attributes` array.

When a model has a `name` column, the value is not stored as a normal public property:

```php
$user->name;
```

This access looks like property access, but Eloquent resolves it dynamically.

### Attributes vs Original

`attributes` represents the current in-memory state of the model.

`original` represents a snapshot of the model as it was loaded from the database or last synchronized after saving.

Conceptually:

```text
attributes = current model values
original   = database snapshot
```

Eloquent compares these two states to decide what changed.

### Dynamic Access with `__get` and `__set`

Eloquent uses PHP magic methods to make model attributes feel like normal properties.

`__get` reads values from the model's internal attribute storage.

`__set` writes values into the model's internal attribute storage.

That means this code:

```php
$user->email = 'new@example.com';
echo $user->email;
```

is effectively routed through Eloquent's attribute system, not through a public `$email` property.

### Dirty Checking

Dirty checking is the process of comparing current `attributes` against `original`.

If a value exists in `attributes` and differs from the same value in `original`, Eloquent treats the attribute as dirty.

When `save()` runs, Eloquent can update only changed fields instead of blindly updating every column.

### Hydration

Hydration is the process of turning database rows into model instances.

Hydration is different from normal assignment. When Eloquent hydrates a model, it loads database values directly into model state and marks them as the original database snapshot.

This matters because hydrated values should not immediately be considered dirty. They came from the database, so they represent the clean baseline.

### `save()` and SQL Updates

When `save()` is called on an existing model, Eloquent checks whether the model is dirty.

If no attributes changed, a second `save()` usually does not issue an `UPDATE` query.

Example:

```php
$user = User::find(1);
$user->name = 'Alex';
$user->save(); // usually issues UPDATE
$user->save(); // usually no UPDATE because nothing changed
```

After the first successful save, Eloquent synchronizes the current attributes into `original`, so the model becomes clean again.

### Convention Over Configuration

Eloquent uses conventions to reduce repetitive configuration.

By default, a model class name maps to a plural snake-case table name:

```text
User -> users
BlogPost -> blog_posts
```

You can override this with `$table`, but the default convention keeps most models simple.

### Eloquent Builder and Query Builder

Eloquent Builder is a model-aware wrapper around the lower-level Query Builder.

Query Builder knows how to build SQL.

Eloquent Builder keeps model-level knowledge: model class, table conventions, hydration, scopes, relationships, and model instances.

The important separation:

```text
Query Builder    = SQL construction
Eloquent Builder = model-aware querying
Model            = model state and domain-facing API
```

## Architecture Breakdown

Eloquent separates responsibilities across layers.

| Part | Responsibility |
| --- | --- |
| Model | Stores model state, exposes attribute access, knows table conventions, coordinates persistence. |
| `attributes` | Holds current in-memory values. |
| `original` | Holds the clean database snapshot. |
| Dirty checking | Decides which values changed. |
| Hydration | Creates model instances from database rows without marking them dirty. |
| Eloquent Builder | Provides model-aware query operations. |
| Query Builder | Builds SQL queries and talks to the database layer. |

This design lets Eloquent look simple at the call site while preserving enough internal state to make persistence efficient and predictable.

## Key Trade-Offs

### Dynamic Attributes vs Public Properties

Dynamic attributes allow Eloquent to support casting, accessors, mutators, dirty checking, hidden attributes, appended attributes, relationships, and mass assignment rules.

Public properties would be simpler to understand at first, but they would bypass the machinery that makes the ORM useful.

Trade-off:

- Dynamic attributes are more magical.
- Public properties are simpler but too limited for a production ORM.

### Convention vs Explicit Configuration

Conventions reduce boilerplate and make common cases fast.

Explicit configuration is still available when the database schema does not follow Laravel conventions.

Trade-off:

- Convention keeps code clean.
- Explicit configuration handles legacy or unusual schemas.

### Model-Aware Builder vs Raw Query Builder

Eloquent Builder gives a rich model-level API.

Query Builder gives lower-level SQL control.

Trade-off:

- Eloquent Builder improves expressiveness and hydration.
- Query Builder can be clearer for complex SQL-heavy operations.

## Common Mistakes

- Thinking `$model->name` is a real public property.
- Forgetting that `attributes` and `original` are different states.
- Assuming every `save()` call issues an `UPDATE`.
- Treating hydration like manual assignment.
- Overriding conventions too early instead of following Laravel defaults.
- Putting SQL-specific thinking into the model when Query Builder is the better layer.
- Ignoring dirty checking when debugging why a database update did or did not happen.

## Production Notes

- Dirty checking helps reduce unnecessary writes.
- Fewer unnecessary updates can reduce database load and avoid touching timestamps unexpectedly.
- Hydration must preserve clean state, otherwise every loaded model would appear changed.
- Dynamic attributes enable casts, accessors, mutators, encryption, JSON columns, and custom value transformations.
- Understanding Eloquent internals makes debugging persistence bugs much easier.
- Senior Laravel code should respect Eloquent conventions but know when to drop down to Query Builder for performance or query clarity.

## Summary

Eloquent models are not simple PHP objects with public properties for columns. They are stateful ORM objects.

Current values live in `attributes`. The database snapshot lives in `original`. Magic methods like `__get` and `__set` route property-style access through Eloquent's attribute system. Dirty checking compares current state to original state so `save()` can update only changed fields.

Hydration creates models from database rows without making them dirty. Eloquent conventions map model names to table names by default. Eloquent Builder wraps Query Builder to provide a model-aware querying API while delegating SQL construction to the lower-level query layer.
