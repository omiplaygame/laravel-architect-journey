# Lesson 012 Practice: API Resources

## Implementation

1. Run `php artisan make:resource UserResource`.
2. Implement `toArray()` with only the approved public fields:
   - `id`
   - `name`
   - `email`
   - `created_at`
3. Return `new UserResource($user)` directly from a controller action.
4. Send a request in Postman with `Accept: application/json`.

## Verify

- Response contains the expected public fields.
- Sensitive/internal fields such as `password` and `remember_token` are not present.
- The default response contains the `data` envelope.
- `created_at` is serialized as an ISO 8601 UTC timestamp.
- Resource serialization itself does not update the database.

## Result

User implemented the Resource and confirmed the expected JSON response.