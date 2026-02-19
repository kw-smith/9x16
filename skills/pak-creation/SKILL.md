---
name: pak-creation
description: Expert knowledge for creating PAKs (Packaged Asset Kits) using the 9x16 semantic vocabulary. Use when creating presentations, decks, media packages, or any visual content via the 9x16 MCP tools.
---

# PAK Creation — Expert Guide

A PAK (Packaged Asset Kit) is an interactive, narrated 9x16 portrait media package. NOT a slide deck. NOT PowerPoint. It's a self-contained web experience with voice narration, video export, charts, and mobile-first design.

## Architecture

- **Renderer:** PHP semantic renderer at 9x16.run generates complete HTML from pure JSON data
- **CSS:** 9x16-core.css targets custom HTML element tag names directly (not classes)
- **Narration:** voice.js v3 extracts text from semantic tags and sends to ElevenLabs TTS
- **Video:** Slides rendered as PNGs, narration generated per-slide, stitched via ffmpeg into 1080x1920 H.264/AAC. Video slides (`type: "video"` with direct MP4) are spliced in at full quality — no screenshot.
- **Charts:** Chart.js reads CSS custom properties at runtime — charts auto-theme with the deck

## Catalogs — Pre-Built Deck Structures

Before building from scratch, check if a catalog fits. Call `list_catalogs` to see 10 pre-built structures:

| Catalog ID | Category | Best For |
|-----------|----------|----------|
| quarterly-report | business | Executive performance summaries |
| product-launch | marketing | New product announcements |
| company-pitch | business | Investor/partner pitches |
| case-study | marketing | Client success stories |
| sales-proposal | business | Client-facing proposals |
| project-update | business | Stakeholder status reports |
| training-module | education | Educational content |
| event-recap | creative | Post-event summaries |
| brand-overview | marketing | Company story + values |
| data-dashboard | business | Analytics-heavy reports |

**Catalog workflow:**
1. `list_catalogs` — browse available structures
2. `get_catalog` with catalog_id — get the full deck JSON with placeholder content
3. Replace placeholders with real content (keep slide types + structure)
4. Adjust theme if needed
5. Submit to `create_presentation`

Catalogs save time and ensure diverse slide types. The structure is already optimized — you just swap content.

## The Golden Rule

**Tag name = CSS target = narration target = MCP data key = API schema field.**

When you set `slide_title` in the JSON, it becomes a `<slide-title>` HTML element, styled by CSS, narrated by voice.js, and exported in video. One vocabulary, one pipeline.

## Slide Types & JSON Structure

### Hero (opening slide)
```json
{"type": "hero", "icon": "🧬", "slide_title": "Company Name", "slide_subtitle": "Tagline or Context"}
```

### Stats (metric cards — max 4)
```json
{"type": "stats", "slide_label": "Key Metrics", "slide_title": "Q4 at a Glance", "stats": [{"value": "$847M", "label": "Revenue"}, {"value": "34%", "label": "YoY Growth"}], "slide_callout": "Strongest quarter in company history."}
```

### Chart (Chart.js — bar, doughnut, line, pie)
```json
{"type": "chart", "slide_label": "Revenue", "slide_title": "Quarterly Trend", "chart": {"type": "bar", "data": {"labels": ["Q1","Q2","Q3","Q4"], "datasets": [{"label": "Revenue ($M)", "data": [580,640,720,847]}]}}, "chart_title": "Revenue by Quarter", "chart_caption": "Q4 driven by product launch."}
```

### Bullets (unordered list)
```json
{"type": "bullets", "slide_label": "Pipeline", "slide_title": "Highlights", "slide_body": "Three programs advanced:", "bullets": ["Item one", "Item two", "Item three"]}
```

### Numbers (ordered list)
```json
{"type": "numbers", "slide_label": "Timeline", "slide_title": "What's Next", "numbers": ["Q1: Launch in Japan", "Q2: Phase III data", "Q3: EU expansion"]}
```

### Comparison (before/after, vs)
```json
{"type": "comparison", "slide_label": "Impact", "slide_title": "Before & After", "compare": [{"label": "Old Way", "text": "Manual, slow, 45% error rate", "variant": "old"}, {"label": "New Way", "text": "Automated, fast, 2% error rate", "variant": "new"}, {"label": "Result", "text": "47% cost reduction", "variant": "winner"}]}
```

### Table (headers + rows)
```json
{"type": "table", "slide_label": "Financials", "slide_title": "Summary", "table": {"headers": ["Metric","Q4","Q3","Change"], "rows": [["Revenue","$847M","$720M","+18%"],["Margin","78%","74%","+4pts"]]}}
```

### Testimonial (quotes with attribution)
```json
{"type": "testimonial", "slide_label": "Feedback", "slide_title": "What Clients Say", "testimonials": [{"text": "Transformed our workflow.", "name": "Dr. Sarah Chen", "role": "Chief of Endocrinology"}]}
```

### Quote (pull quote)
```json
{"type": "quote", "slide_label": "Leadership", "slide_title": "CEO Message", "slide_quote": "Patient-centered design and commercial success aren't opposing forces.", "slide_source": "— Maria Torres, CEO"}
```

### Features (icon + title + text cards)
```json
{"type": "features", "slide_label": "Roadmap", "slide_title": "2026 Priorities", "features": [{"icon": "🚀", "title": "Expand", "text": "Launch in 6 new markets"}, {"icon": "🔬", "title": "Innovate", "text": "3 new products"}]}
```

### CTA (closing slide — always end with one)
```json
{"type": "cta", "slide_label": "Close", "slide_title": "Thank You", "slide_body": "Contact us at info@company.com", "cta_heading": "Company Name", "cta_text": "Transforming the Industry", "cta_button": "Learn More →"}
```

### Code (code blocks)
```json
{"type": "code", "slide_label": "API", "slide_title": "Quick Start", "code_content": "curl -X POST https://api.example.com/v1/create", "code_label": "bash", "code_caption": "Create a new resource with a single API call."}
```

### Video (full-viewport 9:16 — direct MP4 only)
```json
{"type": "video", "video_url": "https://example.com/clip.mp4"}
```
Video slides take over the full viewport (TikTok-style). Direct MP4 URLs are **spliced into exported video** at full quality — no screenshot fallback. TTS narration is skipped for video slides. YouTube/Vimeo URLs are NOT supported on video type — use them on other slide types where they render as embedded iframes.

### Media (inline image or video embed)
```json
{"type": "media", "slide_label": "Demo", "slide_title": "Product in Action", "image_url": "https://example.com/screenshot.png", "image_caption": "Dashboard view showing real-time analytics"}
```
For video embeds (YouTube/Vimeo):
```json
{"type": "body", "slide_label": "Demo", "slide_title": "Watch the Demo", "video_url": "https://youtube.com/watch?v=abc123", "video_caption": "3-minute product walkthrough"}
```

### Images & Logos (on any slide type)
- `logo_url` — brand logo. Centered on hero slides, top-left on content slides. Use PNG with transparency.
- `image_url` — inline image with frame. Works on any slide type. Add `image_caption` for description.
- `bg_image` — full background image for any slide.
- Feature cards accept `image_url` instead of `icon` for logo/image cards.

## Theme System

12 presets available via `list_themes`. Set in the JSON:
```json
{"theme": {"preset": "pharma-clean"}}
```

Override individual values:
```json
{"theme": {"preset": "corporate-dark", "accent": "#e85d04"}}
```

### Theme Recommendations by Use Case
| Use Case | Preset | Why |
|----------|--------|-----|
| Pharma/Healthcare | pharma-clean | Clinical white + teal, professional |
| Corporate/Finance | corporate-dark | Navy + blue, authoritative |
| Internal Demo | default | Dark + orange, energetic |
| Client-Facing | corporate-light | White + blue, clean |
| Creative/Marketing | sunset or midnight | Bold colors, memorable |
| Data-Heavy | monochrome | Zero distraction, data focus |

## Quality Checklist

Before creating a PAK, verify:
- [ ] At least 3 different slide types used (never all-bullets)
- [ ] Hero slide opens, CTA slide closes
- [ ] Every slide has `slide_label` + `slide_title` (except video type)
- [ ] Stats limited to 4 per slide
- [ ] Charts have `chart_title` + `chart_caption`
- [ ] Theme matches the audience/industry
- [ ] `slide_callout` used on data slides for key takeaways
- [ ] Testimonials have name + role
- [ ] 10-17 slides is the sweet spot (enough depth, not overwhelming)
- [ ] Video slides use direct MP4 URLs only (not YouTube/Vimeo)
- [ ] `logo_url` uses PNG with transparency for best results

## After Creation

- Share the `view_url` with the user
- Ask "Ready to finalize?" before calling `approve_deck`
- `approve_deck` triggers background video pre-caching (narration + encode)
- Video export: Mac Studio hardware encoder when available (~20s), local ffmpeg fallback (~3-4 min)
- Video slides with direct MP4 are spliced into the export at full quality
- Second video request for same PAK is instant (cached)

## Available MCP Tools

| Tool | Purpose |
|------|---------|
| `list_catalogs` | Browse 10 pre-built deck structures |
| `get_catalog` | Get full deck JSON for a catalog (replace content, submit) |
| `list_templates` | List legacy HTML templates (62+) |
| `list_themes` | Browse 12 theme presets |
| `get_template` | Get schema for a legacy template |
| `get_example_deck` | Complete 12-slide reference with ALL slide types |
| `create_presentation` | Create a PAK from JSON data |
| `export_slides` | Export slides as PNGs |
| `export_pdf` | Export as PDF |
| `export_video` | Export as narrated 9:16 video (async) |
| `check_video_status` | Poll video generation progress |
| `approve_deck` | Finalize PAK + trigger video pre-cache |
