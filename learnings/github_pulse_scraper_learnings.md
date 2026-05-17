---
type: agent_learnings_ace
agent: github_pulse_scraper
schema_version: 1.0
last_curated: 2026-05-12
---

# Insights (ACE-structured — read top-3 active by helpful_count DESC at Process Step 2)

- insight_id: ins_001
  insight: "Clamp last_commit_days = max(0, computed) — pushed_at can be slightly future vs today-anchor-UTC, causing negative values"
  helpful_count: 1
  harmful_count: 0
  last_applied: 2026-05-10
  sources:
    - dispatch_2026-05-10_phase_b_first_run
  ace_status: active
  added: 2026-05-10
  why: "2 of 7 repos in 2026-05-10 first-run had last_commit_days=-1 due to UTC midnight-anchor boundary"

- insight_id: ins_002
  insight: "Non-English-description repo with single keyword-occurrence is high-risk false-positive (Thai chat-app pattern)"
  helpful_count: 0
  harmful_count: 0
  last_applied: 2026-05-10
  sources:
    - dispatch_2026-05-10_phase_b_first_run
  ace_status: active
  added: 2026-05-10
  why: "kerlos/pordee Thai-language chat-app keyword-matched 'claude-code' but is Claude-Code-USER not Claude-Code-TOOL — needs ≥3× confirmation before becoming exclusion-rule"

- insight_id: ins_003
  insight: "Strict TIER-1 heuristic + 7-day-trending-window are mutually exclusive — all young repos go TIER-2 by design"
  helpful_count: 1
  harmful_count: 0
  last_applied: 2026-05-10
  sources:
    - dispatch_2026-05-10_phase_b_first_run
  ace_status: active
  added: 2026-05-10
  why: "User-Q8 explicit: safety > content-volume. Don't relax heuristic to make Brief look fuller. Community-Validator handles upgrade-path."

# Historical Run Notes (narrative — context preservation)

> Format below is original narrative pre-ACE. Append new run-narratives here AND extract structured insights to # Insights section above.

---

# learnings.md — github_pulse_scraper

> Append-only. Every run appends Findings, Edge-Cases, False-Positives, Suggested-Improvements. Read at START of next run BEFORE executing. When pattern stabilizes (≥3× confirmed) → propose CLAUDE.md update via Self-Edit-Trigger section.

## Format per Entry

```
## YYYY-MM-DD (run-N)

### Findings
- ...

### Edge-Cases Encountered
- ...

### False-Positives (keyword-matched but irrelevant)
- ...

### Tool-Call Issues
- ...

### Suggested CLAUDE.md Improvements (apply after 3× confirmation)
- ...

### What Next-Run Should Do Differently
- ...
```

---

## 2026-05-10 (run-1, first live-test)

### Findings

- **Trending Search Volume:** `gh api search/repositories created:>2026-05-03 stars:>20` returned 100 raw items (capped at API max=100, total available was 570)
- **Keyword-match Yield:** 8 matches out of 100 raw (8% signal). Most "trending stars>20 last 7d" repos are NOT Claude-Code-ecosystem
- **Kept after exclusion:** 7 (1 excluded: `awesome-agentic-ai-zh` filtered by `awesome-*` pattern)
- **Tool-budget actual:** 15 calls (1 search + 7×2 metadata) — well under 101 budget
- **Real Claude-Code finds today:** `strukto-ai/mirage` ⭐1683 (Apache-2.0, agent-sandbox), `NirDiamant/Agent_Memory_Techniques` ⭐253 (anthropic topic, agent-memory cookbook), `kerlos/pordee` ⭐216 (false-positive — Thai chat-app keyword-matched "claude-code")

### Edge-Cases Encountered

- **`last_commit_days = -1` for 2 repos:** When `pushed_at` is slightly in the future relative to UTC midnight anchor (e.g. `pushed_at: 2026-05-10T02:00Z` vs anchor `2026-05-10T00:00Z`). CEO trust-tier function should clamp `last_commit_days = max(0, computed)`.
- **Single-Contributor Hot-Repos:** `strukto-ai/mirage` ⭐1683 in 3 days has only 1 contributor + Apache-2.0 license. Strict TIER-1 heuristic (contributors≥2) excludes this. → Goes to TIER-2 → community_validator MUST handle this kind of case effectively.
- **Topic-list-Heavy Repos:** `NirDiamant/Agent_Memory_Techniques` has 20 topics including `anthropic`, `langchain`, `openai`, `mem0`, `letta` — broader than just Claude-Code-ecosystem. Keyword-match was correct but repo is more general "AI agent memory cookbook" than Claude-specific. CEO should consider in classification whether topics show Claude-Code-PRIMARY-focus vs Claude-Code-INCLUDED.

### False-Positives (keyword-matched but irrelevant)

- **`kerlos/pordee` ⭐216** — Thai-language chat app for "answering things briefly". Keyword-match was likely on the description's mention of `claude-code` integration as one of MANY tools. Not really a Claude-Code-ecosystem TOOL, more a Claude-Code-USER. Filter idea: if description-language is non-English AND keyword-occurrence is ≤1, deprioritize.

### Tool-Call Issues

- None — gh CLI worked perfectly. 5000/hour rate limit was nowhere near.

### Suggested CLAUDE.md Improvements (apply after 3× confirmation)

- **[1× obs]** Add "non-English-description + single-keyword-occurrence" deprioritization heuristic to exclusion-list (case: kerlos/pordee). NOT yet applied — needs 3× confirmation across multiple runs that this is a recurring false-positive class.
- **[1× obs]** Consider adding new keyword `agent-memory` to Optional-Context-Keywords list (case: NirDiamant repo legitimately Claude-Code-relevant via this).

### What Next-Run Should Do Differently

- Same approach. Heuristic working as designed. Watch for: (a) timezone clamping needed if CEO doesn't handle, (b) recurring false-positive patterns, (c) does community_validator actually upgrade trending-young repos?
- After 7 days of data: review if 8% keyword-match-rate is stable, or if seasonal (e.g. higher around AI-conference releases).

### Meta-Learning for Future Self

- **Heuristic-Tradeoff is real:** Strict TIER-1 means trending repos must wait for community-validation. This IS the design (User-decision Q8). Don't relax heuristic to "make Brief look fuller" — that's bias toward content-volume over safety.

---

## 2026-05-12 (run-2, cloud daily run)

### Findings

- **Trending Search Volume:** GitHub unauthenticated API search returned 611 total matching repos (>100 available), 100 fetched.
- **Keyword-match Yield:** 10 matches (7 from core keyword-match + 3 spotted in raw list that matched via description). Core matches via API jq filter: 7.
- **Official Anthropic repos in trending window:** `anthropics/cwc-long-running-agents` ⭐268 and `anthropics/cwc-workshops` ⭐173 — both created 2026-05-06, published as Code with Claude 2026 conference materials.
- **False-positive recurrence:** `kerlos/pordee` ⭐239 appeared again (3rd occurrence: Phase B + run-1 + run-2). Qualifies for ≥3× exclusion-rule addition.
- **New false-positive class:** `BigPizzaV3/CodexPlusPlus` + `fendouai/CodexSaver` — OpenAI Codex tools matching `codex` keyword. 1st observation of this pattern.

### Edge-Cases Encountered

- **GitHub API rate-limit hit:** Unauthenticated per-repo metadata calls (contributors + closed-issues) all returned null after ~10–15 calls total. The first bulk search succeeded (100 items), but subsequent detail calls failed. Cloud runs without `gh auth` token will consistently hit this wall for per-repo metadata.
- **`anthropics/cwc-*` repos had null description in API:** Both official repos had empty `description` field in the GitHub API response. Context was recovered via WebSearch.

### False-Positives (keyword-matched but irrelevant)

- **`kerlos/pordee`** ⭐239 — Thai-language chat app. 3rd recurrence (Phase B 2026-05-10 + run-1 2026-05-10 + run-2 2026-05-12). **≥3× threshold reached → qualify for exclusion-list addition.** CEO should edit this CLAUDE.md.
- **`BigPizzaV3/CodexPlusPlus`** ⭐1006 — OpenAI Codex App enhancement. `codex` keyword is too broad — catches OpenAI Codex repos. 1st observation.
- **`fendouai/CodexSaver`** ⭐420 — OpenAI Codex + DeepSeek cost optimizer. Same pattern. 1st observation.

### Tool-Call Issues

- GitHub unauthenticated API limited after ~15 calls — contributors/closed-issues metadata not fetched. Cloud run environment lacks `gh auth` token. Per-repo metadata partially inferred from first bulk JSON (pushed_at, open_issues available from search result).

### Suggested CLAUDE.md Improvements (apply after 3× confirmation)

- **[3× obs — READY]** Add `kerlos/pordee` (or generic "Thai-language-UI-app" pattern) to exclusion list. Rule: repos with non-English description + single-keyword-occurrence + no English readme → deprioritize/skip.
- **[1× obs]** `codex` keyword in keyword-filter is too broad — catches OpenAI Codex repos. Consider requiring "codex-cli" OR "openai/codex" to be absent when matching `codex` keyword, OR restrict codex keyword to only match when title/topics contain "claude" co-occurrence.
- **[1× obs]** Official `anthropics/` org repos should have an auto-TIER-1 fast-path in trust-tier heuristic (CEO-level rule). Current heuristic rejects them due to age_days<30 even though they're official Anthropic-published materials.

### What Next-Run Should Do Differently

- **If running unauthenticated (no gh token):** Pre-budget for rate-limit; use the bulk search result's included fields (open_issues, pushed_at, forks) for classification, skip per-repo metadata calls OR batch them early before hitting limit.
- Apply the `kerlos/pordee` exclusion (Thai-language pattern) — 3× confirmed.
- Watch `codex`-keyword false-positive class for 2nd occurrence.

### Meta-Learning for Future Self

- **Official org fast-path needed:** When `anthropics/` org repos appear in trending, they should not need to wait 30 days to become TIER-1. These are official Anthropic-published materials. CEO should apply org-trust rule.

---

## 2026-05-18 (run-3, cloud daily run)

### Findings

- **Trending Search Volume:** 613 total repos matching (>20 stars, created since 2026-05-11), 100 fetched.
- **Keyword-match Yield:** 7 repos (5 from filter + 2 added post-filter for agent-skills: yetone/native-feel-skill + WantongC/journal-adapt-writing-skill)
- **No official anthropics/ org repos in trending window:** curl org:anthropics created:>2026-05-11 returned []. First run without an official Anthropic org repo. (Observation: cwc-* repos from run-2 were event-driven; not a steady-state pattern.)
- **Spam cluster detected ×2:** BharathKumarSuresh/claude-design-system-hooks + mikesheehan54/Claude-Code-Design-AI share IDENTICAL topic set including claude-cowork-free, claude-design-download, claude-design-free, claude-design-install, claude-design-installer. Both repo authors pushed within 10-15 minutes of creation = near-zero development activity. **First confirmation of coordinated spam-cluster pattern.**
- **kerlos/pordee:** Absent from this run's bulk results. Exclusion not needed this run, but exclusion-rule correctly in place.

### Edge-Cases Encountered

- **Agent Skills repos not caught by keyword filter:** yetone/native-feel-skill (description = "Agent Skill for..." but no 'claude', 'anthropic', 'mcp' keyword) and WantongC/journal-adapt-writing-skill (matched via `claude` topic but not description keyword). Post-filter addition required. Suggests adding "agent-skill" OR "agent-skills" as required-keyword.
- **GitHub API rate-limit confirmed again (3rd run):** Unauthenticated per-repo metadata returns null for contributors/closed-issues. Cloud runs consistently hit this wall. Bulk search fields sufficient for classification.

### False-Positives (keyword-matched but irrelevant)

- **`PlaceNL2026/best-of-algorithmic-trading`** ⭐214 — only has `mcp` topic but content is algorithmic trading. 1st occurrence of `mcp` false-positive on trading repos.
- **BharathKumarSuresh/claude-design-system-hooks + mikesheehan54/Claude-Code-Design-AI** — spam cluster (coordinated SEO topic-stuffing). **1st confirmed spam-cluster pattern across 2 repos in same network.**

### Suggested CLAUDE.md Improvements (apply after 3× confirmation)

- **[1× obs — NEW PATTERN]** Spam-cluster detection: when ≥2 repos in same trending window share ≥5 identical topics from a "download/free/install/installer/alternative" set → flag entire cluster as TIER-3 SPAM, don't validate individually.
- **[1× obs]** Add "agent-skill" OR "agent-skills" as explicit required-keyword to catch yetone/native-feel-skill class — Agent Skills repos often don't mention "claude" in description but are clearly Claude-Code-ecosystem.
- **[2× obs]** Official `anthropics/` org fast-path: run-2 (2 repos) + observation this run (absent but noteworthy). Not 3× yet — pattern is "occurs when Anthropic runs events". Hold.
- **[1× obs]** `mcp` topic matched algorithmic trading list. Consider requiring "mcp" keyword to co-occur with "claude" OR "anthropic" OR "model-context-protocol" in description/name, not just topics.

### What Next-Run Should Do Differently

- Add "agent-skill" OR "agent-skills" as a required-keyword in the filter to catch Agent Skills ecosystem repos that don't use 'claude/anthropic/mcp' in description.
- Watch for spam-cluster pattern recurrence (coordinated topic stuffing + zero dev activity within same day) — 1× observed today.
- Watch `codex` keyword for 2nd false-positive if OpenAI Codex repos reappear.
- GitHub unauthenticated API: confirmed pattern (run-2, run-3). Accept null for contributors/closed_issues in cloud runs; use only bulk fields.
