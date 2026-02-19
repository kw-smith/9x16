---
description: Browse PAK catalog presets — pre-built deck structures you can fill with content
disable-model-invocation: true
---

# Browse PAK Catalogs

Call `list_catalogs` to show all available catalog presets. Catalogs are pre-built deck structures (theme + slide types + placeholder content) optimized for specific use cases.

## Available Catalogs (10)

**Business:**
- `quarterly-report` — Executive performance summary (stats, charts, comparison)
- `company-pitch` — Investor/partner pitch (vision, market, traction, ask)
- `sales-proposal` — Client proposal (problem, solution, pricing, ROI)
- `project-update` — Stakeholder status report (progress, blockers, next steps)
- `data-dashboard` — Analytics-heavy report (KPIs, charts, tables)

**Marketing:**
- `product-launch` — New product announcement (problem, solution, features, proof)
- `case-study` — Client success story (challenge, approach, results)
- `brand-overview` — Company story + values (onboarding, partner intros)

**Education:**
- `training-module` — Educational content (objectives, lessons, practice)

**Creative:**
- `event-recap` — Post-event summary (highlights, stats, feedback)

## Workflow

When the user picks a catalog:
1. Call `get_catalog` with the catalog_id
2. Show them the structure (slide types + theme)
3. Ask what content to fill in
4. Replace ALL placeholder content with real data
5. Submit to `create_presentation` with `template: "semantic"`

The slide structure and theme are already optimized — just swap the content.

$ARGUMENTS
