# Walkthrough

Use this as a short 2-3 minute explanation of the case.

## 1. Problem

I wanted a practical tool for finding Junior/Junior+ AI Integration vacancies without manually checking multiple sources every day.

The important constraint was safety: the system must not invent candidate experience and must not send applications automatically.

## 2. Solution

Vacancy Agent collects vacancies, normalizes them, applies deterministic filters, sends promising cases to a local LLM for ranking, stores the result in SQLite and sends selected cards to Telegram.

The user then decides whether to open, review or skip the vacancy.

## 3. Architecture

The workflow is:

```text
external sources -> filters -> local LLM -> SQLite -> Telegram -> human decision
```

This keeps the model useful, but limited. Python owns the business rules and persistence; the model only assesses relevance.

## 4. Evidence

The project has automated tests and a real local run:

- `604 passed`;
- `400 fetched`;
- `1 accepted`;
- `1 notified`;
- Telegram cards with manual actions;
- local API for saved vacancies.

## 5. What It Proves

This case shows practical AI integration skills:

- external API integration;
- workflow automation;
- local LLM usage through LM Studio;
- deterministic filters before model calls;
- persistence and status tracking;
- Telegram user interface;
- human-in-the-loop product design;
- privacy-aware public packaging.
