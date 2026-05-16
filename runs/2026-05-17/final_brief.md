# Morning Brief — 2026-05-17

## Anthropic / Claude Code Releases

- **Claude Code v2.1.143** — Plugin dependency enforcement (`disable` refuses if depended-on; `enable` force-enables transitive deps), projected context cost per-turn and per-invocation in `/plugin` marketplace browse pane, `worktree.bgIsolation: "none"` for direct working-copy edits, PowerShell `-ExecutionPolicy Bypass` default (opt-out via env var), background session model + effort now preserved across idle · [Release](https://github.com/anthropics/claude-code/releases/tag/v2.1.143) _(May 15)_

## New Trust-Vetted GitHub Repos (TIER-1 SAFE)

_No TIER-1 repos today. 6 keyword-matched repos processed: 2 TIER-2 (age<30d, community validation returned 0 distinct third-party domains for both — default-deny), 4 TIER-3 SKIP (SEO-spam cluster ×2, no-license+suspicious-username, false-positive Nintendo game). See [filtered audit](../../briefs/) for details._

## Community-Validated (was TIER-2, web-confirmed safe)

_No community upgrades today. Both TIER-2 repos (nexu-io/html-anything ⭐2425, DenisSergeevitch/agents-best-practices ⭐568) are too new (5d and 1d respectively) — zero independent third-party testimonials indexed yet. Default-deny correctly engaged._

## Coding-AI Ecosystem News

### Releases

- **Cline CLI v3.0.4** — Light theme TUI color improvements (chat, status bar, syntax highlighting); fixes plugin tools failing in production npm builds · [Release](https://github.com/cline/cline/releases/tag/cli-v3.0.4) _(May 16)_
- **OpenAI Codex rust-v0.131.0-alpha.22** — Daily alpha shipping cadence continues; alpha.21 (May 15) + alpha.22 (May 16 00:21Z) both within 24h window · [Releases](https://github.com/openai/codex/releases) _(May 16)_
- **Cursor 3.3** — Build in Parallel (subagents for independent plan steps), Split PRs quick action, quick-action pills for pinned skills; May 11: Bugbot moves to usage-based billing · [Changelog](https://cursor.com/changelog) _(May 7)_

### Anthropic Business & Partnerships

- **Salesforce $300M Anthropic Token Commitment** — CEO Marc Benioff (All-In podcast): Salesforce expects ~$300M in Anthropic Claude token spend in 2026 for coding + product work; plans Claude integration inside Slack · [Source](https://letsdatascience.com/news/marc-benioff-announces-300m-anthropic-token-use-90d52de1) _(May 16)_
- **Anthropic 12 New Legal Plugins** — 12 AI plugins for specific legal practice areas (corporate, regulatory, employment law) deepening Claude's legal vertical · [Bloomberg Law](https://news.bloomberglaw.com/legal-ops-and-tech/anthropic-pushes-deeper-into-legal-work-with-claude-updates) _(May 16)_
- **Anthropic Valuation Approaching $1T** — Raising ~$30B round at ~$950B valuation; Alphabet's stake increasingly central to its AI strategy · [247 Wall St.](https://247wallst.com/investing/2026/05/15/anthropic-valuation-is-key-for-alphabet/) _(May 15)_
- **Anthropic 2028 AGI Leadership Paper** — Predicts AGI by 2028; two US-China scenarios; urges US export controls on chips + restrict frontier model access; recommends maintaining 12-24 month lead · [Anthropic Research](https://www.anthropic.com/research/2028-ai-leadership) _(May 15)_
- **Gates Foundation $200M Partnership** — Anthropic commits $200M over 4 years in grants, credits, and technical support for global health (polio/HPV/eclampsia), K-12 education, and agricultural AI tools · [Anthropic](https://www.anthropic.com/news/gates-foundation-partnership) _(May 14)_
- **PwC + Anthropic Enterprise Expansion** — Claude Code + Cowork deploying to 100K+ global PwC workforce; joint Center of Excellence; 30,000 professionals to be trained on Claude · [PwC](https://www.pwc.com/us/en/about-us/newsroom/press-releases/anthropic-pwc-expand-alliance-agentic-enterprise.html) _(May 14)_

### MCP Ecosystem

- **SurePath AI: Real-Time MCP Policy Controls** — Enterprise real-time governance layer for MCP-based AI agent actions; addresses audit and compliance gaps in MCP deployments · [PR Newswire](https://www.prnewswire.com/news-releases/surepath-ai-advances-real-time-model-context-protocol-mcp-policy-controls-to-govern-ai-actions-302709875.aspx) _(May 16)_

## Statistics

- Repos scanned: 100 raw (728 total available) · Keyword-matched: 6 · TIER-1 SAFE: 0 · TIER-2 community-validated: 0/2 · TIER-3 SKIP: 4
- News items raw: 11 (from 7 sources) · After cross-source dedup: 10 unique events · OpenAI Codex alpha.21+alpha.22 consolidated: 1 entry
- Sources reached: Claude Code releases.atom ✓ · Cline releases.atom ✓ · OpenAI Codex releases.atom ✓ · WebSearch (Anthropic/business news) ✓ · HN Algolia 403 · Reddit not attempted (scrapling unavailable in cloud)
- GitHub API: Unauthenticated (0/60 core rate limit after bulk search — per-repo metadata not available; used bulk search fields for classification)
- Run date: 2026-05-17 JST (UTC 2026-05-16)
