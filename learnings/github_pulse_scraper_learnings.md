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
  insight: "kerlos/pordee exclusion applied (4× confirmed). Broader: non-English-description repo with single keyword-occurrence + exclusion-list = effective filter."
  helpful_count: 3
  harmful_count: 0
  last_applied: 2026-05-13
  sources:
    - dispatch_2026-05-10_phase_b_first_run
    - run_2026-05-10_run1
    - run_2026-05-12_run2
    - run_2026-05-13_run3
  ace_status: active
  added: 2026-05-10
  why: "kerlos/pordee appeared 4× and was correctly excluded via exclusion-list. Exclusion-list mechanism working as designed."

- insight_id: ins_004
  insight: "Keyword substring-collision: bare `kw in text` fails for keywords that are common English substrings (e.g. 'zed' in 'personalized'). Use word-boundary regex."
  helpful_count: 0
  harmful_count: 1
  last_applied: null
  sources:
    - run_2026-05-13_run3
  ace_status: active
  added: 2026-05-13
  why: "DenAB-NVS/submarine matched 'zed' keyword via 'personalized-ai-layer' topic substring. Fix: use re.search(r'\\bzed\\b', text, re.IGNORECASE) same as MCP word-boundary rule. Hold for ≥3× before CLAUDE.md edit."

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

## 2026-05-13 (run-3, cloud daily run)

### Findings

- **Trending Search Volume:** GitHub unauthenticated API returned 542 total matching repos (stars>20, created>2026-05-06), 100 fetched.
- **Keyword-match Yield:** 7 matches (6 kept after exclusions). 6% signal rate.
- **kerlos/pordee exclusion applied:** Repo appeared again (4th occurrence). Exclusion-list successfully blocked it — no manual review needed.
- **New false-positive class: keyword-substring-collision:** `DenAB-NVS/submarine` matched keyword `zed` via substring in topic `personalized-ai-layer` ("personali**zed**"). This is a keyword-filter bug. `zed` as bare substring match on topics/names catches any word containing "zed" (e.g., "personalized", "customized"). **Fix needed:** use word-boundary check `\bzed\b` for the `zed` keyword, same as MCP rule.
- **No official anthropics/ org repos in trending window** — unlike run-2 (cwc-long-running-agents, cwc-workshops). Zero new anthropics repos created this week.
- **Notable repos found (all TIER-2):** HermannBjorgvin/Clawdmeter (ESP32 Claude Code usage dashboard — Adafruit-featured); bidah/skill-set (Claude Code skills collection); nai0om/buddhist-method (Buddhist epistemic principles as Claude Code skill).

### Edge-Cases Encountered

- **`zed` keyword substring-collision:** "personalized" contains "zed" → DenAB-NVS/submarine matched. Fix: word-boundary `\bzed\b`. **Confirmation count: 1.** Hold for ≥3×.
- **GitHub API unauthenticated rate limit (3rd run confirmation):** Per-repo metadata calls not attempted (learned from run-2). Used bulk search fields only. Pattern fully confirmed.

### False-Positives (keyword-matched but irrelevant)

- **`kerlos/pordee`** ⭐(age unknown, same Thai chat-app) — 4th occurrence (Phase B + run-1 + run-2 + run-3). Exclusion-list applied correctly. No manual review.
- **`DenAB-NVS/submarine`** — `zed` keyword-filter-bug (substring "personalized"). Excluded as FALSE_POSITIVE. **1st occurrence of this specific pattern.** Different from OpenAI-Codex false-positive class (run-2).

### Tool-Call Issues

- GitHub API unauthenticated — per-repo metadata (contributors, closed_issues) unavailable. Third confirmation; fully accounted for in pipeline.

### Suggested CLAUDE.md Improvements (apply after 3× confirmation)

- **[3× obs — READY]** `kerlos/pordee` exclusion already in list since run-2 learning; working correctly.
- **[1× obs]** `zed` keyword needs word-boundary: use `\bzed\b` regex instead of bare substring. Prevents "personalized", "customized" etc. matching. **Confirmation count: 1.** Hold for ≥3×.
- **[2× obs]** `anthropics/` org fast-path not triggered this run (no new org repos). Still not ≥3× — hold.
- **[3× obs — CARRY]** Unauthenticated GitHub rate-limit pattern confirmed ×3. This is a cloud-environment infrastructure fact; no heuristic change needed, but briefing could note "skip per-repo metadata in cloud runs."

### What Next-Run Should Do Differently

- Word-boundary `\bzed\b` for zed keyword (avoid substring false-positives).
- Continue watching anthropics/ fast-path for 3rd occurrence.
- Watch `codex` keyword false-positive (was 1st obs in run-2) — if another Codex-CLI/OpenAI repo appears, that's ×2.

### Meta-Learning for Future Self

- **Keyword substring-collision is a new false-positive class** (distinct from run-2's OpenAI-Codex false-positive class). The `zed` match bug hit "personalized" — an obvious fix. Always check word-boundary when keywords are common English substrings.
