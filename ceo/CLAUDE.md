# CLAUDE.md — 07_Morning_Brief_Orchestra

## What

- **Domain:** Daily monitoring of Claude Code ecosystem — new GitHub repos (MCP servers, skills, agents, plugins) + News (Anthropic releases, competing coding-AI tools)
- **Trigger:** /schedule cron `0 7 * * *` (daily 07:00 local) → fires CEO-Prompt = "execute claude code pulse daily run"
- **Pipeline:** parallel dispatch (github_pulse_scraper + news_pulse_monitor) → trust-tier classification → community-validation for TIER-2 → cross-source dedup → write Vault Morning-Brief + Dashboard-feed
- **Output Targets:**
  - `Vault/System/Morning_Brief/runs/YYYY-MM-DD.md` (rendered Morning-Brief, TIER-1-only + community-upgraded TIER-2)
  - `Vault/System/Morning_Brief/Morning_Brief_history.md` (1-line lifetime log entry)
  - `Vault/System/Morning_Brief/Morning_Brief_index.md` (latest 7 days navigation, regenerated)
  - `Vault/System/Morning_Brief/filtered_audit_YYYY-MM.md` (hidden audit, monthly rolling)
  - `runs/YYYY-MM-DD/{github_raw.json, news_raw.json, tier_decisions.json, community_validations.json, final_brief.md}` (technical artifacts)
- **Mode V1:** DRY-NO-CLONE — Agent NEVER clones any repo, NEVER downloads code; outputs reading-recommendations only. User decides manually whether to clone.
- **Mode V2 (deferred):** Telegram-push for high-signal finds (Anthropic same-day releases)

## Why

- **Manual Trending-Scrolling is friction:** User wants to know which Claude-Code-relevant repos drop daily without manual GitHub-Trending check + X-scrolling
- **Safety-First (User-decision Q8 2026-05-10):** Agent must NEVER recommend potentially-malicious code. Trust-tier-filter is mandatory. Only TIER-1 SAFE makes it into the Brief.
- **Community-validation as smart-recovery (User-decision Q8):** TIER-2 NEEDS-REVIEW gets web-research (reddit/HN/blog mentions). If community validates → upgrade to TIER-1 → include. If no community-data → silently skip (no recommend).
- **Cross-source dedup (User-decision Q1):** Anthropic Claude-4.7 release appears simultaneously on RSS (blog) + GitHub (release) + X (tweet). Without dedup user sees 3 entries. CEO consolidates to 1 entry per Event.
- **Out-of-scope discipline:** YouTube-content stays Per-Trading-Strategy (e.g. `youtube_orb_researcher` already in 05_ORB_Strategy_Orchestra). Future Trading-Strategies get their own youtube_<strategy>_researcher. NOT this Orchestra.

## How

### HARD RULE — Never Auto-Clone

- CEO + Sub-Agents MUST NEVER call `git clone`, `gh repo clone`, or any download command on found repos
- Output is ALWAYS reading-recommendations + URLs only
- User decides manually whether to clone after reading the Brief
- **Warum:** Trust-tier-heuristic is statistical, not deterministic-safety-proof. A TIER-1 repo could still be malicious. User-eye + manual-decision is the safety-gate.

### HARD RULE — TIER-1-Only in Output

- Only TIER-1 SAFE (incl. community-validated TIER-2-upgrades) appears in `Vault/System/Morning_Brief/runs/YYYY-MM-DD.md` + Dashboard
- TIER-2 (no community-data) + TIER-3 SKIP go to `filtered_audit_YYYY-MM.md` only — NEVER in Brief, NEVER in Dashboard
- **Warum:** User-decision Q8 — eliminates Wurstkäscher-risk completely; everything in Brief is trust-vetted; user can clone any of them without nochmal-prüfen

### HARD RULE — Cross-Source Dedup

- Before writing final_brief.md: scan all (github + news) entries for same Event-signature
- Event-signature = (repo-name OR product-name) + (release-version OR title-keywords)
- Example: `anthropics/claude-code` GitHub-release v1.5.0 + RSS-blog-post "Claude Code 1.5.0" + X-tweet "@AnthropicAI v1.5.0 is out" → 1 consolidated entry "Claude Code 1.5.0" with 3 source-links
- **Warum:** Otherwise Anthropic-releases bloat the Brief with 3-5 duplicates per event

### HARD RULE — File Deletion HARD-LOCK

- CEO + Sub-Agents NEVER delete any output file. Append + overwrite-same-day OK.
- Old runs/YYYY-MM-DD/ folders stay forever — Archive-Only HARD-LOCK applies
- **Warum:** Global CLAUDE.md rule, no override path

### HARD RULE — Self-Improvement Discipline [CEO-SHARED — read by all sub-agents at Process Step 1]

- **Read-learnings-First:** Every run MUST start by reading all 4 learnings.md files. Skipping = repeating known mistakes.
- **Reflect-at-End:** Every run MUST append observations to `learnings.md` (CEO) per template. Don't append empty/placeholder — write real observations.
- **Self-Edit-Allowed Scope (own files only):**
  - own `learnings.md` (this folder) — append every run
  - own `CLAUDE.md` (this folder) — edit ONLY when ≥3× confirmation of same pattern across runs
  - sub-agent CLAUDE.md (e.g. `github_scraper/CLAUDE.md`) — CEO may edit when sub-agent's learnings.md has ≥3× confirmation
- **Self-Edit-Allowed Edits:** keyword-list extension, exclusion-pattern addition, threshold tuning, format-spec refinement. NOT allowed — change Hard-Rules, change architecture, change User-decisions (Q1-Q8).
- **Bias-Watch:** CEO must guard against drift toward "make Brief look fuller" (content-volume bias). User-Q8 explicit: safety > content. If you catch yourself relaxing trust-tier without 3× community-confirmed evidence — STOP, re-read User Q8 in plan-file, course-correct.
- **Warum:** User-Vision Phase-1 (Learning) → Phase-2 (Autonomous). This Orchestra is a learning-system, not just an automation.

### HARD RULE — NEVER edit Global Files (Proposal-Doc Pattern) [CEO-SHARED — read by all sub-agents at Process Step 1]

CEO + Sub-Agents are **STRICTLY FORBIDDEN** from editing:
- `~/.claude/CLAUDE.md` (Global User-CLAUDE.md)
- `/Users/Claude/Documents/CLAUDE.md` (Project-wide CLAUDE.md)
- `~/.claude/projects/-Users-Claude-Documents/memory/*.md` (Global Memory)
- `~/.claude/projects/-Users-Claude-Documents/memory/MEMORY.md` (Memory Index)

**Instead — PROPOSAL-DOC PATTERN:**

When any learning would benefit a global file:

1. Write a Proposal-Doc to `/Users/Claude/Documents/Vault/System/Morning_Brief/proposals/YYYY-MM-DD_<slug>.md`
2. Use this template:

```markdown
---
title: <short title>
target_file: ~/.claude/CLAUDE.md | Documents/CLAUDE.md | memory/<file>.md
proposed_at: YYYY-MM-DD
proposed_by: Morning_Brief_Orchestra (CEO | github_scraper | news_monitor | community_validator)
status: pending_user_review
confidence: low | medium | high (based on # of confirmations)
---

## Problem Observed

<what pattern triggered this proposal>

## Evidence

- Run YYYY-MM-DD: <observation>
- Run YYYY-MM-DD: <observation>
- Run YYYY-MM-DD: <observation>

## Proposed Change

<exact edit / addition>

## Scope of Impact

<what would change in user's setup; who else affected>

## Why This Should Be Global vs. Local

<reason it shouldn't stay in own learnings.md or own CLAUDE.md>
```

3. Dashboard 7th Section will surface these proposals as actionable items (Phase E reads `proposals/` folder)
4. User reviews → applies/rejects manually → renames file `*.applied.md` or `*.rejected.md`

**Warum HARD-LOCK:** Global files are User-controlled. Auto-edits would corrupt User's curated context across all projects + sessions. Proposals respect User-sovereignty.

## Daily-Run Workflow (CEO-Prompt executes this)

0. **READ-LEARNINGS-FIRST (HARD-STEP):** Read all 4 learnings.md files BEFORE anything else:
   - `/Users/Claude/Documents/05_Orchestra/07_Morning_Brief_Orchestra/learnings.md` (CEO learnings — own past observations)
   - `/Users/Claude/.claude/agents/learnings/github_pulse_scraper_learnings.md` (sub-agent prior runs, ACE-structured)
   - `/Users/Claude/.claude/agents/learnings/news_pulse_monitor_learnings.md` (ACE-structured)
   - `/Users/Claude/.claude/agents/learnings/community_validator_learnings.md` (ACE-structured)
   - **Warum:** Past learnings encode why-decisions and what-failed; without reading you'll repeat known mistakes
1. **Read this CLAUDE.md** (canonical SHARED rules — sub-agents auto-load own definitions when dispatched, no separate briefing-files needed)
2. **Compute today-date** = YYYY-MM-DD local time
3. **Create run-folder** `runs/YYYY-MM-DD/` if not exists; if exists (re-run same day) → log warning + overwrite final_brief.md
4. **Parallel dispatch (single message, 2 Agent tool-calls):**
   - `github_pulse_scraper` → returns raw repo-list as JSON → write to `runs/YYYY-MM-DD/github_raw.json`
   - `news_pulse_monitor` → returns raw news-items as JSON → write to `runs/YYYY-MM-DD/news_raw.json`
5. **Trust-Tier classification** (CEO computes deterministic-rule per repo, see Heuristic below) → write `runs/YYYY-MM-DD/tier_decisions.json`
   - **Apply learnings:** clamp `last_commit_days = max(0, computed)` per learnings 2026-05-10
   - **Cloud-run mode (3× confirmed 2026-05-13):** If WebFetch returns 403 on Anthropic /news, HN Algolia, or status.claude.com → use WebSearch as PRIMARY news source. GitHub releases atom (github.com) remains reliably reachable. GitHub API unauthenticated (no gh token in cloud) → per-repo metadata calls unavailable; use bulk search fields (stars, pushed_at, open_issues) for classification only.
6. **Community-validation dispatch** (parallel, max 3 simultaneous calls) — for each TIER-2 repo:
   - Dispatch `community_validator` with repo-name + url
   - Returns sentiment-classification + upgrade-decision
   - Aggregate to `runs/YYYY-MM-DD/community_validations.json`
7. **Cross-source dedup** (CEO scans github+news for same Event) → consolidate
8. **Render final_brief.md** — Markdown per format spec below
9. **Write Vault outputs:**
   - `Vault/System/Morning_Brief/runs/YYYY-MM-DD.md` ← copy of final_brief.md
   - Append 1-line to `Vault/System/Morning_Brief/Morning_Brief_history.md`: `- YYYY-MM-DD — Repos TIER-1: <n> · TIER-2-upgraded: <m> · News: <k> · See [run](runs/YYYY-MM-DD.md)`
   - Append TIER-2/3 to `Vault/System/Morning_Brief/filtered_audit_YYYY-MM.md` (create file if new month)
   - Regenerate `Vault/System/Morning_Brief/Morning_Brief_index.md` (latest 7 days bullets)
10. **Dashboard polls** 60s, picks up automatically — no CEO-action needed
11. **SELF-REFLECT-LAST (HARD-STEP):** Append to `learnings.md` (this folder) following the format-template — sections: Cross-Sub-Agent Patterns / Trust-Tier Distribution / Cross-Source Dedup Stats / Heuristic-Performance / Self-Reflection (CEO behavior) / Suggested Local-CLAUDE.md Improvements / Suggested Global-File Proposals / What Next-Run Should Do Differently
12. **Self-Edit Trigger Check (HARD-STEP):** Review YOUR-OWN learnings.md for "Suggested Local-CLAUDE.md Improvements" entries with ≥3× confirmation across past runs. If qualify → edit THIS CLAUDE.md (own scope). Same check for sub-agent learnings.md → CEO may edit those sub-agent CLAUDE.md when stable. **NEVER touch global files — use Proposal-Doc pattern instead.**
13. **Proposal-Doc Trigger Check (HARD-STEP):** Review learnings.md for "Suggested Global-File Proposals" entries with ≥2× confirmation. If qualify → write Proposal-Doc to `Vault/System/Morning_Brief/proposals/YYYY-MM-DD_<slug>.md` per template. Dashboard 7th Section will surface these for User-review.
14. **Done** — log completion timestamp in `runs/YYYY-MM-DD/run_complete.txt`

## Trust-Tier Heuristic (deterministic)

```python
def classify(repo):
    # TIER-3 SKIP — auto-reject
    if repo.stars < 20:
        return ("SKIP", "stars < 20")
    if not repo.license and repo.contributors == 1 and repo.age_days < 14:
        return ("SKIP", "no license + single author + <14d age")

    # TIER-1 SAFE — strict criteria, all must pass
    if (repo.stars >= 100
        and repo.age_days >= 30
        and repo.last_commit_days <= 30
        and repo.license is not None
        and repo.contributors >= 2
        and repo.closed_issues_ratio >= 0.4):
        return ("SAFE", "stars>=100, mature, active, licensed, multi-author, healthy issues")

    # Default — needs community validation
    return ("NEEDS_REVIEW", f"stars={repo.stars}, age={repo.age_days}d, contributors={repo.contributors}, license={bool(repo.license)}")
```

**Field definitions** (Sub-Agent must emit these):
- `stars` int — current star count
- `age_days` int — days since repo creation
- `last_commit_days` int — days since last commit
- `license` str|null — license SPDX-id or null
- `contributors` int — distinct contributor count (top-100 OK)
- `closed_issues_ratio` float — closed_issues / (open_issues + closed_issues), 0.0 if no issues at all

## Final Brief Format

```markdown
# Morning Brief — YYYY-MM-DD

## Anthropic / Claude Code Releases
- **<release-name>** (v<version>) — <one-line summary> · [Blog](<rss-link>) · [GitHub](<repo-link>) · [Tweet](<x-link>)

## New Trust-Vetted GitHub Repos (TIER-1 SAFE)
- **<owner/repo>** ⭐ <stars> — <description> · `<keyword-match>` · [link]
  - **Why-trust:** <e.g. "Stars 234, MIT, 5 contributors, last commit 2d ago, 67% closed-issue-ratio">

## Community-Validated (was TIER-2, web-confirmed safe)
- **<owner/repo>** ⭐ <stars> — <description> · [link]
  - **Why-promoted:** <e.g. "Reddit r/ClaudeAI thread positive (12 upvotes), HN comment 'works well in production'">

## Coding-AI Ecosystem News
- **<headline>** — <source> · [link]

## Statistics
- Repos scanned: <n_total> · TIER-1 SAFE: <n_t1> · TIER-2 promoted via community: <n_t2_up> · TIER-2/3 filtered to audit: <n_filtered>
- News items: <n_news> · After dedup: <n_dedup>
- Run-time: <duration> seconds
```

## Sub-Agent Dispatch Reference

Sub-Agents are now **self-contained** in `~/.claude/agents/<name>.md` (template-V1.0-compliant, post-2026-05-10 consolidation). They auto-load their own definition + read this CEO-CLAUDE.md sections "Self-Improvement Discipline" + "NEVER edit Global Files" at their Process Step 1.

| Sub-Agent | subagent_type | Tool-budget | Definition File | Briefing-Prompt template |
|---|---|---|---|---|
| github_pulse_scraper | `github_pulse_scraper` | ~101 gh-calls | `~/.claude/agents/github_pulse_scraper.md` | "Execute daily GitHub-Pulse-Scrape. today_date: YYYY-MM-DD. run_folder: \<absolute-path\>. Return raw repo-list as JSON." |
| news_pulse_monitor | `news_pulse_monitor` | ~14 calls | `~/.claude/agents/news_pulse_monitor.md` | "Execute daily News-Pulse-Monitor. today_date: YYYY-MM-DD. run_folder: \<absolute-path\>. Return raw news-items as JSON." |
| community_validator | `community_validator` | 2-6 calls/repo | `~/.claude/agents/community_validator.md` | "Validate community-sentiment for repo \<owner/name\>. URL: \<github-url\>. today_date: YYYY-MM-DD. Return JSON with upgrade_decision." |

Old briefing CLAUDE.md files at `<sub>/CLAUDE.md` archived 2026-05-10 to `Archiv/2026-05-10_pulse_briefings_consolidated/` per Archive-HARD-LOCK. Empty subfolders kept (don't delete).

## Trigger Phrases

- `"execute claude code pulse daily run"` — full pipeline (also fired by /schedule)
- `"pulse status"` — show last 7 runs from history.md
- `"pulse run <YYYY-MM-DD>"` — re-execute a past day's run (overwrites)
- `"pulse audit <YYYY-MM>"` — show filtered_audit log for a month

## Files in this Orchestra

- `CLAUDE.md` — this file (CEO-briefing, also CANONICAL source for SHARED Hard-Rules read by sub-agents)
- `README.md` — human-readable overview + debug instructions
- `learnings.md` — CEO-level learnings (cross-cutting observations across sub-agent runs)
- `runs/YYYY-MM-DD/*.json` + `final_brief.md` — daily technical artifacts

**Post-Phase-A.2-Consolidation note:** Pre-consolidation sub-agent subfolders (`github_scraper/`, `news_monitor/`, `community_validator/`) were archived 2026-05-10 to `Archiv/2026-05-10_pulse_briefings_consolidated/empty_subfolders/`. Sub-agent briefings + learnings now live at canonical `~/.claude/agents/` locations (see Sub-Agent-Files section below).

## Sub-Agent Files (canonical locations post-2026-05-10 consolidation)

- `~/.claude/agents/github_pulse_scraper.md` — Sub-Agent-1 (v1.0)
- `~/.claude/agents/news_pulse_monitor.md` — Sub-Agent-2 (v1.0)
- `~/.claude/agents/community_validator.md` — Helper-Agent (v1.0)
- `~/.claude/agents/learnings/{github_pulse_scraper,news_pulse_monitor,community_validator}_learnings.md` — ACE-style structured learnings
