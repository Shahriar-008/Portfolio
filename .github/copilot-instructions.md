# AI Coding Agent Guidelines

## Project Overview
- This repo is a single-page personal portfolio site.
- All HTML, CSS, and JavaScript live in one file: [Portfolio/index.html](Portfolio/index.html).
- The site is fully static: no build step, backend, or bundler. Assume it can be served directly (e.g., GitHub Pages).

## Tech & Design
- **Tech stack:** plain HTML5, vanilla JS, and custom CSS with CSS variables; no frameworks.
- **Visual identity:** retro CRT/terminal aesthetic using scanlines, glow, monospaced fonts (Fira Code, JetBrains Mono), and terminal-style components.
- **Theme control:** core colors and effects are defined as CSS custom properties in the `:root` section at the top of [Portfolio/index.html](Portfolio/index.html). Prefer reusing or extending these variables instead of hardcoding new colors.

## Structure & Sections
- Main navigation anchors map to sections in [Portfolio/index.html](Portfolio/index.html): `#home`, `#about`, `#skills`, `#projects`, `#experience`, `#blog`, `#contact`.
- Reusable visual patterns:
  - **Terminal blocks** (e.g., about and contact) use `.terminal`, `.terminal-header`, `.terminal-content`, and `.terminal-line` classes.
  - **Cards** for skills, projects, experience, and blog posts use `.skill-card`, `.project-card`, `.experience-item`, `.blog-card` and related classes.
- When adding new content, prefer cloning these existing patterns and adjusting text/links instead of inventing new layouts.

## JavaScript Behavior
- All JS is inline at the bottom of [Portfolio/index.html](Portfolio/index.html) inside a single `<script>` block.
- Existing behaviors to preserve when extending:
  - Smooth scrolling for nav links by intercepting anchor clicks and calling `scrollIntoView` on target sections.
  - IntersectionObserver that animates elements with `.skill-card`, `.project-card`, `.experience-item`, `.blog-card` on scroll.
  - Blog section loader `fetchBlogPosts()` which:
    - Tries to fetch an RSS feed via `rss2json`.
    - Falls back to a hard-coded `fallbackPosts` array if the fetch fails or returns no items.
  - Periodic "glitch" effect on the hero title using randomized `textShadow`.
- If you introduce new elements that should animate on scroll, either:
  - Reuse one of the existing classes already observed, or
  - Explicitly register them with the existing IntersectionObserver.
- For new dynamic content, follow the pattern used in `renderPosts` (create elements, set classes, apply transitions, and then trigger animation via `requestAnimationFrame`).

## Blog Section Conventions
- The blog UI is built as a grid of `.blog-card` elements inside the `#blog-posts` container.
- Each blog card header mimics a mini terminal window with colored dots and a filename label.
- To add or adjust static/fallback posts, edit the `fallbackPosts` array in the script; keep the object shape (`title`, `date`, `excerpt`, `url`, `categories`, `file`).
- Preserve the try/catch + graceful-degradation pattern when modifying the RSS-fetch logic.

## Styling Conventions
- Use the existing CSS variables in `:root` for colors and theming; introduce new variables only if necessary and keep names consistent with the current style (e.g., `--something-color`).
- Maintain the retro terminal vibe:
  - Prefer monospace-like presentation and minimalistic, code-inspired labels.
  - Reuse hover/border/shadow patterns from `.project-card`, `.blog-card`, `.skill-card`.
- For responsive behavior, extend or mirror styles in the existing `@media (max-width: 768px)` block rather than introducing new breakpoints, unless explicitly requested.

## Workflow Assumptions
- Local development is typically done by opening [Portfolio/index.html](Portfolio/index.html) in a browser or via a simple static server; do not add heavy tooling (frameworks, bundlers) unless the user asks.
- Any new features should remain compatible with static hosting (no server-side dependencies, no required build step).

## Guidance for Agents
- Prefer small, focused edits to [Portfolio/index.html](Portfolio/index.html) over large refactors.
- When adding a new section (e.g., another project, skill, or experience), duplicate the closest existing pattern and adjust content.
- Avoid introducing external JS libraries/CSS frameworks unless explicitly requested; stick to the current vanilla and custom-CSS approach.
- Before restructuring the file (e.g., splitting into multiple HTML/CSS/JS files or adding a build system), ask the user to confirm that change in direction.
