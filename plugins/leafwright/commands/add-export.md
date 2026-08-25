---
description: Add a server-side PDF export to this app
argument-hint: [what should be exportable]
---

# Add PDF export to this app

What should be exportable: $ARGUMENTS

## Steps

1. Identify the framework and where server-side code belongs — a Next.js route
   handler or server action, an Express route, a serverless function.
2. If the user did not say what to export, ask which page or record. Do not
   guess.
3. Add a server-side endpoint that builds the HTML and calls Leafwright.
   **Keep it server-side** — never expose the API key to the browser.
4. Wire up a download in the UI that hits the endpoint and returns the signed
   URL or streams the bytes.
5. Verify with `leafwright_render_html` before wiring the UI, so a layout
   problem does not get mistaken for a plumbing problem.

## Rules that matter here

- The renderer does not fetch remote assets for HTML sources, and does not run
  JavaScript. Inline the CSS and embed images as base64 data URIs. If the page
  genuinely needs its live assets, use `leafwright_render_url` against a
  publicly reachable URL instead.
- `window.print()` and client-side PDF libraries are what this replaces — do
  not fall back to them.
- Add `LEAFWRIGHT_API_KEY` to the env config and mention it in the summary.

If the export is a document users will get repeatedly (invoice, receipt,
report), suggest making it a Leafwright **template** instead of inline HTML, so
it can be edited without a deploy.
