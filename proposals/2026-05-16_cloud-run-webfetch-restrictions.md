---
title: Cloud-run WebFetch restrictions — document and route around in sub-agents
target_file: ~/.claude/CLAUDE.md or cloud-run-runbook
proposed_at: 2026-05-16
proposed_by: Morning_Brief_Orchestra (CEO)
status: pending_user_review
confidence: high (2× confirmed across cloud runs 2026-05-12 and 2026-05-16)
---

## Problem Observed

In cloud-run environments (Claude Code on the Web / remote sessions), WebFetch returns HTTP 403 on several key sources that work fine in local sessions:
- `https://hn.algolia.com/api/v1/search_by_date` (HN news API)
- `https://status.claude.com/history.atom` (Anthropic status)
- `https://www.anthropic.com/news` (Anthropic news page)

WebSearch is a fully reliable substitute and recovers all major news items.

## Evidence

- Run 2026-05-12 (run-2): WebFetch 403 on Anthropic /news, status.claude.com/history.atom, HN Algolia. Switched to WebSearch, recovered all major events (Claude Code v2.1.139, CwC 2026, higher limits, SpaceX deal, Cursor 3.3, Zed 1.0).
- Run 2026-05-16 (run-3): WebFetch 403 on HN Algolia again (confirmed). WebSearch substituted successfully. news_pulse_monitor.md already updated with cloud-run note (3× confirmed local to that sub-agent).

## Proposed Change

Add a note to global CLAUDE.md or a dedicated cloud-run runbook:

```
## Cloud Run Tool Availability

When Claude Code runs in a managed cloud/web environment:
- WebFetch may return HTTP 403 on external APIs (HN Algolia, some Anthropic URLs, news feeds)
- WebSearch is the reliable substitute for news discovery
- mcp__scrapling__* tools may be unavailable
- GitHub API unauthenticated rate limit applies (10 search calls/hour per IP)

Sub-agents should: check tool availability at run start, default to WebSearch-first for community sources, skip scrapling-dependent sources gracefully.
```

## Scope of Impact

Affects any sub-agent or orchestra that uses WebFetch for external news/API sources in cloud-run sessions. The Morning Brief Orchestra already has this documented at the sub-agent level (news_pulse_monitor.md updated 2026-05-16). Global note would benefit other orchestras and future sub-agents.

## Why This Should Be Global vs. Local

Cloud-run tool restrictions are a harness-level constraint, not specific to Morning Brief Orchestra. Any orchestra (Trading, YouTube, etc.) that uses WebFetch for external sources in cloud sessions will hit the same issue. Global documentation prevents re-discovery of this pattern in each new orchestra.
