# Error Responses

## What

A consistent error response format gives clients a predictable structure to handle failures. Every error from your API looks the same way.

## Why It Matters

Inconsistent errors force every client to write custom parsing for each endpoint. A standard format means one error handler works everywhere. It also makes debugging easier when something goes wrong.

## The Problem

Bad — every endpoint does its own thing:

```json
{"error": "User not found"}
{"message": "Validation failed", "fields": {"email": "invalid"}}
{"status": 500, "error": "something went wrong"}
```

Good — one consistent format:

```json
{
  "type": "https://example.com/errors/validation-error",
  "title": "Validation Error",
  "status": 422,
  "detail": "The request body contains invalid fields.",
  "instance": "/users",
  "errors": [
    {
      "field": "email",
      "code": "invalid_format",
      "detail": "Must be a valid email address."
    }
  ]
}
```

## RFC 7807 — Problem Details

RFC 7807 defines a standard format for HTTP API errors. The fields:

| Field     | Required | Description                                  |
|-----------|----------|----------------------------------------------|
| `type`    | Yes      | URI identifying the error type               |
| `title`   | Yes      | Short human-readable summary                 |
| `status`  | Yes      | HTTP status code                             |
| `detail`  | No       | Specific explanation of this occurrence       |
| `instance`| No       | URI that caused the error                    |

The `type` field is the most important part. It is a machine-readable identifier. Clients switch on `type`, not on `status` or `title`.

Content-Type: `application/problem+json`

## Implementation Pattern

```python
# Error type registry
ERROR_TYPES = {
    "validation_error": {
        "type": "/errors/validation-error",
        "title": "Validation Error",
        "status": 422
    },
    "not_found": {
        "type": "/errors/not-found",
        "title": "Resource Not Found",
        "status": 404
    },
    "conflict": {
        "type": "/errors/conflict",
        "title": "Conflict",
        "status": 409
    }
}

def error_response(error_key, detail, instance=None, errors=None):
    base = ERROR_TYPES[error_key]
    body = {
        "type": "https://example.com" + base["type"],
        "title": base["title"],
        "status": base["status"],
        "detail": detail
    }
    if instance:
        body["instance"] = instance
    if errors:
        body["errors"] = errors
    return body
```

## Rules

1. **Always return the right HTTP status code.** Do not return 200 with an error body.
2. **Use `type` for machine parsing.** Clients should check `type`, not parse `detail`.
3. **`detail` is for humans.** Do not put structured data in it. Use the `errors` array for field-level issues.
4. **Never expose stack traces or internal IDs in production.** Log them server-side, return a generic message to the client.
5. **Include a correlation/request ID.** This lets support match a user complaint to server logs.

```json
{
  "type": "https://example.com/errors/internal-error",
  "title": "Internal Server Error",
  "status": 500,
  "detail": "An unexpected error occurred. Please try again.",
  "request_id": "req-abc123"
}
```

## Common Mistakes

- Returning different error shapes per endpoint. Pick one format. Use it everywhere.
- Using non-standard status codes. Stick to the defined ones in the 4xx and 5xx ranges.
- Exposing internal error details. Database errors, stack traces, and file paths belong in logs, not in responses.
- Returning 200 for errors. This breaks every HTTP tool and expectation.
