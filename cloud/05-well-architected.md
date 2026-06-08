# Well-Architected Framework

## What

Cloud providers publish well-architected frameworks — a set of best practices for building reliable, secure, efficient systems. The core ideas are the same across AWS, Azure, and GCP.

## Five Pillars

### 1. Operational Excellence

Run and monitor systems to deliver business value. Improve processes and procedures continuously.

Key practices:
- Automate deployments and runbooks
- Monitor metrics and set alerts
- Conduct post-incident reviews (blameless)
- Document operational procedures
- Use IaC for all infrastructure

### 2. Security

Protect data, systems, and assets. See [Cloud Security](04-cloud-security.md).

Key practices:
- Implement least privilege
- Encrypt data at rest and in transit
- Enable audit logging
- Automate security patching
- Test security controls regularly

### 3. Reliability

Ensure workloads perform their intended functions correctly and consistently.

Key practices:
- Test recovery procedures (chaos engineering)
- Scale horizontally to avoid single points of failure
- Automate failure recovery
- Handle capacity changes automatically
- Use multi-AZ or multi-region for critical workloads

### 4. Performance Efficiency

Use resources efficiently. Scale to meet demand. Maintain efficiency as technologies evolve.

Key practices:
- Right-size resources (don't over-provision)
- Use caching strategically
- Choose the right resource type for the workload
- Measure and optimize continuously
- Experiment with new services and instance types

### 5. Cost Optimization

Achieve business outcomes at the lowest price point.

Key practices:
- Right-size resources
- Use reserved capacity for steady workloads
- Use serverless for variable workloads
- Tag resources for cost attribution
- Set budgets and alerts

## Cost Optimization Strategies

### Right-Sizing

Match resource size to actual usage. An app using 10% CPU on a 4-core instance can likely run on a 2-core instance.

Steps:
1. Monitor CPU, memory, network, and disk usage over 2-4 weeks
2. Identify over-provisioned resources
3. Downsize to the smallest instance that meets your needs
4. Monitor after changes to ensure performance is unaffected

Typical savings: 20-40% on compute costs.

### Reserved Capacity

For workloads that run 24/7, commit to 1 or 3 years in exchange for a discount.

- **Reserved Instances** (AWS) — Commit to a specific instance type in a region. Up to 72% off.
- **Savings Plans** (AWS) — Commit to a dollar amount of compute usage. More flexible.
- **Reserved VMs** (Azure) — Similar concept. Up to 80% off.

Rule: If a workload runs more than 60% of the time, reserve it.

### Serverless for Variable Workloads

Pay only for what you use. Scales to zero when there's no traffic.

Good for: APIs with variable traffic, scheduled jobs, event-driven processing.

Bad for: Constant high-throughput workloads (reserved capacity is cheaper).

### Cost Tags

Tag every resource with metadata: team, project, environment, cost center.

This lets you answer: "How much does the data platform cost per month?" without guessing.

## Common Mistakes

- Optimizing cost at the expense of reliability. Saving money on a single-AZ database means a full outage when that AZ goes down.
- Not setting budget alerts. Without them, a misconfigured auto-scaling group can cost thousands before anyone notices.
- Right-sizing once and never reviewing again. Usage patterns change. Review quarterly.
- Ignoring data transfer costs. Cross-region and cross-AZ data transfer adds up. Keep data close to compute.
