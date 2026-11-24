# Shared Evidence Contract

Every project should emit an evidence bundle with:

- `run_id`
- `git_sha`
- `scenario_id`
- `seed`
- `dependency_lock_hash`
- `test_id`
- `attempt`
- `result`
- `duration_ms`
- `fault_parameters`
- `trace_id` where applicable
- artifact paths
- gate decision
- defect ID and retest link when the run is a regression

Use synthetic data only. Redact request bodies, tokens, secrets, reset links, fake-but-sensitive-looking values, and raw document content before artifacts leave the local environment. Every project must include clean baseline evidence, at least one intentionally failing seeded scenario, a root-cause report, and a verified rerun.
