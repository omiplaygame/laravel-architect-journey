# Practice: Lesson 010 Validation Rules

## Goal

Apply optional-field rules and database-backed validation to real Laravel requests, and verify the resulting HTTP behavior.

## Exercises Completed

### 1. Compare Optional-Field Rules

Verified the distinction between:
- `sometimes|string`
- `sometimes|required|string`
- `sometimes|nullable|string`
- `nullable|string`

Confirmed that `sometimes` controls whether validation runs for a present field, while `required` controls non-empty presence and `nullable` permits null.

### 2. Verify Unique Email on Create

Added:

```php
'email' => 'required|string|email|max:125|unique:users,email',
```

Verified:
- An already-used email fails validation.
- A new email passes.
- An invalid email format fails validation.

### 3. Verify Unique Email on Update

Used a route-bound `User` model and:

```php
Rule::unique('users', 'email')->ignore($this->user)
```

Verified:
- Keeping the current user's email passes.
- Changing to a free email passes.
- Using another user's email fails validation.

### 4. Verify Exists

Used:

```php
Rule::exists('users', 'id')
```

Verified:
- An existing user ID passes.
- A missing ID fails.
- A non-integer value fails when combined with the `integer` rule.
- A missing required `user_id` fails.

## Acceptance Criteria

- Can explain the responsibility of `sometimes`, `nullable`, `required`, and `filled`.
- Can use `unique` for creation and exclude the current model on update.
- Can use `exists` to validate a referenced record.
- Can distinguish validation from authorization and database constraints.
- Can predict the expected validation response for the tested cases.

## Status

Completed in the learning session and manually verified.
