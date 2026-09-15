# Vacancy Agent — portfolio case

Sanitized public showcase of a private AI-integration project: a workflow for finding, filtering, ranking and reviewing Junior AI Integration vacancies with local LLM assessment, SQLite persistence, Telegram delivery and human-in-the-loop decisions.

This repository is a portfolio case, not the full working engineering repo. Source code, secrets, local database, private resume files and internal development history are intentionally excluded.

## What This Shows

- Full case description, my role and what the project proves: [PORTFOLIO.md](PORTFOLIO.md)
- Architecture without private implementation details: [docs/architecture.md](docs/architecture.md)
- Tests and evaluation approach: [docs/evals.md](docs/evals.md)
- Security and privacy boundaries: [docs/security.md](docs/security.md)
- A short walkthrough script: [docs/walkthrough.md](docs/walkthrough.md)
- Screenshots of the workflow: [screenshots/](screenshots/)

## Stack

Python, Pydantic, pytest, SQLite, FastAPI, APScheduler, Telegram Bot API, SuperJob API, Habr Career public catalog, HH API as a reserve source, LM Studio, local DeepSeek/Qwen model.

## Result

- Working private tool for daily vacancy discovery and review.
- Human-in-the-loop workflow: the system suggests vacancies, the user decides.
- Local LLM ranking after deterministic hard filters.
- SQLite persistence for vacancies, filter reasons, model assessments and user statuses.
- Telegram cards with manual actions.
- Local read-only API for saved vacancies.
- `604` automated tests passed locally.
- Latest scan summary used for the public case: `400 fetched`, `383 already known`, `16 filtered out`, `1 accepted`, `1 notified`.

## Limitations

- This is a personal workflow tool, not a commercial SaaS.
- The LLM result is a recommendation, not an automatic decision.
- Resume generation, cover-letter generation and auto-apply are intentionally outside the current production path.
- Public materials do not include real tokens, local database, private resume files, chat IDs, raw LLM reasoning or internal task history.

## Full Project on Request

The full private repository, source code, tests and live walkthrough are available on request.
