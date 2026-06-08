# AGENTS.md

## Content Guidelines

- 200-500 words per file. Short, concise, direct.
- Concept first, then per-language code snippets where relevant.
- Mermaid diagrams where they clarify relationships or flows.
- No emojis. No level badges. No filler.
- Language-agnostic concepts. Show differences across languages only where they matter.

## Commit Convention

- Conventional commits: `feat:`, `fix:`, `docs:`

## File Naming

- Two-digit prefix for ordering: `01-topic.md`
- Lowercase, hyphenated filenames.

## Structure

Each file follows this pattern:
1. What is it (one sentence)
2. Why it matters (one paragraph)
3. How it works (the core concept)
4. Code/example snippets
5. Common mistakes or trade-offs

## Directories

- `api-design/` — REST, versioning, pagination, errors
- `auth-security/` — Auth, OAuth, validation, best practices
- `production-readiness/` — Logging, monitoring, config, health, rate limiting
- `observability/` — Three pillars, distributed tracing
- `concurrency/` — Thread pools, reactive, actor model
- `distributed-systems/` — Communication, events, sagas, failures, orchestration
- `cloud/` — Compute, storage, IaC, security, well-architected
- `devops/` — CI/CD, Kubernetes, monitoring
- `ai/` — AI fundamentals and practical usage
- `nontech/` — Non-technical explanations for business audiences
- `capstones/` — Project-based learning (linked from individual repos)
