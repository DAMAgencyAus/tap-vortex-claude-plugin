# Tap Vortex connector: specification (not built yet)

Status: planning document. Nothing in this repository runs or connects to this server yet, and the plugin has no `.mcp.json`. When the connector is live, add `.mcp.json` pointing at the same URL that is submitted as the connector, so people who have both see one set of tools.

## What it is

A remote MCP server that brings a store's Tap Vortex data into Claude, so a store owner can ask about their vortexes, tests and shoppers in plain English. It reads the store's own Tap Vortex data (from the Tap Vortex Shopify app and done-for-you builds). Read-only in version 1.

- Proposed URL: `https://mcp.tapvortex.com.au/mcp` (domain matches the service, as review requires)
- Transport: Streamable HTTP over HTTPS
- Auth: OAuth 2.0, each store owner signs in with their Tap Vortex account and picks the store
- Data: only the signed-in owner's stores. No shopper names, emails, phone numbers or addresses returned; shoppers are referred to by anonymous IDs.

## Tools (version 1, all read-only)

Every tool has a `title`, `readOnlyHint: true`, a narrow description of what it does (no instructions to Claude about how to behave), input validation with clear error messages, and paginated, summary-sized responses. Names are well under 64 characters.

| Tool | Title | What it returns | Key inputs |
|---|---|---|---|
| `list_vortexes` | List vortexes | The store's vortexes: name, scope (one page / homepage / site-wide), status (draft, live, testing), pages in the journey, last edited | `store_id`, `status?` |
| `get_reports` | Get vortex reports | For one vortex and date range: visitors, revenue per visitor, conversion, order value, step-by-step drop-off, A/B test status and chance to win against the current page | `vortex_id`, `from`, `to` (dates inclusive of today) |
| `shopper_stories` | Get shopper stories | Journeys of individual shoppers, anonymous ones included: entry ad, steps, what they told, what they did, match, bag, order or where they stopped | `vortex_id?`, `from`, `to`, `outcome?` (ordered / almost / left), `limit`, `cursor` |
| `demand_map` | Get demand map | What shoppers are telling the store: answers and requests grouped by theme, counts, and which themes buy, including needs the range doesn't cover | `vortex_id?`, `from`, `to` |
| `almost_bought` | List almost-bought shoppers | Shoppers who were matched or had a bag but didn't order: last step, bag value, match, entry angle, whether they're in Klaviyo (consented) | `vortex_id?`, `from`, `to`, `min_bag_value?`, `limit`, `cursor` |
| `ask_memory` | Ask store memory | Answers a plain-language question over the store's vortex memory (for example "which ad brings the biggest orders?") by running fixed, safe queries over the store's own data and returning the figures plus the query used | `store_id`, `question` |

Possible later tools (write tools need `destructiveHint` where they change data, and must be separate from read tools): `approve_match_suggestion`, `create_audience_from_answers`, `get_monday_briefing`.

## What the docs require for directory submission

From Anthropic's connector docs (claude.com/docs/connectors/building/submission, review-criteria and authentication):

**Server and tools**
- Remote server reachable over `https://`.
- Every tool has a `title` and `readOnlyHint` or `destructiveHint`. Read and write operations in separate tools; no catch-all request tool.
- Tool names 64 characters or fewer; descriptions narrow and accurate; no prompt-injection patterns (no instructions to call other tools, no hidden or encoded text, no promotion in descriptions).
- Every tool returns a successful response with valid inputs; useful error messages, not bare "Bad Request".
- Responses sized to the task; don't collect conversation data beyond what the tool needs; never query Claude's memory, chat history or user files.
- Calls Tap Vortex's own first-party API; server domain matches the service.

**Authentication (OAuth 2.0)**
- Answer unauthenticated requests with `401` and `WWW-Authenticate: Bearer resource_metadata="..."` pointing at protected resource metadata (RFC 9728). `resource` must exactly match the server URL; primary issuer first in `authorization_servers`.
- Authorisation server metadata (RFC 8414 or OIDC Discovery) at `/.well-known/`, reachable from Anthropic's egress range (`160.79.104.0/21`).
- Client registration: Dynamic Client Registration (`registration_endpoint`) or Client ID Metadata Document (advertise `client_id_metadata_document_supported: true` and `none` in `token_endpoint_auth_methods_supported`). CIMD preferred for high traffic.
- PKCE S256 supported and advertised (`code_challenge_methods_supported: ["S256"]`).
- Redirect URIs: `https://claude.ai/api/mcp/auth_callback`, plus loopback `http://localhost/callback` and `http://127.0.0.1/callback` matched on any port (Claude Code).
- Token endpoint accepts `application/x-www-form-urlencoded`; returns `invalid_grant` for dead refresh tokens; rotates refresh tokens for public clients; `offline_access` in `scopes_supported` for refresh tokens.
- Discovery, registration and token endpoints respond within 10 seconds; refresh within 30 seconds.
- Consent screen shows the redirect URI's hostname clearly.

**Listing materials (developer portal, claude.ai/directory/manage, "MCP connector")**
- Server name (up to 100 characters), one-liner (up to 200), description (up to 2,000), one to five categories, URL slug (permanent).
- Documentation URL (public by the publish date), privacy policy URL, support contact, icon.
- Use cases, prerequisites (Tap Vortex account, a store with the app or a done-for-you build), reads/writes.
- Company name, website, primary contact.
- Data handling answers (own API; no health data intended; no sponsored content).
- Test account credentials for a fully populated store, for reviewers only.
- Confirm every tool was run through MCP Inspector or as a custom connector in Claude.
- Seven compliance acknowledgements.
- MCP Apps only: 3 to 5 PNG carousel screenshots at least 1000px wide (not needed for a tools-only server).
- Optional: allowed link URIs (`https://www.tapvortex.com.au`, `https://app.tapvortex.com.au`) if the server opens links.

**After submitting**
- Submit the server as an MCP connector, then pair it with this plugin bundle (same organisation).
- Escalations: `mcp-review@anthropic.com`.

## Privacy

The connector returns only data the store already owns in Tap Vortex. Shopper-level data is anonymous by ID. The privacy policy must cover collection, use and storage, third-party sharing, retention and contact details, and is linked in the listing. Tap Vortex does not use store or shopper data to train AI models.
