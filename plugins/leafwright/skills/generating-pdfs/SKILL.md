---
name: generating-pdfs
description: This skill should be used when the user wants to generate, render, or export a PDF or document — including invoices, receipts, reports, certificates, and statements — or when they are working with Puppeteer, Playwright, headless Chrome, wkhtmltopdf, or a client-side PDF library and want a server-side alternative. Also use it when a Leafwright render fails or a PDF layout looks wrong.
version: 0.3.0
---

# Generating PDFs with Leafwright

Leafwright renders HTML, public URLs, and Handlebars templates into PDFs through
a hosted API, so nothing needs headless Chrome locally or in the deploy bundle.

## First: is the connection live?

The tools are named `leafwright_*`. If none are available, the MCP server is not
authenticated yet — tell the user to run `/mcp`, select
`plugin:leafwright:leafwright`, and complete the browser sign-in. Do not try to
work around a missing connection by generating PDFs some other way; say what is
needed and stop.

Once connected, `leafwright_auth_status` confirms the active organization and
default project.

## Choosing the right tool

| Situation | Tool |
| --- | --- |
| Raw or generated HTML | `leafwright_render_html` |
| A public web page, with its own assets | `leafwright_render_url` |
| A published template plus JSON data | `leafwright_render_template` |
| A document that will be produced repeatedly | Create a template first |

Prefer a **template** over inline HTML for anything recurring — invoices,
receipts, statements. Templates can be edited without a redeploy, and they carry
sample data so layout problems surface before production does.

## HTML rules — check these before rendering

Leafwright validates HTML up front. Catching these while writing the markup is
much cheaper than a failed render:

- No `<script>`, `<iframe>`, `<object>`, `<embed>`, `<form>`, `<link>`, or
  inline event handlers.
- **No inline `<svg>`.** Encode SVG as a base64 data URI inside an `<img>`.
  URL-encoded SVG data URIs are rejected too — it must be base64.
- **No external `http(s)` URLs** in `src` or CSS `url()`. The renderer never
  fetches remote assets for HTML sources, so images and fonts must be embedded
  as data URIs. If the document genuinely needs live remote assets, that is what
  `leafwright_render_url` is for.
- No CSS `@import`.
- JavaScript does not execute during render. Produce final markup.
- Limits: HTML 250 KB, CSS 120 KB, 200 pages.

Full documents are fine — doctype, `<html>`, `<head>`, `<style>` blocks all
work. `leafwright_validate_template` checks markup without spending a render.

## When a render fails or looks wrong

This is the loop worth knowing, because a coding agent cannot see the PDF it
produced:

1. `leafwright_get_job_logs` with the job ID — the logs name the real cause: a
   font that 404'd, an asset that timed out, content overflowing the page.
2. Explain the cause before changing anything.
3. `leafwright_ai_fix_from_logs` for a template, or apply the HTML/CSS fix
   directly for raw HTML.
4. Re-render and confirm. Never call it fixed based on the diff alone.

## Working with templates

`leafwright_ai_generate_template` drafts from a description;
`leafwright_ai_revise_template` edits from an instruction. Parameterise variable
content with Handlebars placeholders (`{{customer.name}}`) rather than baking in
sample values.

**AI only ever drafts.** Publishing a template version requires a human in the
Leafwright dashboard — never tell the user a template is live.

## Cost awareness

Renders consume credits. `leafwright_get_usage` shows what is left, and
`leafwright_estimate_usage_cost` estimates a planned batch before running it.
For a bulk job, estimate first and confirm with the user.
