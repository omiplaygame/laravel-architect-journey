# Practice: Lesson 008 Controllers (Basics)

## Goal

Move route handling into a controller, return Eloquent data, use implicit Route Model Binding, and implement a basic create endpoint with validation and a deliberate HTTP response.

## Exercises Completed

### 1. Create a Controller

Created `ProductController` with:

```bash
php artisan make:controller ProductController
```

### 2. Move a Route Closure to a Controller

Mapped the products index route to:

```php
Route::get('/products', [ProductController::class, 'index']);
```

Verified that the controller action returned a JSON response.

### 3. Return an Eloquent Collection

Implemented an index action that returns:

```php
return Product::all();
```

Observed that Laravel automatically serialized the Eloquent collection into JSON.

### 4. Show a Product with Implicit Binding

Route:

```php
Route::get('/products/{product}', [ProductController::class, 'show']);
```

Controller action:

```php
public function show(Product $product)
{
    return $product;
}
```

Verified that an existing product was returned and a missing model is resolved as HTTP 404.

### 5. Explore HTTP Request Input

Used a controller action with an injected `Illuminate\Http\Request`.

Verified:
- `input()` reads request input and can read values supplied in the body or query string.
- `query()` reads only URL query parameters after `?`.
- `header()` reads request headers.
- `all()` returns all input data.

Compared a body field and query parameter with the same name.

### 6. Validate Product Input

Rules:

```php
[
    'name' => 'required|string|max:255',
    'price' => 'required|numeric|min:0',
    'description' => 'nullable|string',
]
```

Observed successful validation and JSON validation errors with HTTP 422 for invalid input. Confirmed that the returned validated array contains only fields covered by validation rules.

### 7. Explore Mass Assignment

Configured the Product model with an explicit allowlist:

```php
protected $fillable = [
    'name',
    'price',
    'description',
];
```

Verified that:
- `create()` persists a model using mass assignment.
- An attribute not included in `$fillable` is discarded by default, or causes an exception when strict discarded-attribute handling is enabled.
- Adding `description` to `$fillable` allows it to be mass-assigned.
- `fill()` assigns allowed attributes without persisting the model.
- `save()` persists an already populated model.
- Explicit assignment followed by `save()` does not rely on `$fillable`.

### 8. Compare HTTP Responses

Reviewed:
- `200 OK` for a successful request with a response body.
- `201 Created` for resource creation.
- `204 No Content` for a successful response without a body.
- `response()->json($data, $status)` for explicit JSON response construction.

### 9. Implement and Verify Product Store

Route:

```php
Route::post('/products', [ProductController::class, 'store']);
```

Controller action:

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

The user verified:
- A valid payload creates a Product and returns its JSON representation with HTTP 201.
- Invalid price and missing required name produce the expected validation errors (HTTP 422).
- `description` may be omitted or supplied as null.
- Extra fields such as `is_admin` do not enter the validated data or persisted model.
- The created response includes the generated ID and timestamps.

## Status

Completed — controller basics, Request input, validation, Mass Assignment, response semantics, and the Product store endpoint were implemented and manually verified.
