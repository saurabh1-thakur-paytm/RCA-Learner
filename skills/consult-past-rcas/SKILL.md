---
name: consult-past-rcas
description: Use when investigating an incident, drafting a new RCA, or looking for similar past failures; search and apply lessons from markdown files under rcas/.
---

# Consult past RCAs

When the user is investigating an incident, writing an RCA, or asking whether a failure pattern has happened before, use this skill to learn from the approved corpus in `rcas/`.

## Steps

1. **Search the corpus** — Read and search markdown files under `rcas/` for similar symptoms, affected systems, root causes, mitigations, or preventive actions. Use filename dates and titles as quick filters; read full documents when a file looks relevant.

2. **Cite concretely** — Prefer citing specific past RCAs by filename (for example `rcas/2026-10-06-example-slug.md`) and summarize only the lesson that applies to the current question. Quote or paraphrase briefly; do not dump entire documents.

3. **No invented facts** — Never invent incident details, metrics, owners, or outcomes that are not stated in those files. If the corpus does not cover something, say so explicitly.

4. **Sensitive content** — If you surface excerpts that may include customer data, credentials, security details, or personal email addresses, flag that the excerpt may be sensitive and avoid reproducing secrets or tokens.

5. **Ground recommendations** — When suggesting preventive actions or patterns, tie them to what appears across multiple past RCAs when possible (for example repeated themes like missing load testing or architecture drift). If only one past RCA mentions a pattern, say it is a single precedent, not a established team norm.

## Output

- List related past RCAs with one-line relevance.
- Summarize applicable lessons in plain language.
- Call out gaps where the corpus does not answer the question.
