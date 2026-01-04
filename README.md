# ClickHouse OLAP Case Studies

This repository documents real-world case studies and architectural decisions
when working with ClickHouse in an OLAP environment.

The focus is on:
- Schema evolution in OLAP systems
- Adding new columns safely
- Backfilling historical data at scale
- Understanding OLTP vs OLAP design differences
- Avoiding UPDATE-heavy anti-patterns

These case studies are based on real production scenarios
with millions of rows and active ingestion pipelines.

---

## Who This Is For

- Data Engineers
- Analytics Engineers
- Backend Engineers working with OLAP systems
- Teams migrating from OLTP to OLAP architectures

---

## Why This Repository Exists

Many engineering issues occur when OLTP patterns are directly applied
to OLAP databases.

This repository exists to:
- Explain *why* those patterns fail
- Show OLAP-native alternatives
- Provide technical justification for architectural decisions

---


## Repository Structure

clickhouse-olap-case-studies/

├── README.md

├── schema-evolution/

│ ├── README.md

│ └── adding-columns-backfill-clickhouse.md

├── oltp-vs-olap/

│ └── oltp-vs-olap-updates.md

└── LICENSE


---

## Disclaimer

All schemas, examples, and queries are generalized and anonymized.
No proprietary data or business logic is shared.


