# Lesson 008: Controllers (Basics)

## Goal

Move HTTP handling out of route closures into controller actions and understand how controllers receive requests, validate input, persist models, and return HTTP responses.

## Key Concepts

### Controller Responsibility

A controller groups related HTTP actions and keeps route definitions focused on mapping an HTTP method and URI to an action.

Create a controller with Artisan:

```bash
php artisan make:controller ProductController
```

The generated class is placed under `app/Http/Controllers`.

### Mapping a Route to a Controller Action

```php
use App\Http\Controllers\ProductController;

Route::get('/products', [ProductController::class, 'index']);
```

The route maps the request to the `index` method. Laravel's router resolves and invokes the controller action.

### Returning Eloquent Data

A controller can return an Eloquent collection directly:

```php
use App\Models\Product;

public function index()
{
    return Product::all();
}
```

Laravel automatically converts Eloquent models and collections into JSON responses when returned from a route action. Calling `response()->json(...)` explicitly is also valid when explicit response construction is useful.

By default, serialized models include their visible attributes, which can include `created_at` and `updated_at`. Model serialization settings such as `$hidden`, `$visible`, casts, and appended attributes affect the output.

### Route Model Binding in a Controller

A controller action can receive the already-resolved model:

```php
public function show(Product $product)
{
    return $product;
}
```

For a route declared as `/products/{product}`, Laravel resolves the product before calling the action. The default lookup uses the model's route key (normally its primary key). If the record does not exist, Laravel returns 404 without a manual query in the controller.

The route placeholder and typed argument must correspond. Binding resolves a resource; it does not authorize access to it.

### HTTP Request Input

Laravel injects the current request into a controller action through method dependency injection:

```php
use Illuminate\Http\Request;

public function store(Request $request)
{
    // ...
}
```

Important accessors:

- `$request->input('name')` reads input data from the request payload and query string. It is not limited to POST requests.
- `$request->query('page')` reads only query-string parameters (the part of the URL after `?`).
- `$request->header('Accept')` reads a request header.
- `$request->all()` returns all input data.

For example, with `POST /products?page=10` and JSON body `{"page": 20}`, `input('page')` reads the body value while `query('page')` reads the URL value. Prefer the narrowest accessor that expresses the intended source.

### Validation

The request's `validate()` method validates incoming data and returns only the validated fields:

```php
$validated = $request->validate([
    'name' => 'required|string|max:255',
    'price' => 'required|numeric|min:0',
    'description' => 'nullable|string',
]);
```

Rules used in the exercise:

- `required`: field must be present and non-empty.
- `string`: value must be a string.
- `numeric`: value must be numeric.
- `max:255`: for strings, maximum length; for numeric values, maximum numeric value.
- `min:0`: for strings, minimum length; for numeric values, minimum numeric value.
- `nullable`: a null value is allowed.

For requests that expect JSON (for example, with `Accept: application/json`), validation failures are automatically returned as HTTP 422 with an `errors` object. Traditional web requests use redirect-and-error-session behavior instead.

Do not confuse the full request input with validated data. `$request->all()` may contain arbitrary client-supplied fields; the result of `validate()` contains only fields covered by validation rules and accepted by those rules.

### Eloquent Mass Assignment

Mass assignment applies multiple model attributes from an array:

```php
$product->fill($validated);
$product->save();

// Or create and persist in one operation:
$product = Product::create($validated);
```

Use an explicit allowlist in the model when appropriate:

```php
protected $fillable = [
    'name',
    'price',
    'description',
];
```

`$fillable` defines which attributes may be assigned through mass-assignment operations such as `fill()` and `create()`. Attributes not on the allowlist are discarded by default, unless Laravel is configured to throw when unfillable attributes are encountered.

An alternative is `$guarded`, which defines attributes excluded from mass assignment. Prefer one strategy rather than configuring both; an explicit `$fillable` allowlist is a clear default for this project.

The protection applies to mass assignment, not every way of setting a model attribute. Explicit assignment such as `$product->price = 200` followed by `save()` does not use `$fillable`.

During development, Laravel can be configured to throw instead of silently discarding unfillable attributes:

```php
Model::preventSilentlyDiscardingAttributes(
    ! app()->isProduction()
);
```

Validation and mass-assignment protection are separate boundaries: validation checks whether input is acceptable; `$fillable` controls which model attributes can be mass-assigned. Pass validated data to `create()`, not the entire request payload.

### HTTP Responses

An HTTP response has a status code, headers, and (when applicable) a body.

Laravel can serialize a model or collection returned from a route action automatically. Use `response()->json()` when explicitly constructing a JSON response or choosing its status and headers:

```php
return response()->json($product, 201);
```

Common statuses:

- `200 OK`: request succeeded, commonly used for reads and successful updates with a response body.
- `201 Created`: a resource was created.
- `204 No Content`: request succeeded and the response has no body.
- `404 Not Found`: requested resource does not exist.
- `422 Unprocessable Content`: request input failed validation.

The application controls the response status. Returning `null` with an explicitly selected 200 status does not automatically turn it into 404. Use mechanisms such as implicit model binding or `findOrFail()` when a missing resource should produce 404.

### Product Store Action

The exercise combined request input, validation, mass assignment, persistence, and an explicit creation response:

```php
public function store(Request $request)
{
    $validated = $request->validate([
        'name' => 'required|string|max:255',
        'price' => 'required|numeric|min:0',
        'description' => 'nullable|string',
    ]);

    $product = Product::create($validated);

    return response()->json($product, 201);
}
```

Route:

```php
Route::post('/products', [ProductController::class, 'store']);
```

The action validates input before persistence, passes only validated fields to Eloquent, and returns the created model with HTTP 201.

## Common Mistakes

- Keeping substantial request-handling logic inside route closures.
- Assuming `input()` reads only POST body data; it also considers query-string input.
- Using `all()` as the data source for persistence instead of the validated result.
- Assuming validation replaces model mass-assignment protection, or vice versa.
- Assuming `$fillable` protects explicit attribute assignment followed by `save()`.
- Configuring both `$fillable` and `$guarded` without a clear reason.
- Returning HTTP 200 for a newly created resource when 201 is appropriate.
- Assuming Laravel changes an explicitly selected status based on arbitrary business logic.
- Returning every model attribute without considering API data exposure.

## Production Considerations

- Keep controllers focused on HTTP concerns: accepting input, coordinating application behavior, and returning responses.
- Do not move complex business workflows into controllers; introduce a dedicated class when the use case warrants a separate boundary.
- Use validated data as the input boundary for persistence.
- Keep model mass-assignment configuration explicit.
- Enable exceptions for silently discarded attributes in development to catch mistakes early.
- Be deliberate about model serialization. For stable public APIs, API Resources provide an explicit response contract and reduce accidental data exposure.
- Use Form Requests for non-trivial request validation and authorization; this will be covered separately.
- For browser-facing routes in the web middleware group, retain CSRF protection. API routes and web routes have different middleware expectations; do not disable CSRF globally just to make a Postman request pass.

## Summary

Controllers provide a structured home for HTTP actions. Laravel injects Request objects, validates input, supports mass-assignment safeguards in Eloquent, and provides explicit response construction. The basic store flow is: validate request input, pass only validated data to `create()`, and return the created resource with HTTP 201.
