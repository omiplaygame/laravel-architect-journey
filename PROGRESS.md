# Progress

This file tracks the current learning state and should be updated after every lesson.

## Current State

| Field | Value |
| --- | --- |
| Current lesson | Lesson 015: Middleware Basics |
| Current milestone | Middleware |
| Overall progress percentage | 20% |
| Last updated | 2026-11-25 |

## Completed Lessons

- Lesson 006: Eloquent Internals
- Lesson 007: Routing and Route Groups
- Lesson 008: Controllers (Basics)
- Lesson 009: Form Requests
- Lesson 010: Validation Rules
- Lesson 011: Request Authorization and Input Normalization
- Lesson 012: API Resources
- Lesson 013: Resource Collections and Response Structure
- Lesson 014: API Response Envelope and Resource Wrapping

## In Progress

- Lesson 015: Middleware Basics — understand how middleware participates in the HTTP request/response pipeline.

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

### Request Authorization and Input Normalization

- Form Request `authorize()` determines whether a request may proceed; a denied request returns HTTP 403 before the controller action.
- Authentication, resource binding, authorization, and validation are separate concerns.
- `prepareForValidation()` runs before validation and can normalize request input with `input()` and `merge()`.
- Guard string normalization with `is_string()` when input may be missing or malformed.
- Standard `TrimStrings` middleware may trim input before the Form Request; normalization such as lowercasing email is an explicit application policy.

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

### API Resources

- `JsonResource::toArray()` defines an explicit public representation of a model.
- Returning `new UserResource($user)` directly lets Laravel create the JSON response.
- Single Resources are wrapped in `data` by default. A Resource class can customize its wrapper with `$wrap`.
- `Resource::collection($paginator)` transforms each model while preserving pagination links and metadata.
- Paginated Resource responses contain `data`, `links`, and `meta`; pagination fields such as `current_page` are inside `meta`.
- `paginate(10)` returns a length-aware paginator; page parameters select records, and an out-of-range page may have an empty `data` array.
- Model serialization and the API contract are separate; explicitly selecting fields prevents accidental exposure when the database model grows.
- `JsonResource::withoutWrapping()` removes the default wrapper for single Resources and non-paginated collections; paginated Resources retain their `data`, `links`, and `meta` structure.
- Global Resource wrapping changes affect the API contract and should be configured consistently.

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
- Distinguishing authorization from validation and verifying HTTP 403 behavior.
- Normalizing request data at the request boundary before validation.
- Designing explicit API output contracts with JsonResource.
- Applying the same Resource representation to single models and paginated collections.
- Reading pagination links and metadata as part of an API response contract.
- Choosing and verifying a consistent Resource wrapping strategy, including the special structure of paginated responses.


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
- Lesson 011: Request Authorization and Input Normalization — verified HTTP 403 for denied authorization and normalized input before validation.
- Lesson 012: API Resources — generated UserResource, explicitly selected public fields, returned the Resource directly, and verified the `data` envelope and ISO 8601 timestamp.
- Lesson 013: Resource Collections and Response Structure — tested paginator directly and through `UserResource::collection()`, verified page navigation, Resource field filtering, pagination metadata under `meta`, and empty data for an out-of-range page.
- Lesson 014: API Response Envelope and Resource Wrapping — verified custom `$wrap`, disabled wrapping for a single Resource, confirmed paginated Resources retain `data`, and restored the default configuration.

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
