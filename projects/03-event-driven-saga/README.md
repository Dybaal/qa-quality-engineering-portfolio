# 03 — Event-Driven Order Saga and Webhook Correctness Lab

**Roles:** SDET, QA Automation, Integration Test Engineer, Quality Engineer.

## Goal

Prove exactly-once business effects while allowing at-least-once event delivery, duplicates, out-of-order messages, retries, restarts, and compensation.

## Build

Order state machine; Inventory, Payment, Shipping, Notification, and read-model services; PostgreSQL; Redpanda/Kafka or durable queue; transactional outbox; inbox/dedup table; dead-letter queue; signed payment-webhook emulator; and OpenTelemetry.

## Test portfolio

- Legal/illegal state transitions without hidden mutation.
- Duplicate checkout and duplicate-message replay.
- Dropped acknowledgement and timeout-after-commit.
- Out-of-order reserve/release, payment/refund, and shipping events.
- Consumer restart, poison payload, invalid signature, stale timestamp, payment decline, and notification retry.
- Inventory contention and no-negative-stock invariants.
- Metamorphic event-order permutation and post-fault convergence.

## Evidence

Event envelope schema, state diagram, fault manifest, event ledger, inbox/outbox tables, expected-versus-observed state table, broker offsets, trace links, SQL invariants, and incident postmortem.

## Acceptance targets

1,000 seeded saga runs and 100 concurrent checkouts yield zero duplicate charges, negative inventory, duplicate shipments, or duplicate notifications; accepted webhooks are signature-checked and deduplicated; committed transitions have events; reordered batches converge.

## Resume bullet

Implemented and tested a synthetic order saga with transactional outbox/inbox, signed webhook emulation, duplicate/out-of-order replay, deterministic concurrency faults, and trace-linked invariants proving at-most-one business effect under at-least-once delivery.
