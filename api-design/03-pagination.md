# Pagination

## What

Pagination splits large result sets into chunks. Instead of returning 100,000 records, you return 50 at a time with a way to get the next batch.

## Why It Matters

Unbounded queries are slow, expensive, and crash clients. Pagination protects your database, your network, and your users' patience.

## Two Main Strategies

### Offset Pagination

```
GET /users?offset=0&limit=50     → Page 1 (records 1-50)
GET /users?offset=50&limit=50    → Page 2 (records 51-100)
GET /users?offset=100&limit=50   → Page 3 (records 101-150)
```

Implementation:

```sql
SELECT * FROM users ORDER BY id LIMIT 50 OFFSET 100;
```

Pros: Simple. Supports jumping to any page. Easy to implement.
Cons: Slow for deep offsets. The database still scans all previous rows. Inconsistent results if data changes between requests (a new row inserted on page 1 shifts everything).

Use offset pagination when:
- You need random page access ("go to page 5")
- The dataset is small to medium (under 10,000 rows)
- Users navigate pages sequentially

### Cursor Pagination

```
GET /users?limit=50                      → First 50 users
GET /users?cursor=eyJpZCI6NTB9&limit=50  → Next 50 after user 50
```

The cursor is an opaque token encoding the position. Usually the last record's ID or a sorted column value.

Implementation:

```sql
SELECT * FROM users WHERE id > 50 ORDER BY id LIMIT 50;
```

Pros: Consistent results even if data changes. Fast — the database uses an index seek, not a scan. Works at any scale.
Cons: No random page access. You must traverse sequentially. The cursor is opaque — clients cannot construct it.

Use cursor pagination when:
- The dataset is large (10,000+ rows)
- You have an "infinite scroll" or "load more" UI
- Data changes frequently
- Performance matters more than page jumping

## Response Format

```json
{
  "data": [...],
  "pagination": {
    "total": 1042,
    "limit": 50,
    "next_cursor": "eyJpZCI6MTAwfQ==",
    "has_more": true
  }
}
```

For offset pagination, replace `next_cursor` with `offset` and include `page`.

## Common Mistakes

- Returning the total count on every request. `SELECT COUNT(*)` on a large table is expensive. Make it optional or cache it.
- Not setting a default limit. Always set a server-side maximum (e.g., cap at 100 even if the client asks for 10,000).
- Exposing database offsets directly in cursors. Encode them. The cursor is a contract you control.
- Forgetting to order results. Pagination without ORDER BY is non-deterministic. Different calls may return different or duplicate results.

## Which to Pick

Default to cursor pagination for APIs. It scales better and is more consistent. Use offset only when you need page numbers in a UI.
