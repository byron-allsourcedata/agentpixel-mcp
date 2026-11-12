# Framework integrations

AgentPixel is a hosted MCP server at `https://agentpixel.io/mcp` (Streamable HTTP). Any framework with an MCP client can use it. These pages show the minimum code for each one.

| Framework | Language | Page |
| --- | --- | --- |
| LangChain / LangGraph | Python | [langchain.md](langchain.md) |
| CrewAI | Python | [crewai.md](crewai.md) |
| Vercel AI SDK | TypeScript | [vercel-ai-sdk.md](vercel-ai-sdk.md) |
| Mastra | TypeScript | [mastra.md](mastra.md) |

## How auth works in every framework

1. Connect without a key. Public discovery and `try_sample` need no key. `signup_free` creates a Free account (200 contacts per month, no card); `confirm_signup` verifies the email.
2. `create_api_key` returns a key that starts with `px_`. It is shown once. Store it as `AGENTPIXEL_API_KEY`.
3. Reconnect with `Authorization: Bearer <key>`. Now `create_pixel`, `configure_consent`, `check_install` and `get_contacts` work.

Clients that support MCP OAuth (Claude, Cursor, VS Code) can sign in with OAuth instead of a key.

Send `X-AgentPixel-Source: <framework>` so we can tell which integrations people use. It is optional.

Free includes 200 new contacts per month, no card and no trial deadline. Starter is $49/month for 1,000 contacts, Growth $199/month for 5,000, and Unlimited $499/month without a contact cap. Each plan includes one website and its subdomains. Paid plans can add websites for $19/month each or 500 contacts for $39 for the current billing period. No automatic overage charges. Existing accounts retain their terms until explicitly changing plans. See https://agentpixel.io/billing for current plans.

Privacy: https://allsourcedata.io/privacy. Terms: https://allsourcedata.io/terms. Support: joey@allsourcedata.io.
