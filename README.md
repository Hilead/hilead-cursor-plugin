# Hilead plugin

Connect your assistant to your [Hilead](https://hilead.co) outreach workspace. Hilead runs multichannel outreach (email + LinkedIn); this plugin gives the assistant a structured, read-mostly view of it through Hilead's hosted MCP server.

## What the assistant can do

- **Analyze**: campaigns, funnel and deliverability reports, recent activity, prospect progress, inbox conversations, sender health, the analytics overview.
- **Find people**: search the leads already in your workspace, browse audiences and their health diagnosis, discover fresh public profiles within your discovery quota.
- **Prepare**: draft campaigns and audiences, edit draft sequences, import leads into a draft, pause a live campaign. Every draft waits for your review in the Hilead app.

## What it never does

- Launch or resume a campaign.
- Send an email or a LinkedIn message.
- Reveal email addresses or phone numbers, or spend enrichment credits.

Write actions are only available if you grant the `read_write` scope on the consent screen. Read-only connections do not even see the write tools.

## Install

1. Add the plugin from the marketplace (Cursor Marketplace, or **Connect Apps** in Grok Bot).
2. The first time the assistant uses it, a browser tab opens Hilead's consent screen. Sign in with your Hilead account, pick the workspaces to share and allow.
3. Ask away: "Which of my campaigns got the most replies this month?", "Find founders of B2B SaaS companies in California that are hiring", "Draft a 3-step sequence for the people in my Q4 audience".

Requirements: a Hilead account. The Free plan works too, within its limits. The connection can be revoked at any time from **Settings → Integrations** in the Hilead app.

## Workspaces

A connection is scoped to the workspaces you approved on the consent screen. Every tool accepts a `workspace_id` to target one of them; `list_workspaces` enumerates the grant.

## Privacy

The plugin talks only to `https://api.app.hilead.co/mcp` with an OAuth 2.1 token (PKCE, rotating refresh tokens). Nothing is stored by the plugin itself. Tool calls are audited on Hilead's side without copying arguments or message content.

## Develop and test locally

Copy this folder to `~/.cursor/plugins/local/hilead` and restart Cursor; the plugin appears as a local plugin. Point `mcpServers.hilead.url` at a staging server if you have one.

## Support

Questions and issues: [support.hilead.co](https://support.hilead.co).

## License

MIT, see [LICENSE](LICENSE). The Hilead service itself is governed by Hilead's terms.
