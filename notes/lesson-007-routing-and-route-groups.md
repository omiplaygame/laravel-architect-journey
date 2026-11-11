# Lesson 007: Routing and Route Groups

## Goal

Organize routes, understand how Laravel resolves route parameters, and apply route-level middleware and naming conventions without mixing these independent concerns.

## Key Concepts

### Route Parameters vs Model Binding

A plain route parameter is a value extracted from the URI:

```php
Route::get('/users/{user}', function (string $user) {
    return response()->json(['user' => $user]);
});
```

The value is not automatically a model. It is just a route parameter.

Implicit Route Model Binding resolves a parameter into an Eloquent model when the route placeholder and the typed controller/closure argument match:

```php
use App\Models\User;

Route::get('/users/{user}', function (User $user) {
    return $user;
});
```

Laravel looks up the model using its route key (normally the primary key). If no record exists, Laravel returns 404.

A custom key can be specified in the route:

```php
Route::get('/users/{user:email}', function (User $user) {
    return $user;
});
```

This resolves the user by the `email` column.

### Manual Lookup vs Route Model Binding

This is a manual Eloquent query, not Route Model Binding:

```php
Route::get('/users/{email}', function (string $email) {
    return User::where('email', $email)->firstOrFail();
});
```

Both approaches can produce a 404 when no record exists, but the responsibility differs:

- Manual lookup: application code explicitly performs the query.
- Route Model Binding: Laravel resolves the model before invoking the route action.

### Explicit Route Binding

A custom resolver can be registered by route parameter name:

```php
use Illuminate\Support\Facades\Route;

Route::bind('email', function (string $value) {
    return User::where('email', $value)->firstOrFail();
});
```

The resolver applies to routes using the `{email}` parameter. It is useful when a parameter needs custom resolution behavior.

Binding answers "which resource is this?". It does not answer "is the current user allowed to access it?". Authorization belongs to middleware, Gates, or Policies.

### Route Groups

Groups apply shared route configuration:

```php
Route::prefix('api/v1')
    ->name('api.')
    ->middleware('throttle:10,1')
    ->group(function () {
        Route::get('/users', ...)->name('users.index');
        Route::get('/products', ...)->name('products.index');
    });
```

These group attributes are independent:

| Attribute | Responsibility |
| --- | --- |
| `prefix()` | Adds a URI prefix. |
| `name()` | Adds a prefix to route names. |
| `middleware()` | Applies middleware to the group's routes. |

The resulting route names are `api.users.index` and `api.products.index`. A prefix does not create a controller or change model binding behavior.

### Named Routes

A route name is a stable identifier for generating URLs:

```php
$url = route('api.products.index');
```

Prefer named routes over duplicating URL strings when generating links inside the application.

### Throttling

Middleware can limit request frequency. For example, `throttle:10,1` configures a limit of 10 requests per minute for the applicable route/group. Exceeding the limit produces HTTP 429.

The exact limiter strategy and keying behavior depend on the middleware configuration and Laravel version.

## Common Mistakes

- Treating a plain route parameter as an Eloquent model.
- Confusing manual Eloquent lookup with Route Model Binding.
- Assuming model binding performs authorization.
- Assuming `prefix()`, `name()`, and `middleware()` do the same kind of work.
- Hardcoding URLs instead of using named routes.
- Applying throttling to only one route when the policy should cover a whole group.

## Production Considerations

- Use implicit binding for conventional resource lookup; use explicit binding when resolution rules genuinely differ.
- Keep authorization separate from resource resolution.
- Group routes by shared URI, middleware, or naming policy where it improves clarity.
- Apply rate limits according to endpoint sensitivity and expected traffic; authentication and expensive operations may need different limits.
- Inspect registered routes with `php artisan route:list` when debugging URI, name, middleware, and action configuration.

## Summary

Route parameters, model binding, URI prefixes, route names, and middleware are separate mechanisms. Route Model Binding can resolve a route parameter into a model and produce 404 automatically. Route groups reduce duplication by applying shared configuration, while named routes provide stable URL generation and middleware provides cross-cutting request behavior.
