# 01 — Synthetic Commerce API Contract and Reliability Mesh

**Roles:** QA Engineer, SDET, QA Automation, API Test Engineer, Quality Engineer.

## Goal

Prove that consumers and providers can evolve safely and that ambiguous distributed failures do not create duplicate business effects.

## Build

Docker Compose services: Order REST API, GraphQL operations facade, checkout/fulfillment consumers, Inventory virtual provider, Payment virtual provider, gateway/rate limiter, PostgreSQL or SQLite, fault proxy, Pact/OpenAPI/SDL artifacts, OpenTelemetry Collector, and local trace viewer.

## Test portfolio

- Consumer-driven Pact contracts with provider states.
- OpenAPI response/error/pagination/authorization checks.
- GraphQL additive/deprecation/breaking-change checks.
- Idempotency replay after lost response and concurrent duplicate submissions.
- 408/409/429/5xx/timeout/connection-reset fault matrix.
- Retry classification, bounded attempts/deadlines, backoff/jitter, Retry-After, and retry-storm checks.
- Cross-tenant and business-invariant checks.
- Trace propagation from client through gateway and provider.

## Evidence

Compatibility matrix, schema-diff report, provider verification report, fault matrix, retry/throttle histogram, redacted trace, reproducible scenario manifest, seeded defect report, and CI release-gate output.

## Acceptance targets

100% contracts verify; zero undocumented breaking changes; 100/100 duplicate-submit repetitions create one logical effect; no retry/deadline budget violations; zero cross-tenant effects; same seed produces same verdict.

## Resume bullet

Built a Dockerized synthetic commerce API quality mesh with Pact/OpenAPI/GraphQL contracts, deterministic provider faults, idempotency and retry tests, rate-limit measurements, and OpenTelemetry evidence; gated releases on compatibility, tenant isolation, and exactly-one business-effect invariants.
