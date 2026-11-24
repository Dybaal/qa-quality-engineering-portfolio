# 06 — Synthetic RAG Evaluation and Prompt-Injection Red-Team Gate

**Roles:** AI QA, SDET, Quality Engineer, Data Quality, AppSec-minded Automation Engineer.

## Goal

Show that an AI application is evaluated as a measurable system with retrieval, generation, security, cost, latency, and human-review evidence—not just a successful demo prompt.

## Build

Versioned synthetic support corpus with policy revisions, access tiers, conflicting/decoy documents, fake PII, canary secrets, attack labels, expected facts, acceptable citations, and expected actions. Add ingester/chunker, hybrid/vector retriever, RAG API, citation formatter, policy guard, mock tool service, evaluator runner, OpenTelemetry, and HTML/JSON report.

## Test portfolio

- Recall@k, precision@k, MRR/NDCG, context coverage, stale-document handling, and tenant filtering.
- Correctness, completeness, groundedness/faithfulness, citation precision/coverage, schema validity, refusal.
- Direct/indirect prompt injection, system-prompt extraction, canary leakage, unauthorized tool calls, obfuscation, multilingual variants, oversized requests, and unbounded loops.
- Blind human review of 10–20% of cases, agreement reporting, and disagreement analysis.
- Baseline comparisons across corpus, chunk, embedding, prompt, model, and evaluator versions.
- Cost, latency, token, trace, and redaction checks.

## Evidence

Versioned corpus/golden set, qrels, attack corpus, evaluation JSONL/HTML, baseline diff, trace examples, human rubric, failure analysis, and CI gate log.

## Acceptance targets

95% top-5 relevant retrieval; 90% citation coverage; zero canary leakage in response/telemetry; zero privileged-tool/system-instruction override; declared p95/cost target; no more than two-point groundedness regression; human/automated agreement reported.

## Resume bullet

Built an offline-reproducible RAG QA and red-team gate with labeled retrieval metrics, groundedness/citation checks, synthetic canaries, prompt-injection attacks, tool-authorization contracts, human calibration, and trace-linked release evidence.
