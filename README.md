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

## Commands

| Command | What it does |
| --- | --- |
| `/leafwright:pdf` | Render HTML, a file, or a URL to a PDF |
| `/leafwright:invoice` | Create an invoice template with AI |
| `/leafwright:report` | Create a multi-section report template |
| `/leafwright:template` | Create any template from a description |
| `/leafwright:fix` | Diagnose and fix a render from its logs |
| `/leafwright:migrate-puppeteer` | Replace Puppeteer PDF code with Leafwright |
| `/leafwright:add-export` | Add a server-side PDF export to your app |

You can also just ask in plain language — the commands are shortcuts to the
same tools.

## Skill

The plugin also carries a `generating-pdfs` skill that activates on its own
whenever you're working on document or PDF generation — including when you're
looking at Puppeteer or headless-Chrome code. It teaches Claude the HTML rules
that would otherwise cost a failed render, and the log-driven repair loop.

## What it adds

The plugin connects to Leafwright's hosted MCP server — nothing to install, and
Claude Code runs the OAuth flow natively. It exposes tools for rendering
(`leafwright_render_html`, `leafwright_render_url`,
`leafwright_render_template`), template authoring (`leafwright_create_template`,
`leafwright_update_template_draft`), and the debugging loop that makes it useful
to an agent that cannot see its own output:

1. The agent renders a template — it fails or looks wrong.
2. It calls `leafwright_get_job_logs` and reads the real cause: a font that
   404'd, an asset that timed out, a rule that overflowed the page.
3. It calls `leafwright_ai_fix_from_logs`, gets a corrected template,
   re-renders, and confirms it's clean.

Full tool reference: [leafwright.co/docs/mcp](https://leafwright.co/docs/mcp).

## Using something other than Claude Code?

The same server is available everywhere:

```sh
# Cursor / any client with a remote MCP config
https://leafwright.co/mcp

# Codex (TOML, in ~/.codex/config.toml)
codex mcp add leafwright --url https://leafwright.co/mcp

# Local stdio, any client
npx -y @leafwright/mcp
```

It is also listed in the official MCP registry as `co.leafwright/leafwright`.

## Safety

The server only calls the public Leafwright API. It runs no shell commands from
agent input and supports a read-only mode via `LEAFWRIGHT_MCP_READ_ONLY`. AI
**drafts** templates; publishing a template version always requires a human in
the dashboard.

## Layout

```
.claude-plugin/marketplace.json        the marketplace manifest
plugins/leafwright/
  .claude-plugin/plugin.json           the plugin manifest
  .mcp.json                            points at https://leafwright.co/mcp
  commands/*.md                        the /leafwright:* slash commands
  skills/generating-pdfs/SKILL.md      auto-activating usage + rules
  README.md                            what users see
```

MIT · [leafwright.co](https://leafwright.co)
