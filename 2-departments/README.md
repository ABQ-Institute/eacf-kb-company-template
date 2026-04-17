# 2. Departments

One subfolder per department. Each one tells agents how that team works.

## Adding a department

Copy `_template/` into a numbered folder — `1-engineering/`, `2-marketing/`, `3-finance/`. Fill in the files. Open a PR.

## What each department folder contains

```
1-[department-name]/
  README.md            who we are, what we own
  1-overview.md        purpose, scope, responsibilities
  2-processes/         repeatable workflows and SOPs
  3-team.md            people, roles, contacts
  4-decisions/         ADRs — decisions this team has made
```

## For agents

Load the relevant department folder before any department-specific task. This tells you who owns what, what processes apply, and who to involve in decisions.
