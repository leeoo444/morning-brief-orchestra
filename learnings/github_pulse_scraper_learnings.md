---
type: agent_learnings_ace
agent: github_pulse_scraper
schema_version: 1.0
last_curated: 2026-05-10
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
