# Walkthrough

Use this as a short 3-4 minute explanation of the case.

## 1. Problem

I wanted a practical tool for finding Junior/Junior+ AI Integration vacancies without manually checking multiple sources every day.

The important constraint was safety: the system must not invent candidate experience, must not send applications automatically, and — once it started calling an LLM as a tool — must not let that model authorize its own actions either.

## 2. Solution

Vacancy Agent collects vacancies, normalizes them, applies deterministic filters, sends promising cases to a local LLM for ranking, stores the result and sends selected cards to Telegram. The user then decides whether to open, review or skip the vacancy.

A second stage turned that MVP into a small, measurable production-like service: a FastAPI + PostgreSQL API, a background job queue with retries and idempotency, Docker Compose + CI, a versioned evaluation regression gate, structured observability, and a safe MCP tool that prepares a local application draft only after a human approves it through the protocol itself.

## 3. Architecture

The base workflow is:

```text
external sources -> filters -> local LLM -> storage -> Telegram -> human decision
```

Stage 2 added:

```text
API -> job queue -> worker (retries, idempotency) -> structured logs/metrics
     -> MCP tool: schema -> preflight -> human approval -> re-validation -> draft
```

Python owns the business rules, persistence and the approval boundary; the model only assesses relevance and drafts text — it never decides or sends anything on its own.

## 4. Evidence

Automated (see [docs/evals.md](evals.md) for the full screenshot set):

- [`761 passed, 32 skipped`](../screenshots/12_tests_761_passed.png) (the skips need a live test PostgreSQL, which CI provides);
- [Docker Compose](../screenshots/08_docker_compose_healthy.png) bringing up `api`, `worker` and `postgres`, all `healthy`;
- [an offline evaluation regression gate](../screenshots/09_eval_gate_pass.png) on a versioned 50-case dataset — quality, latency and cost compared against an approved baseline, no live LLM call;
- [one full incident](../screenshots/10_incident_diagnosis_passed.png) (transient failure → retry → final failure) diagnosed purely from structured logs and metrics;
- [a live MCP approval run](../screenshots/11_mcp_approval_demo.png): a real elicitation prompt answered interactively, then fail-closed without a configured LLM, an unsupported-client case and an ineligible-vacancy case — all fail-closed except the genuine approval.

From the original MVP run:

- [`604 passed`](../screenshots/04_tests_604_passed.png); [`400 fetched`, `1 accepted`, `1 notified`](../screenshots/05_scan_summary_400_fetched_1_notified.png);
- [Telegram cards with manual actions](../screenshots/06_telegram_vacancy_cards.png); [local API startup](../screenshots/07_local_api_startup.png).

## 5. What It Proves

This case shows practical AI integration skills, from a working MVP to a small production-like service:

- external API integration and workflow automation;
- local LLM usage through LM Studio, with deterministic filters before any model call;
- persistence and status tracking, including a PostgreSQL-backed job queue with retries and idempotency;
- a reproducible local stack (Docker Compose) and a CI pipeline (lint, tests, secret scan, eval gate);
- versioned evaluation with an honest regression gate, not a one-off demo number;
- minimal but real observability: diagnosing an incident from logs and metrics alone;
- safe LLM tool-calling: the model can propose and draft, but only the backend validates and only a human — through the protocol itself — approves;
- privacy-aware public packaging, and being explicit about what's left out and why.
