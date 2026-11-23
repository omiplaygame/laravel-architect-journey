# Lesson 009: Form Requests

## Goal

Move request-specific validation out of controller actions into dedicated Form Request classes, understand their validation and authorization responsibilities, and connect them with Route Model Binding.

## Key Concepts

### Creating a Form Request

Generate a request class:

```bash
php artisan make:request StoreProductRequest
```

A Form Request extends `Illuminate\Foundation\Http\FormRequest` and normally defines:

- `authorize()`: whether the current request is authorized to proceed.
- `rules()`: validation rules for the incoming data.

Example:

```php
public function authorize(): bool
{
    return true;
}

public function rules(): array
{
    return [
        'name' => 'required|string|max:255',
        'price' => 'required|numeric|min:0',
        'description' => 'nullable|string',
    ];
}
```

Returning `true` is appropriate for an open practice endpoint; production authorization should reflect the application's access policy.

### Injecting a Form Request

Laravel resolves and validates the Form Request before invoking the controller action:

```php
public function store(StoreProductRequest $request)
{
    $validated = $request->validated();

    $product = Product::create($validated);

    return response()->json($product, 201);
}
```

If authorization fails, the request is rejected (typically 403). If validation fails, Laravel returns its standard validation response. For requests expecting JSON, validation errors use HTTP 422. The controller action is not executed when Form Request authorization or validation fails.

`validated()` returns the accepted fields. `safe()` provides a ValidatedInput object when operations such as `only()` or `except()` are useful.

### Authentication, Authorization, and Validation

These are distinct concerns:

- Authentication establishes who the caller is.
- Authorization determines whether that caller may perform the action.
- Validation determines whether supplied data satisfies the request contract.

A Form Request can participate in authorization through `authorize()`, but it is not a universal place for authentication mechanisms, signature verification, or complex business workflows.

### Route Model Binding with a Form Request

A route such as:

```php
Route::patch('/users/{user}', [UserController::class, 'update']);
```

can bind the route parameter to an Eloquent model:

```php
public function update(UpdateUserRequest $request, User $user)
{
    return response()->json([
        'user' => $user,
        'validated' => $request->validated(),
    ]);
}
```

The placeholder `{user}` and typed argument `User $user` correspond. Laravel resolves the model before the controller action and returns 404 if it does not exist.

The Form Request can access the route-bound model using `$this->user`, which is useful for request rules that depend on the resource being updated. Binding resolves the resource; it does not grant permission to modify it.

### Form Requests as Request Contracts

A `StoreProductRequest` and an `UpdateProductRequest` may have different rules because create and update operations have different semantics. Keep separate request classes when their contracts differ; extract shared rules only when that reuse is genuinely helpful.

## Common Mistakes

- Leaving validation rules in the controller after introducing a Form Request.
- Using `$request->all()` instead of validated data for persistence.
- Treating `authorize() === true` as a production authorization policy.
- Assuming Route Model Binding performs authorization.
- Assuming a route placeholder alone resolves a model; a matching typed argument or another binding resolver is required.
- Treating authentication, authorization, and validation as the same concern.

## Production Considerations

- Keep request validation close to the HTTP boundary.
- Use distinct Form Requests when endpoint contracts differ.
- Use route-bound models as trusted resource references, then authorize access separately.
- Prefer validated input for persistence and downstream application operations.
- Use policies or other explicit authorization mechanisms as application access rules grow.

## Summary

Form Requests provide a dedicated request contract for validation and authorization. Laravel resolves them before controller execution. Combined with Route Model Binding, they allow an update request to validate data against the resource identified by the route while keeping validation and resource resolution out of the controller body.
