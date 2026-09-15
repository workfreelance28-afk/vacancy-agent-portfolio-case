# Tests and Evals

The project is tested as an engineering workflow, not only as a script that runs once.

## Automated Checks

Current local result:

```text
604 passed
```

The test suite covers:

- domain entities and status transitions;
- vacancy normalization from external sources;
- hard-filter logic;
- settings validation;
- SQLite persistence and migrations;
- scan pipeline behavior;
- Telegram card formatting and callbacks;
- local API list/detail/health routes;
- expected failure paths for source, LLM and Telegram errors.

## Evaluation Approach

The project separates three layers:

- **Hard filters:** deterministic rejection of clearly unsuitable vacancies.
- **LLM ranking:** local model assessment after filtering.
- **Human review:** the final decision remains with the user.

This is important because a false negative can hide a potentially useful vacancy, while a false positive only costs a short manual review.

## Live Evidence

The portfolio screenshots include:

- `604 passed` test result;
- scan summary with `400 fetched`, `383 already known`, `16 filtered out`, `1 accepted`, `1 notified`;
- Telegram vacancy cards with manual actions;
- local API startup.

These screenshots show that the workflow is not only documented, but has been run locally.
