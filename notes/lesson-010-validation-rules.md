# Lesson 010: Validation Rules

## Goal

Understand conditional and nullable validation, database-backed uniqueness and existence checks, and how validation rules interact without confusing their responsibilities.

## Key Concepts

### Optional Fields: `sometimes`, `required`, `nullable`, and `filled`

These rules have different responsibilities:

- `sometimes`: apply the field's validation rules only when the field is present in the input.
- `required`: the field must be present and non-empty.
- `nullable`: a null value is allowed; ordinary non-implicit rules are skipped for that null value.
- `filled`: if the field is present, it must not be empty; it does not require the field to be present.

Examples:

```php
// May be omitted; if supplied, it must be a non-empty string.
'summary' => 'sometimes|required|string',

// May be omitted; if supplied, null or a string is accepted.
'summary' => 'sometimes|nullable|string',
```

`sometimes` does not itself require a supplied value to be non-empty and does not reject null. In `sometimes|string`, a present null fails because the `string` rule does not accept null. In `sometimes|nullable|string`, `nullable` permits null.

A field with `nullable|string` can also be omitted when no rule requires its presence. `nullable` does not override `required`; `required|nullable|string` still rejects a null value because the required rule fails.

With Laravel's standard request middleware, an empty string is commonly converted to null before validation. If that middleware is disabled or changed, the validator may receive the empty string directly; an empty string is still a string, though rules such as `required` or `filled` reject it.

For PATCH requests, omission and explicit null can represent different intentions: omitted means do not update that attribute; null may mean clear it. The validated array preserves the distinction between an absent key and a supplied key with null.

### Unique Values

A string rule can check a value against a database table:

```php
'email' => 'required|email|unique:users,email',
```

The format is `unique:table,column`. An object-based rule is useful for more complex conditions:

```php
use Illuminate\Validation\Rule;

'email' => [
    'required',
    'email',
    Rule::unique('users', 'email'),
],
```

For updates, exclude the current model from the uniqueness check:

```php
Rule::unique('users', 'email')->ignore($this->user)
```

Here `$this->user` is the model resolved from the matching route parameter through Route Model Binding. This allows the user to keep their own email while still rejecting another user's email.

Do not pass an untrusted user-supplied ID directly to `ignore()`; use a trusted model or identifier resolved by the application.

### Existing Records

The `exists` rule checks that a value matches a record in a database table:

```php
'category_id' => [
    'required',
    'integer',
    Rule::exists('categories', 'id'),
],
```

The format is `exists:table,column`.

A successful `exists` check means the record exists; it does not mean the current user is authorized to use it.

### Validation vs Database Integrity

Database-backed validation is useful for clear request errors, but it is not a substitute for database constraints:

- `unique` validation should be backed by a UNIQUE constraint/index when uniqueness is an integrity requirement.
- `exists` validation should be backed by a FOREIGN KEY when a relational reference must remain valid.

A race can occur between validation and persistence. The database constraint remains the final integrity boundary. The application should also handle relevant database constraint exceptions appropriately.

### Validation vs Authorization

- `exists`: does the referenced record exist?
- Authorization: may this caller use or modify that record?
- Foreign key: does the database reference point to a valid row?

These checks solve different problems and may all be necessary.

## Common Mistakes

- Treating `sometimes` as a non-empty requirement; use `required` when that is the contract.
- Assuming `sometimes` rejects null.
- Assuming `nullable` makes a field required or overrides `required`.
- Forgetting that empty strings may be converted to null by request middleware.
- Using `unique` on an update without excluding the current record.
- Passing user-controlled IDs to `Rule::unique()->ignore()`.
- Treating `exists` as authorization.
- Relying only on validation for database uniqueness or referential integrity.

## Practice Verified

- Compared `sometimes|string` with `sometimes|nullable|string` for missing, null, empty-string, string, and invalid-type values.
- Verified `required` and `nullable` do not override one another.
- Verified `unique:users,email` rejects an already-used email.
- Verified `Rule::unique('users', 'email')->ignore($this->user)` allows the current user's email and rejects another user's email.
- Verified `Rule::exists('users', 'id')` accepts an existing user ID and rejects a missing or invalid ID.
- Observed JSON validation errors with HTTP 422.

## Summary

Validation rules express the request contract, but each rule has a distinct responsibility. Conditional application (`sometimes`), null handling (`nullable`), presence/non-empty requirements (`required` and `filled`), uniqueness (`unique`), and existence (`exists`) should be combined deliberately. Database constraints and authorization remain separate production boundaries.
