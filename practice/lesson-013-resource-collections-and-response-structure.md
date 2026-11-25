# Lesson 013 Practice: Resource Collections and Response Structure

## Part 1 — Pagination without Resource

1. Create a temporary GET endpoint for users.
2. Execute `User::query()->paginate(10)` and return the paginator directly.
3. Request page 1 and page 2.
4. Verify the response contains pagination information and that the records change by page.

## Part 2 — Resource Collection

1. Return the same paginator through `UserResource::collection($users)`.
2. Compare the response with Part 1.
3. Confirm that each item contains only fields defined in `UserResource::toArray()`.
4. Find the current page in the response metadata.

## Part 3 — Boundary Case

Request a page beyond the available records.

Expected: the request succeeds and the `data` array is empty; pagination metadata still describes the requested/available pagination state.

## Completion Evidence

- Pagination tested directly and through a Resource collection.
- Page navigation verified.
- Resource field filtering verified.
- Pagination metadata location identified.
