# API Versioning

## What

API versioning lets you evolve your API without breaking existing clients. When you change the contract, clients on the old version keep working.

## Why It Matters

You will change your API. Fields get renamed, endpoints move, behavior shifts. Without versioning, every change is a breaking change for someone. With it, you can iterate safely.

## Three Approaches

### 1. URL Path Versioning

```
GET /v1/users/42
GET /v2/users/42
```

The version is part of the URL path.

Pros: Obvious. Easy to test in a browser. Cacheable.
Cons: URL changes mean different endpoints. Feels like a different API per version.

This is the most common approach. Pick this unless you have a specific reason not to.

### 2. Header Versioning

```
GET /users/42
Accept: application/vnd.myapi.v2+json
```

Or a custom header:

```
GET /users/42
X-API-Version: 2
```

Pros: Clean URLs. Version is metadata, not part of the resource identity.
Cons: Hidden from casual inspection. Harder to debug in a browser. Easy to miss.

### 3. Query Parameter Versioning

```
GET /users/42?version=2
```

Pros: Simple. Easy to add to existing APIs.
Cons: Easy to forget. Muddies the URL. Doesn't play well with caching (the query param is part of the cache key even for the default version).

## Trade-offs

| Factor        | URL Path      | Header          | Query Param    |
|--------------|---------------|-----------------|----------------|
| Visibility    | High          | Low             | Medium         |
| Caching       | Clean         | Clean           | Can be tricky  |
| Browser test  | Easy          | Hard            | Easy           |
| Existing API  | Route change  | No route change | Minimal change |

## How to Choose

Default to **URL path versioning** (`/v1/`, `/v2/`). It is visible, testable, and cacheable. The downsides are cosmetic.

Use header versioning only if you have strong API gateway infrastructure and your consumers are other engineering teams who read docs carefully.

Avoid query parameter versioning for new APIs. It works for quick fixes on legacy systems.

## Rules for Versioning

1. **Version from day one.** Even if you only have `v1`. It sets the expectation.
2. **Support at most 2 versions simultaneously.** Maintain `v1` and `v2`. When you ship `v3`, deprecate `v1`.
3. **Give clients a migration window.** Announce deprecation 6-12 months ahead. Return a `Sunset` header.
4. **Only bump versions for breaking changes.** Adding a field is not breaking. Removing or renaming one is.
5. **Non-breaking changes are free.** New optional fields, new endpoints, new query parameters — none of these need a version bump.

## What Counts as Breaking

- Removing or renaming a field
- Changing a field type (string to int)
- Changing an error response format
- Removing an endpoint
- Changing required fields

What is not breaking:
- Adding a new optional field
- Adding a new endpoint
- Adding a new query parameter
- Changing internal implementation

## Common Mistake

Skipping versioning because "we control all the clients." You won't forever. Mobile apps take time to update. Third-party integrations appear. Version from the start.
