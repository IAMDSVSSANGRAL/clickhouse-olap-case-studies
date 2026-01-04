# OLTP vs OLAP: Why Updates Work Differently

## OLTP Systems

Examples:
- PostgreSQL
- MySQL
- SQL Server

Characteristics:
- Row-based storage
- Frequent UPDATEs and DELETEs
- Transaction-heavy workloads
- Optimized for point queries

Updates are expected and cheap.

---

## OLAP Systems

Examples:
- ClickHouse
- BigQuery
- Redshift

Characteristics:
- Columnar storage
- Append-heavy workloads
- Large scans and aggregations
- Optimized for analytics

Updates are expensive and discouraged.

---

## Mental Model Shift

![OLTP vs OLAP Mental Model](images/oltp-vs-olap-mental-model.png)

---

## Rule of Thumb

If your solution involves:
- Updating millions of rows
- Correlated subqueries for mutations
- Frequent backfills in-place

You are likely using OLTP thinking in an OLAP system.

---

## Final Thought

Databases are opinionated tools.
Performance comes from respecting those opinions.
````
