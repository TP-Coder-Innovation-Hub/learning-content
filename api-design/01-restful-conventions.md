# RESTful Conventions

## What

REST (Representational State Transfer) is an architectural style for designing networked APIs. You interact with resources using standard HTTP methods.

## Why It Matters

A consistent REST API is predictable. Developers can guess the endpoints without reading docs. This reduces onboarding time and bugs.

## Core Principles

1. **Resources as nouns** — URLs name things, not actions.
2. **HTTP methods as verbs** — GET, POST, PUT, PATCH, DELETE do the work.
3. **Stateless** — Each request contains everything the server needs.
4. **Standard status codes** — 200, 201, 400, 404, 500 each mean something specific.

## Resource Naming

```
GET    /users          → List users
GET    /users/42       → Get user 42
POST   /users          → Create a user
PUT    /users/42       → Replace user 42
PATCH  /users/42       → Update part of user 42
DELETE /users/42       → Delete user 42

GET    /users/42/orders → List orders for user 42
```

Rules:
- Plural nouns: `/users` not `/user`
- Nested resources for ownership: `/users/42/orders`
- Never verbs in URLs: not `/getUser` or `/deleteOrder`
- Use query params for filtering and sorting: `/users?role=admin&sort=-created`

## HTTP Methods

| Method  | Idempotent | Safe | Use For          |
|---------|-----------|------|------------------|
| GET     | Yes       | Yes  | Read             |
| POST    | No        | No   | Create           |
| PUT     | Yes       | No   | Full replacement |
| PATCH   | No        | No   | Partial update   |
| DELETE  | Yes       | No   | Remove           |

Idempotent means calling it once or ten times produces the same result.

## Status Codes — The Ones You Actually Need

```
200 OK              — Success (GET, PUT, PATCH, DELETE)
201 Created         — Resource created (POST)
204 No Content      — Success, no body (DELETE)

400 Bad Request     — Client sent invalid data
401 Unauthorized    — Not authenticated
403 Forbidden       — Authenticated but not allowed
404 Not Found       — Resource doesn't exist
409 Conflict        — Duplicate or state conflict
422 Unprocessable   — Valid format, business rule violation
429 Too Many Requests — Rate limited

500 Internal Server Error — Something broke on your end
502 Bad Gateway          — Upstream service failed
503 Service Unavailable  — You're down or overloaded
```

## When REST Isn't Enough

REST works well for CRUD-heavy APIs. It struggles when:

- **Real-time updates** — Use WebSockets or Server-Sent Events
- **Complex queries** — GraphQL lets clients request exactly the fields they need
- **High-performance service-to-service** — gRPC uses Protocol Buffers and HTTP/2 for smaller payloads and bi-directional streaming
- **Event-driven workflows** — Use message queues or event buses

## Common Mistakes

- Returning 200 with an error body. Use the right status code.
- Using POST for everything. Each HTTP method has a meaning.
- Exposing internal IDs or database structure in URLs. Your API is a contract, not a database dump.
- Ignoring HATEOAS. Most REST APIs don't need it, but if you claim Level 3 maturity, implement it.
