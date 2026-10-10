---
name: hilead
description: Work with a Hilead outreach workspace through the Hilead MCP tools. Use when the user mentions Hilead, their outreach or prospecting campaigns, leads, audiences, prospects, replies, or email and LinkedIn sequences.
---

# Hilead

Hilead runs multichannel outreach (email + LinkedIn). The `hilead` MCP server exposes the workspace to you. You analyze and prepare; the human launches from the app.

## Boundaries

- You never launch or resume a campaign, never send a message, never reveal email or phone, never spend enrichment credits. These are not exposed; do not promise them.
- Write tools (`create_campaign`, `update_campaign_sequence`, `import_leads_to_campaign`, `pause_campaign`, `create_audience`, `update_audience`, `discover_leads`, …) exist only when the user granted `read_write`. If a write tool is missing, say the connection is read-only and point to Settings → Integrations in Hilead.
- Create an audience only when the user explicitly asks for one. A plain "find me leads" request uses `discover_leads`, which never creates an audience.

## Picking the right tool

- **Questions about results**: `list_campaigns`, `get_campaign_report`, `get_campaign_activity`, `list_campaign_prospects`, `get_analytics_overview`, `list_conversations` / `get_conversation`.
- **People already in the workspace**: `search_leads` (filters, `cities`, `regions`, `country_codes`) and `list_audience_leads`. The pool only contains people already harvested; when the request is not covered, say so and offer discovery instead of filtering by hand.
- **Fresh people**: `discover_leads` (25 profiles by default, 100 max per call), then `get_discovery_job` until it finishes; read `stopReason` and `industryFilter` to explain what happened. Prefer LinkedIn taxonomy labels in `company_industries` (`Software Development`, not `B2B SaaS`): free text only matches headlines.
- **Audiences**: `list_audiences`, `get_audience_health` for dead keywords and uncovered titles.
- **Drafting**: `create_campaign` (draft only), `validate_campaign_draft` before handing over, `update_campaign_sequence` with the `expected_updated_at` you read from `get_campaign` (compare-and-set; refetch on conflict).
- **Several workspaces**: `list_workspaces` shows the grant; pass `workspace_id` explicitly on every call that targets another workspace.

## Presenting people

When you show leads, prospects or contacts and the user did not ask for a format, use a table with, in this order: Name (Founder when the request is about founders), Title, Company, Location, Employees, Open roles (30 days), Signal (type and date), LinkedIn (as a link). Leave a cell empty rather than dropping the column. `openRoles30d` and `companySignal` on each row fill the last columns.

## Sequence contract

Steps have a unique `id`, a `type` (`email`, `li-chat`, …) and a `delay` from the previous step; content sits under the channel key (`email.subject`, `email.body`, `liChat.message`). There is no wait step: waiting is the next step's delay. A/B variants live under `variants` with their own `id`. The server validates the tree on every save.
