---
title: Cloud-run environment requires WebSearch-first for news sources
target_file: ~/.claude/CLAUDE.md
proposed_at: 2026-05-15
proposed_by: Morning_Brief_Orchestra (CEO)
status: pending_user_review
confidence: medium (2× confirmed across run-2 + run-3)
---

## Problem Observed

In cloud-run environments (Claude Code web-sessions), several key news sources return 403 Forbidden when accessed via WebFetch:
- `https://www.anthropic.com/news` → 403
- `https://status.claude.com/history.atom` → 403
- `https://hn.algolia.com/api/v1/search_by_date` → 403

WebSearch retrieves the same information reliably in cloud-run context.

## Evidence

- Run 2026-05-12 (run-2): "WebFetch 403 on multiple sources — Anthropic /news, status.claude.com/history.atom, HN Algolia all blocked via WebFetch in cloud run. WebSearch proved fully reliable substitute — recovered all major events."
- Run 2026-05-15 (run-3): Same 403 pattern confirmed. WebSearch-first strategy used successfully. Consistent across both cloud runs.

## Proposed Change

Add to `~/.claude/CLAUDE.md` Web-Access or Tool-Priority section:

```
## Cloud-Run Environment Note (Morning Brief + similar scheduled agents)

In cloud-run sessions (Claude Code web / scheduled /schedule runs):
- WebFetch is blocked (403) on certain domains: anthropic.com/news, status.claude.com, hn.algolia.com
- WebSearch is the reliable primary for news discovery in cloud context
- Pattern: if WebFetch returns 403 on a news URL, immediately fallback to WebSearch with the topic keywords
- This is NOT a temporary condition — it's a structural cloud-environment constraint
```

## Scope of Impact

Would inform any sub-agent running in cloud-session context about this tool-availability difference. Primarily affects Morning Brief Orchestra but would also benefit any scheduled agent that polls news sources.

## Why This Should Be Global vs. Local

Currently documented only in news_pulse_monitor_learnings.md (local sub-agent file). Since this applies to ANY agent running in cloud-session context — not just Morning Brief — it warrants a global note so future orchestras don't rediscover the same constraint. The observation is harness-behavior (tool availability in cloud vs local), not orchestra-specific.
