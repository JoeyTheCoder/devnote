# INSTRUCTIONS

This file provides context for Opencode and for you when opening the `devnote` Obsidian vault.

Purpose
- This vault is the user's personal knowledge base (PKB) for all project notes, personal notes, and to‑do lists.
- Goal: maintain a standardized, searchable note-taking system enhanced by AI and reliable to‑do lists for projects and personal matters.

Principles
- Standardize notes so they are easy to read, link, and automate with AI.
- Keep tasks actionable, short, and tracked as checklists with context and links to relevant notes.
- Prefer atomic notes (one idea or task per note) and link them together.
- Keep private or sensitive info out of shared or synced copies if privacy is a concern.

Recommended Vault Structure
- `notes/` — evergreen notes, concepts, references.
- `projects/` — project folders with project-specific notes and to‑do lists.
- `tasks/` — global or inbox-style task captures and recurring task lists.
- `templates/` — note and task templates for consistent capture.
- `daily/` — daily notes, journals, and quick captures.

Note Conventions
- Use a clear title and a short summary line under the title.
- Use YAML frontmatter for metadata when helpful, e.g.:
  ```yaml
  ---
  title: Short descriptive title
  tags: [project, idea]
  status: draft
  created: 2025-12-09
  ---
  ```
- Use tags for topic-level indexing and links for contextual connections.
- Keep action items in checklists with a single action per line: `- [ ] Do X (link)`.

Templates & AI enhancement
- Store standardized templates in `templates/` for:
  - Project intake
  - Meeting notes
  - Task capture
  - Daily note
- AI workflows to use with notes:
  - Summarize long notes into short abstracts and action items.
  - Extract and generate to‑do lists from meeting notes or project pages.
  - Propose tags and links to related notes.
  - Reformat or normalize notes to the standard template on request.

Task / To‑Do Guidelines
- Write tasks as: "verb + context" (e.g., "Write project README for X").
- Add a single link to the note that provides context for the task.
- Mark tasks with priority or due date in frontmatter or inline if needed.
- Regularly triage `tasks/inbox` into `projects/` or `tasks/` lists.

Agent (Opencode) Responsibilities
- Always read this `INSTRUCTIONS.md` on startup to get context for the vault.
- When asked, use the templates in `templates/` to create new notes or normalize existing ones.
- Offer to generate summaries, extract action items, and create linked to‑do lists from notes.
- Suggest or create tags and links to improve discoverability.
- Respect privacy: never upload or exfiltrate vault contents; all AI enhancement is intended to run locally or per your chosen service.

Startup Checklist
1. Load this `INSTRUCTIONS.md` into the agent context.
2. Open `templates/` and confirm templates exist for meetings, project intake, and tasks.
3. Open `tasks/inbox` (or create it) for capturing quick tasks.
4. Scan `projects/` for active projects and surface outstanding tasks.
5. Offer to normalize one sample note to the vault standard and extract tasks.

Notes
- You can edit this file to tune the conventions and AI workflows as needs change.
- If you want, I can scaffold templates, example project notes, and a sample workflow next.

(End of file)
