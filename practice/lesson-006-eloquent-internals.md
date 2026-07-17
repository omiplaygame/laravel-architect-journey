# Practice: Lesson 006 Eloquent Internals

## Goal

Explain and apply the internal model-state concepts that make Eloquent persistence work.

## Exercises

1. Explain `attributes` vs `original` in your own words.
2. Describe hydration vs manual assignment.
3. Explain why Eloquent uses `__get` and `__set`.
4. Explain why a second `save()` often does nothing.
5. Explain why public properties for columns are a bad idea in Eloquent models.

## Refactoring Task

Given this model usage:

```php
$user = User::find(1);
$user->name = 'Alex';
$user->email = 'alex@example.com';
$user->save();
$user->save();
```

Write a short explanation of:

- Which assignment changes `attributes`.
- What `original` contains after `find(1)`.
- Which fields are dirty before the first `save()`.
- Why the second `save()` usually does not issue an `UPDATE`.

## Debugging Task

Imagine a model is not updating the database after `save()`.

List three possible explanations connected to Eloquent internals:

- The value was not actually changed.
- The attribute was synchronized into `original`.
- The code wrote to a real public property instead of an Eloquent attribute.

Then explain how you would inspect the model state using methods such as:

```php
$model->getAttributes();
$model->getOriginal();
$model->isDirty();
$model->getDirty();
```

## Production Scenario

A team member proposes adding public properties to a model for every database column to make the code more explicit.

Write a review response explaining:

- Why this bypasses Eloquent's attribute system.
- How it can break dirty checking.
- How it can interfere with casts, accessors, and mutators.
- Why Laravel conventions are usually better for model columns.
- When explicit configuration, such as `$table`, is still acceptable.

## Acceptance Criteria

- You can explain `attributes` and `original` without looking at notes.
- You can describe why hydration should not mark a model as dirty.
- You can explain why `save()` is change-aware.
- You can explain the difference between Eloquent Builder and Query Builder.
- You can defend dynamic Eloquent attributes in a code review.

## Reflection Questions

- What debugging signals would tell you a model is dirty?
- When would Query Builder be a better choice than Eloquent Builder?
- Which parts of Eloquent feel like magic, and what internal mechanism explains them?
- How do conventions reduce code, and what risks appear when conventions are not understood?
