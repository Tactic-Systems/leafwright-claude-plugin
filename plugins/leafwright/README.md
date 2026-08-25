# Leafwright for Claude Code

Give Claude Code the ability to **generate PDFs, build templates, and debug
failed renders from their logs** — no headless Chrome, no Puppeteer in your
bundle.

## Install

```
/plugin marketplace add Tactic-Systems/leafwright-claude-plugin
/plugin install leafwright@leafwright
```

On first use Claude Code opens a browser window to sign in to Leafwright and
pick an organization and default project. Get a free account (100 PDFs/month,
no card) at [leafwright.co](https://leafwright.co).

## What it adds

The plugin connects to Leafwright's hosted MCP server, which exposes tools for
rendering (`leafwright_render_html`, `leafwright_render_url`,
`leafwright_render_template`), template authoring
(`leafwright_create_template`, `leafwright_update_template_draft`), and the
debugging loop that makes it useful to an agent that cannot see its own output:

1. The agent renders a template — it fails or looks wrong.
2. It calls `leafwright_get_job_logs` and reads the real cause: a font that
   404'd, an asset that timed out, a rule that overflowed the page.
3. It calls `leafwright_ai_fix_from_logs`, gets a corrected template,
   re-renders, and confirms it's clean.

Full tool reference: [leafwright.co/docs/mcp](https://leafwright.co/docs/mcp).

## Safety

The server only calls the public Leafwright API. It runs no shell commands from
agent input and supports a read-only mode. AI **drafts** templates; publishing a
template version always requires a human in the dashboard.

MIT · [leafwright.co](https://leafwright.co)
