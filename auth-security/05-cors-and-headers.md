# CORS and Security Headers

## What

Two security mechanisms that control how browsers interact with your API:

- **CORS** (Cross-Origin Resource Sharing) — Controls which websites can call your API from a browser
- **Security Headers** — HTTP response headers that tell the browser to enforce security policies

## CORS

### The Same-Origin Policy

Browsers block JavaScript from making requests to a different origin than the page it loaded on. This prevents malicious sites from making requests to your bank API using the user's cookies.

```
Page loaded from: https://app.example.com
API at:           https://api.example.com

Different origin (different subdomain). Browser blocks the request by default.
```

### How CORS Works

Your API tells the browser which origins are allowed via response headers.

#### Simple Request

```text
Request:
  GET /api/users
  Origin: https://app.example.com

Response (if allowed):
  Access-Control-Allow-Origin: https://app.example.com
  [data]

Response (if not allowed):
  (no Access-Control-Allow-Origin header)
  Browser blocks JavaScript from reading the response
```

#### Preflight Request

For requests that might modify data (POST, PUT, DELETE) or use custom headers, the browser sends a "preflight" OPTIONS request first:

```text
Preflight:
  OPTIONS /api/users
  Origin: https://app.example.com
  Access-Control-Request-Method: POST
  Access-Control-Request-Headers: Content-Type, Authorization

Preflight Response:
  Access-Control-Allow-Origin: https://app.example.com
  Access-Control-Allow-Methods: GET, POST, PUT, DELETE
  Access-Control-Allow-Headers: Content-Type, Authorization
  Access-Control-Max-Age: 3600

Actual Request (only sent if preflight passes):
  POST /api/users
  Origin: https://app.example.com
```

### CORS Configuration Rules

1. **Whitelist specific origins** — Never use `*` with credentials. Be explicit about which domains can call your API.
2. **Allow credentials explicitly** — If your API uses cookies or Authorization headers, set `Access-Control-Allow-Credentials: true`. This cannot be combined with `Access-Control-Allow-Origin: *`.
3. **Cache preflight responses** — Set `Access-Control-Max-Age` to reduce preflight round-trips (typically 3600 seconds).

```text
# Good: specific origin
Access-Control-Allow-Origin: https://app.example.com
Access-Control-Allow-Credentials: true

# Bad: wildcard with credentials (browser rejects this)
Access-Control-Allow-Origin: *
Access-Control-Allow-Credentials: true
```

### CORS Is Not Security

CORS protects the browser user, not your API. A malicious server-side script (curl, Postman, another backend) ignores CORS entirely. Always validate authentication and authorization on the server, regardless of CORS.

## Security Headers

### Content-Security-Policy (CSP)

Controls which sources the browser is allowed to load resources from. Prevents XSS attacks by blocking inline scripts and unauthorized sources.

```text
Content-Security-Policy: default-src 'self'; script-src 'self' https://cdn.example.com; style-src 'self' 'unsafe-inline'
```

- `default-src 'self'` — Only load from the same origin by default
- `script-src 'self' https://cdn.example.com` — Scripts only from self and this CDN
- Never use `unsafe-inline` for scripts unless absolutely necessary

### Strict-Transport-Security (HSTS)

Forces the browser to always use HTTPS. Prevents protocol downgrade attacks.

```text
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

### X-Content-Type-Options

Prevents the browser from guessing (sniffing) the MIME type of a response. Forces it to respect the declared Content-Type.

```text
X-Content-Type-Options: nosniff
```

### X-Frame-Options

Controls whether your page can be embedded in an iframe. Prevents clickjacking attacks.

```text
X-Frame-Options: DENY
```

Modern equivalent via CSP: `Content-Security-Policy: frame-ancestors 'none'`

### Referrer-Policy

Controls how much referrer information is sent with requests leaving your site.

```text
Referrer-Policy: strict-origin-when-cross-origin
```

## Common Mistakes

- **Using CORS as authentication.** CORS is enforced by the browser, not the server. A non-browser client bypasses it entirely. Always validate auth on the server.
- **`Access-Control-Allow-Origin: *` with credentials.** Browsers reject this. Use specific origins when credentials are involved.
- **Missing security headers.** Headers like HSTS and CSP are free protection. Set them on every response, not just "important" endpoints.
- **Overly permissive CSP.** `unsafe-inline`, `unsafe-eval`, or broad `*` sources defeat the purpose. Start strict, relax only what's needed.
