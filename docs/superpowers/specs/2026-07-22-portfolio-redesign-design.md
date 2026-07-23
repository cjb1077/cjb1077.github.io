# Portfolio Redesign — Design Spec

Date: 2026-07-22

## Purpose

Redesign the visual presentation of `index.html` — a static GitHub Pages portfolio for CJ Bingham (CS student / grant-funded AI researcher, job-seeking in data/AI roles). Content (bio, project write-ups, 11 data-viz images, contact info) is carried over and restructured, not rewritten. The resume section is removed for now. No backend, no build step — this stays a single static site served directly by GitHub Pages.

## Constraints

- **Static only.** No server, no build tooling, no framework, no package.json. GitHub Pages serves the repo's files as-is.
- **Single-file convention preserved.** Like the current site, `index.html` holds structure, inline `<style>`, and inline `<script>`. No new files beyond what already exists (`index.html`, `images/*.png`, `resume.pdf`) unless a follow-up asks for it.
- **`resume.pdf` stays in the repo** (not deleted) — only the UI section/nav link referencing it is removed, since the user said "for now."
- **No external font/JS CDNs beyond what's already used** (the existing site links Google Fonts for Inter + a Plausible analytics script). The redesign swaps the font link to Geist + Geist Mono via Google Fonts and keeps the Plausible snippet as-is.
- All 11 existing images in `images/viz-1.png` … `images/viz-11.png` are used with their real captions — no invented content.

## Visual system

**Direction:** industrial / technical, dark, single-theme by design (this is a deliberate "terminal aesthetic" choice, not an oversight — no light-mode variant).

**Color tokens (OKLCH):**

```css
--color-paper:      oklch(14% 0.014 45);   /* near-black, amber-tinted */
--color-paper-2:    oklch(17% 0.016 45);   /* card surface */
--color-paper-3:    oklch(21% 0.018 45);   /* elevated surface */
--color-ink:        oklch(92% 0.010 45);   /* primary text */
--color-ink-2:      oklch(78% 0.010 45);   /* secondary text */
--color-rule:       oklch(30% 0.014 45);   /* borders/dividers */
--color-rule-2:     oklch(24% 0.014 45);   /* secondary dividers */
--color-muted:      oklch(58% 0.012 45);   /* de-emphasized text */
--color-accent:     oklch(68% 0.18 45);    /* amber/safety-orange */
--color-accent-ink: oklch(12% 0.02 45);    /* text on accent fill */
--color-focus:      oklch(73% 0.20 45);    /* focus ring */
```

**Typography:** Geist Mono (display/labels/nav) + Geist (body) — same type family at different widths, the deliberate single-family "terminal" exception. Loaded via Google Fonts `<link>` (same mechanism the current site already uses for Inter). Type scale: major-third-ish, display headline `clamp(2.2rem, 4vw + 1rem, 3.6rem)`.

**Spacing:** 4pt scale (`--space-3xs` … `--space-4xl`), named tokens, used via `gap` not stacked margins.

**Motion:** page-load stagger reveal on hero elements; scroll-triggered reveal-once via `IntersectionObserver` on project/gallery cards; hover lift (`translateY(-2px)` + border-to-accent) on cards and buttons; instant `:focus-visible` ring. All easing via `cubic-bezier(0.16, 1, 0.3, 1)` (ease-out). Respects `prefers-reduced-motion`.

**Decorative texture:** a faint blueprint grid-line background (`repeating-linear-gradient`, CSS-only, no image asset) reinforcing the industrial/technical feel.

## Structure (top to bottom)

1. **Nav** — terminal-command style bar: `> cj-bingham` + `--projects` / `--gallery` / `--contact` links + a blinking cursor (CSS `steps()` animation, decorative, respects reduced-motion).
2. **Hero / Identity** — two-column: left holds name, role line, the existing bio paragraph (verbatim), and CTA buttons (View Projects / LinkedIn / Email — no Resume button); right holds skills reorganized from pill-chips into a tabular "spec sheet" (category → items, hairline rows), grouped as: Languages, Data/ML, Viz, AI/LLM, Databases, Tooling — same skill content as today's chip list, regrouped.
3. **Projects section** — heading + grid of the 3 existing project write-ups as cards (MonoSpace Studio featured/wider since it's a live linked product; Child Welfare AI and All of Us Hackathon as standard cards). Copy carried over verbatim from the current site.
4. **Data Visualization section** — heading + uniform grid of the 11 existing images (`images/viz-1.png` … `viz-11.png`), same captions as today, real `<img>` tags (not the placeholder tiles used in the mockup preview).
5. **Footer** — dense monospace colophon: name/role, email, LinkedIn, copyright line. No separate "Contact" section — contact info lives in the footer and the hero CTA row.

**Explicitly removed:** the `#resume` section (heading, PDF iframe, download/open buttons) and its nav link. `resume.pdf` file itself stays in the repo untouched.

## Accessibility & responsive

- All interactive elements ≥44×44px hit target on mobile.
- `:focus-visible` ring on every link/button, instant (no transition on appearance).
- Grid layouts collapse to single column below `60rem`; nav/hero/spec-sheet stack below `40rem`.
- Images keep real `alt` text (carried over from current `alt` attributes).
- Verified no horizontal scroll via `overflow-x: clip` on `html`/`body`.

## Out of scope

- Rewriting bio, project, or gallery copy (content is carried over as-is, just restructured).
- Any backend, CMS, or build pipeline.
- Bringing the resume section back (tracked as a future follow-up, not part of this change).
- Changing/reducing the 11 gallery images.
