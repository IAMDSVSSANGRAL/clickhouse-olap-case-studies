# Case Study: Adding New Columns and Backfilling Data in ClickHouse

## Background

We had an analytical table (`order_core`) containing ~8.5 million rows.
New attributes such as `warehouse` and `payment_method`
were required for reporting and analytics.

These attributes existed in a raw ingestion table (`Orders`)
with multiple records per `order_id`, arriving incrementally.

The challenge:
How do we populate these new columns without impacting production
performance or ingestion reliability?

---

## The OLTP-Style Approach

In OLTP systems, the common solution is:
- Add new columns
- Run UPDATE statements with joins or subqueries
- Mutate existing rows in-place

Conceptually:

```sql
UPDATE order_core
SET warehouse = ...
FROM Orders
WHERE Orders.order_id = order_core.order_id;
````

---

## Why This Fails in ClickHouse

ClickHouse is a columnar, append-optimized OLAP database.

Key issues with UPDATE-heavy logic:

* Updates rewrite entire data parts
* Large mutations cause merge pressure
* Performance degrades at scale
* Mutations are asynchronous and operationally risky
* Failure recovery becomes complex

At millions of rows, this approach is unsafe.

---

## The OLAP-Native Solution

Instead of mutating existing rows, we derive the latest attributes
using aggregation and joins.

### Step 1: Build a Latest-Attributes Table

```sql
CREATE TABLE order_latest_attrs AS
SELECT
    order_id,
    argMax(warehouse, _peerdb_synced_at) AS warehouse,
    argMax(payment_method, _peerdb_synced_at) AS payment_method
FROM Orders
GROUP BY order_id;
```

This table:

* Is compact
* Supports incremental refresh
* Avoids large-scale mutations
* Aligns with ClickHouse design

---

### Step 2: Join at Query Time

```sql
SELECT
    oc.*,
    ola.warehouse,
    ola.payment_method
FROM order_core oc
LEFT JOIN order_latest_attrs ola
ON oc.order_id = ola.order_id;
```

Alternatively, a new wide table can be built if performance demands it.

---

## Why This Works

* No UPDATE on millions of rows
* Predictable performance
* Easy rollback
* Production-safe
* OLAP-aligned design

---

## Key Takeaway

In OLAP systems:

* Derive data
* Append data
* Rebuild when needed

In OLTP systems:

* Mutate data
* Update rows
* Rely on transactions

Applying the wrong model leads to instability.

---
