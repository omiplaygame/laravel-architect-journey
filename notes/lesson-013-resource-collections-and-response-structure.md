# Lesson 013: Resource Collections and Response Structure

## Objectives

- Apply a Resource to a collection of Eloquent models.
- Paginate a query instead of loading every row.
- Understand the JSON structure returned by a paginated Resource collection.

## Resource Collections

A single Resource represents one model:

```php
return new UserResource($user);
```

For multiple models, use the Resource collection factory:

```php
return UserResource::collection($users);
```

Each model is transformed using `UserResource::toArray()`. This keeps the public representation consistent between single-item and collection endpoints.

## Pagination

```php
$users = User::query()->paginate(10);

return UserResource::collection($users);
```

- `paginate(10)` requests 10 records per page.
- `?page=1` and `?page=2` select the requested page.
- The length-aware paginator obtains the total count and the page's records.
- If the requested page is beyond the available records, the `data` array can be empty.

Pagination metadata is retained when a paginator is passed to `Resource::collection()`.

## Response Shape

A paginated Resource collection has three top-level parts:

- `data`: transformed resource items.
- `links`: navigation URLs.
- `meta`: pagination metadata such as `current_page`, `last_page`, `per_page`, and `total`.

Without a Resource, returning the paginator directly also serializes pagination information, but model fields are serialized according to model serialization rules rather than the explicit Resource representation.

## Verified Practice

- Created test users with a factory.
- Tested the paginator directly and with `UserResource::collection()`.
- Requested multiple pages and confirmed that records changed between pages.
- Confirmed that an out-of-range page returns an empty `data` array.
- Observed that `current_page` is under `meta` in the Resource collection response.

## Engineering Notes

- Prefer pagination for collection endpoints instead of `all()` when the dataset can grow.
- Use Resources consistently to keep API fields intentional.
- Pagination does not automatically authorize access to records; access constraints still belong in the query or authorization layer.
- `paginate()` is appropriate when clients need total-page information. Other pagination strategies can be explored when query cost or dataset size warrants them.
