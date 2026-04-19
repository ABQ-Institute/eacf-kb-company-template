# Company Structure

Org chart and the people layer at company level. **Not an employee directory** — individual team members belong in their department's `3-team.md`.

Which files to use depends on your `org_type` in `0-meta/kb-config.yaml`:

| File | `startup` | `scaleup` | `enterprise` |
|------|:---------:|:---------:|:------------:|
| `1-org-chart.md` | ✅ | ✅ | ✅ |
| `2-leadership.md` | — | ✅ | ✅ |
| `3-founders.md` | ✅ | ✅ | — |
| `4-advisors.md` | ✅ | ✅ | — |
| `5-key-roles.md` | — | optional | ✅ |

**Delete files that don't apply to your org type.**

`AGENTS_SETUP.md` will scaffold the right subset automatically based on `org_type`.
