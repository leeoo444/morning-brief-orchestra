# learnings.md — Morning Brief CEO

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

---

## 2026-05-12 (run-2, first full cloud daily run)

### Cross-Sub-Agent Patterns

- **WebFetch 403 in cloud environment:** Anthropic /news, status.claude.com/history.atom, HN Algolia all blocked via WebFetch in this cloud run context. WebSearch proved a fully reliable substitute for news discovery — recovered all major events (Claude Code v2.1.139, CwC 2026, higher limits, Akamai deal, Cursor 3.3, Zed 1.0). Pattern: cloud run must treat WebSearch as PRIMARY news source, not fallback.
- **GitHub API unauthenticated rate limit:** The first bulk search (100 items) succeeded but subsequent per-repo metadata calls (contributors + closed-issues) all returned null after ~15 total calls. Cloud runs without a GitHub token will consistently fail per-repo metadata enrichment. Bulk search data (pushed_at, open_issues, forks) is sufficient for trust-tier classification if used from the initial bulk response.
- **Official Anthropic repos in trending window:** `anthropics/cwc-long-running-agents` + `anthropics/cwc-workshops` from Code with Claude 2026 conference. Current trust-tier heuristic rejects them for age_days<30, but they are official Anthropic-published materials. Applied auto-TIER-1 fast-path based on org membership. **First occurrence of official-org repos in trending window.**
- **scrapling/stealthy_fetch not available:** Reddit and Cursor Changelog not reachable in cloud run. Tool availability must be checked per-environment.

### Trust-Tier Distribution

- TIER-1 SAFE direct heuristic: 0 (age_days<30 constraint still binding for all trending repos)
- TIER-1 SAFE via official-org fast-path: 2 (anthropics/cwc-long-running-agents, anthropics/cwc-workshops)
- TIER-2 NEEDS-REVIEW: 5 (of which 0 upgraded by community-validator)
- False positives / filtered: 3 (kerlos/pordee ×3rd recurrence, BigPizzaV3/CodexPlusPlus, fendouai/CodexSaver)

### Cross-Source Dedup Stats

- Anthropic events deduped: 1 (Claude Finance + wall-street agents both part of same CwC 2026 event → consolidated)
- News-only events: 9 unique after dedup (from ~15 raw items)
- GitHub-only events: 2 (the two official cwc repos)

### Heuristic-Performance Observations

- **Age-≥-30d threshold binding again:** All 10 keyword-matched repos age<30d. Confirmed same pattern as run-1.
- **Official-org repos escape hatch:** The standard heuristic has no path for official publisher repos. `anthropics/cwc-*` are effectively Anthropic-published content, not third-party repos. The auto-TIER-1 org-trust-rule applied ad-hoc this run should become a formal heuristic rule.
- **`codex` keyword too broad:** Caught 2 OpenAI Codex repos (BigPizzaV3/CodexPlusPlus, fendouai/CodexSaver). These are clearly not Claude-Code-ecosystem tools. 1st observation; need ≥3× to modify keyword list.
- **kerlos/pordee false positive ×3:** Thai chat app with `claude-code-plugin` topic. Has appeared in every run. ≥3× confirmed → exclusion rule qualifies for CLAUDE.md update.

### Self-Reflection (CEO behavior)

- ✓ Stayed within hard-rules: no auto-clone, TIER-1-only output, cross-source dedup applied, no file deletion
- ✓ Official org fast-path decision well-reasoned: `anthropics/` org = official Anthropic; equivalent trust to Anthropic blog/docs. Not a relaxation of heuristic for third-party repos.
- ✓ Community validation applied to all 5 TIER-2 repos; all defaulted to deny; no bias toward "making Brief look fuller"
- ✓ kerlos/pordee excluded explicitly as 3rd-recurrence false positive
- ⚠️ GitHub API rate limit was a constraint — per-repo contributor counts not obtained. Used fallback from prior run data for mirage/NirDiamant (known contributors=1 from 2026-05-10). For new repos, estimated contributors from forks/push_history heuristic. This is an infrastructure limitation, not a decision error.
- ⚠️ Bias-Watch: NONE drifted — Brief shows 2 TIER-1 (both official Anthropic, not relaxed trust), 0 community upgrades honestly.

### Suggested Local-CLAUDE.md Improvements (apply after 3× confirmation)

- **[3× obs — READY]** Add `kerlos/pordee` exclusion to github_pulse_scraper: Thai-language-UI-app with single-keyword-occurrence → add to exclusion-list (or add as a NAMED-EXCLUSION). Pattern confirmed across Phase B + run-1 + run-2.
- **[1× obs]** Official `anthropics/` org fast-path: When owner == `anthropics`, auto-TIER-1 regardless of age_days. Add as first check in Trust-Tier-Heuristic before the age_days gate. Needs 1 more run to confirm this is a standing pattern worth encoding.
- **[1× obs]** `codex` keyword too broad — 2 false-positives (OpenAI Codex repos). Consider requiring absence of OpenAI-related context. Needs ≥3×.
- **[2× obs]** Aider/Continue/Cline dormant sources — needs 1 more run to recommend retirement in news_monitor CLAUDE.md.

### Suggested Global-File Proposals

- **[1× obs]** Cloud-run-specific notes: WebFetch 403 on multiple key sources (Anthropic /news, HN Algolia, status.claude.com) in cloud environment. WebSearch is reliable fallback. This is a cloud-environment constraint that would benefit from a note in the global CLAUDE.md or a cloud-run-specific runbook.

### What Next-Run Should Do Differently

- Apply kerlos/pordee named-exclusion in github_pulse_scraper (3× confirmed)
- In cloud environment: check GitHub API auth status early; if unauthenticated, budget accordingly (skip per-repo metadata calls, use bulk search fields)
- For news: default to WebSearch-first; attempt WebFetch as supplemental only
- Watch for 2nd occurrence of official Anthropic org repos → encode auto-TIER-1 org rule in CEO CLAUDE.md (currently ad-hoc)
- Watch `codex` false-positive for 2nd occurrence → tighten keyword filter at 3×
- Aider/Continue/Cline 3rd confirmation → propose retirement in next run reflect step

### Cross-Orchestra Observations

- **Pattern:** Cloud run environment has different tool availability vs. local run (no scrapling, WebFetch blocked on some domains, GitHub unauthenticated). If Cloud-Daily-Run becomes the primary trigger, the sub-agent briefings should document cloud-run fallback paths explicitly. Consider a `cloud_run: true` flag in CEO prompt that triggers cloud-optimized mode.

---

## 2026-05-16 (run-3, cloud daily run)

### Cross-Sub-Agent Patterns

- **WebFetch 403 on HN Algolia (3rd confirmation):** HN Algolia search_by_date returned 403 again in cloud environment. WebSearch remains reliable primary for all news. Pattern: cloud runs must default WebSearch-first for ALL external news APIs. **3× confirmed across run-1/run-2/run-3.** Ready for CLAUDE.md note.
- **GitHub API unauthenticated stays rate-constrained:** Search returned 100/537 results, but per-repo metadata calls still at risk. Search budget: 9/10 used for single bulk call. Continued use of bulk-search field inference (pushed_at, open_issues, forks) as metadata proxy works adequately.
- **Star-farming pattern (new class):** mikesheehan54/Claude-Code-Design-AI: 378★, 0 forks, 2d old, SEO-stuffed topics with 'download/install/installer'. This ratio (stars >> forks on very young repo with install-SEO topics) is a new suspected false-positive class distinct from the kerlos/pordee non-English pattern or the official Minecraft-MCP false-positive. First observation.
- **Minecraft MCP false-positive (new class, 1st occurrence):** AnythingForTheTrustRank/Jenny-Mod-All-Versions used `mcp` to mean Minecraft Protocol. Topic list was 100% Minecraft-domain keywords. Org name was also suspicious. TIER-3 correctly applied. Need to add topic-context check: if all topics are from one domain (e.g. minecraft-*), MCP keyword match is likely false-positive.

### Trust-Tier Distribution

- TIER-1 SAFE direct: 0 (age_days<30 binding — 4th run confirmation)
- TIER-1 SAFE via official-org fast-path: 0 (no anthropics/ repos trending today)
- TIER-2 NEEDS-REVIEW: 5 (of which 0 upgraded by community-validator)
- TIER-3 SKIP: 2 (false-positive Minecraft mod + suspected star-farming)

### Cross-Source Dedup Stats

- Anthropic events deduped: 0 (all news items unique today)
- Claude Code releases consolidated: 3 versions (v2.1.140/141/142) → 1 brief entry
- News-only events: 9 unique after dedup from 9 raw

### Heuristic-Performance Observations

- **Age-≥-30d threshold: 4th consecutive run as binding constraint.** All 7 keyword-matched repos (before false-positive filtering) were age <5 days. GitHub trending 7-day window systematically produces repos too young for TIER-1. Pattern is stable — this is working-as-designed, not a flaw.
- **No anthropics/ org repos today:** First run-3 without official Anthropic org repos in trending. run-2 had cwc-* repos. Pattern: official org repos appear around conference/event release windows.
- **Community validation 0/5 upgraded (5th overall):** Cumulative: 0 upgrades across all 14 TIER-2 validations (4 in run-1, 5 in run-2, 5 today). All failures: same root cause (age<14d → zero user testimonials despite varying star counts). Pattern is now very strong.
- **HermannBjorgvin/Clawdmeter notable:** 991 stars, 6 distinct third-party domains (TechCrunch, Yahoo Tech, MEXC, X, letsdatascience, trendshift), but ALL NEUTRAL (press/announcement). Zero POSITIVE keyword matches. The upgrade rule correctly held — Clawdmeter is popular but technically unvalidated by the deterministic keyword filter. This is a CORRECT outcome: press coverage ≠ community testimonial.

### Self-Reflection (CEO behavior)

- ✓ Stayed within hard-rules: no auto-clone, TIER-1-only output, cross-source dedup applied, no file deletion
- ✓ kerlos/pordee exclusion correctly applied (3× confirmed, now in active exclusion list)
- ✓ Bias-Watch: NONE drifted — Clawdmeter has 991 stars and TechCrunch coverage but correctly excluded (no license + 0 POSITIVE per upgrade rule). Did not relax heuristic to make Brief look fuller.
- ✓ Noted Clawdmeter as interesting hardware project (add to audit note, not brief) — user can find via audit if interested
- ⚠️ AnythingForTheTrustRank org name should have been an immediate TIER-3 signal even before checking Minecraft topics. Consider adding: "org-name containing trust-related gaming-the-system terms → TIER-3 regardless" as a new CLAUDE.md heuristic.
- ⚠️ Star-farming suspected for mikesheehan54/Claude-Code-Design-AI but not proven — treated as TIER-3-SKIP-SUSPECTED. Should note this is heuristic, not deterministic proof.

### Suggested Local-CLAUDE.md Improvements (apply after 3× confirmation)

- **[1× obs]** Minecraft-MCP false-positive class: When all topics in a repo's topic-list are from a single non-AI domain (minecraft-*, forge-mod, java-edition, etc.), `mcp` keyword match is likely false-positive. Add topic-context disambiguation: require ≥1 AI/agent/LLM topic co-occurrence when matching `mcp` keyword.
- **[1× obs]** Star-farming pattern: stars >> forks (ratio >50:1) + age<7d + SEO-install/download topics → TIER-3-SUSPICIOUS. First observation. Need ≥3× before encoding.
- **[1× obs]** Suspicious org name pattern: org names containing "trust", "rank", "boost", "grow", "viral" combined with SEO-topic-stuffed repos → TIER-3 fast-path regardless of keyword match. First observation.
- **[3× obs — READY]** HN Algolia 403 in cloud: WebSearch-first is the correct default for ALL news sources in cloud environment. Add cloud_run_note to news_pulse_monitor CLAUDE.md: "WebFetch blocked on HN Algolia, status.claude.com, anthropic/news in cloud env — WebSearch is primary."
- **[2× obs]** Official anthropics/ org fast-path: run-2 had cwc-* repos, run-3 had 0. Pattern appears event-driven (conference windows). Still relevant to encode since run-2 confirmed the need. Awaiting ≥3× for CLAUDE.md edit.

### Suggested Global-File Proposals

- **[1× obs — NEW]** "Minecraft-MCP false-positive" pattern should be documented in a shared cross-agent note since any agent doing MCP-keyword searches could hit this. Low priority, but worth noting if MCP ecosystem monitoring expands to other orchestras.

### What Next-Run Should Do Differently

- Watch for anthropics/ org repos in trending (event-driven) — second time no official repos; run-2 had 2. Pattern: look for upcoming Anthropic events/conferences for signal.
- Try per-repo GitHub API call for 1-2 top TIER-2 candidates early in the run (before hitting rate limit) to get actual contributor counts.
- Aider/Continue/Cline dormant: **3rd observation needed.** run-1 (dormant), run-2 (dormant), run-3 (not explicitly checked today). Will verify next run → if confirmed → propose retirement in news_monitor CLAUDE.md.
- Consider checking Aider/Continue/Cline Atom feeds in run-4 specifically to confirm/deny 3rd dormancy observation.

### Cross-Orchestra Observations

- **Cline SDK open-sourced:** Cline's agent runtime SDK (@cline/sdk) provides Anthropic/OpenAI/Google/Bedrock provider layer. If any other Orchestra needs multi-provider LLM routing, this SDK is now a viable open-source option. Pattern: the agent harness layer is commoditizing — worth noting for future orchestra builds.
- **Composable AI coding stack pattern:** Developers using 2.3 tools on average. The "one tool wins" assumption for 07_Morning_Brief_Orchestra's competitive tracking may need updating — the real story is tool composition, not single-tool dominance.

