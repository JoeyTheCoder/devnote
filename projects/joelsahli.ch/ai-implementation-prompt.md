You are an expert front-end developer. Repository path: `projects/joelsahli.ch`. Task: implement the site improvements listed below. Detect the framework/build system used (plain HTML/CSS/JS, React, Vue, etc.) and adapt changes accordingly. If you need to change multiple files, create logical commits per feature and provide diffs or a summarized patch.

Deliverables (for each item):
- Code changes (HTML/CSS/JS or component updates).
- Short commit message per feature and a brief changelog entry.
- Instructions to manually test each change (one-liner per item).
- Accessibility checks: ensure animations respect `prefers-reduced-motion`, buttons/links have focus states, and text contrast meets WCAG AA.
- Do not push to remote; provide patches/PR-ready diffs or code snippets.

Features to implement:
1. Increase SVG grid visibility in hero
   - Make the bottom-right of the hero background SVG grid more visible while preserving the fade from top-left to bottom-right.
   - Acceptance: bottom-right opacity/contrast increased, no layout shift, looks good on mobile and desktop.

2. Typed headline animation
   - Animate the tagline `Fullstack Software Engineer / DevOps Engineer / Agile Leader Based in Switzerland 🇨🇭` with a typewriter effect on page load (or when hero becomes visible).
   - Respect `prefers-reduced-motion`; make animation configurable in duration.
   - Acceptance: types in on load or when visible; fallback shows text statically.

3. Improve hero CTA button visibility
   - Increase contrast and add clear hover/focus styles.
   - Acceptance: buttons meet WCAG AA contrast; visible states on hover/focus.

4. Arrow button scroll
   - Make the bottom-of-page arrow smoothly scroll to the next section.
   - Account for the fixed navbar height so the target section is fully visible below the navbar.
   - Acceptance: scroll lands section content below navbar on all viewports.

5. About section readability
   - Break long paragraphs into a lead sentence + short paragraphs + bullets or highlights; add typographic spacing improvements.
   - Acceptance: shorter lines, headings/subheadings, better mobile reading.

6. Ensure all in-page anchor scrolls respect fixed navbar
   - Globally fix internal anchors and programmatic scrolls so targets appear below navbar.
   - Suggestion: add CSS `scroll-margin-top` to section targets or central JS helper.

7. Revamp Skills section
   - Replace static list with visually appealing chips, grouped badges, progress bars, or an interactive grid grouped by category (Frontend, Backend, DevOps, Tools).
   - Acceptance: skills are visually scannable, grouped logically, responsive layout, icons have accessible text alternatives; no heavy dependencies.

8. Add Career timeline
   - Present career milestones on a vertical timeline with nodes and alternating cards left/right on wide viewports and stacked on mobile.
   - Acceptance: clear vertical line, readable milestone cards with dates and bullets.

9. Footer fixes
   - Show dynamic current year, correct contact email, and improve layout (divider, icon links).
   - Acceptance: `© [Current Year]`, corrected email, accessible links.

Constraints & extras:
- Prefer CSS-first solutions for animations (hardware accelerated) and minimal JS for behaviors that CSS cannot handle (e.g., scroll offset).
- Keep bundle size small and avoid heavy libraries unless budgeted; if a library is required, justify it.
- Provide unit or manual test steps to validate cross-browser behavior (Chrome, Firefox, Safari).
- Provide screenshots or short GIFs for visual changes where possible.

Example code samples you can use or adapt:
- Smooth scroll offset JS:
`const navbarHeight = document.querySelector('.navbar').offsetHeight; const targetY = document.querySelector('#target').getBoundingClientRect().top + window.scrollY - navbarHeight; window.scrollTo({ top: targetY, behavior: 'smooth' });`
- `scroll-margin-top` CSS:
`section { scroll-margin-top: var(--navbar-height); }`
- Typewriter fallback for reduced motion included.

Output requested:
- A numbered list summarizing what you changed for each task, with file paths and diffs or code blocks.
- Commit messages (one line) for each feature.
- Manual test steps (2–3 steps each).

If anything is ambiguous in the codebase (framework, file locations), detect it and ask one targeted question before proceeding.