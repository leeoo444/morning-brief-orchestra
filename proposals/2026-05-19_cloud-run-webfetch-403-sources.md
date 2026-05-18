---
title: Cloud run mode — WebFetch 403 on key monitoring sources
target_file: ~/.claude/CLAUDE.md
proposed_at: 2026-05-19
proposed_by: Morning_Brief_Orchestra (CEO)
status: pending_user_review
confidence: high (3× confirmed across all cloud runs run-1/run-2/run-3)
---

## Problem Observed

In all 3 cloud daily runs (2026-05-10, 2026-05-12, 2026-05-19), WebFetch returns HTTP 403 Forbidden for several key monitoring sources:
- `https://www.anthropic.com/news` (Anthropic news page)
- `https://status.claude.com/history.atom` (Anthropic status)
- `https://hn.algolia.com/api/v1/search_by_date?...` (HN Algolia API)

WebSearch reliably recovers the same information. scrapling tools are unavailable in cloud run contexts.

## Evidence

- Run 2026-05-10 (run-1): Anthropic News 403, HN Algolia blocked → WebSearch workaround
- Run 2026-05-12 (run-2): Same 403 pattern confirmed for all three sources
- Run 2026-05-19 (run-3): Pattern confirmed 3rd consecutive cloud run; WebSearch primary recovered all news

## Proposed Change

Add to `~/.claude/CLAUDE.md` under a "Cloud Run Environment Notes" section:

```
### Cloud Run Environment Notes
- WebFetch is blocked (403) for: Anthropic /news, status.claude.com, HN Algolia API — use WebSearch as primary source
- scrapling/stealthy_fetch tools unavailable in cloud context — Reddit, X/nitter not fetchable
- GitHub API unauthenticated in cloud — per-repo metadata (contributors, closed-issues) rate-limited after ~15 calls
- Recovery: WebSearch provides reliable fallback for major news sources in cloud context
```

## Scope of Impact

This would document cloud-run-specific tool constraints for future Orchestras building cloud-daily-run pipelines. Useful for any Agent Orchestra that fetches web content in cloud-scheduled runs.

## Why This Should Be Global vs. Local

The constraint is environmental (cloud sandbox), not specific to Morning_Brief_Orchestra. Any orchestra deployed in cloud context will encounter the same 403 pattern. A global note prevents each new orchestra from rediscovering this independently.
