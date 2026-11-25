# Lesson 014 Practice: API Response Envelope and Resource Wrapping

## Completed

- [x] Set a custom `$wrap` on `UserResource` and inspect a single-resource response.
- [x] Return a single Resource and confirm the configured wrapper.
- [x] Return `UserResource::collection($paginator)` and inspect `data`, `links`, and `meta`.
- [x] Configure `JsonResource::withoutWrapping()` in `AppServiceProvider::boot()`.
- [x] Confirm wrapping is removed from a single Resource.
- [x] Confirm pagination keeps its `data` envelope.
- [x] Remove the temporary global configuration and restore Laravel's default wrapping.

## Expected Understanding

Explain why disabling wrapping for a single Resource does not remove the `data` key from a paginated Resource response, and why a stable response shape matters to API clients.
