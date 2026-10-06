# RCA Learner

A curated corpus of approved root-cause analysis (RCA) documents plus a **Cursor Plugin** that helps agents draft new RCAs and learn from past incidents in this repository.

## Cursor Plugin (`rca-learner`)

This repo is packaged as a single-plugin Cursor Plugin (manifest at `.cursor-plugin/plugin.json`).

### Install locally

1. Clone the repository:

   ```bash
   git clone https://github.com/saurabh1-thakur-paytm/RCA-Learner.git ~/.cursor/plugins/local/rca-learner
   ```

2. Restart Cursor or reload the window so skills and rules are picked up.

3. Enable the plugin under **Customize** if it is not already active.

You can also install from a Git repository in Agent chat (for example `/add-plugin` with this repo’s GitHub URL), per [Cursor’s plugin documentation](https://cursor.com/docs/plugins).

### Team marketplace (optional)

Admins can **Import from Repo** in **Dashboard → Plugins & MCPs** and distribute the plugin to a team. End users install from **Customize** in the sidebar.

## What’s included

| Path | Purpose |
|------|---------|
| `.cursor-plugin/` | Plugin manifest (`plugin.json`) |
| `skills/consult-past-rcas/` | Skill to search and apply lessons from past RCAs |
| `skills/draft-rca/` | Skill to draft structured RCA markdown |
| `rules/rca-document-standards.mdc` | Editor rules for `rcas/**/*.md` |
| `rcas/` | Approved RCA markdown documents (source of truth) |

## Using the skills

- **Investigating or comparing incidents** — The agent should use `consult-past-rcas` to find similar past failures under `rcas/`.
- **Writing a new RCA** — Use `draft-rca` to produce a document that matches the repository’s section template and filename conventions.

Do not treat README content as incident data; refer to individual files under `rcas/` for specifics.

## License and contributions

Maintain the integrity of approved RCA content. New RCAs should follow the standards rule and existing document shape.
