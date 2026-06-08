# Monitoring and Logging in Production

## What

Production monitoring answers: is the system healthy right now? Logging answers: what happened? Together, they are how you detect, diagnose, and resolve incidents.

## Prometheus Pull Model

Prometheus scrapes metrics from your services on a fixed interval. Your service exposes a `/metrics` endpoint. Prometheus calls it.

```
Every 15 seconds:
  Prometheus → GET http://service-a:8080/metrics → Store time-series data
  Prometheus → GET http://service-b:8080/metrics → Store time-series data
```

Why pull:
- The service doesn't need to know about Prometheus
- Easy to discover new services
- If a service is down, the scrape fails — that itself is a signal
- Prometheus controls the scrape frequency, not the service

PromQL example queries:

```
# Request rate per service
rate(http_requests_total[5m])

# P99 latency
histogram_quantile(0.99, rate(http_request_duration_seconds_bucket[5m]))

# Error rate
sum(rate(http_requests_total{status=~"5.."}[5m])) 
/ 
sum(rate(http_requests_total[5m]))
```

## Grafana Dashboards

Grafana queries Prometheus (and other data sources) to visualize metrics.

A practical dashboard layout:

1. **Top row** — High-level health: request rate, error rate, P99 latency (RED method)
2. **Middle row** — Resource usage: CPU, memory, connections per service
3. **Bottom row** — Business metrics: orders per minute, active users, queue depth

Dashboard rules:
- Link to related dashboards (infrastructure → service → logs)
- Use consistent time ranges
- Add annotations for deployments (so you can see the impact of changes)
- Each panel answers one question

## Log Aggregation

Centralized log collection. Services send logs to a central system instead of writing to local files.

### ELK Stack

- **Elasticsearch** — Search and analytics engine
- **Logstash / Fluentd** — Log collection and processing
- **Kibana** — Visualization and search

### Loki + Grafana

Lighter alternative. Loki indexes labels only (not full text), making it cheaper and faster for structured logs.

```
# LogQL query
{service="payment-service"} |= "error" | json | status >= 500
```

## RED Method in Practice

For every service, build a dashboard with:

- **Rate** — `rate(http_requests_total[5m])` by service, method, path
- **Errors** — `rate(http_requests_total{status=~"5.."}[5m])` as a percentage of total
- **Duration** — `histogram_quantile(0.50, ...)` and `histogram_quantile(0.99, ...)` for P50 and P99

Set alerts on:
- Error rate above 1% for 5 minutes
- P99 latency above 2 seconds for 5 minutes
- Rate dropping to zero (service might be down)

## The Investigation Flow

1. Alert fires (metric threshold crossed)
2. Open the service dashboard (RED metrics)
3. Check logs for the service around the alert time
4. Correlate with deployment annotations (did a deploy cause it?)
5. Check downstream services (is the database slow?)
6. Fix, deploy, verify

## Common Mistakes

- Building dashboards that look pretty but are useless during incidents. Design for the 3 AM scenario.
- Logging too much. Log business events and errors, not every function call. Volume costs money and makes searching harder.
- Not correlating metrics and logs. A link from a Grafana panel to the relevant Loki/Elasticsearch query saves minutes during an incident.
