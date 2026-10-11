# AgentPixel agent plugin

<!-- mcp-name: io.agentpixel/agentpixel -->

**Most people who visit your website never fill out a form. AgentPixel helps you meet the ones who click Accept.**

## Why use it

You pay to bring people to your site through ads, content and events, and most of them leave without a name. With AgentPixel, visitors click Accept to be identified, and they come back as contacts, so you can:

- **Follow up with prospects who never filled out a form.** Add them to nurture and convert them later.
- **Get more from the ad spend you already have.** Paid clicks from visitors who click Accept become contacts, not just anonymous sessions.
- **Know who your traffic really is.** Job titles and companies of visitors who click Accept show whether your traffic fits your ideal customer profile.

Every result depends on visitors who click Accept, and not every visitor who clicks Accept can be matched to a contact.

**How it works.** Consent-first website pixel: set up the consent bar, install the tag, verify it and read contacts. Your AI agent does the setup. It can show labeled sample output with `try_sample` (no account, no key), start a 30-day trial (no card) with a 6-digit code from your email, install the snippet from `configure_consent` and read contacts by day. Then $1 per account per month.

## Install

[![Install in VS Code](https://img.shields.io/badge/VS_Code-Install_AgentPixel-0098FF)](https://insiders.vscode.dev/redirect/mcp/install?name=AgentPixel&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fagentpixel.io%2Fmcp%22%2C%22headers%22%3A%7B%22X-AgentPixel-Source%22%3A%22vscode%22%7D%7D)
[![Install in Cursor](https://cursor.com/deeplink/mcp-install-dark.svg)](https://agentpixel.io/connect)
[![AllMCPs Verified](https://allmcps.com/api/badge/agentpixel?style=shield)](https://allmcps.com/mcp/agentpixel)

Claude Code: `claude mcp add --transport http agentpixel https://agentpixel.io/mcp --header "X-AgentPixel-Source: claude_code"`

Gemini CLI: `gemini extensions install https://github.com/byron-allsourcedata/agentpixel-mcp`

This package connects compatible clients to the hosted [AgentPixel MCP service](https://agentpixel.io/connect) and supplies one workflow skill. The backend stays private and hosted. No executables, credentials or customer records are bundled.

## Connection and authentication

Endpoint: `https://agentpixel.io/mcp`. Public discovery and signup need no credential. Protected tools support account OAuth with dynamic client registration and S256 PKCE. When a compatible client offers authorization, use the first-party AgentPixel page to connect your own account. Its masked **AgentPixel API key** field expects your private account key, not the operator console password. Review the client callback destination and account permissions before approving.

After signup, confirm the email and obtain your account key using the documented onboarding tools. Never put a real key, signup token or email-confirmation token in this package, a public repository, chat or a URL. Direct `Authorization: Bearer` fallback is available for clients whose documentation supports remote request headers, including Cursor and Cline. Configure it only in private user settings. OAuth protocol and account binding have been tested. Direct public discovery using the client-specific URL configuration below was verified with Cursor CLI 2026.10.01 and returned all 12 tools that existed then (14 today). On 2026-10-10, Cline's MCP client (cline CLI 3.0.70) connected with the `llms-install.md` configuration, listed all 14 tools and ran `try_sample`. Plugin installation, account OAuth in Cursor, the native Cursor GUI, a full Cline agent session and marketplace approval remain untested.

The portable `mcp.json` follows [Agent Plugins 1.0.0](https://agent-plugins.org/). Do not copy its root `mcp.json` directly into native Cursor or Cline settings. The portable `streamable-http` transport spelling is not their direct settings format; use the client-specific snippets below. Copying the portable configuration directly into native Cursor settings failed discovery in the isolated test. The fixed `X-AgentPixel-Source` labels are untrusted reported sources, not proof of identity, installation or independent adoption.

## Cursor

Follow [Cursor's plugin instructions](https://cursor.com/docs/plugins) for a compatible Agent Plugin. This package uses root `plugin.json`, `mcp.json` and `skills/agentpixel/SKILL.md`; it is not a marketplace-approved listing.

For a direct hosted connection, use [Cursor's documented MCP configuration](https://cursor.com/docs/mcp). Merge this entry into your private user `~/.cursor/mcp.json`, preserving existing servers:

```json
{
  "mcpServers": {
    "agentpixel": {
      "url": "https://agentpixel.io/mcp",
      "headers": {"X-AgentPixel-Source": "cursor"}
    }
  }
}
```

Use client authorization when offered. For the documented header fallback, add `"Authorization": "Bearer ${env:AGENTPIXEL_API_KEY}"` to this entry's headers and set your own private environment variable. Confirm the tool list, then use `whoami` to check the intended account. This URL-based example discovered all 12 tools that existed then (14 today) in an isolated Cursor CLI 2026.10.01 test. That result verifies public discovery only, not plugin installation, account authorization, tool outcomes or the native GUI.

## Cline

Follow [Cline's remote-server instructions](https://github.com/cline/cline/blob/main/docs/mcp/mcp-overview.mdx). In the Cline panel, open **MCP Servers**, **Configure**, then **Configure MCP Servers**. Merge this entry into the user's private settings, preserving other entries:

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

Use OAuth authorization when the client offers it. If using Cline's documented header fallback, add `"Authorization": "Bearer REPLACE_WITH_YOUR_PRIVATE_ACCOUNT_KEY"` to this entry's headers in private settings only. Never commit the replacement value. Leave `autoApprove` empty and review tool requests. Check that tools appear, then call `whoami`. Native Cline setup and marketplace acceptance remain untested.

## Gemini CLI

The root `gemini-extension.json` provides the hosted Streamable HTTP connection. Follow [Gemini CLI's extension instructions](https://geminicli.com/docs/extensions/reference/) to install:

```text
gemini extensions install https://github.com/byron-allsourcedata/agentpixel-mcp
```

Review the installation prompt and tool requests. Public discovery requires no key. Private account tools require authorization through the client. Keep credentials in private client settings only, never in the repository or chat. The companion `skills/agentpixel/SKILL.md` describes the website owner and consent workflow. Native Gemini CLI installation, private account authorization and gallery acceptance remain unverified.

## Framework integrations

Use AgentPixel from agent frameworks with any MCP client. Minimal examples, including keyless signup and the `px_` key:

- [LangChain and LangGraph](docs/integrations/langchain.md) (Python)
- [CrewAI](docs/integrations/crewai.md) (Python)
- [Vercel AI SDK](docs/integrations/vercel-ai-sdk.md) (TypeScript)
- [Mastra](docs/integrations/mastra.md) (TypeScript)

See [docs/integrations](docs/integrations/README.md) for how auth works across frameworks.

## Useful tasks

- [Create a consent-aware pixel for a website you control](https://agentpixel.io/guides/consented-visitor-identity). Reuse an existing domain registration when available.
- [Verify tag installation and consent configuration](https://agentpixel.io/guides/consent-pixel-installation). Returning a tag is not proof it was installed.
- [Export permitted contacts with safe cursor handling](https://agentpixel.io/guides/consented-contact-export). Date and cursor retrieval are separate modes. A date query may return `high_water: null`. For cursor retrieval, start with `after: 0`, then commit a returned non-null integer cursor only after processing succeeds. Retry from the last committed cursor and deduplicate returned stable identifiers when available.

See [the public connection guide](https://agentpixel.io/connect) and [API documentation](https://agentpixel.io/docs) for live schemas and troubleshooting. Registry identifier: `io.agentpixel/agentpixel` (`mcpName` in registry package metadata where applicable). This README marker does not prove registry publication or ownership verification; package-type requirements differ.

## Pricing

The 30-day trial requires no card. Afterward, service costs $1 per account per month, covering up to 3 pixels and 500 new contacts per UTC calendar month, with no extra usage charges. After trial expiry, the account pauses until subscribed. New resolutions stop at the monthly limit and resume with the next UTC month's allowance. Subscription does not increase that limit. Consult account status and the current contract for available reads and next actions.

## License, privacy, terms and support

The distributed connection package, workflow text and wordmark are covered by the [MIT License](LICENSE), copyright 2026 AllSource Data Corporation. The license grants no hosted-backend source, service credentials, account access or private AllSource systems. `logo.png` is a 400x400 text wordmark derived from AgentPixel's existing public brand name and colors; it carries no certification mark.

Website owners supply their controller identity, consent purposes and site policy. Privacy policy: [allsourcedata.io/privacy](https://allsourcedata.io/privacy). Terms: [allsourcedata.io/terms](https://allsourcedata.io/terms). Support: [joey@allsourcedata.io](mailto:joey@allsourcedata.io).
