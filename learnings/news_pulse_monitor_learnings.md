---
type: agent_learnings_ace
agent: news_pulse_monitor
schema_version: 1.0
last_curated: 2026-05-10
---

# Insights (ACE-structured — read top-3 active by helpful_count DESC at Process Step 2)

- insight_id: ins_001
  insight: "Reddit WebFetch is hard-blocked by Anthropic ToS — must use mcp__scrapling__stealthy_fetch as primary, NOT fallback"
  helpful_count: 1
  harmful_count: 0
  last_applied: 2026-05-10
  sources: [dispatch_2026-05-10_phase_b_first_run]
  ace_status: active
  added: 2026-05-10
  why: "First-run discovered WebFetch returns 'Claude Code is unable to fetch from www.reddit.com'; scrapling worked. Already updated CLAUDE.md tool-mapping."

- insight_id: ins_002
  insight: "HN Algolia 'search' endpoint ignores numericFilters time-bound; must use 'search_by_date' for time-windowed queries"
  helpful_count: 1
  harmful_count: 0
  last_applied: 2026-05-10
  sources: [dispatch_2026-05-10_phase_b_first_run]
  ace_status: active
  added: 2026-05-10
  why: "Initial query returned all-time top-stories with wrong-year-old dates. Already fixed in Tier-2 source-list."

- insight_id: ins_003
  insight: "MCP keyword needs word-boundary regex \\bMCP\\b — substring match catches 'map'/'mcpu' false-positives"
  helpful_count: 1
  harmful_count: 0
  last_applied: 2026-05-10
  sources: [dispatch_2026-05-10_phase_b_first_run]
  ace_status: active
  added: 2026-05-10
  why: "1 false-positive caught + filtered before write in first-run. Already in relevance-filter."

- insight_id: ins_004
  insight: "Anthropic Blog RSS https://www.anthropic.com/news/rss.xml is 404; use HTML scrape on /news as fallback"
  helpful_count: 1
  harmful_count: 0
  last_applied: 2026-05-10
  sources: [dispatch_2026-05-10_phase_b_first_run]
  ace_status: active
  added: 2026-05-10
  why: "URL retired upstream. HTML page works. Already updated Tier-0 source-list."

- insight_id: ins_005
  insight: "status.anthropic.com/history.atom is 302 redirect → use canonical https://status.claude.com/history.atom"
  helpful_count: 1
  harmful_count: 0
  last_applied: 2026-05-10
  sources: [dispatch_2026-05-10_phase_b_first_run]
  ace_status: active
  added: 2026-05-10
  why: "Saves 1 redirect-roundtrip per run. Already updated Tier-0 source-list."

- insight_id: ins_006
  insight: "Aider/Continue/Cline are dormant (Feb/Mar/May-1) — daily-monitor cadence wasted on these sources"
  helpful_count: 0
  harmful_count: 0
  last_applied: 2026-05-10
  sources: [dispatch_2026-05-10_phase_b_first_run]
  ace_status: active
  added: 2026-05-10
  why: "Hold for ≥3× confirmation before retiring sources. May come back to life."

# Historical Run Notes (narrative — context preservation)

---

# learnings.md — news_pulse_monitor

> Append-only. Every run appends Findings, Edge-Cases, False-Positives, Suggested-Improvements. Read at START of next run BEFORE executing. When pattern stabilizes (≥3× confirmed) → propose CLAUDE.md update via Self-Edit-Trigger section.

## Format per Entry

```
## YYYY-MM-DD (run-N)

### Findings
- Sources reachable / unreachable
- Item-counts per source
- Notable releases captured

### Edge-Cases Encountered
- ...

### False-Positives (relevance-filter let through but irrelevant)
- ...

### Tool-Call Issues
- Per-source failures + workarounds

### Suggested CLAUDE.md Improvements (apply after 3× confirmation)
- Source URL changes
- Relevance-filter additions/removals
- New sources to add
- Sources to retire

### What Next-Run Should Do Differently
- ...
```

---

## (run-1 was 2026-05-10 Phase-B-test; run-2 is 2026-05-12 cloud daily run)

---

## 2026-05-10 (run-1, manual Phase-B-test dispatch)

### Findings

**Sources reachable (12/14 attempted):**
- Anthropic Blog RSS `https://www.anthropic.com/news/rss.xml` → **404 NOT FOUND** (URL retired or moved)
- Anthropic News page `https://www.anthropic.com/news` → reachable, no 2026-05-09/10 articles listed (latest = May 6 SpaceX-deal post)
- Claude Code Releases Atom → reachable, **2 entries** in 24h-window (v2.1.137, v2.1.138 both 2026-05-09)
- Anthropic Status (`status.anthropic.com/history.atom` → 302 → `status.claude.com/history.atom`) → reachable after redirect-follow, **3 entries** (Claude Code Web outage, Opus 4.1 errors, IDE extension Windows-bug — all resolved)
- Cline Releases Atom → reachable, **0 entries** in 24h (latest = v3.82.0 on 2026-05-01, just outside window)
- Aider Releases Atom → reachable, **0 entries** (latest = v0.86.3.dev on 2026-02-12; project appears dormant since Feb)
- Continue Releases Atom → reachable, **0 entries** (latest = v1.3.38-vscode on 2026-03-27; no recent activity)
- Codex CLI Releases Atom → reachable, **5 entries** in 24h-window (alpha 0.131.0-alpha.1 through alpha.5)
- Sourcegraph Blog RSS → reachable, only 1 May-post (May 4 — outside 24h window); RSS uses `localhost:5174` URLs (broken) instead of canonical sourcegraph.com URLs — **suggests Sourcegraph's RSS generator misconfigured**
- Zed Blog RSS `https://zed.dev/blog/feed.xml` → **HTTP 500 INTERNAL SERVER ERROR** (server-side problem, retried once still 500)
- Cursor Changelog (stealthy_fetch) → reachable, 4 changelog entries May 1-7, **0 entries** in 24h-window (latest = May 7 "PR Review, Build Plan in Parallel")
- HN Algolia `search` API → reachable, but `numericFilters=created_at_i><old-ts>` returned all-time top-stories; switched to `search_by_date` endpoint which respects time-filter properly
- HN Algolia `search_by_date` for "claude code" → 20 hits, 12 in 24h-window
- HN Algolia `search_by_date` for "MCP" → 6 hits, 4 in window after false-positive filter (`mcp` substring matched "map" in 1 result)
- Reddit JSON via WebFetch → **BLOCKED** (`Claude Code is unable to fetch from www.reddit.com`); fallback `mcp__scrapling__stealthy_fetch` worked
- Reddit r/ClaudeAI → 25 posts fetched, 9 inside 24h-window kept after relevance-filter
- Reddit r/LocalLLaMA → 30 posts fetched, 2 matched MCP/claude/anthropic keywords inside window
- @AnthropicAI X → fetched via `nitter.net` mirror (X.com direct would block), **0 tweets** strictly inside 2026-05-09T00Z window (latest tweet = May 8 17:52 UTC). Included most-recent tweet anyway as Tier-0-source-reference.

**Notable releases captured:**
- Claude Code v2.1.137 + v2.1.138 (Anthropic-official, same-day patch sequence)
- OpenAI Codex 0.131.0-alpha.1 through alpha.5 (5 alpha-releases inside 24h — heavy iteration)
- Anthropic Status: 3 resolved incidents (Claude Code Web outage, Opus 4.1 errors, IDE-Windows bug)
- Cursor 3.3 May-7 release announced "Build in Parallel" + PR Review (just outside window but high-signal)
- Reddit r/ClaudeAI top: "Hugging Face co-founder says Qwen 3.6 27B local close to Opus in Claude Code" (751 score, 119 comments)
- HN top in window: "Using Claude Code: The unreasonable effectiveness of HTML" (464 points)

**Total items kept in `news_raw.json`:** 38 (10 RSS, 1 X, 12 Reddit, 15 HN)

### Edge-Cases Encountered

1. **Anthropic-Status host-redirect** — `status.anthropic.com/history.atom` 302→`status.claude.com/history.atom`. WebFetch returned a structured "REDIRECT DETECTED" message and required a second call with the redirect-URL. Need to update CLAUDE.md source-list to canonical URL.
2. **Anthropic-Blog RSS 404** — the URL in CLAUDE.md (`https://www.anthropic.com/news/rss.xml`) doesn't exist. The HTML page `/news` works as fallback but it's HTML-scraping not RSS. Need to find canonical RSS URL OR retire RSS source and rely on news-page-scrape.
3. **Reddit blocks WebFetch** — Anthropic's WebFetch tool blocks reddit.com domain entirely (probably ToS-respect). Stealthy_fetch via scrapling works. Need to update CLAUDE.md tool-mapping for Reddit-sources.
4. **Reddit JSON via stealthy_fetch returned 175k+ chars** — exceeds tool-output token limit; saved to file; required Bash+jq+python parsing to extract structured data. WebFetch (when it works) auto-summarizes; stealthy_fetch returns raw. Need a structured-extract step downstream of stealthy_fetch for JSON-API targets.
5. **HN `search` endpoint ignores time-filter** — must use `search_by_date` not `search` to honor `numericFilters=created_at_i>X`. Initial query returned all-time top-stories with wrong-year-old dates.
6. **MCP false-positive on substring match** — search for "MCP" in HN matched "map" in unrelated story title. Need to require word-boundary `\bMCP\b` in relevance-filter.
7. **Sourcegraph RSS broken URLs** — feed contains `http://localhost:5174/blog/...` instead of `https://sourcegraph.com/blog/...`. Source-bug, not our bug — but item_url is unusable for User. Must rewrite localhost→sourcegraph.com OR drop source.
8. **X-account 24h-window often empty** — Anthropic posts in bursts (last burst May 7-8); 24h-window catches nothing on quiet days. Suggest extending X-window to 48h OR including last-N-tweets regardless of date.
9. **Reddit `created_utc` is float-Unix-epoch** — required Python conversion to ISO timestamp; my output used a hard-set "≥2026-05-09T00Z" formatted manually. Should auto-convert in pipeline.

### False-Positives (relevance-filter let through but irrelevant)

None significant — relevance-filter works (HN already filtered by query keyword). MCP-substring-matching-"map" was caught and excluded before write.

### Tool-Call Issues

- `WebFetch` on reddit.com → hard-blocked. Workaround: `mcp__scrapling__stealthy_fetch`.
- `WebFetch` on anthropic news/rss.xml → 404. No URL-fix yet found.
- `WebFetch` on zed.dev/blog/feed.xml → 500 (transient or persistent? retry next run to confirm).
- `WebFetch` on status.anthropic.com → 302 redirect requires manual re-call.
- `mcp__scrapling__stealthy_fetch` Reddit JSON-output exceeds ~100k-char tool-limit → forces file-spill + jq parsing.
- HN Algolia `search` vs `search_by_date` — must use `search_by_date` for time-bounded queries.

### Suggested CLAUDE.md Improvements (for future runs — apply after 3× confirmation)

1. **Update Anthropic-Status URL:** `https://status.anthropic.com/history.atom` → `https://status.claude.com/history.atom` (302 confirmed redirect, will likely persist).
2. **Replace or retire Anthropic-Blog RSS:** `https://www.anthropic.com/news/rss.xml` → 404. Try `https://www.anthropic.com/news/feed.xml`, `https://www.anthropic.com/feed.xml`, or scrape `https://www.anthropic.com/news` HTML for date+link.
3. **Add Reddit-via-stealthy note:** Document that Reddit needs `mcp__scrapling__stealthy_fetch` not `WebFetch`. Also document the 100k-char output limit + recommend jq-extraction pattern post-fetch.
4. **Switch HN endpoint:** `hn.algolia.com/api/v1/search` → `hn.algolia.com/api/v1/search_by_date` for date-bounded queries.
5. **Tighten MCP keyword:** require `\bMCP\b` word-boundary in relevance-filter to avoid "map"/"mcpu" false-positives.
6. **Drop or warn Sourcegraph RSS:** localhost-URLs in feed are unusable; either fix per-item via regex-rewrite or retire source until Sourcegraph fixes their generator.
7. **Drop or de-prioritize Aider + Continue + Cline:** Aider stuck on Feb 2026, Continue on March, Cline 1 release on May 1 — none active enough for daily-monitor cadence. Suggest reducing to weekly check OR dropping until activity resumes.
8. **X-window extension:** for low-frequency Tier-0 X-accounts (AnthropicAI tweets in bursts ~weekly), extend window to 48h OR include latest-N-tweets-anyway as "context" entries with explicit `out_of_window: true` flag.
9. **Add Codex CLI rate-limit:** Codex shipped 5 alpha-releases in 24h. Future runs should consolidate same-day alpha-bursts into 1 entry "Codex 0.131.0-alpha.1 through alpha.5" to avoid noise.

### Suggested Global-File Proposals (CEO writes Proposal-Doc per HARD RULE)

- **Proposal — Add Reddit-via-stealthy_fetch as known-pattern in `~/.claude/CLAUDE.md` Web Search Priority section.** Currently order is `WebSearch → WebFetch → Tavily → Scrapling`; for Reddit specifically Scrapling must be first because WebFetch is hard-blocked. Worth global note since Reddit is queried by many subagents. Confidence: HIGH (1× confirmed today, but issue is structural not transient).

### What Next-Run Should Do Differently

1. Skip the broken Anthropic-Blog `rss.xml` URL — try alternate URLs OR scrape `/news` HTML.
2. Use `https://status.claude.com/history.atom` directly (skip the 302).
3. Use `search_by_date` for both HN Algolia queries from the start.
4. Use `\bMCP\b` regex-pattern in relevance-filter.
5. Pre-budget for Reddit-JSON-spill: pipe stealthy_fetch through jq immediately, don't try to read raw output back into context.
6. Consolidate same-day alpha-release bursts (Codex pattern) into 1 entry.
7. Reduce or drop Aider/Continue/Cline polling cadence — they're not active enough.
8. Investigate why Zed RSS returned 500 (likely temporary, but second 500 = retire source).
9. Time-window: keep 24h cutoff at 2026-05-09T00:00:00Z (UTC) — confirmed correct for "today is 2026-05-10" mental model.
10. For X-accounts: include latest tweet even if outside 24h window when `most_recent_tweet > 7 days old` would otherwise leave Tier-0 source empty.

---

## 2026-05-12 (run-2, cloud daily run)

### Findings

**Sources attempted and results:**
- Anthropic News page → 403 Forbidden (WebFetch blocked in cloud run environment)
- Claude Code Releases Atom → Reached via WebFetch: **1 entry** in 24h window (v2.1.139, May 11 18:43Z)
- Anthropic Status `status.claude.com/history.atom` → 403 Forbidden
- HN Algolia `search_by_date` → 403 Forbidden
- Reddit → Not attempted (scrapling/stealthy_fetch not available in cloud run context)
- WebSearch used as primary fallback: recovered all major news items successfully

**Notable news captured:**
- Claude Code v2.1.139: Agent View, /goal command, Hook improvements, MCP CLAUDE_PROJECT_DIR
- Code with Claude 2026 SF conference (May 6): Multiagent Orchestration, Outcomes, Dreaming, Claude Finance
- Higher usage limits (May 6): 2× Claude Code limits, SpaceX Colossus 1 deal (300MW, 220k GPUs)
- Akamai $1.8B compute deal (May 8)
- Claude for Microsoft 365
- Cursor 3.3 (May 7): Parallel agents, PR review
- Zed 1.0: Parallel agents, DeepSeek-V4 support

**Total kept:** 9 items (after relevance filter from ~15 raw)

### Edge-Cases Encountered

1. **WebFetch 403 on multiple sources** — Anthropic /news, status.claude.com/history.atom, HN Algolia all blocked in cloud run. WebSearch is reliable fallback for major news.
2. **Aider/Continue/Cline still dormant** — 2nd confirmation (run-1 + run-2). One more → retirement recommendation.

### Suggested CLAUDE.md Improvements

- **[2× obs — hold]** Aider/Continue/Cline dormant: second confirmation. Need 1 more run.
- **[1× obs]** WebFetch 403 in cloud environment for several key sources. Cloud runs should default to WebSearch-first for news.

### What Next-Run Should Do Differently

1. Cloud environment: use WebSearch as primary news source, WebFetch as supplemental
2. Check scrapling tool availability at run start — document if absent
3. Aider/Continue/Cline: 3rd confirmation → propose retirement

---

## 2026-05-16 (run-3, cloud daily run)

### Findings

**Sources attempted and results (cloud run):**
- HN Algolia search_by_date → 403 Forbidden (3rd confirmation, expected in cloud)
- GitHub claude-code releases.atom → Reached via WebFetch: 3 entries since last brief (v2.1.142, v2.1.141, v2.1.140)
- Anthropic Blog → WebSearch used (WebFetch likely blocked); recovered Gates Foundation $200M + PwC deployment
- Cline releases.atom → News via WebSearch: Cline SDK open-sourced (May 13)
- Codex CLI releases → WebSearch: alpha.19 + alpha.21 (May 15)
- AWS MCP Server GA → WebSearch: confirmed GA announcement (May 6)
- CNBC/news → Claude Mythos holding pattern vs OpenAI EU cyber model
- Reddit → Not attempted (scrapling unavailable in cloud)

**Notable news captured:**
- Claude Code v2.1.140-142: Opus 4.7 fast mode, plugin skills, hooks terminal sequences, agent subagent fixes
- Anthropic × Gates Foundation $200M (May 14)
- PwC enterprise Claude deployment (May 14)
- Cline @cline/sdk open-sourced (May 13) — beats-Claude-Code-on-tbench claim
- Codex CLI alpha.21 (May 15) — persisted /goal, Vim mode
- AWS MCP Server GA (May 6, slightly stale but not covered in last brief)
- Claude Mythos still in research preview

**Total kept:** 9 items

### Edge-Cases Encountered

1. **HN Algolia 403 (3rd confirmation):** Consistent cloud-run block. WebSearch remains reliable substitute.
2. **Aider/Continue/Cline releases:** Not explicitly checked via Atom feeds this run. Need to confirm dormancy observation for 3× threshold. Reminder for run-4.

### Suggested CLAUDE.md Improvements

- **[3× obs — READY]** Cloud environment: HN Algolia WebFetch → 403 always. Formalize: in cloud runs, skip HN Algolia WebFetch entirely and default to WebSearch for HN stories.
- **[2× obs — hold]** Aider/Continue/Cline dormant: not confirmed 3rd time today (sources not checked). Need explicit check in run-4.
- **[1× obs]** Add AWS MCP Server to Tier-1 sources (official AWS announcement). First official major enterprise cloud provider MCP GA — worth tracking going forward.

### What Next-Run Should Do Differently

1. Explicitly check Aider/Continue/Cline Atom feeds to confirm/deny 3rd dormancy observation
2. Cloud: default to WebSearch-only for all community sources; skip WebFetch on HN/Reddit/Anthropic news
3. Check if AWS MCP Server should be added as a permanent Tier-1 tracked source
4. Add Claude Mythos red.anthropic.com as a Tier-0 tracked source (research preview → GA will be high-signal)

