# Redesign Tasks — Enhanced

This file lists the redesign tasks for the `joelsahli.ch` developer portfolio, enhanced with acceptance criteria, implementation hints, priorities and estimates.

1) Increase SVG grid visibility (hero)
- Description: Make the bottom-right area of the hero SVG grid more visible while preserving the existing fade from top-left to bottom-right.
- Acceptance criteria: bottom-right grid opacity or contrast increased without introducing layout shift; visual balance preserved on mobile and desktop; no new accessibility or performance regressions.
- Implementation hints: tweak SVG stop colors/opacity, increase stroke-opacity on the lower-right stops, or add a subtle overlay/gradient to boost contrast only in the bottom-right; keep file size minimal and test retina screens.
- Priority: Medium
- Estimate: Small

2) Typed headline animation
- Description: Animate the tagline `Fullstack Software Engineer / DevOps Engineer / Agile Leader Based in Switzerland 🇨🇭` with a typewriter effect on page load or when the hero becomes visible.
- Acceptance criteria: text types in smoothly; respects `prefers-reduced-motion` (static text shown when reduced motion is requested); animation duration configurable (~2–4s) and keyboard accessible.
- Implementation hints: prefer a CSS-first solution (monospace-ish animation with `steps()`), or a small JS typewriter that writes to an element dataset; ensure screen readers read the full text and not the animated partial text (e.g., set `aria-live` appropriately or reveal the element to assistive tech after animation).
- Priority: High
- Estimate: Small

3) Improve hero CTA visibility
- Description: Make the hero call-to-action buttons visually distinct from the hero background so they don't blend in.
- Acceptance criteria: buttons meet WCAG AA contrast ratio against the background; clear hover and focus states; preserved responsive spacing.
- Implementation hints: add semi-opaque solid background, subtle box-shadow or outline, increase border-radius/size for better affordance; ensure focus ring is visible and follows the site style.
- Priority: High
- Estimate: Small

4) Arrow button scroll behavior
- Description: The bottom arrow button should smoothly scroll to the next section when clicked.
- Acceptance criteria: smooth scroll behavior that accounts for the fixed navbar height so the target section is visible below the navbar on all viewports; supports keyboard activation; respects `prefers-reduced-motion`.
- Implementation hints: use `scroll-margin-top` on target sections (CSS) or compute offset in JS: `targetY = target.getBoundingClientRect().top + window.scrollY - navbarHeight; window.scrollTo({top: targetY, behavior: 'smooth'})`.
- Priority: High
- Estimate: Small

5) About section readability
- Description: Improve presentation of the About content for better scannability.
- Acceptance criteria: content split into a lead sentence + short paragraphs and bullets; typographic scale improved; line length comfortable on narrow viewports; mobile reading is clear.
- Implementation hints: extract a one-line summary at the top, create a short set of bullets for highlights/skills, optionally add a small profile photo or callout box for key facts.
- Priority: Medium
- Estimate: Small–Medium

6) Global anchor/scroll offset (fixed navbar)
- Description: Ensure that all in-page anchor scrolls (buttons, links, programmatic scrolls) account for the fixed navbar so content is not hidden behind it.
- Acceptance criteria: internal anchors land with the top of the target visible below the navbar across desktop and mobile; consistent behavior for all links and programmatic scrolls.
- Implementation hints: prefer a CSS approach using `scroll-margin-top: var(--navbar-height);` applied to `section` targets; for JS-driven scrolls, subtract `navbar.offsetHeight` from the computed scroll position.
- Priority: High
- Estimate: Small

7) Skills section revamp
- Description: Make the Skills section more engaging — consider chips, grouped badges, progress bars, or an interactive grid grouped by category (Frontend, Backend, DevOps, Tools).
- Acceptance criteria: skills are visually scannable, grouped logically, responsive layout, icons have accessible text alternatives; no heavy dependencies.
- Implementation hints: implement CSS-based skill chips with hover/tooltips, or lightweight SVG badges; consider animated counts or small progress bars for proficiency (optional) but ensure reduced-motion fallback.
- Priority: Medium
- Estimate: Medium

8) Career timeline
- Description: Replace the current career list with a vertical timeline: a central vertical line and milestone cards alternating left and right on wide viewports and stacked on mobile.
- Acceptance criteria: clear vertical line with nodes, alternating milestone cards with role/company/date and short bullets; accessible markup (semantic headings and lists), and mobile-first behavior that stacks items.
- Implementation hints: implement with CSS grid/flex or a simple timeline component; use pseudo-elements for the vertical line; test keyboard and screen-reader order.
- Priority: Medium
- Estimate: Medium

9) Footer rework
- Description: Update the footer to show the current year dynamically, correct the contact email, and improve visual interest.
- Acceptance criteria: `© [Current Year]` is displayed (generated via template or JS), correct email address present, accessible links, and visually separated from page content.
- Implementation hints: use a template variable or small JS to set the year; add a subtle divider and optional social links with accessible labels.
- Priority: Low
- Estimate: Small

---

Notes
- For animations prefer CSS-first solutions and keep JS minimal.
- Respect `prefers-reduced-motion` for all motion/animation changes.
- Keep bundle size small; avoid adding heavy libraries unless necessary and justified.

Prompt file created: `ai-implementation-prompt.md` in the same folder (contains the AI assistant prompt for implementing the changes; not printed here).
