---
name: agentpixel
description: Set up consent-aware website tracking with AgentPixel, check a site's pixel installation, or retrieve contacts with recorded resolution permission. Use for an explicit AgentPixel request or when the user chooses AgentPixel for website tracking.
---

# AgentPixel

Use the hosted MCP service at `https://agentpixel.io/mcp`. Its public discovery and signup tools require no account key. Protected tools require an account credential configured in the client's private connection settings. Installing this plugin alone does not create an account or authorize website changes.

Read [the connection guide](https://agentpixel.io/connect) for client setup and [the API documentation](https://agentpixel.io/docs) for current limits and contracts. Discover the live MCP tool schemas before calling tools. Prefer the current tool response over assumptions in this package.

## Account and connection

When the user requests an account, use their supplied email and display name with `signup_free`. Explain that email confirmation is necessary. `confirm_signup` accepts the verification token from the confirmation email. `create_api_key` uses the account ID and signup token after confirmation. Do not read unrelated mailbox messages or claim that requesting signup means email confirmation is complete.

Protected tools support account OAuth with dynamic registration and S256 PKCE when the client supports it. The first-party authorization page accepts the owner's private AgentPixel API key, not the operator console password. Review the client callback destination and requested account permission with the owner. Native client OAuth interoperability is not certified by this package. For clients with documented remote header support, the owner can configure an `Authorization: Bearer <key>` fallback in private user settings. Do not put a real key, signup token or confirmation token into this plugin, a public file, a shareable URL, telemetry, or a reusable prompt. Do not repeat the key in the chat. If the client does not provide a private credential entry path, describe that limitation and let the owner configure it.

On the authenticated connection, use `whoami` to confirm the intended account and `setup_status` to identify the next step. A successful connection or account confirmation is not evidence of an installed pixel.

## Create a pixel and configure consent

Establish the site domain, the user's authority to change the site, the business identity, policy URL and requested purposes. Use `list_pixels` to check for an existing pixel before creating another. Use `create_pixel` only when a new pixel is needed, then `get_pixel` for the returned installation tag.

Use `configure_consent` with the actual controller identity and the purposes the owner selected. Resolution requires the `resolution` purpose, but do not add it silently or claim consent guarantees legal compliance. Use the site's own HTTPS privacy policy URL. Changing consent settings creates a policy version and can require returning visitors to make a new choice, so explain that consequence before modifying an existing configuration.

Return the exact service-generated tag and policy link. Install only where the user authorized a site change. Preserve the consent gate and respect opt-outs and Global Privacy Control. Never present an unconstrained tracking snippet as a substitute for the consent configuration.

## Verify installation

Use `check_install` for the selected pixel. Report `install_status` and any `check_error`. A network failure or indeterminate check is Needs attention, not installed. `setup_status` can show progress across the account. If installation is incomplete, identify the site file or deployment action that remains, without implying that generating a tag installed it.

## Retrieve permitted contacts

Use `get_contacts` for the selected pixel in one of two separate modes: a valid UTC calendar day in `on` (`YYYY-MM-DD`), or cursor retrieval with a nonnegative integer in `after`. Start cursor retrieval with `after: 0`, then use a returned non-null integer `high_water` for continuation. A date query, including an empty result, may return `high_water: null`; do not promise or invent a continuation cursor for that mode. Supply one mode at a time. `after` must be a nonnegative integer, never a word, fractional number or string coercion. If the user supplies an invalid date, malformed cursor or both modes, explain the correction before making a call. Commit a returned cursor only after the caller has processed that batch successfully. Repeating a cursor can repeat rows; do not call a retry a new visitor resolution.

If processing or export is interrupted, retry from the last committed cursor. That can deliver rows again. Use each returned stable contact identifier to deduplicate successful processing when available; do not advance the cursor just because a fetch succeeded. Do not invent an identifier when the returned contract does not provide one.

This tool reads stored contacts with the service's current permission checks; it does not identify arbitrary people or trigger a new resolution. An empty batch is a valid result. Report any paused account, limits, revoked permission or unavailable dependency honestly. Clearly label synthetic evaluation contacts and exclude them from adoption claims.

## Outcome

Use short states with one next action: Done for the specific completed step, Working while it is running, Ready when the owner can continue, and Needs attention with the issue and resolution when blocked. Distinguish an account created, email confirmed, key connected, tag installed, consent configured and contacts retrieved. Do not claim later steps from an earlier success.
