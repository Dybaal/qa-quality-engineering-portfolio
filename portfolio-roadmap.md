# Deep Research Roadmap: Seven Flagship QA/SDET Projects

## Portfolio strategy

The strongest version of this portfolio is not seven unrelated full-stack applications. It is one deterministic, synthetic commerce platform with seven focused quality-engineering capabilities around it. The shared platform contains an Order API, Inventory service, Payment emulator, event bus, data warehouse, tenant-aware identity layer, RAG knowledge base, and thin storefront. The same synthetic orders, users, products, events, documents, traces, and CI evidence can support different interview stories.

This structure is commercially useful because it demonstrates that quality is not just a collection of UI scripts. It shows contract thinking, business invariants, distributed failure reasoning, security boundaries, data reconciliation, AI evaluation, observability, accessibility, and release decisions. The seven projects are complementary and can be presented as six or seven resume entries rather than seven separate products.

## 1. Synthetic Commerce API Contract and Reliability Mesh

**Target roles:** QA Engineer, SDET, QA Automation, API Test Engineer, Quality Engineer.

**Business risk:** A checkout may time out after the server committed, retry a non-safe operation, create duplicate orders, mishandle 429/503 responses, or silently break a consumer when a provider changes its schema. Pact contracts verify interactions exercised by consumers, but business-invariant and provider-conformance tests are still required. [1]

**Build:** Create an Order REST API, GraphQL operations facade, checkout worker, fulfillment worker, Inventory virtual provider, Payment virtual provider, gateway/rate limiter, and small dashboard. Use Docker Compose, PostgreSQL or SQLite, Pact, OpenAPI/GraphQL schema tooling, an injectable fault proxy, OpenTelemetry, and a local trace/metrics viewer.

**Test design:** Generate consumer contracts from real client calls and verify them against provider states such as `order_exists`, `inventory_reserved`, `payment_authorized`, `tenant_isolated`, and `transient_failure`. Test status/error shapes, headers, pagination, authorization, and business invariants. Replay the same idempotency key after a lost response and concurrently; assert one logical order/payment effect. Inject 408, 409, 429, 500, 502, 503, timeout, and connection-reset faults. Assert that only safe or idempotent operations retry, attempts and deadlines are bounded, backoff is within a declared range, and no retry storm occurs. Evolve the GraphQL schema additively, deprecate fields, and reject breaking removal/type/nullability changes. Trace the request from client through gateway and virtual provider.

**Evidence:** OpenAPI and SDL, Pact files and verification report, compatibility matrix, fault matrix, retry/throttle histograms, schema diff, run manifest, redacted trace, defect ledger, and CI gate output.

**Acceptance targets:** 100% of committed consumer contracts verify; zero undocumented breaking changes; 100/100 deterministic duplicate-submit runs create one logical effect; no request exceeds retry/deadline budgets; every injected fault has an assertion; zero cross-tenant reads/writes; identical seed produces identical verdict.

**Resume bullet:** Built a Dockerized synthetic commerce API quality mesh with Pact/OpenAPI/GraphQL contracts, deterministic provider faults, idempotency and retry tests, rate-limit measurements, and OpenTelemetry evidence; gated releases on compatibility, tenant isolation, and exactly-one business-effect invariants.

## 2. Multi-Tenant OAuth/OIDC Authorization and Abuse-Resistance Laboratory

**Target roles:** Security QA, SDET, QA Automation, API Security Tester, Quality Engineer.

**Business risk:** Authentication does not automatically establish tenant isolation. A valid user or token can still access another tenant through an object ID, cache, export, worker, or repository query. OWASP treats object, property, and function authorization, unrestricted resource consumption, and broken authentication as distinct API risks. [2] [3]

**Build:** Create a B2B order console with two tenants, colliding-looking resource IDs, viewer/editor/admin roles, department and region attributes, session and reset flows, and a local OIDC simulator. Add an intentionally vulnerable mode for missing tenant predicates, weakened audience checks, and unsafe logging. Use PostgreSQL with `tenant_id`, Redis for sessions/quotas, OPA or a small auditable policy module, an audit stream, and OpenTelemetry.

**Test design:** Exercise Authorization Code with PKCE, state/nonce binding, exact redirect handling, issuer/audience/signature/expiry validation, refresh-token rotation, logout, reset, and revocation. Build a subject-tenant-role-resource-action matrix. Test object-ID swapping, over-posting, hidden fields, admin routes, unknown roles, missing attributes, cache leakage, exports, queues, and background jobs. Verify Secure/HttpOnly/SameSite cookie policy, session renewal after login and privilege change, invalidation after logout/reset/revocation, bounded enumeration responses, and per-subject/IP/tenant quotas. Scan all artifacts for tokens, passwords, reset links, and secrets.

**Evidence:** Threat model, requirements-to-test traceability matrix, authorization matrix, vulnerable-versus-fixed run, redacted audit schema, token/session diagrams, contract and denial-shape report, secret scan, policy decision logs, and defect table.

**Acceptance targets:** at least 30 security cases; 100% of protected endpoints have coverage or a documented waiver; all cross-tenant attempts cause zero side effects; invalid issuer/audience/signature/nonce/PKCE/expiry/redirect values are rejected; revoked credentials cannot be reused within the declared synthetic SLA; zero credential leakage.

**Resume bullet:** Created a synthetic OAuth/OIDC security regression harness covering PKCE, token validation, RBAC/ABAC, tenant isolation across API/cache/worker paths, session lifecycle, abuse quotas, and redacted audit evidence; mutation-tested missing tenant predicates and token checks.

## 3. Event-Driven Order Saga and Webhook Correctness Lab

**Target roles:** SDET, QA Automation, Integration Test Engineer, Quality Engineer.

**Business risk:** At-least-once delivery, dropped acknowledgements, duplicate webhooks, out-of-order events, and service restarts can double-charge, oversell inventory, create duplicate shipments, or send a success notification before payment is confirmed. Stripe documents that webhook order is not guaranteed and recommends tracking event IDs; AWS recommends transactional outbox plus idempotent consumers. [4] [5]

**Build:** Model `CREATED`, `INVENTORY_RESERVED`, `PAYMENT_PENDING`, `PAID`, `FULFILLMENT_REQUESTED`, `SHIPPED`, `NOTIFIED`, `CANCELLED`, `COMPENSATING`, and `FAILED` states. Add Order, Inventory, Payment, Shipping, Notification, and read-model services. Use PostgreSQL, Redpanda/Kafka or a durable queue abstraction, transactional outbox, inbox/dedup table, dead-letter queue, signed payment-webhook emulator, and OpenTelemetry.

**Test design:** Reject illegal state transitions without mutation. Cover timeout after commit, duplicate checkout, duplicate messages, dropped acknowledgements, out-of-order reserve/release and payment/refund events, read-model delay, broker redelivery, restart, poison payload, invalid signatures, stale timestamps, inventory contention, payment decline, shipping timeout, and notification retry. Permute delivery order and replay batches; after quiescence, final order state and business balances must be identical. Run concurrent reservations against limited stock and assert no negative inventory or over-reservation.

**Evidence:** Event envelope schema, state diagram, fault manifest, event ledger, inbox/outbox tables, expected-versus-observed state report, broker offsets, trace linking producers/consumers, SQL invariant queries, and incident-style postmortem.

**Acceptance targets:** 1,000 seeded saga runs and 100 concurrent checkouts produce zero duplicate successful charges, negative inventory, duplicate shipments, or duplicate notifications; all accepted webhooks are signature-checked and deduplicated; every committed state transition has an event/outbox record; reordered/replayed batches converge.

**Resume bullet:** Implemented and tested a synthetic order saga with transactional outbox/inbox, signed webhook emulation, duplicate/out-of-order replay, deterministic concurrency faults, and trace-linked invariants proving at-most-one business effect under at-least-once delivery.

## 4. Synthetic Data Pipeline, Reconciliation, Freshness, and Lineage Control Plane

**Target roles:** Data Quality Engineer, Analytics QA, ETL Test Engineer, Quality Engineer.

**Business risk:** A dashboard can be available while being wrong because of duplicate events, late partitions, schema drift, orphan facts, silent truncation, or a failed incremental load. dbt documents freshness as an SLA-oriented check and notes it must be explicitly scheduled; OpenLineage defines data-quality assertion facets with status, severity, target, expected, and actual values. [6] [7]

**Build:** Generate synthetic commerce events into JSON/CSV/Parquet, load DuckDB or PostgreSQL, transform Bronze/Silver/Gold models, and publish revenue/inventory datasets. Include refunds, late events, duplicates, currency metadata, malformed records, missing partitions, and versioned schemas. Use dbt Core or dbt-style SQL, pytest, JSON Schema, and OpenLineage-compatible events.

**Test design:** Check primary-key uniqueness, not-null, accepted values, types, partition completeness, schema compatibility, foreign keys, orphans, duplicate/idempotency keys, source-to-target row counts, aggregate reconciliation, revenue math, inventory totals, late-arrival handling, rerun idempotency, freshness, null-rate/distribution drift, and quarantine. Mutate by deleting columns, duplicating rows, shifting timestamps, altering amounts, and dropping partitions. Link every assertion to run ID, input hash, dataset version, Git SHA, seed, model, and remediation state.

**Evidence:** Generator and scenario manifests, schema versions, compiled SQL, quality report, reconciliation report, freshness dashboard, lineage events/graph, defect fixtures, data dictionary, and runbook.

**Acceptance targets:** 20,000 synthetic facts over 30 partitions; 12 injected defect scenarios; zero clean-baseline blocking failures; breaking schema changes rejected while compatible additions pass; a 0.1% amount discrepancy detected; freshness failure when the SLA is exceeded; at least 95% of assertion results appear in lineage.

**Resume bullet:** Built a reproducible synthetic commerce warehouse-quality control plane with seeded fixtures, dbt-style structural/relational/semantic tests, reconciliation tolerances, freshness gates, schema evolution checks, and OpenLineage-compatible assertion evidence.

## 5. Synthetic Payment Ledger, State Machine, and Reconciliation QA Lab

**Target roles:** Fintech QA, Payments QA, SDET, Data Quality Engineer.

**Business risk:** Payment retries, duplicate or reordered events, refunds, fees, payout files, and late failures can create or lose economic value while leaving an audit trail that cannot explain what happened. Stripe describes immutable event-log ledgers and double-entry balance invariants; currency handling is provider- and currency-specific. [8] [9]

**Build:** Create a provider-neutral payment API and state machine with `created`, `requires_action`, `authorized`, `captured`, `failed`, `canceled`, `refunded`, `partially_refunded`, `disputed`, and `settled`. Add immutable journal, event inbox/outbox, deterministic risk-rule service, synthetic payout file, reconciliation engine, and append-only audit export. Represent money as integer minor units plus ISO currency; never use binary floating point.

**Test design:** Reject illegal transitions without ledger mutation. Assert debits equal credits per currency, unique immutable event IDs, replayed balances, clearing-account closure, additive reversals, and one economic transaction per external reference. Deliver 10,000 duplicated/reordered events; quarantine same-ID/different-payload collisions. Reconcile matched, missing-in-ledger, missing-in-provider, amount mismatch, currency mismatch, duplicate, and timing-difference cases. Test velocity, amount, country/currency mismatch, repeated declines, missing attributes, rule precedence, shadow mode, and rollout.

**Evidence:** Money/rounding decision record, state table, ledger schema, reconciliation exception report, provider-feed fixtures, tamper-evident audit export, invariant SQL, trace from API through ledger/risk/reconciliation, mutation report, and postmortem.

**Acceptance targets:** 100% illegal-transition tests rejected without mutation; 10,000 duplicate/reordered deliveries create zero duplicate journal entries and zero balance drift; golden reconciliation classifications reach 100%; zero unreviewed high-severity exceptions; 95%+ line coverage and 90%+ mutation score on critical accounting/rules code as declared portfolio targets.

**Resume bullet:** Developed a synthetic payment QA lab with integer-minor-unit money, immutable double-entry ledger invariants, idempotent webhook ingestion, payment-state transition tests, deterministic risk rules, and payout reconciliation evidence.

## 6. Synthetic RAG Evaluation and Prompt-Injection Red-Team Gate

**Target roles:** AI QA, SDET, Quality Engineer, Data Quality, AppSec-minded Automation Engineer.

**Business risk:** A RAG system can retrieve the wrong document, cite unsupported claims, leak synthetic secrets, follow instructions embedded in documents, or incur unbounded token and latency cost. AWS separates retrieve-only from retrieve-and-generate evaluation and lists context relevance, coverage, correctness, completeness, faithfulness, citation precision, citation coverage, harmfulness, and refusal as distinct dimensions. OWASP lists prompt injection, sensitive-information disclosure, excessive agency, vector weaknesses, and unbounded consumption among GenAI risks. [10] [11]

**Build:** Create a versioned synthetic support knowledge base with policy revisions, access tiers, conflicting documents, decoys, fake PII, canary secrets, attack labels, expected facts, acceptable citations, and expected actions. Provide offline deterministic mode and optional model adapter. Record corpus hash, model/config version, prompt ID, embedding ID, seed, trace ID, latency, tokens, and estimated cost.

**Test design:** Use qrels for Recall@k, precision@k, MRR/NDCG, context coverage, stale-document handling, and access filtering. Check correctness, completeness, groundedness, citation precision/coverage, output schema, prohibited strings, and refusal. Run direct/indirect prompt injection, system-prompt extraction, canary leakage, unauthorized mock-tool calls, obfuscation, multilingual variants, oversized requests, and unbounded-loop cases. Blind-review 10–20% of cases and report agreement; never use an LLM judge as the sole oracle.

**Evidence:** Versioned corpus and golden set, qrels, attack corpus, evaluation JSONL/HTML, baseline comparison, trace examples, human-review rubric, failure analysis, and release-gate log.

**Acceptance targets:** 95% of labeled retrieval cases include a relevant document in top five; 90% citation coverage on answerable cases; zero synthetic PII canaries in responses or telemetry; zero privileged-tool/system-instruction overrides; p95 local latency and median cost remain within declared portfolio targets; no more than a two-point groundedness regression; human/automated agreement reported.

**Resume bullet:** Built an offline-reproducible RAG QA and red-team gate with labeled retrieval metrics, groundedness/citation checks, synthetic canaries, prompt-injection attacks, tool-authorization contracts, human calibration, and trace-linked release evidence.

## 7. Cross-Browser Checkout, Accessibility, Visual Regression, and Flake Evidence Lab

**Target roles:** QA Automation, Web QA, SDET, Accessibility QA, Quality Engineer.

**Business risk:** A correct API can still fail customers through browser-specific checkout regressions, inaccessible error states, visual breakage, or an unstable suite whose retries hide defects. Playwright projects support browser/device configurations; retries classify tests as passed, flaky, or failed based on first and subsequent attempts. [12] [13]

**Build:** Create a thin storefront over the shared commerce APIs: catalog, cart, guest checkout, promo code, payment success/failure simulator, confirmation, and responsive error states. Freeze time, locale, timezone, fonts, seed, and network profile. Use Playwright projects for Chromium/Firefox/WebKit and one mobile profile, CI matrix/shards, trace/screenshot/video artifacts, and axe checks.

**Test design:** Cover happy path, validation, payment decline, timeout, promo boundaries, refresh/replay, and confirmation. Use PR smoke on Chromium desktop/mobile and nightly full matrix. Scan initial, menu, cart, error, and confirmation states. Assert roles/names/states, keyboard path, visible focus, modal focus restoration, zoom/reflow, and reduced motion. Snapshot stable regions only; mask narrowly; record first-attempt and final outcomes separately; quarantine only with owner, ticket, and expiry.

**Evidence:** Browser/device rationale, journey tests, WCAG criterion-to-test map, axe JSON, keyboard checklist, visual baseline/diff, retry/flaky report, run manifest, traces, and limitations log.

**Acceptance targets:** critical checkout passes declared matrix; every failure has seed and trace/screenshot; zero untriaged automated accessibility findings; critical journey works by keyboard; seeded visual defect fails with readable diff; first-attempt/final/flake rates are reported separately.

**Resume bullet:** Automated a deterministic synthetic checkout across browser/mobile Playwright projects with API-backed fixtures, accessibility checks, visual baselines, diagnostic retries, and commit-linked traces; separated flaky from genuinely passing outcomes.

## Shared architecture and evidence contract

```text
qa-portfolio/
  apps/ order-api/ inventory-provider/ payment-provider/ gateway/
  workers/ saga/ storefront/ rag-api/
  contracts/ openapi/ graphql/ pact/ event-schemas/
  security/ oidc-fixtures/ policy/ authorization-matrix/
  data/ generator/ scenarios/ golden/ corruptions/
  ledger/ money/ state-machine/ journal/ reconciliation/
  tests/ unit/ api/ contract/ integration/ property/ mutation/
  tests/ concurrency/ security/ accessibility/ visual/ rag-evaluation/
  observability/ otel-collector/ dashboards/ redaction/
  evidence/ manifests/ reports/ traces/ screenshots/ schemas/
  scripts/ seed.sh run-scenario.sh verify-gates.sh clean.sh
  .github/workflows/
  docs/ architecture.md threat-model.md test-strategy.md
  docs/ fault-matrix.md data-classification.md runbook.md limitations.md
```

Every evidence bundle should contain `run_id`, `git_sha`, `scenario_id`, `seed`, `dependency_lock_hash`, `test_id`, `attempt`, `result`, `duration_ms`, `fault_parameters`, `trace_id`, artifact paths, and gate decision. Never upload raw secrets or unredacted request bodies.

## 90-day build order

**Days 1–10:** Create the monorepo, Docker Compose, pinned runtime/dependencies, synthetic-data generator, scenario manifest, telemetry redaction rules, run ID format, and CI artifact convention. Implement one order endpoint and one seeded test.

**Days 11–25:** Build the API mesh, OpenAPI/SDL, first Pact contracts, idempotency storage, fault proxy, compatibility matrix, and first trace.

**Days 26–38:** Add OIDC/PKCE, tenant-scoped repositories, policy matrix, audit events, saga, outbox/inbox, signed webhook emulator, and replay harness.

**Days 39–52:** Add warehouse-quality models, reconciliation, schema versions, freshness checks, defect operators, lineage events, and report joins.

**Days 53–66:** Add integer-minor-unit money, journal postings, payment transitions, payout fixtures, reconciliation classifications, and mutation tests.

**Days 67–77:** Add offline RAG golden set/red-team gate and the thin storefront with Playwright matrix.

**Days 78–84:** Run clean and seeded-defect suites from a fresh checkout; remove unexplained flake; pin fonts/time/locale; redact artifacts; document thresholds.

**Days 85–90:** Record a five-minute demo, publish architecture/threat diagrams, add one-page test strategies, run final CI, and prepare two failure stories for interviews.

## What to avoid

Do not build seven independent full-stack products. Do not use real cards, credentials, customers, addresses, production logs, third-party load targets, or clinical/medical data. Do not claim PCI, SOC 2, WCAG conformance, production fraud accuracy, cloud resilience, or payment-provider equivalence. Do not blanket-retry UI failures. Do not treat Pact as a complete provider test, schema diff as semantic proof, ledger balance as business correctness, or an LLM judge as the sole RAG oracle.

## References

[1]: https://docs.pact.io/ "Pact Documentation"
[2]: https://owasp.org/API-Security/editions/2023/en/0x11-t10/ "OWASP API Security Top 10 2023"
[3]: https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html "OWASP Authorization Cheat Sheet"
[4]: https://docs.stripe.com/webhooks "Stripe Webhooks"
[5]: https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/transactional-outbox.html "Transactional outbox pattern"
[6]: https://docs.getdbt.com/docs/deploy/source-freshness "dbt Source Freshness"
[7]: https://openlineage.io/docs/spec/facets/dataset-facets/data_quality_assertions/ "OpenLineage Data Quality Assertions Facet"
[8]: https://stripe.dev/blog/ledger-stripe-system-for-tracking-and-validating-money-movement "Stripe Ledger: Tracking and Validating Money Movement"
[9]: https://docs.stripe.com/currencies "Stripe Currencies"
[10]: https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base-evaluation-metrics.html "Amazon Bedrock Knowledge Base Evaluation Metrics"
[11]: https://genai.owasp.org/llm-top-10/ "OWASP Top 10 for LLM Applications"
[12]: https://playwright.dev/docs/test-projects "Playwright Test Projects"
[13]: https://playwright.dev/docs/test-retries "Playwright Test Retries"
