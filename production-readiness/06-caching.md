# Caching

## What

Caching stores frequently accessed data in fast storage so you don't have to fetch or recompute it every time. A cache is a temporary, fast copy of data that lives closer to the consumer than the source.

## Why Cache

- **Reduce latency** — Memory is 100x faster than disk or network
- **Reduce load** — Fewer database queries, fewer API calls
- **Reduce cost** — Fewer compute cycles, fewer external API bills

## Cache-Aside (Lazy Loading)

The most common pattern. The application checks the cache first. On a miss, it loads from the source and fills the cache.

```python
def get_user(user_id):
    # 1. Check cache
    cached = cache.get(f"user:{user_id}")
    if cached:
        return cached

    # 2. Cache miss — load from database
    user = db.query("SELECT * FROM users WHERE id = ?", user_id)

    # 3. Fill cache
    cache.set(f"user:{user_id}", user, ttl=300)

    return user
```

Pros: Simple. Cache only contains what's actually requested.
Cons: Cache miss is slow (full database round-trip). Stale data between TTL expiry and write.

## Write-Through

On every write, update both the database and the cache simultaneously.

```python
def update_user(user_id, data):
    db.update("users", user_id, data)
    cache.set(f"user:{user_id}", data, ttl=300)
```

Pros: Cache is always consistent with the database. No stale reads.
Cons: Write latency increases (two operations). Cache fills with data that may never be read.

## Write-Behind (Write-Back)

Write to the cache only. Asynchronously flush to the database later.

```python
def update_user(user_id, data):
    cache.set(f"user:{user_id}", data, ttl=300)
    queue.publish("flush_to_db", {"id": user_id, "data": data})
```

Pros: Very fast writes. Database sees batched, smooth writes.
Cons: Data loss risk if cache crashes before flush. Complex to implement.

## TTL (Time to Live)

Every cache entry should have a TTL — how long before it expires.

- **Too short** — Cache is ineffective. Most requests are misses.
- **Too long** — Stale data. Users see outdated information.
- **Sweet spot** — Long enough to reduce load, short enough that staleness is acceptable.

Rule of thumb: Start with 5 minutes. Measure hit rate. Adjust.

## Cache Invalidation

The hardest problem in caching. When the source data changes, the cache must reflect it.

Strategies:
1. **TTL expiry** — Let it go stale and expire naturally. Simplest, most common.
2. **Explicit invalidation** — Delete the cache key when the source changes. More accurate, more code.
3. **Versioning** — Include a version number in the cache key (`user:42:v7`). Bump version on write. Old entries expire via TTL.

```python
def update_user(user_id, data):
    db.update("users", user_id, data)
    cache.delete(f"user:{user_id}")  # Explicit invalidation
```

## Cache Stampede

When a popular cache entry expires, hundreds of requests miss simultaneously. They all hit the database at once.

Solutions:
1. **Mutex/lock** — Only one request fetches from the database. Others wait.
2. **Early refresh** — Refresh the cache before it expires (e.g., at 80% of TTL).
3. **Probabilistic early expiry** — Add randomness to TTL so entries don't all expire at the same moment.

## In-Memory vs Distributed

| | In-Memory | Distributed (Redis) |
|---|---|---|
| **Speed** | Nanoseconds | Milliseconds |
| **Shared** | Per-instance | Across all instances |
| **Scalability** | Limited by RAM | Horizontally scalable |
| **Data loss** | Lost on restart | Survives restart |

Use in-memory for single-instance or low-stakes caching. Use distributed (Redis) when you have multiple instances or need persistence.

## Common Mistakes

- **Caching everything.** Cache only what is read often and changes rarely. Caching frequently-written data creates more invalidation overhead than it saves.
- **No TTL.** Without TTL, stale data lives forever. Always set one.
- **Caching errors.** If the database query fails, don't cache the error or null result. Every subsequent request gets the cached failure.
- **Not measuring hit rate.** If your hit rate is below 50%, caching is adding complexity without benefit. Measure, then decide.
