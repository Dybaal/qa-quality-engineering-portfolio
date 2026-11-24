# 05 — Synthetic Payment Ledger, State Machine, and Reconciliation QA Lab

**Roles:** Fintech QA, Payments QA, SDET, Data Quality Engineer.

## Goal

Test money movement as a stateful, auditable system rather than treating payments as a collection of HTTP status codes.

## Build

Provider-neutral payment API; integer-minor-unit money model; payment state machine; immutable journal; event inbox/outbox; deterministic risk-rule service; synthetic payout file; reconciliation engine; append-only audit export; PostgreSQL; OpenTelemetry.

## Test portfolio

- `created` → `requires_action` → `authorized` → `captured` → `settled` and failure/refund/dispute paths.
- Illegal transitions without journal mutation.
- Debit/credit balance per currency, unique event IDs, replayed balances, clearing-account closure, additive reversals.
- 10,000 duplicate/reordered deliveries and same-ID/different-payload collision quarantine.
- Reconciliation classifications: matched, missing, amount/currency mismatch, duplicate, timing difference.
- Deterministic risk rules, rule precedence, boundary values, shadow mode, and rollout.
- Tamper-evident audit and trace from API through ledger/risk/reconciliation.

## Evidence

Money/rounding decision record, state table, ledger schema, invariant SQL, reconciliation exception report, provider fixtures, audit export, trace, mutation report, and postmortem.

## Acceptance targets

100% illegal transitions rejected without mutation; 10,000 replay/reorder cases produce zero journal duplication or balance drift; golden exceptions classified 100%; zero unreviewed high-severity exceptions; 95%+ coverage and 90%+ mutation score on critical code as declared portfolio targets.

## Resume bullet

Developed a synthetic payment QA lab with integer-minor-unit money, immutable double-entry ledger invariants, idempotent webhook ingestion, payment-state transition tests, deterministic risk rules, and payout reconciliation evidence.
