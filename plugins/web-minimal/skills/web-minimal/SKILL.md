---
name: web-minimal
description: Build minimalist monospace web pages in the "content is the design" style — single-file HTML, no build tools, no frameworks, system monospace font, two shades of gray, zero decoration. Use when creating a personal site, blog, landing page, archive, link page, or any page that should read like a text file that happens to be on the web. Triggers on requests to build a simple/minimal/brutalist/no-frills site, a static HTML page, or "make it look clean and plain."
---

# web-minimal

Build web pages where the content *is* the design. The page should feel like a text file that happens to be on the web — made by a person, not a template. If a visual element doesn't serve the content, it doesn't exist.

Reach for this when someone wants a simple, plain, content-first page: a personal site, blog, link/archive page, changelog, or small landing page. Do **not** use it for product UIs that need rich interaction, dashboards, or marketing pages that want visual punch — those want a different aesthetic.

There is a ready-to-edit starting point at [`template.html`](./template.html). Copy it and replace the content rather than building from scratch — it already encodes every rule below.

## The philosophy (hold this above the rules)

Less is more. Personality comes from the writing and the structure, not from colors or effects. When in doubt, remove the element. Two of everything: two type weights, two grays, two states. No third option.

## Typography

- **Font:** system monospace only, no web fonts:
  `font-family: 'SF Mono', 'Menlo', 'Monaco', 'Courier New', monospace;`
- **Sizes:** body 13px, site title/header 14px, group labels 12px. Line-height 1.7 everywhere.
- **Weight:** normal (400) for body, bold (700) for section headers — nothing in between. No light, no semi-bold.
- **Case:** normal case. Not all-lowercase, not ALL CAPS, not Title Case On Every Word. Write like a person.

## Color — two colors: text and not-text

```css
color: #1a1a1a;        /* text */
color: #888;           /* secondary: metadata, labels, inactive controls */
border-color: #e0e0e0; /* dividers */
background: #fff;       /* page */
```

No brand colors, no accent colors, no colored badges/pills/tags/highlights. To emphasize something, make it **bold** or put it **first** — never color it. Links inherit text color; underline only where it's standard. Hover = `opacity: 0.6`.

## Layout

- Max width 600px, centered.
- Padding `3rem` top/bottom, `1.5rem` left/right. On mobile (<480px): `2rem` / `1rem`.
- Single column, top to bottom. No grids, columns, sidebars.

## Spacing

Generous between sections, tight within them.

- Between major sections: `3rem`
- Section header → content: `1.5rem`
- Between paragraphs: `1rem`
- Between list rows: `3px` vertical padding
- Dividers `1px solid #e0e0e0`, used only to separate major zones (e.g. before a footer).

## Components

- **Header:** site name on its own line (14px), optional author/tagline below in secondary color. No logo, no nav bar, no hamburger. If there's nav, it's plain text links, never buttons.
- **Section headers:** just `<strong>` inside a `<p>`. No `<h2>`, no uppercase, no decorative lines. The bold word is enough.
- **Bullet lists:** use a literal `* ` prefix in paragraphs, not `<ul>/<li>`. Keep date ranges as `2009-2022` (hyphen, no spaces); open-ended trails with a comma: `2025-,`.
- **Lists / tables of data:** flex rows, not `<table>`. Name is primary color and takes remaining space (truncate with ellipsis); metadata is secondary color, fixed-width or right-aligned. Row hover `background: #fafafa`. Group headers: secondary, 12px, bottom border.
- **Interactive controls:** bracket notation — `[A–Z]`, `[By city]`, `[All]`. Active = primary color, inactive = `#888`. No background, border, or pill shape. Precede with a secondary-color label like `View:` or `Source:`.
- **Inputs / search:** underline only — `border: none; border-bottom: 1px solid #e0e0e0; background: transparent; padding: 6px 0;`. Focus = `border-bottom-color: #1a1a1a`.
- **Stats / counts:** inline, secondary color, separated by ` · ` (space-middot-space): `300+ lists · 40+ cities`.
- **Footer:** minimal — a single "home" link in secondary color. Nothing else unless genuinely needed.

## File structure

One `index.html` with inline `<style>` and `<script>`. No build tools, no npm, no framework, no dependencies. Put any data in a JS array at the top of the script block, clearly commented for easy editing. If the project grows, each page is its own standalone HTML file — duplicate the small CSS rather than introducing a build step.

## Voice (when you write copy)

First person, casual, direct, unpretentious. Short sentences. No marketing language, no superlatives, no "welcome to my."

- Good: "This is my public archive."
- Bad: "Welcome to my curated collection of the world's finest destinations."

## Before you finish — verify none of these slipped in

Treat this as a hard checklist. If any appears, remove it:

- [ ] gradients, shadows, `border-radius`, or rounded corners
- [ ] any colored background on any element
- [ ] badges, pills, tags, or status chips with background color
- [ ] icons, emoji, or SVG decoration
- [ ] hero/header images
- [ ] animations or transitions (the only exception: hover `opacity`)
- [ ] web fonts (must be system monospace)
- [ ] a dark-mode toggle (if dark mode is truly required, invert the two grays — don't add a switcher unless asked)
- [ ] framework or "designed by" branding
- [ ] loading spinners or skeleton screens
- [ ] modals, tooltips, popovers, overlays
- [ ] sticky headers or scroll effects
- [ ] more than two type weights or more than two grays
