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

## 2026-05-18 (run-3, cloud daily run)

### Findings

**Sources attempted and results:**
- Claude Code Releases Atom → Reached via WebFetch: releases v2.1.140–2.1.143 (May 12–15). No releases in strict 24h window (latest was May 15). Used 7-day window to capture week's full release activity.
- Anthropic News page → Not attempted (403 confirmed in run-2; using WebSearch as primary)
- Anthropic Status → Not attempted (403 confirmed in run-2)
- HN Algolia → 403 confirmed again (run-3 third confirmation). WebSearch recovered HN discussions.
- Reddit → Not attempted (scrapling not available in cloud run)
- WebSearch used as PRIMARY: recovered all major news items

**Notable news captured:**
- Claude Code v2.1.140–2.1.143 (4 releases, May 12–15): Fast Mode → Opus 4.7, claude agents flags, SKILL.md surfacing, plugin dependency enforcement, context cost in marketplace
- Agent SDK Credits policy (May 13–14): Third-party agent reinstatement with separate credit pool, June 15 effective
- Claude Code weekly limits +50% through July 13 (May 13): 3rd consecutive rate-limit expansion
- Claude for Small Business HN thread (May 17)
- Zed 1.1.5 (agent edits fix)
- Cursor Bugbot usage-based billing change

**Total kept:** 7 items (6 after dedup consideration)

### Edge-Cases Encountered

1. **HN Algolia 403 confirmed ×3:** run-1 (worked) → run-2 (403) → run-3 (403). Pattern established: HN Algolia unreliable in cloud run. WebSearch covers HN well via `site:news.ycombinator.com`.
2. **24h window too strict for quiet days:** No Claude Code releases or major Anthropic news strictly in May 17 UTC 24h window. Extended to 7-day window (since last run May 12) to capture week's developments. This is correct behavior — the brief should cover "what happened since last run."
3. **Aider/Continue/Cline:** Still dormant — 3rd consecutive confirmation of inactivity. Qualifies for retirement proposal.

### Suggested CLAUDE.md Improvements

- **[3× obs — READY]** HN Algolia always 403 in cloud run. Replace with WebSearch `site:news.ycombinator.com "claude code"` as the Tier-2 community source. Retire the direct HN Algolia URL for cloud runs.
- **[3× obs — READY]** Aider (dormant since Feb 2026), Continue (dormant since Mar 2026), Cline (last release May 1, quiet): all 3 confirmed dormant across 3 runs. Propose retiring from daily monitor cadence → weekly check at most.
- **[1× obs]** Brief run-window should adapt to days-since-last-run, not strict 24h. If last run was 6 days ago, extend to 7-day window. Prevents "quiet 24h" gaps misrepresenting actual week's news.

### What Next-Run Should Do Differently

1. Use WebSearch `site:news.ycombinator.com "claude code"` instead of HN Algolia API (3× 403 confirmed)
2. Retire Aider/Continue/Cline daily polling — 3× confirmed dormant
3. Default to run-window = days_since_last_run (not hardcoded 24h)
4. Continue WebSearch-first for all news discovery
