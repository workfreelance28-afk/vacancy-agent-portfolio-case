# Vacancy Agent — portfolio case

Sanitized public showcase of a private AI-integration project. Started as a personal workflow for finding, filtering, ranking and reviewing Junior AI Integration vacancies with local LLM assessment and Telegram delivery (human-in-the-loop decisions throughout). A second stage turned that MVP into a small, measurable production-like service: FastAPI + PostgreSQL, a background job queue with retries, Docker Compose + CI, versioned evaluation with a regression gate, structured observability, and a safe MCP tool with human-in-the-loop approval.

This repository is a portfolio case, not the full working engineering repo. Source code, secrets, the live database, private resume files and internal development history are intentionally excluded.

## What This Shows

- Full case description, my role and what the project proves: [PORTFOLIO.md](PORTFOLIO.md)
- Architecture without private implementation details: [docs/architecture.md](docs/architecture.md)
- Tests and evaluation approach: [docs/evals.md](docs/evals.md)
- Security and privacy boundaries: [docs/security.md](docs/security.md)
- A short walkthrough script: [docs/walkthrough.md](docs/walkthrough.md)
- Screenshots of the workflow: [screenshots/](screenshots/)

## Stack

Python, Pydantic, pytest, FastAPI, PostgreSQL (SQLite for local/dev), Docker Compose, GitHub Actions, APScheduler, Telegram Bot API, SuperJob API, Habr Career public catalog, HH API as a reserve source, LM Studio, local DeepSeek/Qwen model, the official MCP Python SDK.

## Result

MVP (stage 1):

- Working private tool for daily vacancy discovery and review.
- Human-in-the-loop workflow: the system suggests vacancies, the user decides.
- Local LLM ranking after deterministic hard filters, delivered as Telegram cards.

Production-like hardening (stage 2):

- FastAPI + PostgreSQL with stable API contracts and migrations (SQLite still supported for local/dev).
- A background job queue and worker with bounded retries, timeouts and idempotency — a repeat delivery cannot create a duplicate, and a final failure is diagnosable, not silent.
- A one-command reproducible local stack via Docker Compose; GitHub Actions CI runs lint, the full test suite, a secret scan and an offline evaluation regression gate on every push.
- A versioned 50-case golden evaluation dataset with a baseline/candidate regression gate (quality, latency and cost), run entirely offline in CI.
- Structured JSON logs and a minimal metrics endpoint with request/job correlation IDs — one full incident (transient failure → retry → final failure) is diagnosable from logs and metrics alone, with no database access, no repeated LLM call, and no vacancy text or secrets in the output.
- A safe, human-approval-gated MCP tool: schema validation → backend preflight → human-in-the-loop approval through the MCP protocol's own elicitation mechanism (not a boolean argument a model could fake) → backend re-validation → local draft generation. A client that doesn't support approval, or a user who declines, fails closed — nothing is generated.
- `761` automated tests passed locally (unit + integration), plus a separate MCP protocol smoke test run against a real SQLite-backed server over the real MCP protocol.

## Limitations

- This is a personal workflow tool, not a commercial SaaS.
- The LLM result is a recommendation, not an automatic decision.
- Resume generation, cover-letter generation and auto-apply are intentionally outside the current production path; the MCP tool only prepares a local draft and never sends anything.
- The MCP server is local-only (stdio transport) — no remote MCP, no public endpoint.
- Public materials do not include real tokens, connection strings, the local database, private resume files, chat IDs, raw LLM reasoning or internal task history.

## Full Project on Request

The full private repository, source code, tests and a live walkthrough (API, worker, evals, observability, MCP approval) are available on request.
