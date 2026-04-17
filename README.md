# EACF Company Knowledge Base Template

> Powered by the [Enterprise AI Context Framework](https://abq.institute/eacf) by ABQ Institute.

## What is this?

A ready-to-use knowledge base template for any organisation adopting EACF. Clone this, fill in your details, and your AI agents have full context from day one.

**Includes:**
- ✅ Standard 5-layer KB structure (company, departments, products, projects, agents)
- ✅ Pre-populated template files with prompts for every section
- ✅ Universal AI skill files — auto-loaded by Claude Code, Cursor, Gemini, Copilot
- ✅ Guided setup: the agent walks you through `kb-config.yaml` automatically

## How to Use

1. Click **"Use this template"** on GitHub → create your org's KB repo
2. Clone the repo locally
3. Open your AI tool (Claude Code, Cursor, Gemini, Copilot) in the repo root
4. The agent detects this is an EACF KB and guides you through the 4-question setup
5. Fill in the template files with your organisation's actual content
6. Done — your KB is live and your agents have context

## Structure

| Folder | Purpose |
|--------|---------|
| `0-meta/` | KB configuration (`kb-config.yaml`) |
| `1-company/` | Organisation-wide: identity, structure, strategy, policies, brand |
| `2-departments/` | Per department: overview, processes, team, decisions |
| `3-products/` | Per product: specs, architecture, decisions, roadmap |
| `4-projects/` | Per project: scope, plan, decisions, meetings, retrospective |
| `5-agents/` | Agent configurations and skill assignments |

Each layer has a `_template/` subfolder — copy it to create a new department, product, or project.

## After Setup

Once `kb-config.yaml` is fully configured (no `# TODO` markers):
- Your agent switches from SETUP MODE to OPERATIONS MODE automatically
- Start filling in `1-company/` — begin with `1-identity/1-mission-vision.md`
- Copy `_template/` folders to create departments, products, and projects

## Learn More

- [EACF Documentation](https://abq.institute/eacf)
- [ABQ Institute](https://abq.institute)

---

*Template maintained by [ABQ Institute](https://abq.institute)*
