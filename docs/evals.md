# Tests and Evals

The project is tested as an engineering workflow, not only as a script that runs once.

## Automated Checks

Current local result:

```text
761 passed, 32 skipped
```

(The 32 skips need a live test PostgreSQL instance that isn't running in every environment; they pass in CI, which does run one.)

The test suite covers:

- domain entities and status transitions;
- vacancy normalization from external sources;
- hard-filter logic and settings validation;
- SQLite and PostgreSQL persistence, migrations and repository contracts;
- the background job queue and worker: retries, timeouts, idempotency, diagnosable final failure;
- the FastAPI routes: health/ready, vacancy list/detail, job enqueue/status;
- structured observability: one full incident (transient failure → retry → final failure) diagnosed purely from logs and metrics;
- the MCP tool's safety chain: schema validation, backend preflight, human-in-the-loop approval, decline/cancel/no-capability fail-closed, and a model trying to fake approval through a tool argument;
- Telegram card formatting and callbacks;
- expected failure paths for source, LLM, database and Telegram errors.

## Evaluation Approach

The project separates three layers:

- **Hard filters:** deterministic rejection of clearly unsuitable vacancies.
- **LLM ranking:** local model assessment after filtering.
- **Human review:** the final decision remains with the user.

This is important because a false negative can hide a potentially useful vacancy, while a false positive only costs a short manual review.

On top of that, a versioned 50-case golden dataset backs an offline regression gate: a candidate run is compared against an approved baseline on accuracy, macro-F1, per-class precision/recall, latency (p50/p95) and token cost. The gate runs entirely offline in CI — no live LLM call, no network access to LM Studio — using previously recorded runs, so it can't silently drift because a model or prompt changed without a review.

## Live Evidence

From the original MVP run (kept as evidence that the workflow was actually run, not only documented):

- [`604 passed` test result](../screenshots/04_tests_604_passed.png);
- [scan summary](../screenshots/05_scan_summary_400_fetched_1_notified.png) with `400 fetched`, `383 already known`, `16 filtered out`, `1 accepted`, `1 notified`;
- [Telegram vacancy cards](../screenshots/06_telegram_vacancy_cards.png) with manual actions;
- [local API startup](../screenshots/07_local_api_startup.png).

From stage 2:

- [`761 passed, 32 skipped`](../screenshots/12_tests_761_passed.png) — the full test suite today;
- [Docker Compose](../screenshots/08_docker_compose_healthy.png) bringing up `api`, `worker` and `postgres`, all reported `healthy`;
- [the offline eval regression gate](../screenshots/09_eval_gate_pass.png) — full metrics table and `Gate: PASS`;
- [the incident-diagnosis test](../screenshots/10_incident_diagnosis_passed.png) passing;
- [a live MCP approval run](../screenshots/11_mcp_approval_demo.png) — a real elicitation prompt answered interactively, then fail-closed without a configured LLM, an unsupported-client case and an ineligible-vacancy case, all in one run.
