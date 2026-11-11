# Practice: Lesson 008 Controllers (Basics)

## Goal

Move route handling into a controller, return Eloquent data, and use implicit Route Model Binding in controller actions.

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

Expected behavior:
- Existing product ID returns HTTP 200 and the serialized product.
- Missing product ID returns HTTP 404.
- The controller does not need a manual `where()`, `find()`, or `firstOrFail()` query.

## Next Exercise: Store a Product

Implement a `store()` action for a POST route.

The exercise will introduce request input and validation boundaries. Do not mark this exercise complete until the endpoint has been implemented and tested.

## Acceptance Criteria So Far

- Controller created with Artisan.
- Route points to the controller action.
- Index returns an Eloquent collection as JSON.
- Show uses implicit Route Model Binding.
- Existing and missing product behavior is verified.

## Status

In Progress — basic controller actions and implicit binding completed; `store()` is the next exercise.
