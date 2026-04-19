---
data_status: verified  # verified | plausible | demo-only (see 0-meta/README.md for definitions)
---

# [Document Title]

**Layer:** `1-company` / `2-departments` / `3-products` / `4-projects` / `5-agents`
**Owner:** <!-- GitHub username or Name -->
**Level owner:** <!-- Who owns the parent section (dept head, product owner, etc.) -->
**Last reviewed:** <!-- YYYY-MM-DD -->
**Review cadence:** `Annual` / `Quarterly` / `At-change` / `Monthly`
**Status:** `Draft` / `Active` / `Archived`
**Data status:** `verified` / `plausible` / `demo-only`

---

## Changelog

| Date | Change | Author |
|------|--------|--------|
| <!-- YYYY-MM-DD --> | Initial version | <!-- Name --> |

---

## Purpose

*1–2 sentences: what question does this document answer? What decision or action does it enable?*
*Write this for both a new team member and an AI agent.*

---

## Scope

**In scope:**
- <!-- What this document covers -->

**Out of scope** *(see linked documents below):*
- <!-- What belongs elsewhere, with a pointer to where -->

---

## [Main Content Section 1]

*Use specific, descriptive headings. Avoid "Overview" or "Details".*

---

## [Main Content Section 2]

---

## [Main Content Section 3]

---

## Decision authority

*Required for `1-company` and `2-departments` documents. Optional for others.*

| Decision type | Authority | Escalation |
|---------------|-----------|------------|
| <!-- Type --> | <!-- Role/Name --> | <!-- Who if primary unavailable --> |

---

## Related documents

| Document | Layer | Relationship |
|----------|-------|-------------|
| <!-- path/to/doc.md --> | <!-- layer --> | <!-- Why read together --> |

---

## How AI agents should use this document

*Recommended for all `2-departments`, `3-products`, and `5-agents` documents.*

When processing a task that involves [domain], an agent should:
1. Load this document as part of standard context
2. <!-- Specific instruction -->
3. <!-- Specific instruction -->

Anti-patterns to avoid:
- Do not <!-- specific thing that would be wrong for an agent to do with this content -->

---

*Instructions for using this template:*
*— Delete all italicised instruction text before publishing*
*— Fill all header fields before marking Status: Active*
*— Every document must have a named Owner before it is considered Active*
*— Remove sections that do not apply, but always keep: Changelog, Purpose, Scope*
