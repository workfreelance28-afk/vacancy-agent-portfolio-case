# Security and Privacy

This public repository is intentionally sanitized. It is meant to show the architecture and product thinking without exposing private data or operational secrets.

## Excluded From Public Materials

- `.env` files;
- real API keys and Telegram bot tokens;
- local SQLite database;
- private resume files and personal contact data;
- Telegram chat IDs and private chats;
- raw LLM prompts, responses and reasoning with personal context;
- internal task history, handoffs and long development notes;
- virtual environments, caches and generated local artifacts.

## Runtime Boundaries

- The working project uses local configuration for secrets.
- Telegram delivery is limited to allowed chats in the private project.
- Vacancy text from external sources is treated as untrusted data.
- The local API is intended for `127.0.0.1`, not public exposure.
- Paid APIs are not added without explicit approval.

## AI Safety Boundary

The LLM does not send applications and does not rewrite the candidate's experience automatically. It can suggest and score vacancies, but user-facing career actions stay under human control.

This follows the project rule:

```text
LLM proposes -> backend validates -> human approves -> backend executes
```
