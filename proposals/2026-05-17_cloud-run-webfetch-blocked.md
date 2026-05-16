---
title: Cloud-Run WebFetch Blocked — Add Cloud-Run Runbook Note to Global CLAUDE.md
target_file: ~/.claude/CLAUDE.md
proposed_at: 2026-05-17
proposed_by: Morning_Brief_Orchestra (CEO)
status: pending_user_review
confidence: high (2× confirmed across run-2 + run-3)
---

## Problem Observed

In cloud-run environments (Claude Code on the web / remote container), `WebFetch` consistently returns HTTP 403 Forbidden for a predictable set of high-value sources:
- `https://www.anthropic.com/news` (Anthropic blog)
- `https://status.claude.com/history.atom` (Anthropic status feed)
- `https://hn.algolia.com/api/v1/search_by_date?...` (HackerNews Algolia API)
- `https://www.reddit.com/r/...` (Reddit — blocked by Anthropic ToS)

Additionally, `mcp__scrapling__stealthy_fetch` is unavailable in cloud-run environments, removing the Reddit fallback.

`WebSearch` is fully functional and recovers all major news items reliably. GitHub Atom feeds (releases.atom) work via `WebFetch`.

## Evidence

- Run 2026-05-12 (run-2): WebFetch 403 on Anthropic /news, status.claude.com/history.atom, HN Algolia. WebSearch used as primary, recovered all major news (Claude Code v2.1.139, CwC 2026, higher limits, Akamai deal, Cursor 3.3, Zed 1.0). Logged in ceo/learnings.md as 1× obs.
- Run 2026-05-17 (run-3): Same WebFetch 403 pattern on all same sources confirmed. WebSearch recovered all major news (Claude Code v2.1.143, Salesforce $300M, Gates Foundation $200M, PwC expansion, legal plugins, AGI 2028 paper). Confirmed as stable infrastructure constraint.

## Proposed Change

Add to `~/.claude/CLAUDE.md` (or a cloud-run-specific runbook file):

```markdown
## Cloud-Run Tool Availability

When running Claude Code in a remote/cloud container (web, GitHub Action, API-triggered):

**WebFetch is blocked on:**
- anthropic.com/news (use WebSearch instead)
- status.claude.com (use WebSearch instead)
- hn.algolia.com (use WebSearch instead)
- reddit.com (blocked by Anthropic ToS in all environments; no fallback in cloud)

**What works in cloud:**
- WebSearch — fully functional, primary news source
- WebFetch on GitHub Atom/RSS feeds (releases.atom, etc.)
- GitHub REST API (unauthenticated, 60 req/hr core limit)

**Scrapling/stealthy_fetch:** NOT available in cloud-run containers.

**Pattern:** Default to WebSearch-FIRST in cloud sub-agents. Don't waste tool budget on WebFetch calls to blocked domains.
```

## Scope of Impact

This would apply globally to any sub-agent or task running in a cloud-run context. The note is additive (no changes to existing behavior) — it just documents a known constraint.

## Why This Should Be Global vs. Local

The Morning Brief Orchestra already has this in its `ceo/learnings.md`. But other Orchestras (Trading, Strategy, any future) will hit the same blocked-domains issue when they first run in cloud. A global note saves each Orchestra from re-discovering this constraint independently. Confidence is HIGH because the pattern is structural (it's about the cloud container network policy, not about specific tool configuration).
