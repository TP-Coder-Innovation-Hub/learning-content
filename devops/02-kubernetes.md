# Kubernetes

## What

Kubernetes (K8s) is a container orchestration platform. It automates deploying, scaling, and managing containerized applications.

## Core Concepts

### Pod

The smallest deployable unit. A pod runs one or more containers that share network and storage.

Most pods run a single container. Sidecar containers (logging agent, proxy) are the common exception.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web-app
spec:
  containers:
    - name: web
      image: myapp:1.2.3
      ports:
        - containerPort: 8080
```

### Deployment

Defines the desired state for your pods: how many replicas, which image, update strategy.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web-app
  template:
    metadata:
      labels:
        app: web-app
    spec:
      containers:
        - name: web
          image: myapp:1.2.3
          ports:
            - containerPort: 8080
```

### Service

A stable network endpoint for a set of pods. Pods have changing IPs. Services provide a fixed address.

```
Service (ClusterIP: 10.96.0.1) → Pod 1 (10.1.0.5)
                                 → Pod 2 (10.1.0.6)
                                 → Pod 3 (10.1.0.7)
```

Types:
- **ClusterIP** — Internal only. Default.
- **NodePort** — Expose on a port on each node.
- **LoadBalancer** — Provision a cloud load balancer.

### Ingress

An HTTP router. Routes external traffic to services based on host and path.

```
External traffic → Ingress Controller
  /api/* → API Service
  /web/* → Web Service
  ws://* → WebSocket Service
```

## The Reconciler Loop

Kubernetes is a declarative system. You tell it what you want. It continuously works to make reality match your desired state.

```
You write: "I want 3 replicas of myapp:1.2.3"

Kubernetes:
  - Current state: 2 replicas of myapp:1.2.0
  - Desired state: 3 replicas of myapp:1.2.3
  - Action: Start 3 new pods, wait for them to be ready, then kill the 2 old ones
```

This loop runs continuously. If a pod crashes, K8s restarts it. If a node dies, K8s reschedules pods elsewhere.

## When You Need Kubernetes

- Running 10+ microservices that need coordination
- Need auto-scaling based on demand
- Multi-team environment with many deployments per day
- Need blue/green or canary deployments built in
- Running on multiple cloud providers or on-premises

## When You Don't

- A single application or a few services
- Small team, low deployment frequency
- No dedicated platform/DevOps engineer
- Your cloud provider's managed container service (Cloud Run, ECS, Container Apps) is sufficient

Kubernetes has a steep learning curve. The operational complexity is real. You need someone who understands it or you will spend more time managing K8s than building your product.

## Common Mistakes

- Starting with K8s before you need it. If one server or a managed container service works, use that.
- Not setting resource limits on pods. Without limits, one pod can consume all node resources.
- Running single replicas. One replica means zero redundancy. A pod restart means downtime.
- Using latest tag for images. It is not reproducible. Pin versions: `myapp:1.2.3`, not `myapp:latest`.
