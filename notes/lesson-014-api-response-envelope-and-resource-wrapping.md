# Lesson 014: API Response Envelope and Resource Wrapping

## Goal

Understand how Laravel JsonResource controls the JSON representation and how wrapping differs between single resources and paginated collections.

## Concepts

### Resource wrapping

- `JsonResource` wraps a single resource in `data` by default.
- A Resource class can customize its wrapper through the static `$wrap` property, e.g. `public static $wrap = 'user';`.
- `JsonResource::withoutWrapping()` disables the default wrapper for top-level single resources and non-paginated resource collections.
- Wrapping configuration is global framework state; use it deliberately and consistently rather than changing it per request.

### Pagination

- A paginated Resource response retains the conventional `data`, `links`, and `meta` structure.
- The `data` key is part of the pagination response shape and remains even when wrapping is disabled.
- Pagination metadata should be treated as part of the API contract, not as incidental output.

## Verified Experiment

1. Set a custom `$wrap` on `UserResource`; a single resource used that wrapper.
2. Returned a paginated collection using `UserResource::collection($paginator)`; it retained `data`, `links`, and `meta`.
3. Added `JsonResource::withoutWrapping()` in `AppServiceProvider::boot()`.
4. Verified that a single Resource no longer had the `data` wrapper.
5. Verified that the paginated collection still retained `data`.
6. Removed the temporary `withoutWrapping()` configuration and restored the default `data` wrapper.

## Production Notes

- Prefer a consistent API response contract. Laravel's default `data` envelope works naturally with pagination.
- Avoid changing global Resource wrapping casually; it affects API response shape and can break clients.
- Keep Resource transformation separate from validation, authorization, and persistence.
