# 02 — Multi-Tenant OAuth/OIDC Authorization and Abuse-Resistance Laboratory

**Roles:** Security QA, SDET, QA Automation, API Security Tester, Quality Engineer.

## Goal

Turn authentication, authorization, tenant isolation, session lifecycle, abuse controls, and safe evidence handling into executable regression tests.

## Build

Two synthetic tenants; viewer/editor/admin roles; colliding-looking resource IDs; local OIDC simulator; BFF/API; PostgreSQL with `tenant_id`; Redis sessions/quotas; OPA or auditable policy module; audit stream; OpenTelemetry; and a vulnerable mode with missing tenant predicates and weakened token checks.

## Test portfolio

- Authorization Code + PKCE, state/nonce, exact redirect, issuer/audience/signature/expiry.
- Refresh-token rotation, logout, reset, revocation, session renewal, cookie attributes.
- RBAC/ABAC matrix for subject, tenant, role, resource, action, and environment.
- Object-ID swapping, over-posting, hidden fields, admin routes, cache, export, worker, and queue isolation.
- Enumeration, reset/OTP abuse, quota boundaries, replay, and bounded error responses.
- Secret/token/password/reset-link scanning across logs, traces, screenshots, and artifacts.

## Evidence

Threat model, requirements-to-test matrix, authorization matrix, policy-decision logs, redacted audit records, vulnerable-versus-fixed run, secret scan, token/session diagrams, and defect/retest table.

## Acceptance targets

30+ cases; every protected endpoint covered or waived; cross-tenant attempts produce zero side effects; invalid protocol claims rejected; revoked credentials unusable within the declared SLA; zero raw credential leakage.

## Resume bullet

Created a synthetic OAuth/OIDC security regression harness covering PKCE, token validation, RBAC/ABAC, tenant isolation across API/cache/worker paths, session lifecycle, abuse quotas, and redacted audit evidence; mutation-tested missing tenant predicates and token checks.
