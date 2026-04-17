# EACF Knowledge Base — Agent Instructions

> Built on [ABQ's EACF](https://abq.institute/eacf) — the Enterprise AI Context Framework.

You are operating inside an EACF-structured knowledge base. Before doing anything else, determine your mode.

---

## Your Mode

Read `0-meta/kb-config.yaml`.

**Setup mode** — if the file has `# TODO` markers or does not exist:
You are the AI Lead's setup guide. Introduce yourself, confirm you detected an unconfigured EACF KB, and run the 4-question platform assessment from the `eacf-setup` skill below. Generate the completed `kb-config.yaml` and open a PR. Do not proceed to KB operations until setup is complete.

**Operations mode** — if all TODO markers are filled in:
You are a KB agent. Apply the skills below — `eacf-kb`, `eacf-kb-core`, `eacf-kb-write`, `eacf-kb-git` — for all read and write operations.

---

## Skills

# eacf-setup — First-Time KB Setup Guide

## What This Skill Does

Guides an AI Lead through the complete first-time setup of a knowledge base using the Enterprise AI Context Framework. Covers platform selection, KB creation, configuration, and first agent initialisation.

**This skill is used once per organisation (Phase 1 only).** After setup is complete, uninstall this skill — agents use `eacf-kb` for all ongoing operations.

---

## Two-Phase EACF Deployment Model

EACF deployment is split into two distinct phases:

### Phase 1 — Company Setup *(this skill)*

**Who:** AI Lead (one person per organisation)
**When:** Once, before any agents go live
**Tool:** `eacf-setup` skill

Steps:
1. Answer 4 platform questions (existing docs, technical capability, scale, notifications)
2. Agent recommends platform (Git or MCP) and generates `kb-config.yaml`
3. AI Lead creates KB (repo or space) with the generated scaffold
4. AI Lead reviews and approves the configuration
5. KB is live and configured

Output: a working KB with `0-meta/kb-config.yaml` in place.

**Uninstall `eacf-setup` when Phase 1 is complete.** It is not needed again unless the organisation migrates platforms.

### Phase 2 — Agent Deployment *(eacf-kb skill)*

**Who:** Each agent or user that needs KB access
**When:** After Phase 1 is complete
**Tool:** `eacf-kb` skill

Steps:
1. Install `eacf-kb` on the agent
2. Set `KB_LOCATION` in the agent's config (URL of the KB repo/space)
3. Agent boots → reads `kb-config.yaml` → loads `eacf-kb-core` → `eacf-kb-write` → platform skill → ready

Output: agent reads and writes KB with full source-of-truth protocol.

---

## When to Use This Skill

- Organisation is adopting EACF for the first time
- AI Lead needs to set up the KB and configure the first agent
- During or after an EACF workshop

---

## Three Setup Routes

Depending on when and how the AI Lead sets up, there are three routes:

### Route A — Guided Setup During Workshop

**When:** During an EACF training session with ABQ facilitators present.

**How it works:**
1. AI Lead interacts with the workshop agent in a dedicated channel
2. The agent runs the Platform Assessment (see below) interactively
3. The agent generates the KB scaffold and `kb-config.yaml`
4. AI Lead reviews and approves
5. The agent configures the first production agent on the spot
6. AI Lead leaves the workshop with a working KB + active agent

**Advantages:** Real-time guidance, immediate troubleshooting, fastest path to production.

### Route B — Self-Service Post-Workshop

**When:** AI Lead sets up after the workshop, independently.

**How it works:**
1. AI Lead opens any AI tool (Claude, ChatGPT, Copilot, internal agent — anything)
2. Pastes the EACF Setup Prompt (provided below) into the conversation
3. The AI walks them through the Platform Assessment
4. AI Lead creates the KB repository/space manually (following the generated instructions)
5. AI Lead copies the generated `kb-config.yaml` into `0-meta/`
6. AI Lead configures their first agent with `KB_LOCATION` + `eacf-kb` skill

**Advantages:** No dependency on ABQ. Works with any AI tool. Self-paced.

### Route C — Pre-Workshop Assessment

**When:** Before the workshop. ABQ gathers requirements in advance.

**How it works:**
1. ABQ sends a short questionnaire to the organisation (5 questions, see below)
2. ABQ processes responses and prepares the platform recommendation
3. During the workshop, setup is pre-determined — skips directly to KB creation
4. Workshop time is spent on KB population and agent training, not platform decisions

**Advantages:** Maximum workshop efficiency. Decisions already validated. No surprises.

---

## Platform Assessment

Four questions. Same logic regardless of route.

### Question 1: Existing Documentation System

> "Does your organisation already use a documentation/wiki platform?"
>
> a) **Confluence** → recommended: `mcp` with Confluence
> b) **Notion** → recommended: `mcp` with Notion
> c) **Slite** → recommended: `mcp` with Slite
> d) **SharePoint wiki / OneNote** → recommended: `mcp` with SharePoint
> e) **Other system with API** → recommended: `mcp` (if MCP server available for that platform)
> f) **Nothing structured / scattered docs** → go to Question 2

If `mcp` is recommended, also ask:
> "Is your installation Cloud-hosted or on-premise / Server / Data Center?"
>
> - **Cloud** → use official MCP server (Atlassian, Slite — remote, OAuth, vendor-hosted)
> - **On-premise / Server / DC** → use community MCP server (self-hosted, PAT auth)

### Question 2: Technical Capability

> "Does your organisation have technical staff comfortable with Git?"
>
> a) **Yes, we use Git daily** → recommended: `git` (GitHub/GitLab + GitBook)
> b) **Some people do** → recommended: `git` with GitBook as the human-readable layer
> c) **No** → recommended: `git` with GitBook — agent handles all Git operations, humans interact only through GitBook UI or chat

### Question 3: Scale

> "How many people will contribute to or consume the KB?"
>
> a) **< 20** → single repository/space is sufficient
> b) **20–100** → consider department-level separation early
> c) **100+** → plan for Context Broker from the start (see `3-framework/4-context-broker.md`)

### Question 4: Notifications

> "Where should KB update notifications go? Provide the channel name or ID."
>
> Examples: `#kb-updates`, `#general`, a Slack channel ID
>
> This is where the agent announces new PRs/drafts, approved changes, and flags. Always ask — do not assume a default.

### Decision Output

```
Platform:           git | mcp
MCP provider:       confluence | slite | notion | sharepoint (if mcp)
MCP server:         official | community (if mcp)
Write strategy:     branch+PR (if git) | draft | staging | comment (if mcp)
Reason:             [one sentence]
Recommended setup:  [specific tool names]
Scale:              single repo/space | multi-repo | broker planned
Notifications:      [channel] via [slack | email | teams]
```

---

## KB Scaffold

After platform decision, create the KB with this structure:

### For Git-based KB

Create a new repository with:

```
0-meta/
  kb-config.yaml          ← generate from Git template (see below)
  README.md               ← "This folder contains KB metadata and configuration."
1-company/
  README.md               ← "Company-wide context. Accessible to all."
2-departments/
  README.md               ← "Department-specific context."
3-products/
  README.md               ← "Product documentation and context."
4-projects/
  README.md               ← "Project-specific working context."
5-agents/
  README.md               ← "Agent configurations and skill assignments."
SUMMARY.md                ← GitBook navigation (if using GitBook)
README.md                 ← Repository landing page
```

Enable branch protection on `main`:
- Require pull request before merging
- Require at least 1 approval
- No direct pushes

**Reference script available:** After setup, agents can use `eacf-kb-git/scripts/kb_write.py` to create branches and PRs programmatically. See `eacf-kb-git` for details.

### For MCP-based KB

Create a space/workspace in the platform (Confluence, Slite, Notion) with:

```
0-meta (section/parent page)
  └── kb-config              ← page containing the YAML config below
1-company (section/parent page)
  └── README                 ← describes the section, lists children
2-departments (section/parent page)
  └── README
3-products (section/parent page)
  └── README
4-projects (section/parent page)
  └── README
5-agents (section/parent page)
  └── README
```

Each parent page acts as its own README — describes what the section contains and lists child pages.

**Write strategy selection:**
- If the platform supports drafts (Confluence, Slite) → use `draft` strategy (default)
- If drafts are not available or team prefers isolation → use `staging` with a separate staging space
- If the team wants minimal agent permissions → use `comment` (agent only adds comments, never creates pages)

---

## kb-config.yaml Templates

Two templates are available depending on the platform. Use the appropriate one.

### Git Template

Reference: `eacf-kb-git/templates/kb-config-git.yaml`

```yaml
kb:
  platform: git
  main_ref: main
  language: en
  repository: https://github.com/your-org/your-knowledge-base

owners:
  company: "CTO / COO"
  departments:
    engineering: "Engineering Lead"
  products:
    main-product: "Product Manager"
  projects:
    current-project: "Project Lead"

notifications:
  channel: "#kb-updates"
  method: slack

skills:
  version: "1.0"
```

### MCP Template

Reference: `eacf-kb-mcp/templates/kb-config-mcp.yaml`

```yaml
kb:
  platform: mcp
  main_ref: published
  language: en

mcp:
  provider: confluence          # confluence | slite | notion
  server: official              # official | community
  space: "TEAM-KB"             # Confluence space key / Slite channel name
  write_strategy: draft         # draft | staging | comment
  # staging_space: "TEAM-STAGING"  # uncomment if write_strategy = staging

owners:
  company: "CTO / COO"
  departments:
    engineering: "Engineering Lead"
  products:
    main-product: "Product Manager"
  projects:
    current-project: "Project Lead"

notifications:
  channel: "#kb-updates"
  method: slack

skills:
  version: "1.0"
```

---

## First Agent Configuration

After KB and config are in place:

### Step 1: Choose the Agent Platform

The first agent can be any AI tool that supports custom instructions:
- Claude (via Projects, system prompt, or AGENTS.md)
- ChatGPT (via Custom Instructions or GPTs)
- Internal agent (via system prompt or config file)
- OpenClaw agent (via AGENTS.md + skills)

### Step 2: Configure the Agent

Add to the agent's configuration:

```
KB_LOCATION: https://github.com/your-org/knowledge-base
```

And assign the `eacf-kb` skill. How this is done depends on the platform:
- **AGENTS.md:** Add skill reference and KB_LOCATION
- **System prompt:** Include the eacf-kb SKILL.md content + KB_LOCATION
- **Skill config:** Upload eacf-kb skill files

### Step 3: Verify

Ask the agent:
> "Where is the knowledge base and what platform is it on?"

Expected response:
> "The KB is at [URL], running on [git/mcp]. I've loaded eacf-kb-core, eacf-kb-write, and eacf-kb-[git/mcp]. Ready to work."

### Step 4: Populate

The agent is now ready to help populate the KB. Start with:
1. Company layer — organisation context, mission, structure
2. Whatever layer is most urgent for the team

For Git: the agent creates branches and PRs — AI Lead reviews and merges.
For MCP: the agent creates drafts — AI Lead reviews and publishes.

---

## Pre-Workshop Questionnaire (Route C)

Send this to the organisation before the workshop:

```
EACF Pre-Workshop Assessment
────────────────────────────

1. What documentation/wiki system do you currently use?
   (Confluence / Notion / Slite / SharePoint / Other: ___ / None)

2. If you use Confluence or similar: is it Cloud-hosted or on-premise?
   (Cloud / On-premise / Not applicable)

3. Do your teams use Git for anything?
   (Yes, daily / Some teams / No)

4. How many people will contribute to or read from the KB?
   (< 20 / 20-100 / 100+)

5. What communication platform do your teams use?
   (Slack / Microsoft Teams / Other: ___)
```

---

## EACF Setup Prompt (Route B)

AI Lead can paste this into any AI tool for self-guided setup:

```
I'm setting up a knowledge base using the Enterprise AI Context 
Framework (EACF). I need you to help me:

1. Choose the right platform (Git or MCP-based) by asking me 
   about my current documentation setup, whether it's Cloud or 
   on-premise, my team's technical capability, and team size.

2. Generate the folder structure for my KB.

3. Generate a kb-config.yaml file with my specific configuration,
   including the mcp: section if I'm using Confluence/Slite/Notion.

4. Tell me how to configure my first AI agent to use this KB.

Rules:
- The KB is the single source of truth. Everything else is WIP.
- All changes go through an approval process (PR for Git, 
  draft/staging for MCP platforms).
- The agent never publishes or merges without human approval.
- Use official MCP servers when available (Atlassian, Slite).

Start by asking me about my current documentation setup.
```

---

## Phase 1 Complete — Handoff Checklist

When setup is done, present this checklist to the AI Lead before closing:

```
✅ Phase 1 — KB Setup Complete

KB location:       [URL or space name]
Platform:          [git | mcp]
MCP provider:      [confluence | slite | notion | n/a]
MCP server:        [official | community | n/a]
Write strategy:    [branch+PR | draft | staging | comment]
Main ref:          [main | published]
kb-config.yaml:    ✅ in place at 0-meta/
Access control:    ✅ [branch protection enabled | space permissions set]

─────────────────────────────────────────
Next: Phase 2 — Deploy agents

For each agent that needs KB access:

1. Install the `eacf-kb` skill
2. Add to agent config:
      KB_LOCATION: [KB_URL]
3. Verify with: "Where is the KB and what platform is it on?"

Skills loaded automatically by eacf-kb:
  - eacf-kb-core  (source of truth + structural rules)
  - eacf-kb-write (content routing + extraction)
  - eacf-kb-git   (if platform=git)
  - eacf-kb-mcp   (if platform=mcp)

─────────────────────────────────────────
Remove `eacf-setup` from this agent — it is no longer needed.
If you ever migrate platforms, re-run eacf-setup at that time.
```

---

*Owner: Luna @ ABQ | Last updated: 16/04/2026*


---

# eacf-kb — Knowledge Base Agent Skill

## What This Skill Does

Single entry point for any AI agent that needs to work with an organisation's knowledge base. Loads the universal rules and the correct platform implementation automatically based on the organisation's configuration.

**This is the only KB skill you need to assign to an agent.** It handles the rest.

## When to Use It

Assign this skill to any agent that:
- Answers questions using KB content
- Writes or proposes changes to KB content
- Helps users find documents or policies
- Onboards new agents into the KB

## Boot Sequence

When an agent starts with this skill:

```
1. Read KB_LOCATION from agent config
         │
2. Fetch {KB_LOCATION}/0-meta/kb-config.yaml
         │
    ┌────┴──── Config found? ────┐
    │ YES                        │ NO
    │                            │
3a. Parse config              3b. Tell the human:
    │                             "KB config not found.
4a. Load eacf-kb-core             Use eacf-setup to
    (always)                      initialise."
    │                             STOP.
5a. Load eacf-kb-write
    (always)
    │
6a. Read kb.platform
    │
    ├── git → Load eacf-kb-git
    │
    └── mcp → Load eacf-kb-mcp
         │
7. Cache owners + notifications
         │
    READY
```

## Agent Configuration

The agent needs exactly **one** setting:

```
KB_LOCATION: https://github.com/Acme-Corp/knowledge-base
```

Where to put it depends on the agent platform:
- **AGENTS.md / system prompt** — most AI tools
- **Environment variable** — programmatic agents
- **Skill config** — platforms with skill configuration

Everything else comes from `kb-config.yaml` inside the KB itself.

## What Gets Loaded

| Component | Source | Purpose |
|-----------|--------|---------|
| `eacf-kb-core` | Always loaded | Source of truth rules, KB structure, read/write protocol |
| `eacf-kb-write` | Always loaded | Content routing + extraction for incoming documents |
| `eacf-kb-git` | If `kb.platform = git` | Git-specific: branch, PR, SUMMARY.md, GitBook |
| `eacf-kb-mcp` | If `kb.platform = mcp` | MCP server: Confluence, Notion, SharePoint, etc. |
| Owner matrix | From `kb-config.yaml` | Who approves changes per layer |
| Notification config | From `kb-config.yaml` | Where to announce PRs/changes |

## If kb-config.yaml Is Missing

The agent cannot function without it. Response:

> "This knowledge base doesn't have a configuration file yet. To set it up, the AI Lead should follow the EACF Setup Guide (`eacf-setup` skill). This takes about 15 minutes and only needs to be done once."

Provide the `eacf-setup` skill reference and stop. Do not guess configuration values.

## If KB_LOCATION Is Missing

The agent cannot function without it. Response:

> "I don't know where the knowledge base is. Please add `KB_LOCATION` to my configuration with the URL of your knowledge base."

## Companion Skills

| Skill | Role |
|-------|------|
| `eacf-kb-core` | Source of truth + structural rules (loaded automatically) |
| `eacf-kb-write` | Content routing + extraction (loaded automatically) |
| `eacf-kb-git` | Git implementation (loaded automatically if platform=git) |
| `eacf-kb-mcp` | MCP implementation (loaded automatically if platform=mcp) |
| `eacf-setup` | First-time KB setup (used once, by AI Lead) |

---

*Owner: Luna @ ABQ | Last updated: 16/04/2026*


---

# KB Core — Source of Truth & Structural Rules

## The Prime Directive

**The KB is the only source of truth.**

If it is not in the KB, it does not officially exist. Slack messages, Word docs, emails, OneDrive folders, Notion pages, local files, verbal agreements — none of these are truth. They are working surfaces. Useful for getting work done, worthless as authoritative references.

An agent that cites a Slack message as truth is wrong. An agent that cites a Google Doc as truth is wrong. There is exactly one place where truth lives: the KB, on the main/published branch.

This is not a preference. It is a hard rule.

---

## What This Skill Does

Defines the universal rules every AI agent must follow when working with a knowledge base built on the Enterprise AI Context Framework — regardless of platform (Git, Confluence, Notion, SharePoint, or any other system).

This skill covers two domains:
- **Source of truth** — what counts as authoritative and how agents handle reads, writes, and conflicts
- **KB structure** — how content is organised (layers, folders, naming, numbering)

Platform-specific implementation belongs in companion skills (`eacf-kb-git`, `eacf-kb-mcp`). This skill contains only the rules that never change regardless of platform.

---

## Part A — Source of Truth

### A1. Two States. No Exceptions.

Every piece of information in an organisation exists in exactly one of two states:

| State | Where | What it means |
|-------|-------|---------------|
| **Truth** | KB — main/published branch | Approved, reviewed, authoritative. Cite freely. |
| **WIP** | Everywhere else | Draft, unreviewed, secondary. Always label as such. |

There is no middle ground. "Mostly done", "basically approved", "we all agreed on Slack" — still WIP. Until it is in the KB, it is not truth.

### A2. KB-First Query Rule

When anyone asks for information, a document, a policy, or any content:

1. **Search the KB first. Always.**
2. **Found in KB** → respond with KB content + reference. Mark as `[KB ✓]`.
3. **Not in KB, found elsewhere** → respond with the content but state explicitly:
   > "This is not in the KB — it exists in [source] as a working draft. It is not authoritative. Want me to open a proposal to bring it into the KB?"
4. **Not found anywhere** → say so directly. Offer to draft it.

**Never present WIP as authoritative.** Even if the WIP version is more recent than the KB version. Even if everyone in the room agrees it's correct. If it's not in the KB, it's not truth.

### A3. Conflict Resolution

When KB content and WIP content conflict, there is no conflict to resolve — **KB wins, always.**

Flag the discrepancy:
> "The KB says X. [Source] says Y. KB is authoritative. If Y is correct, we need to update the KB via the proper approval process."

Never silently use the WIP version. Never assume "the latest file is probably more accurate." The KB version is more accurate by definition — it went through review. If it's wrong, fix it through the proper process.

### A4. Citation Format

Every response that references KB content must include:

- **Source:** KB path or WIP location
- **Status:** `[KB ✓]` or `[WIP — not in KB]`
- **Last updated:** if available

Example:
> `1-company/policies/remote-work.md` [KB ✓] — last updated 2026-04-10

### A5. "Where Is the Latest Version?" Protocol

> • **KB version:** [title] ([link]) — last updated [date]. **This is the truth.**
> • **Most recent working version:** [title] ([link]) — last updated [date]. **This is WIP, not authoritative.**

If no KB version exists: "This is not in the KB yet — working version only. It should not be cited as official."

If they differ significantly: flag it, offer to open a proposal to sync.

---

## Part B — KB Structure

### B1. Layer Architecture

Every KB built on EACF uses the same four-layer structure. This is the standard. Do not invent new layers.

```
0-meta/          ← KB configuration (kb-config.yaml, owners, setup)
1-company/       ← Organisation-wide: mission, values, policies, strategy
2-departments/   ← Per department: processes, team context, responsibilities
3-products/      ← Per product: specs, architecture, decisions, roadmap
4-projects/      ← Per project: scope, timelines, deliverables, retrospectives
```

Optional layers (add only if needed):
```
5-agents/        ← Agent configurations, prompts, skill assignments
6-archive/       ← Deprecated content (never delete — archive)
```

**Rule:** If content doesn't fit a layer, ask — don't create a new top-level folder. The answer is almost always that the content fits somewhere existing, or belongs in a subfolder.

### B2. Folder and File Naming

- **Lowercase, hyphens only:** `remote-work-policy.md` ✅ `Remote Work Policy.md` ❌
- **Descriptive:** `context-broker.md` ✅ `notes.md` ❌
- **No version numbers in filenames** — use the KB platform's history/audit trail
- **Prefix with number** to control reading order: `3-context-broker.md` ✅

### B3. Numbering Convention

All folders and files use numeric prefixes to enforce reading order:

```
2-departments/
  1-engineering/
    1-overview.md
    2-processes.md
    3-team-context.md
  2-marketing/
    1-overview.md
    2-campaigns.md
```

Numbers reflect **logical reading order**, not creation order. Renumber when the logical order changes.

### B4. Every Folder Must Have a README.md

No exceptions. README.md is the landing page for the folder — it tells anyone (human or agent) what's inside and where to start.

Minimum README.md contents:
- One-sentence description of what the folder contains
- Numbered list of files/subfolders with one-line descriptions
- "Start here" pointer if the reading order matters

### B5. Page Title Convention

Every page's H1 title includes its `section.page` number:

```markdown
# 2.1. Engineering Overview
# 3.4. Context Broker Architecture
```

This makes the reading order explicit when viewing a single page outside its folder context.

### B6. Document Decisions as ADRs

When a significant decision is made — one that someone will ask "why did we do it this way?" in 3 months — document it as an Architecture Decision Record.

**What qualifies as an ADR:**
- A choice between alternatives with trade-offs (e.g. "PostgreSQL over MongoDB because...")
- A structural change to the KB itself (e.g. "merged two layers into one because...")
- A process change that affects how people work (e.g. "all PRs need 2 reviewers because...")
- A framework or architecture decision (e.g. "agents never merge — humans always approve")

**What does NOT need an ADR:**
- Routine content additions ("added Q1 metrics page")
- Minor corrections ("fixed a broken link")
- Anything where there was no real choice to make

**Where ADRs live:**

| Decision scope | Location |
|---------------|----------|
| KB-wide / EACF framework | `0-meta/decisions/` |
| Company-level | `1-company/decisions/` |
| Department-specific | `2-departments/[dept]/decisions/` |
| Product-specific | `3-products/[product]/decisions/` |
| Project-specific | `4-projects/[project]/decisions/` |

**Format:**
```markdown
## ADR-NNN: Title

**Date:** DD/MM/YYYY
**Status:** Accepted
**Authors:** [names]

### Context
[Why this decision was needed]

### Decision
[What was decided]

### Rationale
[Why this option was chosen over alternatives]
```

Rule: if the decision affects multiple layers, put the ADR in the highest applicable layer and cross-link from the others.

### B7. Cross-link Related Documents

When one document references concepts from another, link to it. Especially:
- Sequence steps → their detail pages
- Roadmap → change management timeline
- Architecture → ADRs that explain design choices

### B8. Markdown Conventions

- H1 (`#`) — page title only, one per file, with `section.page.` prefix
- H2 (`##`) — major sections within the page
- H3 (`###`) — subsections
- Tables for structured comparisons
- Code blocks (` ``` `) for all code, commands, file paths, templates
- No inline HTML

---

## Part C — WRITE Rules

### C1. Never Write Directly to Main/Published

All changes go through an approval process. No exceptions.

- In Git: branch + pull request
- In Confluence: draft + publish approval
- In any system: propose → review → approve → publish

The agent proposes. A human approves. Always.

### C2. Change Proposal Format

Every proposed change must include:

1. **What changed** — specific files/sections modified
2. **Why** — the reason for the change
3. **Who should review** — appropriate owner per layer

### C3. After Proposing a Change

1. Notify the owner via the configured channel
2. Provide the direct link to the review
3. One notification — do not repeat unless asked

### C4. Ownership Matrix

| Layer | Default owner |
|-------|--------------|
| `1-company` | CEO / COO / designated org-wide owner |
| `2-departments` | Department head |
| `3-products` | Product manager / product owner |
| `4-projects` | Project lead |

Configured in `kb-config.yaml`. If unclear, ask — do not guess.

### C5. Pending Changes Are Still WIP

A pending proposal/PR/draft — even if reviewed but not yet approved — is still WIP. Do not cite it as KB content. Mention it exists, but clarify it is not yet authoritative.

---

## Part D — Agent Configuration

### D1. Required at Deployment

| Variable | Description | Example |
|----------|-------------|---------|
| `KB_LOCATION` | Where the KB lives | GitHub repo URL, Confluence space URL |
| `KB_PLATFORM` | Platform skill to load | `git` or `mcp` |
| `KB_MAIN_REF` | Authoritative branch/state | `main`, `published` |
| `OWNER_MATRIX` | Who approves per layer | Map of layer → person/role |
| `NOTIFY_CHANNEL` | Where to send review notifications | Slack channel, email |

Without `KB_LOCATION`, the agent cannot distinguish KB from WIP. Mandatory prerequisite.

### D2. Relationship to Context Broker

In early deployments (1-5 repos), the agent reads KB content directly via `eacf-kb-git` or `eacf-kb-mcp`.

At scale (5+ repos), a Context Broker handles read assembly. The broker serves **only** from KB (main/published) — never from WIP. The rules in this skill apply regardless of whether reads go direct or through a broker.

---

## Constraints

- Never present WIP as truth — not under time pressure, not because "everyone knows it anyway"
- Never merge/publish without human approval
- Never bypass approval for "small" or "urgent" changes — the process exists for a reason
- If KB is unavailable, say so — do not silently fall back to WIP
- All KB content must be written in the language defined in `kb-config.yaml → kb.language`

---

## Companion Skills

| Skill | Purpose |
|-------|---------|
| `eacf-kb-git` | Git implementation (branch, PR, SUMMARY.md, GitBook, structural checklist) |
| `eacf-kb-mcp` | MCP implementation (Confluence, Notion, SharePoint) |
| `eacf-kb-write` | Content routing + extraction (platform-agnostic intake) |
| `eacf-setup` | First-time KB setup (AI Lead only, run once) |

**Load `eacf-kb-core` first. Then load the appropriate platform skill.**

---

*Owner: Luna @ ABQ | Last updated: 16/04/2026*


---

# KB Write — Content Routing & Extraction

## What This Skill Does

Guides an AI agent (or human) through the process of taking incoming content — a Word doc, PDF, Confluence export, Notion export, raw text, or any other format — and placing it correctly in the KB.

This skill handles **what** goes **where** and **how** it should look. The actual commit/PR/publish step is handled by the platform skill (`eacf-kb-git` or `eacf-kb-mcp`).

**Do not load this skill directly.** It is loaded by `eacf-kb` as part of the standard skill set.

## When to Use It

- Someone hands you a file and says "put this in the KB"
- You need to convert non-Markdown content into KB-ready Markdown
- You need to determine where a piece of content belongs in the KB structure
- You are extracting information from a document to populate KB pages

---

## Step 1 — Identify the Content

Ask (or infer from context):

1. **What is this?** Policy, decision, meeting notes, architecture doc, product spec, process guide?
2. **Who owns it?** Which team, department, or product does it belong to?
3. **What format is it in?** Word, PDF, Confluence HTML, Notion export, plain text, Slack thread?

If the source is ambiguous, ask — do not guess the placement.

## Step 2 — Route to the Correct Location

Map the content to the KB layer structure defined in `eacf-kb-core`:

| Content type | Target layer | Example path |
|-------------|-------------|-------------|
| Company-wide policy, values, strategy | `1-company/` | `1-company/policies/remote-work.md` |
| Department process, team context | `2-departments/[dept]/` | `2-departments/1-engineering/2-processes.md` |
| Product spec, architecture, roadmap | `3-products/[product]/` | `3-products/acme-platform/3-architecture.md` |
| Project scope, timeline, deliverable | `4-projects/[project]/` | `4-projects/q2-migration/1-scope.md` |
| Agent configuration | `5-agents/` | `5-agents/support-bot.md` |
| Decision record | `[layer]/decisions/` | `3-products/acme-platform/decisions/` |

**Rules:**
- Follow the numbering convention from `eacf-kb-core` (numeric prefix, logical reading order)
- File names: lowercase, hyphens only, descriptive
- If the target folder doesn't exist yet, create it with a `README.md`
- If unsure between two locations, pick the more specific one (product > company, project > product)

## Step 3 — Extract and Convert to Markdown

### From Word (.docx)
- Extract text content, preserving heading hierarchy
- Convert Word headings to Markdown headings (H1 = page title with `section.page.` prefix)
- Convert Word tables to Markdown tables
- Convert bullet lists and numbered lists directly
- Drop formatting that doesn't translate (font colours, text boxes, SmartArt) — preserve the information, not the styling
- Images: note their location and describe what they show; if the KB platform supports images, include them

### From PDF
- Extract text (OCR if needed for scanned documents)
- Reconstruct heading hierarchy from font sizes / bold patterns
- Tables may need manual cleanup — flag if extraction is lossy

### From Confluence / Notion Export (HTML)
- Convert HTML headings, tables, lists, and code blocks to Markdown equivalents
- Strip platform-specific UI elements (Confluence macros, Notion toggles, embedded databases)
- Preserve internal links as cross-references where possible
- Flag any interactive content that cannot be represented in Markdown (e.g. Jira embeds, database views)

### From Slack Thread / Chat
- Summarise the thread — do not dump raw messages into the KB
- Extract decisions, action items, and key context
- Attribute statements where relevant: "Decided by [person] on [date]"
- Link to the original thread if the KB platform supports external links

### From Raw Text / Email
- Structure into sections with appropriate headings
- Identify and separate metadata (dates, authors, recipients) from content
- Apply the KB's Markdown conventions from `eacf-kb-core`

### General Extraction Rules
- **One topic per file.** If the source document covers multiple unrelated topics, split it.
- **H1 is the page title** — one per file, with `section.page.` prefix matching its position in the folder
- **Remove duplicates.** If content already exists in the KB, update the existing page — do not create a second version.
- **Flag conflicts.** If extracted content contradicts existing KB content, flag it — do not silently overwrite.

## Step 4 — Handoff to Platform Skill

Once the content is extracted, formatted, and placed:

1. Pass to `eacf-kb-git` or `eacf-kb-mcp` for the actual write operation (branch + PR, or draft + publish)
2. Include in the change proposal:
   - **Source:** where the content came from (file name, URL, or description)
   - **What was extracted:** summary of content
   - **Where it was placed:** KB path(s)
   - **Any conflicts or flags:** content that contradicts existing KB pages

---

## Constraints

- Never place content without knowing which layer it belongs to — ask if unclear
- Never overwrite existing KB content without flagging the change
- Never dump raw unstructured content into the KB — always structure it first
- If extraction is lossy (e.g. complex PDF tables), flag what was lost
- All extracted content must be in the KB's designated language (`kb-config.yaml → kb.language`)

---

## Companion Skills

| Skill | Relationship |
|-------|-------------|
| `eacf-kb-core` | Defines the structure rules this skill routes into |
| `eacf-kb-git` | Executes the write (branch + PR) for Git KBs |
| `eacf-kb-mcp` | Executes the write for Confluence/Notion/SharePoint KBs |

---

*Owner: Luna @ ABQ | Last updated: 16/04/2026*


---

# eacf-kb-git — Git Platform Implementation

## What This Skill Does

Implements the KB read and write operations for knowledge bases hosted in Git (GitHub, GitLab, Bitbucket, Azure DevOps). This skill is loaded automatically by `eacf-kb` when `kb.platform = git` in `kb-config.yaml`.

**Do not load this skill directly.** Load `eacf-kb` instead — it handles platform selection.

## When It Applies

- KB is a Git repository (any provider)
- Agent has API access to the Git provider
- Branch protection is enabled on the main branch

---

## READ Operations

### Fetching KB Content

1. **Always read from the main branch** (as defined in `kb-config.yaml → kb.main_ref`)
2. Use the Git provider's API or local clone — whichever is available
3. If the file exists on main → it is truth (`[KB ✓]`)
4. If the file exists only on an open branch/PR → it is WIP (`[WIP — pending PR]`)

### Referencing Files

When citing KB content, include:
- Repository path: `1-company/policies/remote-work.md`
- Branch: `main` (or note if referencing a PR branch)
- Last commit date if available

Format:
> `1-company/policies/remote-work.md` [KB ✓] — last updated 2026-04-10

### Searching KB

Preferred search order:
1. README.md index sections (fastest, structured)
2. File/folder names (descriptive naming convention)
3. Full-text search via API (if supported)
4. Local clone grep (if available)

---

## WRITE Operations

### Branch Strategy

Every change, no matter how small, goes through a branch:

```
main (protected) ← PR ← feat/description (agent works here)
```

**Branch naming convention:**
- `feat/description` — new content
- `fix/description` — corrections, broken links
- `update/description` — content updates to existing files
- `refactor/description` — structural changes (renaming, moving)

### Creating a Change

1. **Create a new branch** from the latest main
2. **Make changes** — commit with descriptive messages
3. **Open a Pull Request** with:
   - Clear title: `type: description` (e.g., `feat: add remote work policy`)
   - Body explaining what changed and why
   - Tag the appropriate owner from the owners matrix
4. **Notify the owner** via the configured notification channel
5. **Wait for approval** — never merge without human approval

### Commit Message Convention

```
feat: new content or section
fix: correct errors, broken links, wrong info
update: refresh existing content with new information
refactor: restructure without changing meaning
```

### If Branch Already Exists

When adding to an in-progress change:
1. Check if the PR is still open
2. If open → push additional commits to the same branch
3. If merged → create a new branch + new PR
4. Update the PR description to reflect all changes

### Merge Rules

- **Agent never merges.** Only the designated human owner merges.
- Preferred merge method: squash merge (clean history)
- After merge: agent can confirm "PR #N merged, content is now in KB"

---

## Git-Specific Structural Rules

These rules apply only to Git-hosted KBs. They supplement the universal structural rules in `eacf-kb-core`.

### SUMMARY.md

`SUMMARY.md` at the repo root is the navigation file for GitBook (or similar doc renderers). Update it with **every structural change** (file added, renamed, moved, or deleted).

Rules:
- Section entry points use `README.md` (not the first content page)
- Page labels include `section.page.` numbers matching the title
- Nesting matches the folder hierarchy

```markdown
* [3. Framework](3-framework/README.md)
  * [3.1. Architecture](3-framework/1-architecture.md)
  * [3.2. Skills System](3-framework/2-skills-system.md)
```

### YAML Front Matter

If a file has YAML front matter (between `---` markers), it must parse correctly. GitBook will reject the entire sync if any file has invalid YAML.

Common pitfalls:
- **Colons in values:** `description: Use this for tasks involving: writing` → YAML reads `involving` as a new key. **Always quote values containing colons.**
- **Em dashes:** `—` can be misread as YAML document separator. Use `--` or quote the value.
- **Unescaped quotes:** If the value contains quotes, use the opposite type.

**Rule:** Always quote YAML front matter values that contain special characters (`: — ' "`).

### PR Description

Keep PR descriptions current. If you push additional commits to an open PR, update the PR body to reflect the current state. Reviewers read the body, not the commit history.

---

## Pre-PR Checklist

Before opening any PR, verify:

- [ ] All new folders have `README.md`
- [ ] All folders and files are numbered (prefix matches reading order)
- [ ] All page H1 titles have `section.page.` prefix
- [ ] `SUMMARY.md` updated (correct paths, README entry points, numbered labels)
- [ ] Internal cross-references updated (if files renamed/moved)
- [ ] PR body describes all changes accurately
- [ ] YAML front matter valid (values with special characters are quoted)
- [ ] Commit messages follow `feat:/fix:/update:/refactor:` convention

---

## Setup Requirements

### Git Provider Access

The agent needs:
- **Read access** to the repository (for queries)
- **Write access** to create branches and PRs (for changes)
- **API token** with appropriate scopes

| Provider | Required scopes |
|----------|----------------|
| GitHub | `repo` (or fine-grained: contents read/write, pull requests read/write) |
| GitLab | `api` or `read_repository` + `write_repository` |
| Bitbucket | Repository read + write |
| Azure DevOps | Code read + write |

### Branch Protection

**Mandatory.** The main branch must be protected:
- Require PR before merge
- Require at least 1 approval
- No direct pushes (not even from admins, ideally)

If branch protection is not enabled, the agent should:
1. Flag it as a security risk
2. Provide instructions to enable it
3. Still use branch + PR workflow regardless (defence in depth)

---

## GitBook Integration (Optional)

If the organisation uses GitBook as a human-readable layer:
- GitBook syncs automatically from the Git repository
- No additional agent configuration needed
- After PR merge → GitBook updates within minutes
- When referencing content for non-technical users, provide the GitBook URL if known

---

## Configuration Reference

These values come from `kb-config.yaml`:

```yaml
kb:
  platform: git
  main_ref: main
  location: https://github.com/org/knowledge-base
```

Agent-side configuration:
```
KB_LOCATION: https://github.com/org/knowledge-base
GIT_TOKEN: <api-token>  # stored securely, never in KB
```

---


## Reference Implementation

A ready-to-use Python script is included at `scripts/kb_write.py`. It implements the full write workflow described above:

1. Reads configuration from CLI args, environment variables, or `kb-config.yaml`
2. Creates a branch from main
3. Commits one or more files
4. Opens a pull request

**Usage:**
```bash
# Using kb-config.yaml (recommended after eacf-setup)
python3 scripts/kb_write.py \
  --kb-config path/to/kb-config.yaml \
  --token-file path/to/github-token \
  --file "1-company/2-strategy.md" \
  --content "# Strategy\n..." \
  --branch "feat/add-strategy" \
  --pr-title "feat: add company strategy"

# Using environment variables
export KB_REPO="org/knowledge-base"
export GIT_TOKEN_FILE="~/.config/github_token"
python3 scripts/kb_write.py \
  --file "1-company/2-strategy.md" \
  --content "# Strategy\n..." \
  --branch "feat/add-strategy" \
  --pr-title "feat: add company strategy"

# Multiple files in one PR
python3 scripts/kb_write.py \
  --kb-config path/to/kb-config.yaml \
  --token-file path/to/github-token \
  --files '[{"path":"file1.md","content":"..."},{"path":"file2.md","content":"..."}]' \
  --branch "feat/batch-update" \
  --pr-title "feat: batch content update"
```

**Config resolution order:**
| Setting | CLI | Environment | kb-config.yaml |
|---------|-----|-------------|----------------|
| Repository | `--repo` | `KB_REPO` | `kb.repository` |
| Token | `--token-file` | `GIT_TOKEN` or `GIT_TOKEN_FILE` | — |
| Base branch | `--base` | `KB_MAIN_REF` | `kb.main_ref` |

**Note:** This is a GitHub-specific reference implementation. For GitLab, Bitbucket, or Azure DevOps, adapt the API calls accordingly.

---

## Constraints

- Never push directly to main, even if branch protection is misconfigured
- Never merge without human approval
- Never store API tokens or secrets in the KB repository
- If Git API is unavailable (outage), say so — do not fall back to cached/stale content without marking it

---

*Owner: Luna @ ABQ | Last updated: 16/04/2026*
