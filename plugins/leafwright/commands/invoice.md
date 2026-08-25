---
description: Create an invoice template with Leafwright AI
argument-hint: [line items, fields, branding notes]
---

# Create an invoice template

Build a professional invoice template in Leafwright.

Specifics from the user: $ARGUMENTS

## Steps

1. If the user gave no specifics, ask briefly what should appear on it —
   business name, line-item columns, tax/discount handling, payment terms. Do
   not invent a company identity for them.
2. Call `leafwright_ai_generate_template` with `document_type` set to
   `"invoice"`, passing the requirements as the prompt.
3. Render a preview with `leafwright_render_template` using the sample data so
   the layout is actually exercised.
4. If it fails or looks wrong, call `leafwright_get_job_logs`, then
   `leafwright_ai_fix_from_logs`, and re-render.
5. Report the template ID and the preview URL.

## Important

Leave it as a **draft**. Publishing a template version requires a human in the
Leafwright dashboard — never claim it is live.

Handlebars placeholders like `{{customer.name}}` are how data gets in, so make
sure the fields the user named are actually parameterised rather than
hard-coded into the markup.
