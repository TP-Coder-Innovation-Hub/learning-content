# Rate Limiting

## What

Rate limiting controls how many requests a user or service can make in a given time window. It protects your system from being overwhelmed.

## Why Rate Limit

- **Protect the service** — Prevent a single client from consuming all resources
- **Fair usage** — Ensure all users get a fair share
- **Cost control** — Limit expensive operations (API calls to external services, compute-heavy endpoints)
- **Security** — Mitigate brute force attacks and DDoS

## Algorithms

### Token Bucket

Imagine a bucket that holds tokens. Each request consumes one token. Tokens are added at a fixed rate.

```
Bucket capacity: 10 tokens
Refill rate: 1 token per second

If the bucket is full (10 tokens), a burst of 10 requests goes through immediately.
Then requests succeed at 1 per second as tokens refill.
If the bucket is empty, requests are rejected until a token is added.
```

Pros: Allows controlled bursts. Good for APIs where traffic is bursty.
Cons: Requires tracking state per client (tokens in bucket, last refill time).

### Sliding Window Log

Track the timestamp of every request in a time window. Count requests in the window. Reject if over the limit.

```
Limit: 5 requests per 60 seconds

Request at t=0   → accepted (1 in window)
Request at t=10  → accepted (2 in window)
Request at t=20  → accepted (3 in window)
Request at t=30  → accepted (4 in window)
Request at t=40  → accepted (5 in window)
Request at t=45  → rejected (6 in window, limit is 5)
Request at t=61  → accepted (1 in window, t=0 request expired)
```

Pros: Accurate. No burst issues.
Cons: Memory grows with request count. Each request requires storing a timestamp.

### Sliding Window Counter

Hybrid approach. Divide time into fixed windows. Count requests in the current window and estimate from the previous window.

```
Current window (t=45 to t=60): 3 requests
Previous window (t=30 to t=45): 4 requests, 75% has elapsed
Estimated: 3 + (4 * 0.75) = 6 → reject if limit is 5
```

Pros: Memory efficient (one counter per window). Good approximation.
Cons: Not perfectly accurate at window boundaries.

## Implementation

Where to rate limit:

1. **API Gateway / Reverse Proxy** — Best place. Before requests hit your service. Nginx, Kong, AWS API Gateway all support it.
2. **Middleware in your service** — For fine-grained control (different limits per endpoint).
3. **Distributed store** — Redis or similar if you have multiple service instances sharing a limit.

```python
from flask_limiter import Limiter

limiter = Limiter(app, key_func=get_user_id, storage_uri="redis://localhost:6379")

@app.route("/api/expensive")
@limiter.limit("10/minute")
def expensive_operation():
    return compute_result()
```

## Response

When rate limited, return:
- Status: `429 Too Many Requests`
- Header: `Retry-After: 30` (seconds until the client should retry)
- Body: A clear message explaining the limit

```
HTTP/1.1 429 Too Many Requests
Retry-After: 30
Content-Type: application/json

{
  "error": "Rate limit exceeded",
  "limit": 100,
  "remaining": 0,
  "reset_at": "2024-01-15T10:35:00Z"
}
```

## Common Mistakes

- Rate limiting only by IP. Multiple users behind a NAT share one IP. One abusive user blocks everyone. Use API key or user ID.
- Not communicating limits to clients. Return `X-RateLimit-Limit`, `X-RateLimit-Remaining`, and `X-RateLimit-Reset` headers.
- Making the rate limiter a single point of failure. If Redis is down and you reject all requests, you just DoS'd yourself. Fail open (allow traffic) when the rate limiter is unavailable.
