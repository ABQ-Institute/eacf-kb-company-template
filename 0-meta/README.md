# 0-meta — KB Configuration & Standards

| File | Content |
|------|---------|
| `kb-config.yaml` | KB identity, repo, structure, agent settings |
| `kb-document-template.md` | Standard template for all KB documents |
| `kb-audit.md` | Tracked discrepancies between KB and reality |

---

## Data Status Convention

Every KB document should declare the reliability of its content using the `data_status` field.

### Front matter (machine-readable)

```yaml
---
data_status: verified   # confirmed from official/public sources
---
```

### Inline callout (human-readable, at top of document)

```markdown
> ✅ DATA STATUS: VERIFIED — sourced from [source], [date].

> ⚠️ DATA STATUS: PLAUSIBLE — NOT VERIFIED — realistic assumption; confirm with [person] during workshop.

> 🎭 DATA STATUS: DEMO ONLY — fabricated for demo/testing purposes; replace with real content.
```

### Inline `[DEMO]` or `[UNVERIFIED]` tags

Use on specific lines within a document to flag individual pieces of fabricated or unconfirmed data:

```markdown
- Repo URL: `https://github.com/your-org/your-repo`  `[DEMO — replace with real URL]`
- Slack channel: `#team-dev`  `[UNVERIFIED — confirm channel name]`
```

### Status definitions

| Status | Meaning | Action needed |
|--------|---------|---------------|
| `verified` | Confirmed from official sources, internal systems, or direct input from the team | None — keep updated |
| `plausible` | Realistic assumption based on available context; not directly confirmed | Verify with document owner during or after workshop |
| `demo-only` | Fabricated specifically for demo or testing purposes; not real | Replace with real content before production use |

### When to use each

- **New KB built during workshop:** most documents start as `plausible` — upgrade to `verified` as the team confirms content
- **Demo repos (e.g. for Before/After demos):** use `demo-only` for fabricated technical details
- **Pre-seeded content from public sources:** use `verified` with a source citation
