# Progress

This file tracks the current learning state and should be updated after every lesson.

## Current State

| Field | Value |
| --- | --- |
| Current lesson | Lesson 009: Form Requests |
| Current milestone | Requests |
| Overall progress percentage | 8% |
| Last updated | 2026-11-12 |

## Completed Lessons

- Lesson 006: Eloquent Internals
- Lesson 007: Routing and Route Groups
- Lesson 008: Controllers (Basics)

## In Progress

- Lesson 009: Form Requests — next topic. Learn how to extract validation and request authorization into dedicated request classes, and when that separation is justified.

## Knowledge Acquired

### Eloquent Internals

- Eloquent stores model data in `attributes`, not public column properties.
- `__get` reads from internal attributes and `__set` writes into internal attributes.
- `original` is the clean database snapshot used for change detection.
- Dirty checking compares current `attributes` against `original`.
- Hydration loads database rows into model state without marking them dirty.
- `save()` only updates changed fields for existing models.
- A second `save()` with no changes usually does not issue an `UPDATE`.
- Eloquent uses convention over configuration for default table names.
- Eloquent Builder wraps Query Builder while preserving model-level behavior.
- Mass assignment is a separate model operation governed by `$fillable` or `$guarded`.
- `fill()` performs mass assignment without persistence; `create()` mass-assigns and persists; explicit attribute assignment followed by `save()` does not use mass-assignment rules.

### Routing and Route Groups

- A plain route parameter is a URI value; implicit Route Model Binding resolves it into an Eloquent model.
- Implicit binding uses the model route key by default and returns 404 when no model is found.
- A custom binding key can be declared in the route, such as `{user:email}`.
- Explicit `Route::bind()` registers custom resolution behavior by route parameter name.
- Manual Eloquent lookup with `firstOrFail()` is distinct from Route Model Binding.
- Route Model Binding identifies a resource; authorization must be handled separately.
- Route groups can independently apply URI prefixes, route-name prefixes, and middleware.
- Named routes provide stable URL generation through `route()`.
- Throttling middleware can reject excessive requests with HTTP 429.

### Controllers

- Controllers group related HTTP actions and keep route definitions focused.
- Routes map to controller actions using `[Controller::class, 'method']`.
- Eloquent models and collections can be returned directly and serialized to JSON.
- Model serialization is affected by visibility and serialization configuration.
- Implicit Route Model Binding works in controller actions through a matching route placeholder and typed model argument.
- Missing bound models result in HTTP 404 without a manual lookup in the controller.
- Laravel injects the current `Illuminate\Http\Request` into controller actions.
- `input()` reads request input from the body and query string; `query()` reads only query-string parameters.
- `all()` returns all input data and should not be treated as a safe persistence allowlist.
- `$request->validate()` returns only validated fields and automatically produces HTTP 422 for JSON validation failures.
- `nullable` allows a field to be null; validation rules express the accepted input contract.
- `$fillable` is an explicit allowlist for mass-assignment operations.
- `$guarded` is the alternative denylist strategy; configuring both is generally unnecessary.
- `fill()` performs mass assignment without saving; `create()` performs mass assignment and persistence.
- Explicit model attribute assignment followed by `save()` does not use `$fillable`.
- `preventSilentlyDiscardingAttributes()` can make unfillable mass-assignment mistakes visible during development.
- Validation and mass-assignment protection are separate safeguards.
- `response()->json($data, $status)` explicitly creates a JSON response with a chosen status.
- HTTP 201 is appropriate when a resource is created; HTTP 204 represents success without a response body.
- An explicitly selected response status is not automatically inferred from arbitrary business outcomes.
- A basic `store()` action validates request data, passes only validated fields to `Product::create()`, and returns the created model with HTTP 201.

## Senior Skills Acquired

- Reasoning about ORM state instead of treating models as plain PHP objects.
- Debugging persistence behavior through dirty checking and hydration.
- Explaining trade-offs between dynamic attributes and public properties.
- Separating model-level responsibilities from SQL query construction.
- Recognizing when Laravel conventions improve maintainability.
- Separating resource resolution from authorization.
- Organizing routes by shared URI, naming, and middleware concerns.
- Choosing controller actions over route closures for related HTTP behavior.
- Recognizing model serialization as an API data-exposure boundary.
- Separating untrusted request input from validated application input.
- Applying defense in depth through validation and explicit mass-assignment configuration.
- Choosing HTTP response status codes based on the operation's semantics.
- Implementing and manually verifying a basic resource-creation endpoint.

## Practice Completed

- Lesson 006: Eloquent Internals — practice tasks recorded.
- Lesson 007: Routing and Route Groups — route parameters, implicit and explicit binding, route groups, named routes, and throttling verified.
- Lesson 008: Controllers (Basics) — controller creation, index action, Eloquent collection serialization, show action with implicit binding, Request input, validation, Mass Assignment, response semantics, and Product store implementation verified. The user confirmed successful and invalid inputs, omitted and null description, and extra fields behaved as expected.

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
