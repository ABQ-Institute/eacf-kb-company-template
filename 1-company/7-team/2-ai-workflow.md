# AI Workflow

**Status:** <!-- draft | approved -->
**Owner:** <!-- GitHub username -->
**Last reviewed:** <!-- YYYY-MM-DD -->
**Review cadence:** Quarterly

---

## Approved AI Tools

See `1-tools.md` for the full approved tool list.

## Data Classification for AI Use

| Classification | Definition | AI use permitted? |
|----------------|-----------|------------------|
| Public | Information already public | ✅ Yes |
| Internal | Non-sensitive internal information | ✅ Yes, with approved tools |
| Confidential | Client data, financials, PII | ⚠️ Only with approved enterprise tools |
| Restricted | Legal, regulatory, HR | ❌ No AI processing |

## Workflow Patterns

### How we use AI agents

<!-- Describe how the organisation uses AI agents in practice.
     e.g. "Agents read the KB for context before answering questions about our processes."
     e.g. "All code changes require human review regardless of AI assistance." -->

### What agents should always do

- Check this KB before answering questions about the organisation
- Respect data classification above
- Flag uncertainty rather than guess

### What agents should never do

- Access or process Restricted data
- Take external actions (send emails, make payments) without explicit human approval
- Override a human decision

## Notes for Agents

This document defines your operating boundaries. When uncertain about whether a task falls within approved scope, default to flagging and asking rather than proceeding.
