# Background Jobs

## What

A background job is work that happens outside the request-response cycle. Instead of making the user wait while you send an email, generate a PDF, or process a file, you queue the work and return immediately.

## Why Background Processing

- **Faster responses** — The user doesn't wait for work that doesn't affect the response
- **Reliability** — Retry failed jobs without user interaction
- **Resource control** — Limit concurrent heavy operations (don't let 100 PDF generations crash the server)
- **Scheduling** — Run tasks on a schedule (nightly reports, monthly billing, periodic cleanup)

## The Worker Pattern

```text
Request → Handler → Queue → Worker → External System
                        ↓
                    Response (immediate)
```

1. The API handler creates a job and pushes it to a queue
2. The API returns immediately (e.g., `202 Accepted`)
3. A worker process picks up the job and executes it
4. If the job fails, the queue retries it

```python
# API handler (fast, returns immediately)
@app.post("/reports/generate")
def generate_report(user_id):
    job_id = queue.enqueue("generate_monthly_report", user_id=user_id)
    return {"job_id": job_id, "status": "queued"}, 202

# Worker (slow, runs in background)
def generate_monthly_report(user_id):
    data = fetch_user_data(user_id)
    pdf = render_pdf(data)
    storage.save(f"reports/{user_id}.pdf", pdf)
    email.send(user_id, "Your report is ready")
```

## Scheduled Jobs

Run a task on a recurring schedule. Use cron expressions or time intervals.

```text
Every night at 2:00 AM → Run daily reconciliation
Every 1st of month    → Generate payroll
Every 5 minutes       → Poll for external API updates
```

Two approaches:
1. **Cron-based** — An external scheduler (cron, Kubernetes CronJob, cloud scheduler) triggers the job
2. **In-process timer** — The application itself runs a timer and triggers the job (simpler, but the app must be running)

## Job Queues vs In-Process

| | Job Queue (Redis, RabbitMQ, SQS) | In-Process (hosted service, timer) |
|---|---|---|
| **Survives restart** | Yes (persisted) | No (lost if app crashes mid-job) |
| **Scales horizontally** | Yes (multiple workers) | No (single process) |
| **Complexity** | More infrastructure | Simpler setup |
| **Best for** | Critical, long-running work | Lightweight, periodic tasks |

Rule of thumb: If losing a job on crash is acceptable, in-process is fine. If not, use a queue.

## Retry and Idempotency

Jobs fail. Networks timeout, databases go down, external APIs return 500. Retries are essential.

**Retry policies:**
- **Fixed delay** — Retry every 30 seconds. Simple.
- **Exponential backoff** — Retry after 1s, 2s, 4s, 8s... Prevents overwhelming a recovering system.
- **Max attempts** — Stop after N retries (e.g., 5). Move to a dead-letter queue for manual investigation.

**Idempotency is mandatory.** If a job runs twice (due to retry or crash recovery), the result must be the same as running once.

```python
def process_payment(payment_id):
    # Check if already processed
    if db.query("SELECT status FROM payments WHERE id = ?", payment_id) == "completed":
        return  # Already done, skip

    # Process payment
    gateway.charge(payment_id)
    db.update("payments", payment_id, status="completed")
```

## Graceful Shutdown

When the application stops (deploy, scale-down, crash), in-progress jobs need to finish or be re-queued.

1. Stop accepting new jobs
2. Wait for in-progress jobs to complete (with a timeout)
3. If timeout, re-queue incomplete jobs
4. Then shut down

Without graceful shutdown, jobs disappear mid-execution. The user never gets their email.

## Common Mistakes

- **Doing heavy work in the request handler.** Generating a PDF, sending batch emails, calling slow APIs — these should be queued, not done synchronously.
- **Non-idempotent jobs.** If a job charges a credit card and retries on failure, the user gets charged twice. Always check if the work is already done.
- **No retry limit.** A poison message (always fails) blocks the queue forever. Set a max retry count and a dead-letter queue.
- **Blocking the worker.** If the worker does a synchronous 30-second API call, it can't process other jobs. Use async I/O or add more workers.
