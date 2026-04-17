# 3. Products

One subfolder per product. Each one tells agents what you build, how it works, and where it's going.

## Adding a product

Copy `_template/` into a numbered folder — `1-product-name/`. Fill in the files. Open a PR.

## What each product folder contains

```
1-[product-name]/
  README.md            what it is and who owns it
  1-overview.md        what it does, who it's for, current status
  2-architecture.md    how it's built
  3-decisions/         ADRs — architectural decisions
  4-roadmap.md         where it's going
  5-api.md             API reference (if applicable)
```

## For agents

Load the relevant product folder before any product-specific task. This tells you what exists, what decisions have been made, and what's in scope.
