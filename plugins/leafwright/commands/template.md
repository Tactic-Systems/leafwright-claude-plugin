---
description: Create any document template from a description
argument-hint: <what the document should contain and look like>
---

# Create a template from a description

Description: $ARGUMENTS

## Steps

1. If no description was given, ask for one. If the document maps to a known
   type — quote, certificate, packing slip, contract, statement — pass that as
   `document_type` so the model starts from the right structure.
2. Call `leafwright_ai_generate_template`.
3. Render a preview with `leafwright_render_template` and confirm it looks
   right.
4. If it fails, `leafwright_get_job_logs` → `leafwright_ai_fix_from_logs` →
   re-render.
5. Report the template ID and preview URL.

Parameterise the variable parts with Handlebars placeholders
(`{{customer.name}}`) rather than hard-coding sample values.

Leave it as a draft — publishing requires a human in the dashboard.
