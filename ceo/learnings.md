# learnings.md — Claude Code Pulse CEO

> Append-only. Every daily run appends CEO-level cross-cutting observations. Read at START of next run BEFORE dispatching sub-agents. When pattern stabilizes (≥3× confirmed) → propose CEO-CLAUDE.md update OR cross-orchestra observation goes into `~/.claude/projects/-Users-Claude-Documents/memory/` as a new memory-entry.

## Format per Entry

```
## YYYY-MM-DD (run-N)

### Cross-Sub-Agent Patterns
- (e.g. "Both github_scraper and news_monitor had timezone-edge issues today → consider adding UTC-anchor convention to all sub-agent CLAUDE.mds")

### Trust-Tier Distribution
- TIER-1 SAFE direct: N
- TIER-2 NEEDS-REVIEW: M (of which K upgraded by community-validator)
- TIER-3 SKIP: J

### Cross-Source Dedup Stats
- Anthropic events deduped: X (e.g. RSS+GitHub+X same release)
- News-only events: Y
- GitHub-only events: Z

### Heuristic-Performance Observations
- Did current trust-tier-thresholds produce sensible results?
- Were any TIER-3-rejects actually safe (in retrospect)?
- Were any TIER-1-passes actually problematic?

### Self-Reflection (CEO behavior)
- Did I stay within hard-rules? (no auto-clone, TIER-1-only output, dedup)
- Did I make any decision without grounding in briefing?
- Was my Final Brief format clear + scannable?
- Bias-Watch: am I drifting toward "make Brief look fuller" over "safety-first"?

### Suggested CLAUDE.md Improvements (apply after 3× confirmation)
- Heuristic threshold tuning
- New dedup-rule
- New format-section
- Sub-agent dispatch ordering changes

### Cross-Orchestra Observations
- (Anything other Orchestras would benefit from? E.g. "stealthy_fetch on X-accounts works well — Trading-Orchestra could reuse for sentiment")
- (Goes to ~/.claude/projects/.../memory/ as new entry if persistent + cross-cutting)
```

---

## 2026-05-10 (run-0, Phase A+B-partial setup)

### Pre-First-Run Observations (from Phase B github_scraper live-test)

- **Heuristic Strictness:** All 7 keyword-matched repos today fall to TIER-2 (all young, mostly single-contributor). Community-validator will be heavily used. This is OK — it's the designed safety-recovery path.
- **last_commit_days = -1 bug:** Need to clamp `max(0, last_commit_days)` in CEO trust-tier function (timezone-edge between pushed_at-UTC and today-anchor-UTC).
- **False-Positive Class:** Non-English-description repo (Thai chat-app) keyword-matched but is Claude-Code-USER not Claude-Code-TOOL. Watch if this recurs across runs.
- **Subagent-Type Loading:** New `~/.claude/agents/*.md` definitions only available after Claude session restart. For mid-session live-tests, dispatch via `general-purpose` with inline-briefing. Production runs (via /schedule fresh-Claude) WILL load the new subagent_types correctly.

### Self-Reflection — Empfehlung-Bias caught by user

- **Bias observed:** I gave a "balance-feeling" Empfehlung (relax heuristic) without strong reasoning. User caught it: "Wieso empfiehlst du das? Ist es besser oder sagst du das einfach so?"
- **Root cause:** I anchored on "make Brief look fuller" (content-volume bias) instead of User's actual Q8-decision ("nur die guten in Brief = safety-first").
- **Correction applied:** Reverted to strict-heuristic recommendation. Acknowledged bias openly to user.
- **For future Empfehlungen:** Always re-check against User's stated decisions before recommending. Empfehlung-First Rule requires REAL **Warum:**, not template-balance.

### Architecture-Addition this Session

- **Self-Improvement Layer added:** Each sub-agent + CEO now has `learnings.md`. Each CLAUDE.md gets updated with Read-learnings-First + Reflect-at-End workflow steps. Self-Edit-Permission section defines when an agent may edit its own CLAUDE.md (3× confirmation rule).

### What Next-Run Should Do Differently

- Read all 4 learnings.md files (CEO + 3 sub-agents) BEFORE dispatching
- Apply the last_commit_days clamp in trust-tier function
- Watch for non-English-description false-positive recurrence
- Track validator-upgrade-rate over 7 days — adjust heuristic only if >70% TIER-2-stays-TIER-2 (signal that heuristic is too strict in practice)

---

## 2026-05-10 (run-1, Phase C+D end-to-end first complete run)

### Cross-Sub-Agent Patterns

- **Single-author + viral signal pattern (4× confirmed today across mirage / Agent_Memory / ServiceGraph / openpets):** All 4 explicitly community-validated repos failed upgrade for the SAME root-cause: ≥2-domain threshold sometimes met (mirage 6, openpets 5) but ZERO POSITIVE-keyword-matches because all third-party coverage was launch-marketing/announcement-style, not user-testimonials. Pattern: "viral young repo" produces ≠ "trusted young repo". Default-deny correctly engaged in all 4 cases — the rule-design works.
- **Dormant Tier-1 sources (3× confirmed today across Aider / Continue / Cline):** Aider stuck Feb 2026, Continue March, Cline 1 release May 1. Daily-monitor cadence wasted 0 entries × 3 sources today. Suggested CLAUDE.md improvement: drop to weekly-check OR retire until activity resumes — held for ≥3× confirm.

### Trust-Tier Distribution

- TIER-1 SAFE direct: 0
- TIER-2 NEEDS-REVIEW: 7 (of which 0 upgraded by community-validator — 4 explicit + 3 inferred-default-deny)
- TIER-3 SKIP: 0

### Cross-Source Dedup Stats

- Anthropic events deduped: 0 (no duplicates today — releases were unique per-channel)
- Codex CLI consolidation: 5 alpha-releases → 1 brief-entry (5:1 dedup ratio — high signal-savings)
- News-only events: ~10 unique (after dedup from 38 raw)
- GitHub-only events: 0 (all 7 candidates filtered)

### Heuristic-Performance Observations

- **Stars-≥-100 threshold:** 7 of 7 candidates today met this. Threshold is not the constraint.
- **Age-≥-30d threshold:** 0 of 7 met this. THIS is the binding constraint — all trending-7-days-window repos are by definition <7d old. Confirms design-intent: "trending" + "trust-vetted" are mutually exclusive without community-validation.
- **License-present:** 7 of 7 had license (Apache-2.0 or MIT). Not a discriminator today.
- **Contributors-≥-2:** 2 of 7 met this (alvinunreal/openpets, alvinunreal/claude-pets — both same author across 2 repos). Even 2-contributor cases are typically same-author-with-bot. May want to inspect "distinct human authors" not "raw contributors count" in V2.
- **Closed-issues-ratio-≥-0.4:** Only mirage met this (0.6 = 6/(6+10)) but failed on age. Most young repos have 0 issues = ratio 0.0.

### Self-Reflection (CEO behavior)

- ✓ Stayed within hard-rules: no auto-clone, TIER-1-only output, cross-source dedup applied, no file deletion
- ✓ All decisions grounded in briefing (trust-tier-classification deterministic, dedup heuristic explicit)
- ✓ Final Brief format clear + scannable — section-headers, statistics-block, link-rich
- ⚠️ Bias-Watch: NONE drifted today — Brief shows "0 TIER-1 today" honestly instead of relaxing heuristic
- ⚠️ Empfehlung-Bias caught earlier in session (recommended Option A heuristic-relax without strong reasoning, User caught it). Self-correction: revised to Option B strict, acknowledged bias openly. Lesson for future: re-check Empfehlungs against User's stated decisions before recommending. Empfehlung-First Rule requires REAL **Warum:**, not template-balance.

### Suggested Local-CLAUDE.md Improvements (apply after 3× confirmation)

- **[1× obs]** "Distinct-human-authors" metric instead of "contributors" — current rule treats author-with-bot as multi-contrib
- **[1× obs]** Press-coverage vs user-adoption sentiment buckets (community_validator suggestion bubbled up via patterns observed today)

### Suggested Global-File Proposals

- **[1× obs already proposed-doc]** Subagent-loading session-restart constraint → proposals/2026-05-10_subagent-loading-needs-session-restart.md (low-confidence pending more confirms)
- **[1× obs]** Browser-Autoplay-Pre-Unlock pattern from Orion build (referenced in Memory `feedback_browser_autoplay_pre_unlock.md`) suggests there are similar harness-behavior rules accumulating — consider quarterly-review of `~/.claude/projects/.../memory/` for "operational constraints" that should bubble to ~/.claude/CLAUDE.md

### What Next-Run Should Do Differently

- Apply the 4 factual-fixes already made to news_monitor/CLAUDE.md (status URL, news RSS-fallback, Reddit→scrapling tool-mapping, HN search_by_date, MCP word-boundary)
- Try the news_monitor improvements to see if Anthropic-blog-fallback HTML-scrape works better than the 404 RSS
- Watch if Dormant-Tier-1-sources (Aider/Continue/Cline) confirm pattern 3× → propose retirement
- Consider adding "press-coverage" sentiment-bucket suggested by community_validator (3× confirmed across runs today within the SAME run = need cross-run confirmation, not within-run)
- Continue building Phase E (Dashboard 7th Section) so daily output is visually surfaced

### Cross-Orchestra Observations

- **Pattern:** Sub-agent live-testing requires session-restart for new subagent_types (or general-purpose-with-inline-briefing workaround). This is harness-behavior, applies to ALL Orchestra-builds. Surfaced as Proposal-Doc.
- **Pattern:** mcp__scrapling for Reddit (WebFetch blocked) — ALSO useful for Trading-Orchestra sentiment-analysis IF they want to scrape Reddit/X. Logged but not surfaced as separate proposal yet (Trading-Orchestra hasn't asked).
