---
description: Replace Puppeteer PDF generation with Leafwright API calls
---

# Migrate from Puppeteer to Leafwright

Replace headless-Chrome PDF generation in this codebase with Leafwright.

## Steps

1. Find the PDF generation code. Search for `puppeteer`, `puppeteer-core`,
   `@sparticuz/chromium`, `playwright`, `chromium`, `.pdf(`, and `launch(`.
2. Show the user what you found and what it produces before changing anything.
3. Replace each call site with a Leafwright API call — `/api/v1/pdfs` server
   side, authenticated with `LEAFWRIGHT_API_KEY`. Keep the surrounding function
   signature so callers do not change.
4. Verify with `leafwright_render_html` that the same markup renders correctly
   **before** deleting the old code path.
5. Only then remove the Puppeteer dependency and its bundling workarounds
   (custom Chromium layers, `serverExternalPackages` entries, inflated Lambda
   memory or timeouts).

## Watch for

- HTML that relied on remote assets or web fonts: Leafwright does not fetch
  remote assets for HTML sources. Embed them as data URIs, or switch that call
  to `leafwright_render_url`.
- Puppeteer-specific options (`printBackground`, custom `margin`, header and
  footer templates) — map them to the Leafwright equivalents rather than
  silently dropping them.
- Anything relying on JavaScript executing during render: it will not run.
  Render the final markup instead.

Report what changed and what the user still needs to configure — usually just
`LEAFWRIGHT_API_KEY`.
