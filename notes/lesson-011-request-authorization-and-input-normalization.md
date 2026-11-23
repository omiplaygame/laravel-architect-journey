# Lesson 011: Request Authorization and Input Normalization

## Objectives

- Distinguish authentication, resource binding, authorization, and validation.
- Understand the role of Form Request `authorize()`.
- Prepare request input before validation with `prepareForValidation()`.

## Form Request Authorization

`authorize(): bool` determines whether the request is allowed to proceed.

- Returning `false` causes Laravel to reject the request with HTTP 403.
- The controller action is not invoked.
- Authorization is separate from validation and route model binding.
- Route Model Binding identifies a resource; it does not grant permission to access it.
- `$this->user()` refers to the authenticated principal, while a route parameter such as `$this->user` may resolve to the route-bound User model. Avoid confusing the two.

A real ownership check can compare the authenticated user with the route-bound model, but requires authentication to be configured. For larger authorization rules, use a Policy rather than accumulating complex logic in the request.

## Input Normalization

`prepareForValidation()` runs before validation and can normalize request data:

```php
protected function prepareForValidation(): void
{
    $email = $this->input('email');

    if (is_string($email)) {
        $this->merge([
            'email' => mb_strtolower(trim($email)),
        ]);
    }
}
```

- `input()` explicitly reads an input value.
- Check its type before applying string functions when input may be missing or malformed.
- `merge()` updates the request's input representation; it does not alter the HTTP payload already sent by the client.
- Validation then evaluates the prepared value.
- Laravel's standard `TrimStrings` middleware may already trim strings. Lowercasing email is an application-level normalization policy, not a universal requirement.

## Verified Practice

- Set `authorize()` to `false` and observed HTTP 403.
- Restored authorization and verified the request flow.
- Implemented normalization using `input()`, a string check, `trim()`, `mb_strtolower()`, and `merge()`.
- Observed the request input change before validation.

## Engineering Notes

Keep request authorization focused on whether the current actor may perform the action. Keep normalization in the request boundary, validation in rules, and business behavior in the application/controller layer.