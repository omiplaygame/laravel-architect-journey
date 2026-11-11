# Practice: Lesson 007 Routing and Route Groups

## Goal

Demonstrate that you can resolve route parameters, organize routes, apply middleware, and generate URLs using Laravel conventions.

## Exercises Completed

- Compared a plain route parameter with implicit Route Model Binding.
- Used implicit binding with a custom lookup key.
- Distinguished manual Eloquent lookup with `firstOrFail()` from Route Model Binding.
- Registered explicit parameter resolvers with `Route::bind()`.
- Used separate explicit bindings for different user roles and recognized that binding is not authorization.
- Grouped API routes with `prefix()`, `name()`, and `middleware()`.
- Applied throttling and observed HTTP 429 after exceeding the configured limit.
- Generated URLs using named routes and verified route names with `php artisan route:list --name=api`.

## Key Observations

- Implicit binding requires the route placeholder and typed model argument to correspond.
- Missing bound records result in HTTP 404.
- A route group can combine URI prefix, route-name prefix, and middleware without making them the same concern.
- Group name prefixes are concatenated with child route names.
- Middleware can be applied once to a group instead of repeated on every route.

## Acceptance Criteria

- Can explain plain route parameters vs implicit binding.
- Can choose between implicit binding, explicit binding, and manual lookup.
- Can explain why binding is not authorization.
- Can predict resulting URI and route names from a nested route group.
- Can verify middleware and route registration with Artisan.
- Can explain when HTTP 429 is expected.

## Status

Completed in the learning session.
