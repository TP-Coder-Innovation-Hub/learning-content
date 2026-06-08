# Thread Pool

## What

A thread pool is a collection of pre-created threads that wait for tasks. Instead of creating a new thread for every request (expensive), you reuse threads from the pool.

## Why Thread Pools

Creating a thread costs time and memory. Each thread gets its own stack (typically 1MB). If you create unlimited threads for unlimited requests, you run out of memory or spend more time context-switching than doing work.

A thread pool solves this by:
- Reusing threads (no creation overhead per task)
- Limiting concurrency (protects resources)
- Queuing excess work (smooths bursts)

## Core vs Max Pool Size

Most thread pool implementations have two settings:

- **Core pool size** — Minimum number of threads always alive (even when idle)
- **Max pool size** — Maximum number of threads the pool can grow to

Behavior:
1. New task arrives, fewer than core threads running → create a new thread
2. Core threads all busy, queue not full → add task to queue
3. Queue full, fewer than max threads → create a new thread
4. Queue full, at max threads → rejection policy triggers

This means the pool grows only when the queue is full. It does not create threads eagerly.

## Rejection Policies

When the pool cannot accept more work (queue full, max threads reached):

| Policy            | Behavior                                          |
|-------------------|---------------------------------------------------|
| Abort             | Throw an exception. Default in most frameworks.   |
| Caller Runs       | The submitting thread runs the task itself.        |
| Discard           | Silently drop the task.                            |
| Discard Oldest    | Drop the oldest queued task, add the new one.      |

**Caller Runs** is often the best production choice. It naturally throttles the producer without dropping work.

## Queue Types

- **Unbounded queue** — Never rejects. Risk: memory grows until OOM. Never use in production.
- **Bounded queue** — Fixed capacity. When full, rejection policy applies. Good for most cases.
- **Synchronous queue** — No capacity. Every offer must have a waiting consumer. Creates max threads aggressively. Good for short, fast tasks.

## When to Tune

Default pool size works for most applications. Tune when:

- **CPU-bound tasks** (number crunching, compression): pool size = number of CPU cores. More threads just cause context switching.
- **I/O-bound tasks** (database calls, HTTP requests): pool size = CPU cores * (1 + wait_time / compute_time). If your tasks spend 80% of time waiting on I/O, you can afford more threads.
- **Mixed workload**: separate pools for CPU-bound and I/O-bound tasks.

## Per-Language Example

```java
ThreadPoolExecutor pool = new ThreadPoolExecutor(
    4,                          // core pool size
    8,                          // max pool size
    60, TimeUnit.SECONDS,       // idle thread keep-alive
    new ArrayBlockingQueue<>(100),  // bounded queue
    new ThreadPoolExecutor.CallerRunsPolicy()
);
pool.submit(() -> processOrder(order));
```

```python
from concurrent.futures import ThreadPoolExecutor

with ThreadPoolExecutor(max_workers=8) as pool:
    futures = [pool.submit(fetch_url, url) for url in urls]
    results = [f.result() for f in futures]
```

```go
// Go uses goroutines, not thread pools directly.
// But you can limit concurrency with a semaphore:
sem := make(chan struct{}, 8) // max 8 concurrent
for _, url := range urls {
    sem <- struct{}{}
    go func(u string) {
        defer func() { <-sem }()
        fetchURL(u)
    }(url)
}
```

## Common Mistakes

- Using an unbounded queue. One slow downstream service can back up the queue until you run out of memory.
- Sharing one pool for everything. A slow operation consumes all threads, starving fast operations. Use separate pools for different workload types.
- Not monitoring pool metrics. Track active threads, queue size, and rejection count. They tell you when to tune.
