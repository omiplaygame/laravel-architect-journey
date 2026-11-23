# Lesson 011 Practice: Request Authorization and Input Normalization

## Authorization experiment

1. In the existing Form Request, temporarily return `false` from `authorize()`.
2. Send the existing PATCH request with `Accept: application/json`.
3. Verify HTTP 403 and confirm the controller action is not reached.
4. Restore the intended authorization result.

## Normalization experiment

1. Add `prepareForValidation()` to the Form Request.
2. Read the email using `input()`.
3. Guard the string operation with `is_string()`.
4. Normalize with `trim()` and `mb_strtolower()`, then merge the value into the request.
5. Inspect the request/validated value and confirm validation receives the normalized input.

## Verified

- HTTP 403 for denied authorization.
- Request input normalized before validation.

## Notes

Email lowercasing is a chosen application policy. Standard Laravel middleware may trim strings before the Form Request runs.