# NoSQL vs Relational

## What

Relational databases store rows in tables with a fixed schema. MongoDB stores documents in collections with a flexible schema. The right choice depends on your data shape and access patterns, not on hype.

## Why It Matters

Schema choice shapes everything downstream: how you query, how you join, how you scale. Relational databases enforce structure and give you ACID transactions across tables. Document databases let the data shape follow your domain and remove the cost of joins for read-heavy workloads.

Pick wrong and you fight the database daily. Pick right and most queries become trivial.

## How It Works

### The Same Data, Two Shapes

A user with addresses in PostgreSQL:

```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  email TEXT UNIQUE NOT NULL
);

CREATE TABLE addresses (
  id SERIAL PRIMARY KEY,
  user_id INT REFERENCES users(id),
  city TEXT,
  country TEXT
);
```

Reading a user with addresses requires a `JOIN`. In MongoDB, addresses that always belong to one user become an embedded array:

```javascript
{
  _id: ObjectId("..."),
  email: "ada@example.com",
  addresses: [
    { city: "London", country: "UK" },
    { city: "Paris", country: "France" }
  ]
}
```

One read returns the whole aggregate. No join.

### Documents and BSON

MongoDB stores documents in BSON (binary JSON). You get strings, numbers, booleans, arrays, nested objects, dates, and special types like `ObjectId`. A document can be up to 16 MB — enough for any reasonable aggregate.

### The Sample Dataset

Every file in this topic runs against the same dataset — a small store with users, products, and orders. Load it in `mongosh` before running any example:

```javascript
// users — referenced by orders
db.users.insertMany([
  { _id: 1, name: "Ada Lovelace", email: "ada@example.com",   country: "UK", joinedAt: ISODate("2025-11-01") },
  { _id: 2, name: "Grace Hopper", email: "grace@example.com", country: "US", joinedAt: ISODate("2025-12-15") },
  { _id: 3, name: "Alan Turing",  email: "alan@example.com",  country: "UK", joinedAt: ISODate("2026-01-10") }
])

// products — shared, edited independently: their own collection
db.products.insertMany([
  { _id: 101, sku: "TS-01", name: "T-Shirt", price: 350, stock: 120, category: "apparel" },
  { _id: 102, sku: "MUG-9", name: "Mug",     price: 199, stock: 80,  category: "home"    },
  { _id: 103, sku: "CAP-3", name: "Cap",     price: 299, stock: 45,  category: "apparel" }
])

// orders — line items embedded (owned, bounded, read together), user referenced
db.orders.insertMany([
  { _id: "A", userId: 1, status: "paid",
    items: [
      { sku: "TS-01", name: "T-Shirt", qty: 2, price: 350 },
      { sku: "MUG-9", name: "Mug",     qty: 1, price: 199 }
    ],
    total: 899, createdAt: ISODate("2026-01-15T10:00:00Z") },
  { _id: "B", userId: 2, status: "paid",
    items: [ { sku: "CAP-3", name: "Cap", qty: 1, price: 299 } ],
    total: 299, createdAt: ISODate("2026-01-20T09:30:00Z") },
  { _id: "C", userId: 1, status: "pending",
    items: [ { sku: "MUG-9", name: "Mug", qty: 3, price: 199 } ],
    total: 597, createdAt: ISODate("2026-02-01T14:00:00Z") },
  { _id: "D", userId: 3, status: "paid",
    items: [
      { sku: "TS-01", name: "T-Shirt", qty: 1, price: 350 },
      { sku: "CAP-3", name: "Cap",     qty: 2, price: 299 }
    ],
    total: 948, createdAt: ISODate("2026-02-05T16:45:00Z") }
])
```

String `_id` values (`"A"`–`"D"`) keep the example outputs readable; real applications use `ObjectId`.

### Create — `insertOne` / `insertMany`

```javascript
db.products.insertOne({ sku: "SOCK-7", name: "Socks", price: 149, stock: 200, category: "apparel" })
// → { acknowledged: true, insertedId: ObjectId("...") }
```

### Read — `find` / `findOne`

```javascript
// all paid orders, newest first
db.orders.find({ status: "paid" }).sort({ createdAt: -1 })
// → D (Feb 5), B (Jan 20), A (Jan 15)

// reach into arrays and nested fields with dot paths
db.orders.find({ "items.sku": "TS-01" })
// → A, D

// operator combinations: apparel priced 200–400
db.products.find({ category: "apparel", price: { $gte: 200, $lt: 400 } })
// → T-Shirt (350), Cap (299)

// projection — keep only what you need
db.orders.find({ status: "paid" }, { userId: 1, total: 1 }).sort({ total: -1 })
// → { _id: "D", userId: 3, total: 948 }
//   { _id: "B", userId: 2, total: 299 }
//   { _id: "A", userId: 1, total: 899 }  ← sort order applies after projection

// pagination
db.products.find().sort({ price: 1 }).skip(1).limit(2)
// → Mug (199) skipped, Cap (299) and T-Shirt (350) returned

db.orders.countDocuments({ status: "paid" })
// → 3
```

The operators you will use daily:

| Operator | Meaning | Example |
|----------|---------|---------|
| `$eq` / `$ne` | equals / not equals | `{ status: { $ne: "paid" } }` |
| `$gt` `$gte` `$lt` `$lte` | comparisons | `{ price: { $gte: 200 } }` |
| `$in` / `$nin` | membership | `{ status: { $in: ["paid", "shipped"] } }` |
| `$exists` | field present | `{ shippedAt: { $exists: true } }` |
| `$and` / `$or` | logic (often implicit) | `{ $or: [{ status: "paid" }, { status: "shipped" }] }` |

### Update — `updateOne` / `updateMany`

Updates use update operators — they modify fields in place without replacing the document:

```javascript
// sell 2 T-Shirts: stock 120 → 118
db.products.updateOne(
  { sku: "TS-01" },
  { $inc: { stock: -2 } }
)
// → { matchedCount: 1, modifiedCount: 1 }

// ship an order — add a field that did not exist before
db.orders.updateOne(
  { _id: "A" },
  { $set: { status: "shipped", shippedAt: new Date() } }
)

// bulk: mark all pending orders as paid
db.orders.updateMany(
  { status: "pending" },
  { $set: { status: "paid" } }
)
// → { matchedCount: 1, modifiedCount: 1 }
```

| Operator | Meaning |
|----------|---------|
| `$set` / `$unset` | set / remove a field |
| `$inc` | increment a number (negative to decrement) |
| `$push` / `$pull` | add / remove array elements |
| `$mul` | multiply a number |

### Delete — `deleteOne` / `deleteMany`

```javascript
db.orders.deleteOne({ _id: "D" })
// → { acknowledged: true, deletedCount: 1 }

db.products.deleteMany({ stock: 0 })
// → removes everything it matches — no `WHERE` means all rows, same as SQL
```

### MongoDB vs SQL, Side by Side

| MongoDB | SQL |
|---------|-----|
| `db.orders.find({ status: "paid" })` | `SELECT * FROM orders WHERE status = 'paid'` |
| `db.orders.find({}, { total: 1 })` | `SELECT total FROM orders` |
| `.sort({ createdAt: -1 }).limit(2)` | `ORDER BY created_at DESC LIMIT 2` |
| `insertOne(doc)` | `INSERT INTO orders (...) VALUES (...)` |
| `updateOne(filter, { $set: {...} })` | `UPDATE orders SET ... WHERE ...` |
| `deleteOne(filter)` | `DELETE FROM orders WHERE ...` |
| `$lookup` (aggregation) | `JOIN users ON ...` |

### Flexible Schema, Not No Schema

Every document in a collection can have different fields. That is flexibility, and it is also risk: nothing stops one document from storing `email` as a number. Enforce structure at the application layer with schema validation (Mongoose, Zod, class validators) or with MongoDB's collection validators.

### What You Keep and What You Give Up

```mermaid
flowchart TD
    A[Data shape decision] --> B{Access pattern?}
    B -->|Read aggregates whole| C[Embed — document model]
    B -->|Many-to-many, ad-hoc queries| D[Reference — relational model]
    C --> E[Fewer joins, denormalized]
    D --> F[Joins, normalization, constraints]
```

Relational strengths: strict constraints, mature tooling, ad-hoc SQL joins, multi-row ACID by default.

MongoDB strengths: documents match application objects, horizontal scaling built in, flexible fields for evolving products, fast writes for append-heavy workloads.

## Common Mistakes

- **Using MongoDB to avoid design.** You still model relationships — embedding vs referencing is a real schema decision.
- **Treating it as a cache with persistence.** It is a database with transactions, indexes, and aggregation. Use them.
- **Embedding unbounded arrays.** A `comments` array that grows forever will hit the 16 MB limit and rewrite the whole document on each push. Reference instead.
- **Assuming no ACID.** MongoDB has supported multi-document ACID transactions since 4.0.
- **`updateOne` without a filter, or `deleteMany({})` by accident.** An empty filter matches everything — the same footgun as `UPDATE`/`DELETE` without `WHERE`.
