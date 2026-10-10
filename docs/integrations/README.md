# Framework integrations

AgentPixel is a hosted MCP server at `https://agentpixel.io/mcp` (Streamable HTTP). Any framework with an MCP client can use it. These pages show the minimum code for each one.

| Framework | Language | Page |
| --- | --- | --- |
| LangChain / LangGraph | Python | [langchain.md](langchain.md) |
| CrewAI | Python | [crewai.md](crewai.md) |
| Vercel AI SDK | TypeScript | [vercel-ai-sdk.md](vercel-ai-sdk.md) |
| Mastra | TypeScript | [mastra.md](mastra.md) |

## How auth works in every framework

1. Connect without a key. Only the signup tools are useful at this point: `signup_free` starts the 30-day trial (no card) and `confirm_signup` finishes it after the person confirms the email.
2. `create_api_key` returns a key that starts with `px_`. It is shown once. Store it as `AGENTPIXEL_API_KEY`.
3. Reconnect with `Authorization: Bearer <key>`. Now `create_pixel`, `configure_consent`, `check_install` and `get_contacts` work.

Clients that support MCP OAuth (Claude, Cursor, VS Code) can sign in with OAuth instead of a key.

Send `X-AgentPixel-Source: <framework>` so we can tell which integrations people use. It is optional.

After the trial: $1 per account per month for 3 pixels and 500 new contacts per month. No usage charges.

Privacy: https://allsourcedata.io/privacy. Terms: https://allsourcedata.io/terms. Support: joey@allsourcedata.io.
