# Configuration

## What

Configuration is everything that changes between environments but is not code: database URLs, API keys, feature flags, timeout values. It should live outside your application code.

## Why It Matters

Hardcoded configuration means you rebuild and redeploy to change a timeout. Externalized configuration means you change a value and restart (or don't even restart for feature flags). The same artifact (Docker image, binary) runs in every environment.

## 12-Factor App: Factor III

"Store config in environment variables."

The 12-factor methodology defines config as everything that varies between deploys. Code is the same across environments. Config is different.

## Three Levels of Configuration

### 1. Environment Variables

Simple key-value pairs. Good for infrastructure config.

```bash
DATABASE_URL=postgres://user:pass@host:5432/db
PORT=8080
LOG_LEVEL=info
```

Pros: Universal. Every language and framework supports them. Easy to set in Docker, Kubernetes, CI/CD.
Cons: No nesting, no complex types, easy to get messy with many variables.

### 2. Config Files

Structured configuration for complex settings.

```yaml
server:
  port: 8080
  timeout_seconds: 30

database:
  host: ${DB_HOST}
  port: 5432
  name: myapp

cache:
  enabled: true
  ttl_seconds: 300
```

Pros: Structured, versionable, supports complex types.
Cons: Must manage per-environment files. Can accidentally commit secrets.

Pattern: Use config files for structure, env vars for secrets. The config file references env vars for sensitive values.

### 3. Feature Flags

Dynamic configuration that changes behavior without redeployment.

```python
if feature_flags.is_enabled("new_checkout_flow", user_id=42):
    return new_checkout(cart)
return legacy_checkout(cart)
```

Use feature flags for:
- Gradual rollouts (enable for 10% of users, then 50%, then all)
- A/B testing
- Kill switches (disable a feature instantly if it causes problems)
- Beta features for specific customers

Tools: LaunchDarkly, Unleash, Flagsmith, or a simple database table.

## Rules

1. **Never hardcode configuration.** If it changes between environments, it is config.
2. **Separate config from secrets.** Config can be in version control. Secrets go in a secrets manager.
3. **Fail fast on missing config.** If the app cannot start without a value, crash immediately with a clear error. Do not default to something that might work.
4. **Validate config at startup.** Check types, ranges, and required fields before the app starts serving requests.
5. **Document every config value.** What it does, valid values, default if any.

## Common Mistakes

- Using different Docker images per environment. The image is the same. Only config changes.
- Committing `.env` files to git. Add them to `.gitignore`.
- Having 50 environment variables and no documentation. Group them, document them, validate them.
- Not having a default. Some config should have safe defaults (log level = info). Some should not (database URL). Know the difference.
