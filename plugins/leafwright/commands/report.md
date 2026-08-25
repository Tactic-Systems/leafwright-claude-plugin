---
description: Create a multi-section report template with Leafwright AI
argument-hint: [sections, charts, branding notes]
---

# Create a report template

Build a multi-page report template in Leafwright.

Specifics from the user: $ARGUMENTS

## Steps

1. If the user gave no specifics, ask what sections it needs and whether there
   is a cover page, table of contents, or repeating header/footer.
2. Call `leafwright_ai_generate_template` with `document_type` set to
   `"report"`.
3. Render a preview with `leafwright_render_template` using sample data —
   multi-page layouts are where page-break and overflow bugs surface, so a
   preview matters more here than for single-page documents.
4. If it fails or looks wrong, use `leafwright_get_job_logs` then
   `leafwright_ai_fix_from_logs`, and re-render.
5. Report the template ID and the preview URL.

## Notes

Charts must be embedded as base64 data URIs in `<img>` — inline `<svg>` is
rejected, and the renderer will not fetch a remote chart image.

Leave it as a draft; publishing requires a human in the dashboard.
