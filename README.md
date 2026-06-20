# Learning Content

Shared educational content for common backend topics. Language-agnostic concepts with per-language code snippets where relevant.

## Contents

### API Design
- [RESTful Conventions](api-design/01-restful-conventions.md) — REST principles, resource naming, HTTP methods, status codes
- [Versioning](api-design/02-versioning.md) — URL, header, and query param versioning trade-offs
- [Pagination](api-design/03-pagination.md) — Offset vs cursor pagination
- [Error Responses](api-design/04-error-responses.md) — Consistent error format, RFC 7807
- [Frontend Integration](api-design/05-frontend-integration.md) — SPA auth flow, BFF pattern, API client generation, real-time

### Auth & Security
- [Auth Concepts](auth-security/01-auth-concepts.md) — Authentication vs authorization, sessions, JWT
- [OAuth Basics](auth-security/02-oauth-basics.md) — OAuth 2.0 roles and flows
- [Input Validation](auth-security/03-input-validation.md) — SQL injection, XSS, CSRF, OWASP
- [Security Best Practices](auth-security/04-security-best-practices.md) — HTTPS, secrets, rate limiting, logging
- [CORS and Security Headers](auth-security/05-cors-and-headers.md) — Same-origin policy, preflight, CSP, HSTS, X-Frame-Options

### Production Readiness
- [Logging](production-readiness/01-logging.md) — Structured logging, log levels, centralized logging
- [Monitoring](production-readiness/02-monitoring.md) — RED method, Prometheus, Grafana, alerting
- [Configuration](production-readiness/03-configuration.md) — 12-factor app config, env vars, feature flags
- [Health Checks](production-readiness/04-health-checks.md) — Liveness vs readiness, graceful shutdown
- [Rate Limiting](production-readiness/05-rate-limiting.md) — Token bucket, sliding window, implementation

### Observability
- [Three Pillars](observability/01-three-pillars.md) — Metrics, logs, traces
- [Distributed Tracing](observability/02-distributed-tracing.md) — Trace ID, span ID, OpenTelemetry

### Concurrency
- [Thread Pool](concurrency/01-thread-pool.md) — Pool sizing, rejection policies, tuning
- [Reactive](concurrency/02-reactive.md) — Reactive manifesto, backpressure, event loop
- [Actor Model](concurrency/03-actor-model.md) — Actors, mailboxes, supervision, distributed systems

### Distributed Systems
- [Communication Patterns](distributed-systems/01-communication-patterns.md) — Sync vs async, request-reply vs pub-sub
- [Event-Driven](distributed-systems/02-event-driven.md) — Event sourcing, CQRS, Kafka
- [Saga Pattern](distributed-systems/03-saga-pattern.md) — Distributed transactions, choreography vs orchestration
- [Failure Modes](distributed-systems/04-failure-modes.md) — Cascading failures, bulkhead, chaos engineering
- [Workflow Orchestration](distributed-systems/05-workflow-orchestration.md) — Temporal, durable execution

### Cloud
- [Compute Models](cloud/01-compute-models.md) — VM vs container vs serverless
- [Storage & Databases](cloud/02-storage-databases.md) — Storage types, database selection
- [Infrastructure as Code](cloud/03-iac.md) — Terraform concepts, why IaC
- [Cloud Security](cloud/04-cloud-security.md) — IAM, encryption, shared responsibility
- [Well-Architected](cloud/05-well-architected.md) — 5 pillars, cost optimization

### DevOps
- [CI/CD](devops/01-cicd.md) — Pipeline stages, artifact management, release strategies
- [Kubernetes](devops/02-kubernetes.md) — Core concepts, when you need K8s
- [Monitoring & Logging](devops/03-monitoring-logging.md) — Prometheus, Grafana, ELK/Loki

### AI
- [What is AI](ai/01-what-is-ai.md) — Pattern recognition at scale, realistic expectations
- [Prompting](ai/02-prompting.md) — Thinking framework, not magic prompts
- [AI Writing](ai/03-ai-writing.md) — Draft, review, edit workflow
- [AI Research](ai/04-ai-research.md) — AI-assisted research with fact-checking
- [AI Decide](ai/05-ai-decide.md) — AI as brainstorming partner
- [AI Learn](ai/06-ai-learn.md) — Learning anything with AI
- [AI Toolkit](ai/07-ai-toolkit.md) — Evaluating AI tools
- [AI Privacy](ai/08-ai-privacy.md) — What data to never put into AI
- [AI Mindset](ai/09-ai-mindset.md) — AI augments judgment

### Non-Technical
- [How the Internet Works](nontech/01-how-internet-works.md) — Request/response, no jargon
- [Frontend vs Backend](nontech/02-frontend-vs-backend.md) — Dining room vs kitchen
- [What is an API](nontech/03-what-is-an-api.md) — Waiter analogy
- [Data Basics](nontech/04-data-basics.md) — Filing cabinet analogy
- [Cybersecurity](nontech/05-cybersecurity.md) — What every business person should know
- [AI for Business](nontech/06-ai-for-business.md) — Evaluating vendor claims
- [Working with Tech Teams](nontech/07-working-with-tech-teams.md) — Communication and requirements
- [Tech Decisions](nontech/08-tech-decisions.md) — Build vs buy, trade-offs

### Workshops
- [Workshop Projects](workshops/README.md) — TODO
