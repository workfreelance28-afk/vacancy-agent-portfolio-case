# Architecture

Vacancy Agent is built as a small AI workflow with clear boundaries between deterministic logic, model assessment, storage and user decisions. A second stage added the pieces needed to run it as a small, measurable production-like service.

## Main Flow (MVP)

```text
SuperJob / Habr / HH
-> vacancy normalization
-> deterministic hard filters
-> local LLM assessment through LM Studio
-> storage (SQLite / PostgreSQL)
-> Telegram delivery
-> manual user decision
```

## Added in Stage 2

```text
FastAPI (health/ready, jobs, vacancies)
-> background job queue -> worker (bounded retries, timeout, idempotency)
-> structured JSON logs + metrics (request/job correlation IDs)
-> MCP tool: schema validation -> backend preflight -> human-in-the-loop
   approval (protocol elicitation) -> backend re-validation -> local draft
```

## Components

- **Vacancy sources:** external vacancy providers are normalized into one internal vacancy shape.
- **Hard filters:** deterministic Python rules reject clearly unsuitable vacancies before any LLM call.
- **Local LLM assessment:** a local DeepSeek/Qwen model scores borderline or promising vacancies through LM Studio.
- **Storage:** PostgreSQL in the production-like setup, SQLite for local/dev — same repository contract, migrated on startup.
- **Telegram:** delivers selected vacancies as cards with manual actions.
- **API:** a FastAPI service (health/ready, vacancy list/detail, job enqueue/status) — read-only or approval-gated, never auto-applies.
- **Job queue + worker:** background processing with bounded retries, timeouts and idempotency — a repeat delivery cannot create a duplicate, and a final failure is diagnosable, not silent.
- **Observability:** structured JSON logs and a minimal metrics endpoint, correlated by request/job ID, without vacancy text, prompts or secrets.
- **MCP tool:** a single approval-gated tool (prepare a local application draft) built on the official MCP Python SDK, exposed only over a local stdio transport.

## Key Design Choice

The LLM does not submit applications, rewrite the candidate profile or make final decisions. It ranks, explains and can draft — but never on its own authority. The user reviews the result and decides what to do, and for the MCP tool specifically, approval comes through the protocol's own human-in-the-loop mechanism, not a value the model could set itself.

This keeps the workflow useful while avoiding the main risk of job-search automation: letting a model invent candidate experience, send inaccurate applications, or authorize its own actions.
