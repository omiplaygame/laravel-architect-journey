# Lesson 008: Controllers (Basics)

## Goal

Move HTTP handling out of route closures into controller actions and understand how controllers receive route parameters and return API responses.

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

## Common Mistakes

- Keeping substantial request-handling logic inside route closures.
- Forgetting to import the controller or model class.
- Assuming `Product::all()` returns a plain PHP array; it returns an Eloquent collection.
- Performing a manual query in a controller when implicit binding is the intended mechanism.
- Treating route model binding as authorization.
- Returning every model attribute without considering API data exposure.

## Production Considerations

- Keep controllers focused on HTTP concerns: accepting input, coordinating application behavior, and returning responses.
- Do not move complex business workflows into controllers; introduce a dedicated class when the use case warrants a separate boundary.
- Be deliberate about model serialization. For stable public APIs, API Resources provide an explicit response contract and reduce accidental data exposure.
- Use Form Requests for non-trivial request validation and authorization; this will be covered separately.
- Prefer explicit visibility and response contracts for APIs where backward compatibility matters.

## Summary

Controllers provide a structured home for HTTP actions. Routes map requests to controller methods, Eloquent collections and models can be serialized automatically, and implicit Route Model Binding can inject a resolved model into an action. Controllers should remain focused and should not become the home of unrelated business logic.
