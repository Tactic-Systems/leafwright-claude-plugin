---
description: Diagnose and fix a PDF that failed to render or looks wrong
argument-hint: <job id> [template id]
---

# Fix a broken render

Job (and optionally template) to investigate: $ARGUMENTS

This is the loop a code-only agent cannot otherwise close — you cannot see the
PDF, but the render logs tell you exactly what went wrong.

## Steps

1. If no job ID was given, call `leafwright_get_job` on the most recent job, or
   ask which render they mean.
2. Call `leafwright_get_job` and `leafwright_get_job_logs`.
3. **Explain the root cause in plain language before changing anything.** The
   logs name real causes: a font that 404'd, an asset that timed out, content
   that overflowed the page, a validation rejection.
4. Then fix it:
   - Came from a template → `leafwright_ai_fix_from_logs` with the template ID
     and job ID.
   - Came from raw HTML → propose the specific HTML/CSS change.
5. Re-render and confirm it is actually clean. Do not declare it fixed on the
   strength of the diff alone.

## Common causes worth checking first

- An external `http(s)` asset in `src` or CSS `url()` — never fetched for HTML
  sources. Embed as a data URI.
- Inline `<svg>` or a URL-encoded SVG data URI — both rejected. Use base64 in
  an `<img>`.
- A `@import` in the CSS.
- Content wider or taller than the page box, which shows up as overflow rather
  than an error.
