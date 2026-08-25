---
description: Render HTML, a file, or a URL to a PDF with Leafwright
argument-hint: [file path, URL, or a description of what to render]
---

# Render a PDF

Render what the user asked for into a PDF using Leafwright, and give them back
the signed download URL.

Target: $ARGUMENTS

## How to decide which tool to use

- A **public URL** → `leafwright_render_url`. This fetches the page with its
  remote assets behind SSRF protection.
- A **local file** (`.html`, or a component/page the user points at) → read it,
  then `leafwright_render_html`. If it is a framework component rather than
  plain HTML, render the markup it produces, not the source.
- A **published template plus data** → `leafwright_render_template`.
- **Nothing specified** → ask what they want rendered before calling anything.

## HTML rules that will otherwise bite you

Leafwright validates HTML before rendering. Fix these *before* calling the tool
rather than round-tripping through a failed render:

- No `<script>`, `<iframe>`, `<object>`, `<embed>`, `<form>`, `<link>`, or
  inline event handlers.
- No inline `<svg>`. Encode SVG as a **base64** data URI in an `<img>`;
  URL-encoded SVG data URIs are rejected.
- No external `http(s)` URLs in `src` or CSS `url()` — the renderer never
  fetches remote assets for HTML sources. Embed images as data URIs. (If they
  need remote assets, that is what `leafwright_render_url` is for.)
- No CSS `@import`.
- Limits: HTML 250 KB, CSS 120 KB, 200 pages.

## If the render fails or looks wrong

Do not guess. Call `leafwright_get_job_logs` with the job ID, read the actual
cause, fix it, and re-render. Report what was wrong in one line.

Finish by giving the user the download URL.
