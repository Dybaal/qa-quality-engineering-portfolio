# 07 — Cross-Browser Checkout, Accessibility, Visual Regression, and Flake Evidence Lab

**Roles:** QA Automation, Web QA, SDET, Accessibility QA, Quality Engineer.

## Goal

Demonstrate visible product-quality judgment: browser coverage, accessibility, visual change control, reproducible diagnostics, and honest flake reporting.

## Build

Thin storefront over shared APIs: catalog, cart, guest checkout, promo code, payment success/failure simulator, confirmation, and responsive errors. Use Playwright projects for Chromium/Firefox/WebKit and one mobile profile, deterministic fixtures, CI matrix/shards, trace/screenshot/video artifacts, and axe checks.

## Test portfolio

- Happy path, validation, payment decline, timeout, promo boundaries, refresh/replay, confirmation.
- PR smoke and nightly browser/device matrix with explicit locale/timezone/viewport/network metadata.
- Initial, menu, cart, validation-error, and confirmation accessibility states.
- Roles/names/states, keyboard path, visible focus, modal focus restoration, zoom/reflow, reduced motion.
- Stable-region visual snapshots, narrow masking, intentional seeded visual defect.
- First-attempt/final outcome reporting; retries classify flake instead of silently green.</n
## Evidence

Matrix rationale, journey tests, WCAG criterion map, axe JSON, keyboard checklist, visual baseline/diff, flake report, run manifest, traces, and limitations log.

## Acceptance targets

Critical checkout passes the declared matrix; every failure has seed and trace/screenshot; no untriaged automated accessibility findings; critical journey works by keyboard; seeded visual defect fails with readable diff; first-attempt, final, and flaky outcomes remain separate.

## Resume bullet

Automated a deterministic synthetic checkout across browser/mobile Playwright projects with API-backed fixtures, accessibility checks, visual baselines, diagnostic retries, and commit-linked traces; separated flaky from genuinely passing outcomes.
