---
name: draft-rca
description: Draft a structured Markdown RCA matching this repository's document shape and standards under rcas/.
---

# Draft RCA

Produce a new or revised RCA as Markdown that matches the structure used in approved documents under `rcas/`. Do not copy incident content from existing files; only mirror the section template and writing conventions.

## Document structure

Use this outline (adjust title and sections to the source material only):

Optional YAML frontmatter when known (mirror keys used in existing RCAs), for example:

```yaml
---
status: Draft
version: v1
source_message_id: <if known>
source_thread_id: <if known>
generated: YYYY-MM-DD
---
```

Then:

- `# RCA: <title>`
- `## Date` — bullet list of relevant dates from the source
- `## Source` — authors, channels, subjects, references; no fabrication
- `## Summary` — concise narrative of what happened
- `## Impact` — user/system/business impact; use "Not stated in source" when unknown
- `## Timeline` — chronological bullets with dates when available
- `## Root Cause` — technical and process causes supported by the source
- `## Resolution / Fix` — what was done or planned
- `## Preventive Actions / Action Items` — table:

  | # | Action | Owner | Status |

  Include a row for undocumented preventive actions when the source omits them (Action: preventive actions to avoid recurrence; Owner/Status: Not stated in source).

- `## Open Questions` — table or bullets for unresolved items; assign "owner to be assigned" when unknown

## Filename and location

- Save new RCAs as `rcas/YYYY-MM-DD-<short-slug>.md` (ISO date, kebab-case slug).
- Do not modify unrelated files under `rcas/` unless the user explicitly asks to update a specific document.

## Accuracy rules

- Never invent owners, metrics, timelines, or technical details. Use phrases such as **Not stated in source** or **owner to be assigned**.
- Preserve existing frontmatter keys when editing an approved doc; do not remove approval metadata without explicit instruction.

## Before finalizing

Run a **consult-past-rcas** style pass: search `rcas/` for similar incidents. If related lessons exist, add a short note (for example under Summary or Open Questions) listing related filenames and one-line takeaways. Do not duplicate their incident narratives.
