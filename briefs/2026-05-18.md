# Morning Brief — 2026-05-18

> Generated: 2026-05-18 JST · Run-3 (cloud daily run) · Repos scanned: 100 · Trust-tier: see Statistics

---

## Anthropic / Claude Code Releases

- **Claude Code v2.1.143** (May 12–15, four releases) — Agent ecosystem hardening week: Fast Mode now defaults to **Opus 4.7** (v2.1.142); `claude agents` gets 8 new flags (`--add-dir`, `--mcp-config`, `--plugin-dir`, `--permission-mode`, `--model`, `--effort`, etc.) for configuring dispatched background sessions; plugins with root-level `SKILL.md` now surfaced as a Skill automatically; plugin dependency enforcement (can't disable a depended-on plugin); projected context cost shown in `/plugin` marketplace browse pane; `terminalSequence` hook field for desktop notifications; `ANTHROPIC_WORKSPACE_ID` for workload identity federation; `claude agents --cwd` to scope session list to a directory; Rewind "Summarize up to here" menu option. · [GitHub Releases](https://github.com/anthropics/claude-code/releases) · [Changelog](https://code.claude.com/docs/en/changelog)

- **Agent SDK Credit Pool** (May 13–14) — Major policy reversal: Anthropic reinstates third-party agent harnesses (OpenClaw et al.) that were banned in April. New "Agent SDK credits" are a separate monthly pool per plan ($20 Pro / $100 Max 5× / $400 Max 20×). Starting **June 15**, programmatic/third-party usage draws from this pool rather than interactive quota. Addresses capacity issues from unoptimized agents bypassing prompt caching. · [VentureBeat](https://venturebeat.com/technology/anthropic-reinstates-openclaw-and-third-party-agent-usage-on-claude-subscriptions-with-a-catch) · [Gigazine](https://gigazine.net/gsc_news/en/20260514-anthropic-claude-agent-sdk-credits/)

- **Claude Code weekly limits +50% through July 13** (May 13) — Third consecutive rate-limit expansion in 5 weeks (hourly doubled May 6 → peak hours removed → weekly cap now +50%). Explicit competitive response to OpenAI Codex. Applies to all paid plans (Pro/Max/Team/Enterprise). · [pasqualepillitteri.it](https://pasqualepillitteri.it/en/news/2494/claude-code-weekly-limits-50-percent-anti-codex-anthropic-2026) · [APIdog](https://apidog.com/blog/claude-code-weekly-limits-50-percent-increase-july-2026/)

---

## New Trust-Vetted GitHub Repos (TIER-1 SAFE)

_No TIER-1 repos today. All 3 Claude-ecosystem candidates are age ≤4 days — standard heuristic (age ≥30d) is the binding constraint. Community validation confirmed 0 third-party testimonials (too new). See TIER-2 section in filtered audit._

---

## Community-Validated (was TIER-2, web-confirmed safe)

_No TIER-2 repos promoted today. All 3 TIER-2 candidates (yetone/native-feel-skill, DenisSergeevitch/agents-best-practices, WantongC/journal-adapt-writing-skill) had 0 distinct community domains — age<7d pattern confirmed for the 6th consecutive time._

---

## Coding-AI Ecosystem News

- **Claude for Small Business** (HN, May 17) — Active Hacker News discussion on onboarding non-engineer teammates to Claude Code for task automation. Community debate on accessibility vs. capability tradeoffs. · [HN thread](https://news.ycombinator.com/item?id=48130950)

- **Zed 1.1.5** (stable, ~May 17) — Fixed agent edits failing to apply in some cases; fixed mouse cursor hiding bug when "Unsaved edits" prompt appeared. Minor but important agent-workflow stability fix. · [Zed release notes](https://zed.dev/releases/stable/1.1.5)

- **Cursor Bugbot → usage-based billing** (~May 16) — Cursor Bugbot switches from per-seat to usage-based billing for Teams and Individual plans; configurable effort levels for deeper PR reviews. Opt-in available before renewal date of June 8. · [Cursor changelog](https://releasebot.io/updates/cursor)

- **MCP Tool Search (Claude Code)** — New lazy-loading for MCP servers reduces context usage by up to 95% in Claude Code. Tracked via plugin/MCP changelog updates. Context: Claude Code plugin ecosystem now counts 7,159+ MCP server components out of 32k+ active plugins.

---

## Statistics

- **Repos scanned:** 100 (unauthenticated GitHub API, bulk search fields only — rate-limit applies)
- **Keyword-matched:** 7 (3 core keyword-filter + 2 agent-skill additions + 2 out-of-scope false-positives)
- **TIER-1 SAFE direct:** 0 (age_days ≥30 constraint binding for all)
- **TIER-2 NEEDS-REVIEW:** 3 (yetone/native-feel-skill ⭐1273, DenisSergeevitch/agents-best-practices ⭐707, WantongC/journal-adapt-writing-skill ⭐313)
- **TIER-2 promoted via community:** 0 (default-deny: 0 third-party domains across all 3 — age<7d pattern, run-3 confirmation)
- **TIER-3 SKIP:** 4 (2 spam-cluster repos, 1 numeric-username suspicious, 1 out-of-scope)
- **News items raw:** 7 · **After cross-source dedup:** 6 (two Claude Code release entries already group-consolidated)
- **Run-folder:** `runs/2026-05-18/`

---

## Filtered Audit (TIER-2/3 — not in brief, full detail in audit file)

_TIER-3 SKIP:_
- `BharathKumarSuresh/claude-design-system-hooks` ⭐421 — spam indicators: no license, "Free Install" description, download/free/install/installer topic cluster
- `2508965-ship-it/harmonist-orchestral` ⭐420 — no license, numeric-username org, zero development activity (pushed within seconds of create)
- `mikesheehan54/Claude-Code-Design-AI` ⭐396 — same spam topic cluster as above (coordinated), pushed 10min after create
- `PlaceNL2026/best-of-algorithmic-trading` ⭐214 — out of scope: algorithmic trading list, only `mcp` topic match

_TIER-2 (community-validated: upgrade=false):_
- `yetone/native-feel-skill` ⭐1273 — Agent Skill for cross-platform desktop native design, MIT, age=3d. Worth re-validating at +14d.
- `DenisSergeevitch/agents-best-practices` ⭐707 — Provider-neutral agentic harness best-practices Agent Skill, MIT, age=2d. Worth re-validating at +14d.
- `WantongC/journal-adapt-writing-skill` ⭐313 — Academic journal adaptation writing skill, MIT, age=4d. Worth re-validating at +14d.

