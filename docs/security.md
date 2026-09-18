# Security and Privacy

This public repository is intentionally sanitized. It is meant to show the architecture and product thinking without exposing private data or operational secrets.

## Excluded From Public Materials

- `.env` files and any real API keys or Telegram bot tokens;
- database connection strings (DSNs) and other runtime configuration;
- the local/production database and its contents;
- private resume files and personal contact data;
- Telegram chat IDs and private chats;
- raw LLM prompts, responses and reasoning with personal context;
- full vacancy text in logs, metrics or examples;
- internal task history, review rounds, handoffs and long development notes;
- virtual environments, caches and generated local artifacts.

## Runtime Boundaries

- The working project uses local configuration for secrets.
- Telegram delivery is limited to allowed chats in the private project.
- Vacancy text from external sources is treated as untrusted data.
- The API defaults to `127.0.0.1`, not public exposure.
- Structured JSON logs and the metrics endpoint carry only safe fields (timestamps, correlation IDs, status codes, error categories) — never vacancy text, prompts, LLM reasoning or secrets, even when something fails.
- The MCP tool runs only over a local stdio transport — no remote MCP, no public endpoint, no background execution.
- Paid APIs are not added without explicit approval.

## AI Safety Boundary

The LLM does not send applications and does not rewrite the candidate's experience automatically. It can suggest and score vacancies, and prepare a local draft, but user-facing career actions stay under human control.

This follows the project rule:

```text
LLM proposes -> backend validates -> human approves -> backend executes
```

In the MCP tool this is a concrete protocol-level mechanism, not just a guideline: the tool call is schema-validated, the backend re-checks the vacancy before asking anything, and approval comes through the MCP protocol's own human-in-the-loop mechanism — a separate request the client answers on the user's behalf, not a boolean value inside the tool's arguments that a model could set itself. If the connected client doesn't support that mechanism, or the user declines or cancels, the tool fails closed: nothing is generated, nothing is sent.
