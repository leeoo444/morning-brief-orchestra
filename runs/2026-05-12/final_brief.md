# Morning Brief — 2026-05-12

> **Run:** Cloud daily run · JST 2026-05-12 · Pipeline v2

---

## Anthropic / Claude Code Releases

- **Claude Code v2.1.139** — _Agent View (Research Preview)_, `/goal` command (set completion criteria, agent iterates until met), `/scroll-speed` command, Hook improvements (`args: string[]` + `continueOnBlock` config), MCP: `CLAUDE_PROJECT_DIR` env var now set, VS Code `Cmd/Ctrl+Shift+T` to reopen closed session tabs, 50+ bug fixes · [GitHub](https://github.com/anthropics/claude-code/releases/tag/v2.1.139) · Published 2026-05-11

- **Code with Claude 2026 — San Francisco** (May 6) — Anthropic's developer conference kicked off in SF. Keynote announced three new agent features: **Multiagent Orchestration** (fleet of parallel agents for complex tasks), **Outcomes** (define success criteria so agents iterate + improve), **Dreaming** (recall previous sessions, build on past work). Also shipped **Claude Finance**: 10 pre-built agent templates (pitchbook builder, KYC screening, month-end closer, etc.) as Claude Code plugins + Managed Agent cookbooks. API volume 17× YoY. Remaining dates: London May 19, Tokyo June 10 · [Event](https://claude.com/code-with-claude) · [Simon Willison liveblog](https://simonwillison.net/2026/May/6/code-w-claude-2026/)

- **Higher Claude Code + API Limits** (May 6) — 2× Claude Code 5-hour rate limits for Pro/Max/Team/Enterprise; SpaceX Colossus 1 deal: 300 MW + 220,000 NVIDIA GPUs · [Anthropic Blog](https://www.anthropic.com/news/higher-limits-spacex)

- **Anthropic × Akamai $1.8B Compute Deal** (May 8) · [Bloomberg](https://www.bloomberg.com/news/articles/2026-05-08/anthropic-inks-1-8-billion-computing-deal-with-akamai)

- **Claude for Microsoft 365** — Excel, PowerPoint, Word (Outlook coming) · [Fortune](https://fortune.com/2026/05/05/anthropic-wall-street-financial-services-agents-jamie-dimon/)

---

## New Trust-Vetted GitHub Repos (TIER-1 SAFE)

- **[anthropics/cwc-long-running-agents](https://github.com/anthropics/cwc-long-running-agents)** ⭐268 — CwC 2026 conference take-home: patterns for hour-long agents (observed-evidence done-check, independent verifier, clean session handoff). Apache-2.0. **Why-trust:** Official anthropics/ org.

- **[anthropics/cwc-workshops](https://github.com/anthropics/cwc-workshops)** ⭐173 — CwC 2026 workshop materials: model-picking skill, multi-agent composition with Skills+MCP, first Managed Agent. Apache-2.0. **Why-trust:** Official anthropics/ org.

---

## Community-Validated (was TIER-2, web-confirmed safe)

_None today — 5 TIER-2 validated, 0 upgraded._

---

## Coding-AI Ecosystem News

- **Cursor 3.3** (May 7) — Parallel agents, PR-split quick action · [Changelog](https://cursor.com/changelog)
- **Zed 1.0 Stable** — Parallel agents in same window, DeepSeek-V4 support · [Releases](https://zed.dev/releases/stable)
- **HN: Remote MCP Support in Claude Code** · [Thread](https://news.ycombinator.com/item?id=44312363)
- **Show HN: Wombat** — Unix rwxd permissions for MCP tool calls · [Thread](https://news.ycombinator.com/item?id=47418076)

---

## Statistics

- Repos scanned: 10 · TIER-1 SAFE: 2 (official-org) · TIER-2 promoted: 0 · TIER-2/3 filtered: 8
- News items: 9 raw · After dedup: 9
- Run quality: GitHub API rate-limited (unauthenticated); WebFetch 403 on Anthropic/HN/status → recovered via WebSearch
