# Practice: Lesson 009 Form Requests

## Goal

Create Form Request classes, validate request data before controller execution, and use a route-bound model in an update request.

## Exercises Completed

### 1. Create a Form Request

Created `StoreProductRequest` using Artisan and implemented `authorize()` and `rules()`.

### 2. Move Validation out of the Controller

Injected `StoreProductRequest` into the controller, retrieved `$request->validated()`, created a Product using only validated data, and returned HTTP 201.

Verified that valid input persisted and invalid input produced the expected validation response.

### 3. Create an Update Request

Created `UpdateUserRequest` with an email validation rule and injected it into an update action.

### 4. Combine Route Model Binding and Form Request

Used:

```php
Route::patch('/users/{user}', [UserController::class, 'update']);
```

and a controller argument typed as `User $user`.

Verified:
- Existing user resolves to a model.
- A missing user returns HTTP 404.
- Invalid email returns HTTP 422 when the request includes `Accept: application/json`.

### 5. Verify Request Negotiation

Observed that validation failure behavior depends on whether the request expects JSON. Added the `Accept: application/json` header in Postman to receive a JSON validation response with HTTP 422 instead of a browser-style redirect flow.

## Acceptance Criteria

- Can explain `authorize()` and `rules()`.
- Can explain when a Form Request is resolved and validated.
- Can retrieve only validated fields.
- Can distinguish authentication, authorization, and validation.
- Can combine a Form Request with Route Model Binding.
- Can predict 403, 404, and 422 outcomes for the verified scenarios.

## Status

Completed in the learning session and manually verified.
