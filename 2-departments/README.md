# 2. Departments

Per-department knowledge. Each department gets its own subfolder.

## How to Add a Department

1. Copy `_template/` to a new numbered folder: `cp -r _template/ 1-engineering/`
2. Rename it: `1-engineering/`, `2-marketing/`, `3-finance/`, etc.
3. Fill in the template files with your department's actual content
4. Open a PR — department owner approves

## Structure per Department

```
2-departments/
  1-[department-name]/
    README.md            ← who we are, what we own
    1-overview.md        ← purpose, scope, key responsibilities
    2-processes/
      README.md
      process-template.md  ← copy per process/SOP
    3-team.md            ← people, roles, contacts
    4-decisions/
      README.md          ← department-level ADRs
```

## Purpose for Agents

Load the relevant department folder when working on department-specific tasks. Department context tells agents who owns what, what processes to follow, and who to involve in decisions.
