# Security Best Practices

## What

A checklist of practices that should be defaults, not debates. These apply to any backend service in any language.

## HTTPS Everywhere

No exceptions. Not "only for login pages." Not "only in production." HTTPS in development, staging, and production.

- Get certificates from Let's Encrypt (free) or your cloud provider
- Redirect all HTTP to HTTPS
- Use HSTS headers to tell browsers to always use HTTPS
- TLS 1.2 minimum. TLS 1.3 preferred.

## Secrets Management

Never hardcode secrets. Not in source code, not in Docker images, not in environment variables in your CI config.

```python
# Bad
DB_PASSWORD = "mysecretpassword"

# Good — read from a secrets manager
db_password = secrets_manager.get("db-password")
```

Options by maturity level:
- **Start:** Environment variables, `.env` files (never committed to git)
- **Growing:** Cloud secret managers (AWS Secrets Manager, Azure Key Vault, HashiCorp Vault)
- **Scale:** Automatic secret rotation, short-lived credentials, IAM roles instead of passwords

Rules:
- Rotate secrets regularly
- Use different secrets per environment
- Revoke compromised secrets immediately
- Log secret access (who accessed what, when)

## Rate Limiting

Protect your service from abuse and accidental overload.

- Rate limit per user, per IP, per API key
- Return `429 Too Many Requests` with a `Retry-After` header
- Use a sliding window or token bucket algorithm
- Apply stricter limits on expensive operations
- Consider different tiers: anonymous users get lower limits than authenticated ones

## Logging Security Events

Log these events. They are your forensic trail after an incident.

- Authentication successes and failures
- Authorization failures (user tried to access something they shouldn't)
- Password changes and resets
- Data export or bulk operations
- Admin actions
- Rate limit triggers
- Input validation failures (potential attack attempts)

What NOT to log:
- Passwords (even hashed)
- API keys and tokens
- Credit card numbers
- Social security numbers
- Any PII you don't need for debugging

## Dependency Management

- Run `npm audit`, `pip audit`, or equivalent regularly
- Subscribe to security advisories for your dependencies
- Pin dependency versions. Avoid `latest` in production
- Update dependencies on a schedule, not randomly
- Use lock files (`package-lock.json`, `poetry.lock`)

## Security Headers

Set these HTTP headers on every response:

```
Content-Security-Policy: default-src 'self'
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
Strict-Transport-Security: max-age=31536000; includeSubDomains
X-XSS-Protection: 0
Referrer-Policy: strict-origin-when-cross-origin
```

## Principle of Least Privilege

Every service, user, and process should have the minimum permissions needed to do its job.

- Database users: read-only for services that only read, no superuser
- API keys: scoped to specific operations
- IAM roles: narrowly defined, no wildcard permissions
- Container processes: run as non-root

## Incident Response Basics

1. Detect (monitoring + alerts)
2. Contain (revoke access, scale down, block IPs)
3. Investigate (logs, traces, timeline)
4. Remediate (fix the vulnerability)
5. Review (what happened, how to prevent it)

Write this down before you need it.
