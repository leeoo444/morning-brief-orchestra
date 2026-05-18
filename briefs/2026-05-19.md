# Morning Brief — 2026-05-19

> **Coverage window:** 2026-05-12 → 2026-05-19 JST (7-day gap since last cloud run on 2026-05-12)
> **Run environment:** cloud (GitHub API unauthenticated · WebSearch primary news source)

---

## Anthropic / Claude Code Releases

- **Claude Code v2.1.140 → v2.1.143** (May 12–15, four-release sprint) — Consolidated entry:
  - **v2.1.140** (May 12): `subagent_type` matching now case/separator-insensitive; `/goal` hang fix with disabled hooks; background service + loop-scheduling stability
  - **v2.1.141** (May 13): `terminalSequence` hook field → desktop notifications, window titles, bells from hooks; `CLAUDE_CODE_PLUGIN_PREFER_HTTPS` env var; `claude agents --cwd <path>`; 40+ bug fixes (permissions, MCP, rendering, terminal)
  - **v2.1.142** (May 14): New `claude agents` flags — `--add-dir`, `--settings`, `--mcp-config`, `--plugin-dir`, `--permission-mode`, `--model`, `--effort`, `--dangerously-skip-permissions`; **Fast mode now defaults to Opus 4.7**; MCP timeout fixes
  - **v2.1.143** (May 15): Plugin dependency enforcement + disable-chain hints; **projected context cost estimates in plugin marketplace**; PowerShell execution policy bypass flag; background session model/effort preservation
  - [Releases](https://github.com/anthropics/claude-code/releases) · [Changelog](https://code.claude.com/docs/en/changelog)

- **Claude Code Routines** (research preview) — Save a Claude Code config (prompt + repos + connectors) once, trigger it on a cron schedule, via HTTP POST, or on GitHub events (PR opened, release published). Runs on Anthropic-managed cloud infra — no self-hosted cron needed. Supports recurring triage, doc-drift scan, automated PR generation. · [Docs](https://code.claude.com/docs/en/routines) · [InfoQ](https://www.infoq.com/news/2026/05/anthropic-routines-claude/) · [Builder.io guide](https://www.builder.io/blog/claude-code-routines)

---

## New Trust-Vetted GitHub Repos (TIER-1 SAFE)

*No repos qualified this run.* All 9 genuine Claude-Code-ecosystem finds are age 3–8 days. The trust-tier heuristic requires `age_days ≥ 30` for TIER-1 — the trending-window and trust-vetting requirements are structurally incompatible without community validation. See TIER-2 audit section for full list.

---

## Community-Validated (was TIER-2, web-confirmed safe)

*0 repos upgraded this run.* Five repos community-validated: DenisSergeevitch/agents-best-practices, smithersai/claude-p, Siigari/claude-heartbeat, EliasOulkadi/shokunin, wanghuan9/skill-manager. All failed upgrade: zero POSITIVE keyword-matches in independent third-party coverage. Repos are 3–6 days old — pattern consistent with prior runs (age < 14d consistently produces zero "I tried / works well / recommend" testimonials). Full audit in `filtered_audit_2026-05.md`.

---

## Coding-AI Ecosystem News

- **Cursor 3.0** — Agents Window (parallel agent management) + Design Mode (UI-first development). Competes directly with Claude Code's multi-agent session management introduced in v2.1.138+. · [comparison](https://www.shareuhack.com/en/posts/cursor-vs-claude-code-vs-windsurf-2026)

- **Windsurf 2.0** — Agent Command Center + Devin Cloud integration. Google's $2.4B licensing deal officially closed (OpenAI acquisition collapsed due to Microsoft IP dispute). Windsurf now Google-backed. · [comparison](https://www.nxcode.io/resources/news/cursor-vs-windsurf-vs-claude-code-2026)

- **OpenAI Codex** — Now accessible from ChatGPT mobile app (connect Mac running Codex app). IDE extension now works with VS Code forks including Cursor and Windsurf. · [Codex Changelog](https://developers.openai.com/codex/changelog)

- **Skills ecosystem milestone** — `agentskills.io` open standard adopted by 40+ products (Claude Code, Codex CLI, Hermes Agent, OpenClaw, and others). MCP ecosystem hits 10,000+ public servers. Community consensus: MCP = connectivity, Skills = procedural knowledge — complementary not competing. · [agensi.io](https://www.agensi.io/learn/best-mcp-servers-2026) · [Claude Skills vs MCP](https://levelup.gitconnected.com/anthropic-quietly-made-half-my-mcp-servers-obsolete-heres-the-6-skill-setup-that-replaced-them-32eb7e16f538?gi=25599a79d843)

---

## Statistics

- Repos scanned: ~140 (100 bulk + 40 targeted) · Genuine keyword-matched: 9 · TIER-1 SAFE: 0 · TIER-2 promoted via community: 0 · TIER-2 (audit): 9 · TIER-3 SKIP: 5 (3 spam pattern, 1 no-license+0-forks, 1 named-exclusion) · False-positives filtered: 6 (`zed`-in-`optimized` substring ×3, Minecraft `mcp` ×1, not-ecosystem ×2)
- News items raw: 9 · Claude Code releases consolidated: 4→1 (same-event, consecutive days) · Final news items: 6
- GitHub API: unauthenticated → rate-limited after bulk search; per-repo metadata (contributors, closed-issues) not available; bulk search fields used for classification
- News sources: Claude Code Releases Atom ✓ (primary) · WebSearch ✓ (primary for ecosystem news) · WebFetch: HN Algolia 403 · scrapling: unavailable in cloud env
- Aider/Continue/Cline: no releases confirmed in window — **3rd consecutive run with zero activity** → retirement from daily monitor recommended (see learnings update)
