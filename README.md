# QA/SDET Quality Engineering Portfolio

This portfolio contains seven deep, non-clinical projects designed for QA Engineer, SDET, QA Automation, Quality Engineer, Data Quality, API Test, Security QA, and AI QA roles.

The projects share one synthetic commerce platform rather than being seven unrelated toy applications. The common platform contains tenants, users, catalog items, inventory, orders, payments, events, documents, and browser journeys. That shared foundation lets one candidate demonstrate API contracts, distributed-systems testing, security, data quality, payments, AI evaluation, accessibility, visual regression, observability, and release confidence without maintaining seven separate products.

## Flagship projects

1. **Synthetic Commerce API Contract and Reliability Mesh** — REST/GraphQL contract testing, provider states, idempotency, retries, rate limiting, schema evolution, fault injection, and trace evidence.
2. **Multi-Tenant OAuth/OIDC Authorization and Abuse-Resistance Laboratory** — PKCE, token validation, RBAC/ABAC, tenant isolation, session lifecycle, abuse limits, secret redaction, and auditability.
3. **Event-Driven Order Saga and Webhook Correctness Lab** — transactional outbox/inbox, duplicate and out-of-order events, signed webhooks, concurrency, compensation, and exactly-once business effects.
4. **Synthetic Data Pipeline, Reconciliation, Freshness, and Lineage Control Plane** — SQL/dbt-style checks, schema evolution, freshness, reconciliation, quarantine, anomaly detection, and OpenLineage-compatible evidence.
5. **Synthetic Payment Ledger, State Machine, and Reconciliation QA Lab** — integer-minor-unit money, double-entry invariants, payment state transitions, risk rules, payout reconciliation, mutation testing, and audit evidence.
6. **Synthetic RAG Evaluation and Prompt-Injection Red-Team Gate** — labeled retrieval metrics, groundedness/citation checks, synthetic canaries, prompt-injection attacks, tool authorization, human calibration, cost, latency, and regression gates.
7. **Cross-Browser Checkout, Accessibility, Visual Regression, and Flake Evidence Lab** — Playwright browser/device projects, accessibility checks, visual baselines, diagnostic retries, and reproducible evidence.

## Portfolio rules

- Use synthetic identities, orders, cards, addresses, documents, secrets, and metrics only.
- Every run is reproducible from a seed, scenario ID, Git SHA, dependency lock hash, and environment manifest.
- Every seeded defect has a defect report, failing evidence, root cause, fix, and verified rerun.
- Every project produces a machine-readable evidence bundle instead of only screenshots.
- Do not claim PCI, SOC 2, WCAG conformance, production fraud accuracy, cloud resilience, or provider equivalence from a local portfolio lab.

## Recommended build order

Build Projects 1–4 first because they show the broadest quality-engineering judgment. Add Project 5 for fintech roles, Project 6 for AI roles, and Project 7 as the visible UI/accessibility layer. The resume should list six or seven projects at most; the repository can show the shared foundation and related modules.

See [`portfolio-roadmap.md`](portfolio-roadmap.md) for the detailed research-backed plan and [`projects/`](projects/) for the individual project briefs.
