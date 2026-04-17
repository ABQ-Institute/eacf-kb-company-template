# 4. Projects

Per-project working context. Projects are time-bounded; products are ongoing.

## How to Add a Project

1. Copy `_template/` to a new numbered folder: `1-project-name/`
2. Fill in `1-scope.md` first — it defines everything else
3. Open a PR — project lead approves

## Structure per Project

```
4-projects/
  1-[project-name]/
    README.md
    1-scope.md           ← objectives, deliverables, out-of-scope
    2-plan.md            ← timeline, milestones, ownership
    3-decisions/         ← project-level ADRs
    4-meetings/          ← meeting notes
    5-retrospective.md   ← learnings when project closes
```

## Purpose for Agents

Load the relevant project folder for project-specific tasks. Scope and plan tell agents what's committed, what's not, and who owns what.
