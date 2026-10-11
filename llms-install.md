# Installing AgentPixel (for Cline and other agents)

AgentPixel is a hosted remote MCP server. There is nothing to clone, build or run.

1. Add a remote server named `agentpixel`:
   - URL: `https://agentpixel.io/mcp`
   - Transport: Streamable HTTP (`"type": "streamableHttp"` in Cline)
   - Header: `X-AgentPixel-Source: cline`
   - Leave `autoApprove` empty.

```json
{
  "mcpServers": {
    "agentpixel": {
      "type": "streamableHttp",
      "url": "https://agentpixel.io/mcp",
      "headers": {"X-AgentPixel-Source": "cline"},
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

2. Confirm 14 tools are listed. To see what the output looks like before any signup, call `try_sample` (no account, no key; labeled sample data).
3. If the user has no account yet: ask the user for their email and name (never invent them), call `signup_free(email, display_name)` to start the 30-day trial (no card), and keep the returned `account_id` and `signup_token` private. Ask the user for the 6-digit code from the AgentPixel email, then call `confirm_signup(account_id, code, signup_token)`. The first confirmation returns the API key once. If the user clicked the email link instead, call `confirm_signup(account_id, signup_token)` until `email_verified` is true, then call `create_api_key(account_id, signup_token)` once. For a new code, call `resend_code(email)`.
4. Store the key only in the user's private settings: add `"Authorization": "Bearer <key>"` to this server's headers. Never write it to a repository, chat log or URL.
5. Call `whoami` to confirm the account, then `setup_status`.

Consent comes first: run `configure_consent` with `resolution` among the purposes and install the snippet it returns. That snippet shows a Cookie Permissions bar. Visitors click Accept to be identified. Do not install a snippet before consent is configured.
