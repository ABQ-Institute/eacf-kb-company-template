# 3. Products

Per-product knowledge. Each product gets its own subfolder.

## How to Add a Product

1. Copy `_template/` to a new numbered folder: `1-product-name/`
2. Fill in the template files
3. Open a PR — product owner approves

## Structure per Product

```
3-products/
  1-[product-name]/
    README.md
    1-overview.md        ← what it is, who it's for, current status
    2-architecture.md    ← how it's built
    3-decisions/         ← ADRs
    4-roadmap.md         ← where it's going
    5-api.md             ← API reference (if applicable)
```

## Purpose for Agents

Load the relevant product folder for product-specific tasks. Product context tells agents what exists, how it works, what decisions have been made, and where it's going.
