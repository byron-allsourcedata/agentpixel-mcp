# AgentPixel agent plugin

<!-- mcp-name: io.agentpixel/agentpixel -->

This package connects compatible clients to the hosted [AgentPixel MCP service](https://agentpixel.io/connect) and supplies one workflow skill. The backend stays private and hosted. No executables, credentials or customer records are bundled.

## Connection and authentication

Endpoint: `https://agentpixel.io/mcp`. Public discovery and signup need no credential. Protected tools support account OAuth with dynamic client registration and S256 PKCE. When a compatible client offers authorization, use the first-party AgentPixel page to connect your own account. Its masked **AgentPixel API key** field expects your private account key, not the operator console password. Review the client callback destination and account permissions before approving.

After signup, confirm the email and obtain your account key using the documented onboarding tools. Never put a real key, signup token or email-confirmation token in this package, a public repository, chat or a URL. Direct `Authorization: Bearer` fallback is available for clients whose documentation supports remote request headers, including Cursor and Cline. Configure it only in private user settings. OAuth protocol and account binding have been tested; native Cursor/Cline installation, their OAuth interoperability and marketplace approval have not.

The portable `mcp.json` follows [Agent Plugins 1.0.0](https://agent-plugins.org/). Its `streamable-http` transport spelling is not Cline's settings format. The fixed `X-AgentPixel-Source` labels are untrusted reported sources, not proof of identity, installation or independent adoption.

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

Use client authorization when offered. For the documented header fallback, add `"Authorization": "Bearer ${env:AGENTPIXEL_API_KEY}"` to this entry's headers and set your own private environment variable. Confirm the tool list, then use `whoami` to check the intended account. This example has not been tested inside native Cursor.

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

## Useful tasks

- [Create a consent-aware pixel for a website you control](https://agentpixel.io/guides/consented-visitor-identity). Reuse an existing domain registration when available.
- [Verify tag installation and consent configuration](https://agentpixel.io/guides/consent-pixel-installation). Returning a tag is not proof it was installed.
- [Export permitted contacts with safe cursor handling](https://agentpixel.io/guides/consented-contact-export). Date and cursor retrieval are separate modes. A date query may return `high_water: null`. For cursor retrieval, start with `after: 0`, then commit a returned non-null integer cursor only after processing succeeds. Retry from the last committed cursor and deduplicate returned stable identifiers when available.

See [the public connection guide](https://agentpixel.io/connect) and [API documentation](https://agentpixel.io/docs) for live schemas and troubleshooting. Registry identifier: `io.agentpixel/agentpixel` (`mcpName` in registry package metadata where applicable). This README marker does not prove registry publication or ownership verification; package-type requirements differ.

## Pricing

The 30-day trial requires no card. Afterward, service costs $1 per account per month, covering up to 3 pixels and 500 new contacts per UTC calendar month, with no extra usage charges. After trial expiry, the account pauses until subscribed. New resolutions stop at the monthly limit and resume with the next UTC month's allowance. Subscription does not increase that limit. Consult account status and the current contract for available reads and next actions.

## License, privacy and support

The distributed connection package, workflow text and wordmark are covered by the [MIT License](LICENSE), copyright 2026 AllSource Data Corporation. The license grants no hosted-backend source, service credentials, account access or private AllSource systems. `logo.png` is a 400x400 text wordmark derived from AgentPixel's existing public brand name and colors; it carries no certification mark.

Website owners supply their controller identity, consent purposes and site policy. Service privacy: [AllSource privacy policy](https://allsourcedata.io/privacy.html). Support: [byron@allsourcedata.io](mailto:byron@allsourcedata.io).
