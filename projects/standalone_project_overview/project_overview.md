# Standalone Project Overview Module

## Goal

Build a standalone, reusable project overview module that can be hosted and embedded via iframe on multiple websites (e.g., one under your real name and one under your pseudonym). The module will:

- Present a unified list or showcase of your projects.
- Allow central updates, so changes are instantly reflected wherever the module is embedded.
- Use a consistent UI and color scheme, as both websites share similar design language.
- Avoid code duplication and make maintenance easy.

### Approach

- Develop the overview as a self-contained HTML (and CSS/JS if needed) file, hosted at a public URL.
- Insert it into any website via an iframe (`<iframe src="/path/to/project_overview.html">`).
- Updates are made once to the source file, propagating automatically everywhere it’s included.
