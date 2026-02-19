---
name: pak-vocabulary
description: Complete 9x16 semantic vocabulary reference — all 59 tags across 14 categories. Use when you need to know what fields are available for PAK slides.
---

# 9x16 Semantic Vocabulary — 59 Tags, 14 Categories

Every tag name is simultaneously: a CSS target, a voice.js narration target, an MCP data key, and an API schema field.

## Structure Tags
- `slide-deck` — Root container (with optional `snap` attribute for scroll-snap)
- `slide` — Individual slide container (attribute: `type`)
- `slide-header` — Top section of a slide
- `slide-content` — Main content area
- `slide-footer` — Bottom section

## Text Tags
- `slide-label` — Small caps section category (e.g., "Key Metrics", "Pipeline")
- `slide-title` — Main heading
- `slide-subtitle` — Subheading / tagline
- `slide-body` — Paragraph text (can be array of paragraphs)
- `slide-callout` — Emphasized statement with accent border
- `slide-quote` — Pull quote / blockquote
- `slide-source` — Quote attribution
- `slide-caption` — Image/media caption

## List Tags
- `bullet-list` — Unordered list container
- `number-list` — Ordered list container
- `list-item` — Individual list item

## Stat Tags
- `stat-grid` — Grid container for stat cards
- `stat-card` — Individual stat card
- `stat-value` — The big number/metric
- `stat-label` — Description of the metric

## Feature Tags
- `feature-grid` — Grid container for feature cards
- `feature-card` — Individual feature card
- `feature-icon` — Icon (emoji or Font Awesome)
- `feature-title` — Feature heading
- `feature-text` — Feature description

## CTA Tags
- `cta-block` — Call-to-action container
- `cta-heading` — CTA headline
- `cta-text` — CTA supporting text
- `cta-button` — CTA button label

## Testimonial Tags
- `testimonial-grid` — Container for testimonial cards
- `testimonial-card` — Individual testimonial
- `testimonial-text` — The quote
- `testimonial-name` — Person's name
- `testimonial-role` — Person's title/role

## Code Tags
- `code-block` — Code container
- `code-content` — Pre-formatted code
- `code-label` — Language or filename
- `code-caption` — Description of the code

## Media Tags
- `slide-media` — Image/video container
- `media-caption` — Media description

### Media Data Fields (JSON → HTML mapping)
- `image_url` → `<slide-media><img>` — Inline image with frame on any slide type
- `image_caption` → `<media-caption>` — Caption below the image
- `video_url` → `<slide-media><video>` or `<iframe>` — Direct MP4/WebM renders as `<video>`, YouTube/Vimeo auto-converts to `<iframe>`
- `video_caption` → `<media-caption>` — Caption below the video
- `logo_url` → `<img>` in header — Centered on hero slides, top-left on content slides. Use PNG with transparency.
- `bg_image` → CSS `background-image` on `<slide>` — Full background image
- `bg_gradient` → CSS `background` on `<slide>` — CSS gradient background
- Feature cards: `image_url` instead of `icon` renders a logo/image in `<feature-icon>`

## Table Tags
- `data-table` — Table container
- `table-header` — Header row
- `table-row` — Data row
- `table-cell` — Individual cell

## Comparison Tags
- `compare-grid` — Comparison container
- `compare-card` — Individual comparison item (attribute: `variant="old|new|winner"`)
- `compare-label` — Item label
- `compare-text` — Item description

## Chart Tags
- `chart-container` — Chart.js wrapper
- `chart-canvas` — Canvas element for Chart.js
- `chart-title` — Chart heading
- `chart-caption` — Chart description/takeaway

## Layout Tags
- `two-column` — Two-column layout
- `column` — Individual column

## Video Slide Behavior
- `<slide type="video">` — Full-viewport 9:16 video takeover (TikTok-style)
- Direct MP4 URLs play inline and are **spliced into exported video** at full quality
- TTS narration is skipped for video slides
- YouTube/Vimeo on `type="video"` NOT supported — use on other slide types as iframe embeds
- On non-video slides, `video_url` renders as inline 16:9 embed within the slide content

## Nav/Chrome Tags
- `nav-bar` — Top navigation bar
- `nav-burger` — Hamburger menu button
- `nav-menu` — Slide-out menu (pipeline scripts inject controls here)
- `nav-link` — Individual menu link
- `slide-indicator` — Current slide number display
- `hero-footer` — Footer area on hero slides (no indicator)
