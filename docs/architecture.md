# Architecture

Vacancy Agent is built as a small AI workflow with clear boundaries between deterministic logic, model assessment, storage and user decisions.

## Main Flow

```text
SuperJob / Habr / HH
-> vacancy normalization
-> deterministic hard filters
-> local LLM assessment through LM Studio
-> SQLite persistence
-> Telegram delivery
-> manual user decision
```

## Components

- **Vacancy sources:** external vacancy providers are normalized into one internal vacancy shape.
- **Hard filters:** deterministic Python rules reject clearly unsuitable vacancies before any LLM call.
- **Local LLM assessment:** a local DeepSeek/Qwen model scores borderline or promising vacancies through LM Studio.
- **SQLite:** stores vacancies, filter decisions, model assessments and user statuses.
- **Telegram:** delivers selected vacancies as cards with manual actions.
- **Local API:** exposes saved vacancies through a local read-only FastAPI interface.

## Key Design Choice

The LLM does not submit applications, rewrite the candidate profile or make final decisions. It ranks and explains. The user reviews the result and decides what to do.

This keeps the workflow useful while avoiding the main risk of job-search automation: letting a model invent candidate experience or send inaccurate applications.
