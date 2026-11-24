# 04 — Synthetic Data Pipeline, Reconciliation, Freshness, and Lineage Control Plane

**Roles:** Data Quality Engineer, Analytics QA, ETL Test Engineer, Quality Engineer.

## Goal

Demonstrate that an analytics pipeline can detect structural, relational, semantic, freshness, schema, and lineage defects before a consumer trusts the output.

## Build

Deterministic JSON/CSV/Parquet generator; DuckDB or PostgreSQL; Bronze/Silver/Gold transformations; dbt Core or dbt-style SQL; JSON Schema; pytest; OpenLineage-compatible events; and a static quality report.

## Test portfolio

- Keys, nullability, accepted values, types, partitions, and schema compatibility.
- Foreign keys, orphan facts, duplicates, idempotency keys, and source-to-target counts.
- Revenue/refund/inventory reconciliation with explicit rounding tolerances.
- Late-arriving data, reruns, missing partitions, malformed records, quarantine.
- Freshness SLA, null-rate/distribution drift, and anomaly scenarios.
- Mutation cases: dropped column, duplicate rows, shifted timestamps, altered amount, missing partition.
- Lineage from input hash and run ID through model, assertion, failure, and remediation.

## Evidence

Generator/scenario manifests, schema versions, compiled SQL, assertion report, reconciliation report, freshness dashboard, lineage events/graph, defect fixtures, data dictionary, and runbook.

## Acceptance targets

20,000 synthetic facts over 30 partitions; 12 injected defect scenarios; zero clean-baseline failures; breaking schema changes rejected; 0.1% discrepancy detected; stale partition fails freshness; 95% of assertions represented in lineage.

## Resume bullet

Built a reproducible synthetic commerce warehouse-quality control plane with seeded fixtures, dbt-style structural/relational/semantic tests, reconciliation tolerances, freshness gates, schema evolution checks, and OpenLineage-compatible assertion evidence.
