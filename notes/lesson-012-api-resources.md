# Lesson 012: API Resources

## Objectives

- Understand why API output should be an explicit contract rather than an automatic dump of an Eloquent model.
- Create a `JsonResource` and define its output with `toArray()`.
- Return a Resource directly from a controller.

## Resource Responsibility

A Resource transforms a model or other data object into its API representation.

- It does not validate input.
- It does not persist model changes.
- It controls which fields are exposed and their output shape.
- Explicitly selecting fields prevents newly added database columns from accidentally appearing in the response.

Example:

```php
public function toArray(Request $request): array
{
    return [
        'id' => $this->id,
        'name' => $this->name,
        'email' => $this->email,
        'created_at' => $this->created_at,
    ];
}
```

## Returning a Resource

A controller can return a Resource directly:

```php
public function show(User $user): UserResource
{
    return new UserResource($user);
}
```

Laravel converts it to a JSON response. By default, a single Resource is wrapped in a `data` key:

```json
{
    "data": {
        "id": 1,
        "name": "Test User",
        "email": "test@example.com",
        "created_at": "2026-11-11T10:01:40.000000Z"
    }
}
```

The serialized timestamp is ISO 8601 with a UTC `Z` suffix.

## Verified Practice

- Generated `UserResource` with Artisan.
- Selected `id`, `name`, `email`, and `created_at` explicitly.
- Returned `new UserResource($user)` directly from the controller.
- Observed the default `data` envelope and serialized timestamp.

## Engineering Notes

- Model structure and public API structure are separate concerns.
- Use Resources consistently for endpoints that expose models.
- A manually assembled `response()->json()` can still be useful for intentionally custom response shapes, but avoid mixing ad hoc arrays and Resource output without a clear contract.
- A Resource is a presentation boundary, not a replacement for validation, authorization, or database integrity.