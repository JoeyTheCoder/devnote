# Standalone Project Overview Module

## Goal

Build a standalone, reusable project overview module that can be hosted and embedded via iframe on multiple websites (e.g., one under your real name and one under your pseudonym). The module will:

- Present a unified list or showcase of your projects.
- Allow central updates, so changes are instantly reflected wherever the module is embedded.
- Use a consistent UI and color scheme, as both websites share similar design language.
- Avoid code duplication and make maintenance easy.

## Design Requirements

- **Primary color:** `#0f172b` (used as the accent/background for unified branding)
- **Modern UI:** Clean, minimal design with clear project cards or tiles.
- **Responsive layout:** Looks great on desktop and mobile.
- **Project display:** Each project card includes:
  - Screenshot (with unified aspect ratio and size)
  - Title
  - Short description
  - Technologies used
  - Live link (optional)
- **Professional screenshot presentation:** All project images are auto-sized/cropped/letterboxed for a uniform appearance.

## Approach

- Develop the overview as a self-contained HTML (and CSS/JS if needed) file, hosted at a public URL.
- Insert it into any website via an iframe (`<iframe src="/path/to/project_overview.html">`).
- Updates are made once to the source file, propagating automatically everywhere it’s included.

## Steps to Achieve This

1. **Decide on the tech stack** for the standalone overview (e.g., Vite + Vanilla JS or Static HTML/CSS/JS).
2. **Design the layout:**
   - Sketch a grid or card-based overview with primary color #0f172b.
   - Ensure cards have fixed-width/fixed-aspect screenshot areas.
3. **Create placeholder data:**
   - Make a sample JSON/list of projects with all relevant info.
4. **Build the HTML/CSS structure:**
   - Use CSS Grid or Flexbox for layout.
   - Apply #0f172b as background or accent color.
5. **Implement screenshot handling:**
   - Set image containers (e.g., 16:9 box with `object-fit: cover` or `contain`)
   - Apply a letterbox or background color for non-matching aspect ratio screenshots.
6. **Style each project card:**
   - Consistent padding, spacing, font usage
   - Hover and focus styles for interactivity
7. **Make responsive:**
   - Test grid/cards layouts at multiple breakpoints
8. **Hook up dynamic data (if needed):**
   - Optionally use JS to render from a JSON object for easier updates
9. **Test embed via iframe:**
   - Preview in an iframe on a test page; make adjustments for responsiveness and appearance.
10. **Finalize and deploy:**
    - Host project_overview.html (and assets like screenshots)
    - Embed in your main websites with `<iframe src>`

---

**Notes:**
- Use consistent aspect ratio containers for screenshots (CSS: width, height, object-fit, background color) to ensure a professional presentation.
- Document any requirements for screenshot image sizes/formats.
