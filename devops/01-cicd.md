# CI/CD

## What

CI/CD automates the path from code commit to running software. It catches problems early and makes deployments predictable and repeatable.

## CI vs CD vs Continuous Deployment

- **Continuous Integration (CI)** — Every commit triggers an automated build and test pipeline. Catch problems within minutes, not days.
- **Continuous Delivery (CD)** — Every commit that passes CI is ready to deploy. One click (or command) deploys to production.
- **Continuous Deployment** — Every commit that passes CI deploys to production automatically. No human approval.

Most teams start with CI + Continuous Delivery. Continuous Deployment requires high confidence in your tests.

## Pipeline Stages

A typical pipeline:

```
Commit → Build → Unit Tests → Integration Tests → Artifact → Deploy to Staging → E2E Tests → Deploy to Production
```

### Build

Compile code, install dependencies, create the artifact.

```yaml
# GitHub Actions example
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci
      - run: npm run build
      - run: npm run test:unit
```

### Test

- **Unit tests** — Fast, test individual functions. Run on every commit.
- **Integration tests** — Test component interactions. Run on every commit or PR.
- **End-to-end tests** — Test full user flows. Run before deployment.
- **Security scans** — Dependency vulnerabilities, static analysis. Run on every commit.

### Artifact

Package the build output. This is what gets deployed.

- Docker image pushed to a registry (ECR, Docker Hub, GCR)
- ZIP file uploaded to artifact storage (S3, Artifactory)
- The artifact is immutable — the same artifact moves through all environments

This is critical: build once, deploy everywhere. Do not rebuild for each environment.

## Release Strategies

### Rolling Update

Gradually replace old instances with new ones. Zero downtime if done right.

Good for: Stateless services, simple deployments.

### Blue/Green

Run two identical environments (blue and green). Deploy to the inactive one, switch traffic when ready.

Good for: When you need instant rollback (switch back to blue), database migrations.

### Canary

Route a small percentage of traffic (1-5%) to the new version. Monitor. Increase gradually.

Good for: High-traffic services, risky changes, measuring impact before full rollout.

### Feature Flags

Deploy code to everyone but enable new behavior for specific users.

Good for: Gradual rollouts, A/B testing, kill switches.

## Common Mistakes

- Skipping tests in CI because "they're slow." Fix the slow tests. CI without tests is just automated building.
- Deploying different artifacts to different environments. Build once, promote the same artifact.
- Not having a rollback plan. If the deployment breaks production, how do you revert? Practice it.
- Manual steps in the pipeline. If a human copies a file or clicks a button, it will go wrong eventually.
- Long pipelines. If CI takes 45 minutes, developers avoid running it. Keep it under 10 minutes.
