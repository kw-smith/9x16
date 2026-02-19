---
description: Create a PAK (Packaged Asset Kit) — an interactive 9x16 media package with narration and video export
---

# Create a PAK

The user wants to create a PAK. A PAK is NOT a PowerPoint — it's an interactive, narrated, mobile-first media package in 9:16 portrait format.

## Steps

1. **Understand the request.** What's the topic? Who's the audience? What tone?
2. **Check catalogs first.** Call `list_catalogs` to see if a pre-built structure fits (quarterly-report, product-launch, company-pitch, case-study, sales-proposal, project-update, training-module, event-recap, brand-overview, data-dashboard). If one matches:
   - Call `get_catalog` with the catalog_id
   - Replace placeholder content with real content
   - Adjust theme if needed
   - Submit to `create_presentation` — the slide structure is already optimized
3. **If no catalog fits**, build from scratch:
   a. **Pick a theme.** Call `list_themes` to see available presets. Match the theme to the content:
      - Corporate/professional: `corporate-dark`, `corporate-light`, `pharma-clean`
      - Creative/bold: `sunset`, `midnight`, `ocean`
      - Clean/minimal: `light`, `monochrome`
      - Nature/warm: `warm-earth`, `forest`
   b. **Plan diverse slide types.** NEVER make a PAK that's all bullets. Mix these:
      - `hero` — opening slide with icon/logo + title + subtitle
      - `stats` — metric cards (value + label), max 4 per slide
      - `chart` — Chart.js bar, doughnut, line, pie
      - `bullets` / `numbers` — lists (use sparingly)
      - `comparison` — before/after or vs (old/new/winner variants)
      - `table` — headers + rows for structured data
      - `testimonial` — quotes with name + role
      - `quote` — pull quote with source attribution
      - `features` — icon + title + text cards (use `image_url` instead of `icon` for logos)
      - `video` — full-viewport 9:16 video (direct MP4 URL, spliced into exported video)
      - `media` — inline image or video embed (YouTube/Vimeo auto-converts to iframe)
      - `cta` — closing slide with heading + text + button
      - `code` — code blocks with language label
   c. **Call `get_example_deck`** if you need a reference for JSON structure.
4. **Create the PAK** using `create_presentation` with `template: "semantic"`.
5. **Share the URL** with the user.
6. **Ask about approval** — "Ready to finalize this PAK?" If yes, call `approve_deck` to trigger video pre-caching.

## Key Rules

- Always use `template: "semantic"` — it supports all 59 semantic tags
- Every slide should have a `slide_label` (small caps section tag) and `slide_title`
- Use `slide_callout` for emphasized one-liners on stats/data slides
- Charts read CSS theme colors automatically — no need to specify colors
- Stats: max 4 per slide (2-column layout on mobile)
- Features: render 1-column on mobile, include icons or `image_url` for logos
- Testimonials: include name + role for credibility
- The `cta` slide is your closer — always end with one
- Video slides (`type: "video"` + `video_url` with direct MP4) are spliced into exported video at full quality
- YouTube/Vimeo URLs on any slide type render as embedded iframes (screenshot in video export)
- Use `logo_url` on hero slides for brand logos (centered) or content slides (top-left)
- Use `image_url` on any slide for inline images with frame + optional `image_caption`

$ARGUMENTS
