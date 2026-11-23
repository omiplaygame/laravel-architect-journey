# Progress

This file tracks the current learning state and should be updated after every lesson.

## Current State

| Field | Value |
| --- | --- |
| Current lesson | Lesson 010: Validation Rules |
| Current milestone | Requests |
| Overall progress percentage | 11% |
| Last updated | 2026-11-23 |

## Completed Lessons

- Lesson 006: Eloquent Internals
- Lesson 007: Routing and Route Groups
- Lesson 008: Controllers (Basics)
- Lesson 009: Form Requests

## In Progress

- Lesson 010: Validation Rules — optional fields (`sometimes`, `required`, `nullable`, `filled`), database-backed `unique` and `exists` rules, and their relationship to Route Model Binding, authorization, and database constraints. Request authorization and input normalization remain to be covered in the Requests milestone.

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



### Form Requests

- Form Requests encapsulate request-specific validation and authorization through `rules()` and `authorize()`.
- Laravel resolves and validates a Form Request before invoking the controller action.
- `validated()` returns accepted fields; `safe()` provides a ValidatedInput wrapper for selective access.
- Authentication, authorization, and validation are distinct concerns.
- A route placeholder such as `{user}` is resolved to a model when the controller has a matching typed argument such as `User $user`.
- A missing route-bound model produces HTTP 404; binding does not authorize access.
- Form Request validation errors return HTTP 422 for requests that expect JSON; browser-style requests may use redirect-and-error-session behavior.

### Validation Rules

- `sometimes` controls whether field rules run when the field is present; it does not itself require a non-empty value or reject null.
- `required` requires a present, non-empty value; `nullable` allows null but does not override `required`.
- `filled` requires a non-empty value when the field is present, without requiring the field to be present.
- Standard Laravel request middleware commonly converts empty strings to null before validation.
- `unique:users,email` checks uniqueness for a create request.
- `Rule::unique('users', 'email')->ignore($this->user)` excludes the route-bound current model when validating an update.
- A trusted model from Route Model Binding should be passed to `ignore()`; do not use an untrusted user-provided ID.
- `Rule::exists('users', 'id')` verifies that a referenced row exists.
- `exists` does not authorize use of the referenced record.
- Database UNIQUE constraints and FOREIGN KEY constraints remain the final integrity boundary; validation alone can be subject to race conditions.

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


- Separating request-specific validation from controller actions with Form Requests.
- Combining Form Request validation with route-bound models for update contracts.
- Designing PATCH validation around omitted, null, and non-empty values.
- Using database-backed `unique` and `exists` validation while preserving separate authorization and database-integrity boundaries.

## Practice Completed

- Lesson 006: Eloquent Internals — practice tasks recorded.
- Lesson 007: Routing and Route Groups — route parameters, implicit and explicit binding, route groups, named routes, and throttling verified.
- Lesson 008: Controllers (Basics) — controller creation, index action, Eloquent collection serialization, show action with implicit binding, Request input, validation, Mass Assignment, response semantics, and Product store implementation verified. The user confirmed successful and invalid inputs, omitted and null description, and extra fields behaved as expected.


- Lesson 009: Form Requests — created and injected Form Requests, verified validated data, route-bound update model resolution, HTTP 404 for a missing model, and JSON validation HTTP 422 with the appropriate Accept header.
- Lesson 010: Validation Rules — verified `sometimes`, `required`, `nullable`, `filled`, unique email on create, `Rule::unique()->ignore()` on update, and `Rule::exists()` against the users table.

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
