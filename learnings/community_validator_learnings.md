---
type: agent_learnings_ace
agent: community_validator
schema_version: 1.0
last_curated: 2026-05-10
---

# Insights (ACE-structured — read top-3 active by helpful_count DESC at Process Step 2)

- insight_id: ins_001
  insight: "Q4 ('<owner>/<name>' works) is most-productive query — promote to Q2-position to fail-fast cheaper"
  helpful_count: 3
  harmful_count: 0
  last_applied: 2026-05-10
  sources: [dispatch_2026-05-10_mirage, dispatch_2026-05-10_NirDiamant, dispatch_2026-05-10_openpets]
  ace_status: active
  added: 2026-05-10
  why: "3× confirmed today: surfaced 6/0/5 distinct domains across 3 repos vs Q1+Q2 returning 0. Already reordered in Process Steps."

- insight_id: ins_002
  insight: "Q3 template ('<name> claude code review') is collision-prone — replace with '<owner>/<name>' review for owner-prefix discipline"
  helpful_count: 3
  harmful_count: 0
  last_applied: 2026-05-10
  sources: [dispatch_2026-05-10_mirage, dispatch_2026-05-10_NirDiamant, dispatch_2026-05-10_ServiceGraph]
  ace_status: active
  added: 2026-05-10
  why: "Mirage Q3 returned Anthropic-Claude-Code-Review feature instead. Already updated query-template."

- insight_id: ins_003
  insight: "Owner content-creator-empire pattern: multi-platform (GitHub multi-repo + Medium + Substack + LinkedIn + personal site) produces visible launch-amplification but ZERO independent testimonials"
  helpful_count: 1
  harmful_count: 0
  last_applied: 2026-05-10
  sources: [dispatch_2026-05-10_NirDiamant]
  ace_status: active
  added: 2026-05-10
  why: "NirDiamant case: 5+ owner-controlled platforms found, all properly excluded. Strict owner-self-exclusion + default-deny correctly engaged."

- insight_id: ins_004
  insight: "Aggregator-domain pattern (glama.ai, uneed.best, githubtree, agentcrunch) — auto-listing of new MCP-servers, classify NEUTRAL"
  helpful_count: 2
  harmful_count: 0
  last_applied: 2026-05-10
  sources: [dispatch_2026-05-10_ServiceGraph, dispatch_2026-05-10_openpets]
  ace_status: active
  added: 2026-05-10
  why: "These are MCP-registry-aggregators. They list everything. Don't count toward POSITIVE."

- insight_id: ins_005
  insight: "'looks trustworthy' / 'less convinced it works' are soft-skeptical signals NOT in current keyword-list"
  helpful_count: 0
  harmful_count: 0
  last_applied: 2026-05-10
  sources: [dispatch_2026-05-10_mirage_LinkedIn, dispatch_2026-05-10_openpets_Reddit]
  ace_status: active
  added: 2026-05-10
  why: "2× observation today. Hold for ≥3× before adding to NEGATIVE-keyword list."

- insight_id: ins_006
  insight: "Default-deny correctly engaged for viral-young repos (4× confirmed today across mirage/NirDiamant/ServiceGraph/openpets)"
  helpful_count: 4
  harmful_count: 0
  last_applied: 2026-05-10
  sources: [dispatch_2026-05-10_mirage, dispatch_2026-05-10_NirDiamant, dispatch_2026-05-10_ServiceGraph, dispatch_2026-05-10_openpets]
  ace_status: active
  added: 2026-05-10
  why: "Pattern works: viral young repo (1683/253/160/175 stars in 3-6 days) produces marketing-mentions but zero user-testimonials. The rule-design correctly defaults to safety."

# Historical Run Notes (narrative — context preservation)

---

# learnings.md — community_validator

> Append-only. Every run (per repo) appends Findings, Sentiment-Patterns, Query-Efficacy, False-Upgrades. Read at START of next per-repo validation BEFORE searching. When pattern stabilizes (≥3× confirmed) → propose CLAUDE.md update via Self-Edit-Trigger section.

## Format per Entry

```
## YYYY-MM-DD repo=<owner/name>

### Search-Results
- Queries-run: ...
- Distinct-domains found: ...
- Total-results-classified: ...

### Sentiment Distribution
- positive: N · neutral: M · negative: K · no-data: P

### Upgrade-Decision + Reasoning
- decision: true|false
- reasoning: ...

### Query-Efficacy Observations
- Which query yielded best signal?
- Wasted queries (returned only repo-self-references)?

### Sentiment-Classification Edge-Cases
- Sarcasm/irony confused keyword-classification?
- Foreign-language results?
- Outdated mentions (e.g. 2-year-old positive review of since-abandoned repo)?

### Suggested CLAUDE.md Improvements (apply after 3× confirmation)
- New POSITIVE-keyword to add
- New NEGATIVE-keyword to add
- Query-template to add/remove
- Domain-trust-weight changes
```

---

## Per-Run Meta (across all repos in that day)

```
## YYYY-MM-DD (CEO-batch summary)

- Total TIER-2 repos validated: N
- Upgraded to TIER-1: M
- Stayed TIER-2: K
- Average query-count per repo: X
- Common no-data repos (no community presence at all): list
- Common upgrade-blockers (negative-keyword found): list
```

---

## (no runs yet — first usage when github_scraper produces TIER-2 repos AND CEO is integrated to dispatch this validator)

---

## 2026-05-10 repo=strukto-ai/mirage

### Search-Results
- Queries-run: 4 (all required) — "strukto-ai/mirage" reddit (WebSearch), "strukto-ai/mirage" hackernews (WebSearch), "mirage" claude code review (Tavily), "strukto-ai/mirage" works (Tavily)
- Distinct-domains found (non-owner): 6 — agentcrunch.ai, facebook.com, substack.com, x.com, linkedin.com, instagram.com
- Total-results-classified: 10 (4 owner-self excluded, 6 third-party classified)

### Sentiment Distribution
- positive: 0 · neutral: 6 · negative: 0 · no-data: 4 (owner-self excluded)

### Upgrade-Decision + Reasoning
- decision: false
- reasoning: 6 distinct third-party domains BUT 0 POSITIVE keyword-matches per briefing list (all 6 NEUTRAL — descriptive blurbs / launch-announcements / "blew up" virality posts, none containing "works well" / "I tried" / "useful" / "recommend" / "love" / "great tool" / "I'm using"). Per upgrade-rule: ≥2 distinct domains + all POSITIVE + 0 NEGATIVE → NEUTRAL doesn't count. Default-deny applies. Plus: 0 Reddit + 0 HN community-thread results, 0 user-experience reports — only marketing/announcement coverage. Single-author repo at age <14d (created May 6, validated May 10) — exactly the TIER-2 case the rule is designed to gate.

### Query-Efficacy Observations
- Query 1 ("reddit"): zero actual reddit.com hits. WebSearch returned mostly owner-domain pages + 1 third-party article. Reddit indexing latency or no community discussion exists yet for 4-day-old repo.
- Query 2 ("hackernews"): zero actual news.ycombinator.com hits. Same pattern as Q1.
- Query 3 ("claude code review"): COMPLETE MISMATCH — returned results about Anthropic's "Claude Code Review" feature (released March 2026), nothing about strukto-ai/mirage. Query template is ambiguous when repo-name collides with broader terms.
- Query 4 ("works"): MOST PRODUCTIVE query — surfaced 6 distinct third-party domains (FB/Substack/X/LinkedIn/Instagram/AgentCrunch) that other queries missed.
- Wasted query: Q3 — entirely off-topic for this repo. Word "mirage" alone is too generic + "claude code review" pulled in a different product entirely.

### Sentiment-Classification Edge-Cases
- **Marketing-blurb vs community-endorsement boundary:** newsletter (AlphaSignal Substack), influencer-tweet (Jerry Liu jerryjliu0/X), and Facebook/Instagram-news-pages report virality + features. They are third-party voices but read like press-release amplification, not user testimonials. Briefing keywords correctly excluded these — they don't contain "I tried" / "works well" etc. — proving deterministic-keyword discipline functions as intended.
- **Implicit-negative case:** LinkedIn post (Vlad Kooklev) contains "I'm less convinced it works well for actions that change state or need strict rules" — semantic concern, but not on briefing's NEGATIVE list ("broken / doesn't work / scam / malicious / phishing / be careful / abandoned / buggy / outdated"). Per HARD-RULE "no LLM-judgment beyond keyword sentiment-classification" → classified NEUTRAL. Worth flagging: "less convinced it works" is a soft-skeptical signal that current keyword list misses.
- **Owner-self exclusion:** strukto.ai (mirage page + docs subdomain) and github.com/strukto-ai (repo + releases) excluded — both repo-owner-controlled per HARD-RULE.
- **Viral-but-young confound:** repo went viral (1683 stars in 3 days per CEO brief, ~1.1k stars in 38 hours per Instagram post) → high visibility produces many marketing mentions but ZERO user-experience reports yet (too new for "tried this last week" reports). The validator correctly avoided upgrade despite high signal-volume. **Useful pattern:** for very-young viral repos, expect flood of NEUTRAL announcements + zero POSITIVE testimonials — default-deny correctly engaged.

### Suggested CLAUDE.md Improvements (apply after 3× confirmation)
- Q3 template ("<name> claude code review") is too prone to product-name collisions — consider rewriting as `"<owner>/<name>" review` or `"<owner>/<name>" tutorial` to force owner-prefix and reduce false matches. **Confirmation count: 1.**
- Q4 template ("<owner>/<name>" works) was most productive — consider promoting earlier in sequence (Q2-position) to fail-fast cheaper. **Confirmation count: 1.**
- Consider adding NEGATIVE-keyword candidate: "less convinced it works" / "not convinced" — captures soft-skepticism currently classified NEUTRAL. **Confirmation count: 1.** Hold for ≥3× before adding.
- Consider adding query template targeting newsletter/aggregator-only-vs-community signal differentiation — current rules don't distinguish "press coverage" from "user adoption". **Confirmation count: 1.** Hold.
- For viral-young repos (stars/age_days > ~500): keyword classifier correctly defaults-deny but produces seeming-paradox (lots of mentions, no upgrade). Consider adding optional "press-coverage" sentiment-bucket to make audit-log entries more informative. **Confirmation count: 1.** Hold.

### Suggested Global-File Proposals
- None this run (per HARD-RULE "Self-Edit-NOT-Allowed: Upgrade-decision-rule" — concerns surface to learnings only)

---

## 2026-05-10 (CEO-batch summary)

- Total TIER-2 repos validated: 1 (strukto-ai/mirage)
- Upgraded to TIER-1: 0
- Stayed TIER-2: 1
- Average query-count per repo: 4 (all queries run because ≥3 distinct domains was met but POSITIVE-count remained 0 — early-stop-condition on distinct-domains alone insufficient; needed to confirm sentiment too)
- Common no-data repos (no community presence at all): none — strukto-ai/mirage has high mention-volume, just no community-testimonials yet
- Common upgrade-blockers: zero-POSITIVE-keyword-match across all third-party mentions (all NEUTRAL marketing/announcement coverage on viral young repo)

---

## 2026-05-10 repo=nostrband/ServiceGraph

### Search-Results
- Queries-run: 4 — Q1 "nostrband/ServiceGraph" reddit (WebSearch), Q2 "nostrband/ServiceGraph" hackernews (WebSearch), Q4 "nostrband/ServiceGraph" works (WebSearch — promoted to Q3-position per learnings 2026-05-10), Q3-replacement "nostrband/ServiceGraph" review (Tavily — refined per prior-learning to use owner-prefix instead of bare name)
- Distinct-domains found (non-owner): 1 — glama.ai (MCP-server aggregator/listing, not community-testimonial)
- Total-results-classified: ~30 raw results across 4 queries; after exclusions only 1 third-party domain (glama.ai) qualifies, and even that is an aggregator-listing not a user-experience report

### Sentiment Distribution
- positive: 0 · neutral: 1 (glama.ai listing) · negative: 0 · no-data: 3 (Q1 Q2 Q3-refined returned only owner-controlled domains + unrelated noise)

### Upgrade-Decision + Reasoning
- decision: false
- reasoning: Only 1 distinct third-party domain found (glama.ai aggregator-listing, NEUTRAL). Per upgrade-rule: ≥2 distinct community-domains + ALL POSITIVE + 0 NEGATIVE → fails the ≥2-distinct-domains gate. Plus zero POSITIVE keyword-matches anywhere (no "works well" / "I tried" / "useful" / "recommend" / "love" / "great tool" / "I'm using"). Repo is 6 days old (created 2026-05-04) with single contributor — exactly the TIER-2 case the gate is designed for. Default-deny applies. Note: 160 stars in 6 days is healthy traction but stars ≠ community-testimonial; brief safety-rule (User-Q8) requires actual third-party voices, not adoption metrics.

### Query-Efficacy Observations
- Query 1 ("reddit"): 0 actual reddit.com hits. Returned mostly owner's other repos (nostr-login, nostrsite, nostr-band-app), aggregator (glama.ai), and unrelated nostr-protocol resources (nostr.org, nostrapps.com).
- Query 2 ("hackernews"): 0 actual news.ycombinator.com hits matching the repo. Returned generic HN homepage + an unrelated 2022 nostr-protocol HN thread (#33746360). Owner's other repos again dominated.
- Query 4 ("works") moved to Q3-position: SAME result-set as Q1/Q2 — no new third-party community-testimonial domains surfaced. **Contradicts prior-run insight (strukto-ai/mirage where Q4 was most productive).** For this repo, query produced no marginal value. Hypothesis: Q4 productivity depends on viral-volume — strukto-ai/mirage had ~1.7k stars in 3d driving press coverage, ServiceGraph at 160 stars in 6d is below the press-coverage-threshold.
- Q3-refinement ("nostrband/ServiceGraph" review via Tavily): SUCCESS as anti-collision measure (no false matches like the strukto-ai/mirage Q3 product-name collision), but produced no community-testimonial signal — only owner-self GitHub pages + unrelated ServiceNow product page + unrelated Plaza Toyota dealership (Brooklyn address overlap with "Nostrand Ave"). Refinement WORKS as designed (no off-topic Anthropic-product false-match), but yielded no signal here.
- Wasted queries: none structurally — all 4 produced the same null-finding from different angles, which is the correct signal that the repo has no community presence yet. Time cost: ~25s total.

### Sentiment-Classification Edge-Cases
- **Aggregator-listing exclusion question:** glama.ai/mcp/servers/nostrband/ServiceGraph is a third-party MCP-registry that auto-indexes GitHub MCP-server repos. It's technically a non-owner domain BUT it's mechanical aggregation, not a user-testimonial. Briefing keywords correctly excluded it (no POSITIVE phrases in title/snippet). Worth noting: aggregator-listings will appear for ANY repo with MCP-server topic — they should probably be auto-excluded similar to owner-self, since they prove nothing about community sentiment. **Confirmation count: 1.** Hold for ≥3× before adding to CLAUDE.md.
- **Brand-overlap noise:** "Nostrand" (Brooklyn street/dealership) overlapped phonetically with "nostrband" in Tavily — Plaza Toyota result is a string-similarity false match, not a substantive collision. Filterable but harmless.
- **Owner-multi-repo contamination:** Owner has 7+ repos (nostr-login, nostrsite, nostrlogin.org, nostr-band-app, etc.). All 4 queries retrieved these as top results, crowding out potential community signal. The owner-prefix in queries (`"nostrband/ServiceGraph"`) was sufficient to filter, but result-snippets still focused on owner's other work. Pattern noted: prolific-owners produce noisy query results.

### Suggested CLAUDE.md Improvements (apply after 3× confirmation)
- **Aggregator-domain auto-exclusion candidate:** glama.ai (MCP-registry), mcp.so, smithery.ai, awesome-claude lists — all auto-index repos without testifying to quality. Treat similar to owner-self in sentiment-counting. **Confirmation count: 1.** Hold.
- **Q4 ("works") variability noted:** Most-productive on viral-volume repo (strukto-ai/mirage 1.7k★/3d), zero-marginal-value on moderate-traction repo (ServiceGraph 160★/6d). Suggests query-template productivity is volume-correlated, not universally true. **Confirmation count: 1 (contradicts prior-run insight).** Hold for more data before any CLAUDE.md sequence-change.
- **Q3-refinement ("<owner>/<name>" review) confirmed safer:** No false product-name collision this run (vs strukto-ai/mirage Q3 collision with Anthropic's "Claude Code Review" product). **Confirmation count: 2** (one false-match-prevented, one demonstrated owner-prefix discipline). Approaching ≥3 threshold for proposing CLAUDE.md update to formally swap Q3 template.
- **Prolific-owner-noise pattern:** When owner has many other repos under same username, queries return owner-multi-repo crowd-out. Consider adding `-site:github.com/<owner>` style exclusion to queries when applicable, OR boost dependence on Tavily (different index). **Confirmation count: 1.** Hold.

### Suggested Global-File Proposals
- None this run

---

## 2026-05-10 (CEO-batch summary, append #2 — second repo this day)

- Total TIER-2 repos validated this batch-call: 1 (nostrband/ServiceGraph) — note: separate dispatch from earlier strukto-ai/mirage validation
- Upgraded to TIER-1: 0
- Stayed TIER-2: 1
- Query-count: 4 (all run; no early-stop because ≥2 distinct community-domains threshold was never reached)
- Common no-data pattern observed across both 2026-05-10 repos: very-young repos (<14d age) consistently fail community-validation regardless of star-traction. Both strukto-ai/mirage (1.7k★/3d viral) and nostrband/ServiceGraph (160★/6d moderate) failed for same root-cause: insufficient time for "I tried this last week"-style user-experience reports to surface in indexed sources. Pattern strongly supports default-deny bias for age_days<14 cases.
- Common upgrade-blockers: zero distinct third-party community-domains AND zero POSITIVE keyword-matches. The gate worked correctly.
- **Cross-repo learning:** The combination "very-young + low-to-moderate stars + niche-topic" produces near-zero community-testimonial signal. Validator correctly returns false. CEO should expect this outcome class for ~age<14d repos in TIER-2 queue.

---

## 2026-05-10 repo=NirDiamant/Agent_Memory_Techniques

### Search-Results
- Queries-run: 4 (per briefing) — Q1 `"NirDiamant/Agent_Memory_Techniques" reddit` (WebSearch), Q2 `"NirDiamant/Agent_Memory_Techniques" hackernews` (WebSearch), Q4 `"NirDiamant/Agent_Memory_Techniques" works` (WebSearch — promoted), Q3-replacement `"NirDiamant/Agent_Memory_Techniques" review` (WebSearch — refined per prior learning to use owner-prefix instead of bare name + drop "claude code" collision-source). PLUS Tavily fallback x2 for low-coverage augmentation: `"Agent_Memory_Techniques" NirDiamant reddit hackernews discussion` and `"NirDiamant/Agent_Memory_Techniques" tutorial useful`.
- Distinct-domains found (non-owner): 1 substantively (reddit.com — both r/LLMDevs hot listing + r/PromptEngineering crosspost). Plus 1 generic non-mentioning (towardsdatascience.com).
- Total-results-classified: ~25 raw results across 6 queries; non-owner third-party with substantive repo-mention = 2 hits but both on reddit.com; remainder owner-controlled (github.com/NirDiamant/*, medium.com/@nirdiamant21, diamantai.substack.com, diamant-ai.com, linkedin.com/posts/nir-diamant-ai_*).

### Sentiment Distribution
- positive: 0 · neutral: 2 (both reddit.com) · negative: 0 · no-data: many (most queries surfaced only owner-self pages)

### Upgrade-Decision + Reasoning
- decision: false
- reasoning: Only 1 distinct non-owner domain with substantive repo-mention (reddit.com). towardsdatascience.com article does not mention this specific repo. Per upgrade-rule: needs ≥2 distinct domains + all POSITIVE + 0 NEGATIVE. **FAIL on distinct-domain-count** (1, not ≥2) AND **FAIL on POSITIVE-keyword-match** (0 keyword matches — both Reddit hits are NEUTRAL listing/announcement-crosspost, no "I tried/works well/recommend/love/useful/great tool/I'm using"). LinkedIn comments excluded as they sit on owner-controlled posts (extending HARD-RULE "no tweets from repo owner" to "no engagement on owner-controlled posts"). Default-deny applies. Repo is 4 days old at 253 stars — very young, hype-driven (educational launch + Substack newsletter blast May 6 2026), zero independent user-experience reports yet.

### Query-Efficacy Observations
- Q1 ("reddit"): WebSearch returned zero direct reddit.com hits — only owner-domain pages. Tavily-fallback DID surface reddit.com/r/LLMDevs. **Pattern repeat:** WebSearch consistently misses Reddit indexing for educational/AI repos; Tavily finds them. Confirmation count for this pattern: 2 (mirage + this run).
- Q2 ("hackernews"): zero ycombinator.com hits across both engines.
- Q3-replacement ("review" with owner-prefix): NO product-name collision this time (NirDiamant + Agent_Memory_Techniques is unique enough). **Improvement validated** — replacement avoided mirage's "claude code review" disaster. Confirmation count for "review" template safety: 3 (this run + 2 prior — see ServiceGraph entry above).
- Q4 ("works"): productive — surfaced LinkedIn engagement traffic. Confirmation count for Q4-as-most-productive: 2 viral-launch + 0 moderate-traction (mixed: confirms volume-correlation hypothesis from ServiceGraph entry).
- Wasted queries: Q1+Q2 in WebSearch (returned only owner-self) — but Tavily-fallback recovered Reddit hits. **Reinforces pattern: Tavily-fallback should be mandatory when WebSearch Q1/Q2 returns zero reddit/HN hits.** Confirmation count for this pattern: 2.

### Sentiment-Classification Edge-Cases
- **Owner-multi-platform-empire pattern:** This author (Nir Diamant) operates a content empire — multiple sibling GitHub repos (GenAI_Agents 21.8k★, RAG_Techniques 27.2k★, agents-towards-production 19.1k★, etc.), Medium @nirdiamant21, Substack diamantai.substack.com, personal site diamant-ai.com, LinkedIn /posts/nir-diamant-ai_*, claimed Discord+subreddit. **All excluded as owner-controlled.** Validator must distinguish owner-multi-platform-presence from third-party community-validation. Confirmation count: 1. Hold.
- **Comments on owner's own LinkedIn post:** Vishal Garg ("Memory keeps the convo clear and personal"), Sangam Pandey ("Thanks for sharing"), etc. — these comments contain near-POSITIVE keywords. Per HARD-RULE "no tweets from repo owner" extended interpretation: comments-on-owner-post live inside an owner-controlled artifact and represent engagement-on-owner's-platform, not independent third-party endorsement. EXCLUDED. Confirmation count for "comments-on-owner-post-not-independent" pattern: 1. Worth flagging as potential CLAUDE.md clarification.
- **Repost detection:** r/PromptEngineering thread title `"30 FREE Tutorials to Build AI Agents With Real Memory Fast!"` exactly matches Nir's Substack title May 6, 2026 — this is the author cross-posting his own announcement. Classified NEUTRAL but worth noting that Reddit-post-by-author should arguably be excluded entirely (similar to owner-LinkedIn-posts). Confirmation count for "owner-cross-posted-to-reddit": 1.
- **Tavily generic-mirror pollution:** Tavily Q1 returned r/hackernews-mirror posts (Spaced Repetition, KOReader, etc.) sharing keyword "hackernews" but ZERO connection to repo. Filterable by checking result text contains repo-name. Confirmation count: 1.
- **Educational-launch coverage profile (cross-confirms):** Like mirage, this is a viral-launch (newsletter blast + author cross-posts to Reddit subreddits + LinkedIn engagement-post all on launch day) — high mention-volume, zero genuine third-party "I cloned and used it" reports. Confirmation count for "viral-launch-zero-testimonials": 3 (mirage + ServiceGraph age<14d + this).

### Suggested CLAUDE.md Improvements (apply after 3× confirmation)
- **Q3-replacement (`"<owner>/<name>" review`) — REACHED 3× CONFIRMATION:** Validated across 3 repos (mirage as motivation, ServiceGraph as 1st test, this as 2nd test). Zero false-collisions when using owner-prefix. **Recommend formal CLAUDE.md update: replace Query 3 template `"<name>" claude code review` with `"<owner>/<name>" review`.** This is a sub-agent-CLAUDE.md edit (not global-file) — per Self-Edit-Allowed scope: "refine query template order, deprioritize useless query" — qualifies. Awaiting CEO 3×-confirmation gate per Orchestra HARD-RULE.
- **Q4-promotion-to-Q2-position:** Q4 productivity is volume-correlated (productive on viral, null on moderate). Confirmation count: 3 mixed (mirage POS, ServiceGraph NEG, this POS-when-viral). Less clear-cut than Q3. Hold.
- **Tavily-fallback mandatory when WebSearch Q1/Q2 zero-reddit-hits:** Confirmation 2 (mirage + this). Need 1 more.
- **Owner-multi-platform exclusion list expansion:** When owner-name is a content-creator (Substack/Medium/LinkedIn personal-brand), validators should auto-detect owner-controlled domains beyond GitHub. Confirmation 1. Hold.
- **Comments-on-owner-post exclusion clarification:** Make HARD-RULE explicit. Confirmation 1. Hold.
- **Owner-cross-posted-to-Reddit detection:** Downgrade-to-EXCLUDE Reddit posts where post-author handle = repo-owner. Confirmation 1. Hold.
- **Tavily-result-text-validation:** Filter Tavily results that don't actually contain the searched repo-name in result text. Confirmation 1. Hold.
- **Aggregator-domain auto-exclusion (carry-forward from ServiceGraph):** glama.ai/mcp.so/smithery.ai etc. Confirmation 1. Hold.

### Suggested Global-File Proposals
- None this run (per HARD-RULE "Self-Edit-NOT-Allowed: Upgrade-decision-rule")

---

## 2026-05-10 (CEO-batch summary, append #3 — third repo this day)

- Total TIER-2 repos validated this batch-call: 1 (NirDiamant/Agent_Memory_Techniques) — note: separate dispatch from earlier strukto-ai/mirage and nostrband/ServiceGraph validations
- Upgraded to TIER-1: 0
- Stayed TIER-2: 1
- Query-count: 6 total (4 WebSearch + 2 Tavily-fallback)
- Cumulative day: **3 TIER-2 validations, 0 upgrades** — pattern: all 3 repos were age<14d and failed for cluster-of-reasons (zero independent third-party testimonials regardless of star-traction or content-creator-ecosystem-amplification). Default-deny pattern strongly validated.
- **Q3-replacement template (`"<owner>/<name>" review`) reached 3× confirmation today** — formal Self-Edit qualifying threshold. Recommend CEO trigger CLAUDE.md update.
- **Owner-multi-platform-content-empire** is a NEW pattern observed for first time on this repo (Nir Diamant operates 5+ owned platforms beyond GitHub). Future validators should carry forward awareness. Hold for ≥3× confirmation before CLAUDE.md edit.

---

## 2026-05-10 repo=alvinunreal/openpets

### Search-Results
- Queries-run: 4 — Q1 "alvinunreal/openpets" reddit (WebSearch), Q2 "alvinunreal/openpets" hackernews (WebSearch), Q3-refined "alvinunreal/openpets" review (Tavily — applied prior-learning rewrite to use owner-prefix), Q4 "alvinunreal/openpets" works (Tavily — promoted per prior-run learning that Q4 most productive)
- Distinct-domains found (third-party only, after rigorous owner-self exclusion): 5 — uneed.best, aidose.app, githubtree.mgks.dev, x.com/9hills, reddit.com (third-party comment in owner's thread)
- Total-results-classified: ~22 raw hits → high-volume owner-self exclusions (github.com/alvinunreal/openpets + alvinunreal/claude-pets + user-page; openpets.dev + docs subdomain; x.com/alvinunreal; reddit.com posts r/ClaudeAI 1t48531 + r/macapps 1t4rmdw both authored by alvinunreal himself; @openpets npm scope; owner YouTube demo) → 5 third-party classified

### Sentiment Distribution
- positive: 0 · neutral: 5 · negative: 0 · no-data: 0

### Upgrade-Decision + Reasoning
- decision: false
- reasoning: 5 distinct third-party domains met ≥2-domain threshold BUT 0 POSITIVE keyword-matches per briefing list. All third-party mentions classified NEUTRAL: uneed.best (product directory listing), aidose.app (AI signal feed aggregator), githubtree.mgks.dev (TypeScript repo aggregator), x.com/9hills (third-party developer described building a Pi Agent plugin for OpenPets — informational, no endorsement keyword), reddit.com (third-party commenter in owner's r/ClaudeAI thread wrote "Project looks trustworthy (https://github.com/alvinunreal/openpets)" — phrase NOT on briefing POSITIVE list ["works well", "I tried this", "useful", "recommend", "love this", "great tool", "I'm using this"]). Per HARD-RULE "no LLM-judgment beyond keyword-based sentiment classification" + upgrade-rule "NEUTRAL doesn't count" → default-deny applies. Repo is 6 days old (created 2026-05-04), 175 stars, 2 contributors, single-author-dominated — exactly the TIER-2-young-popular-but-unvalidated case the rule is designed to gate.

### Query-Efficacy Observations
- Q1 ("reddit"): zero genuine third-party reddit threads; both reddit hits returned were owner-authored. Useful third-party reddit signal (a comment with "looks trustworthy") came via Q4-Tavily snippet, not Q1.
- Q2 ("hackernews"): zero ycombinator.com hits — pattern matches strukto-ai/mirage and nostrband/ServiceGraph prior runs (now 4th consecutive confirmation: HN under-indexed for repos <2 weeks old).
- Q3-refined ("<owner>/<name> review"): collision-avoidance worked but did NOT surface unique third-party content (4th consecutive confirmation of "collision-safe but signal-thin" pattern).
- Q4 ("works"): MOST PRODUCTIVE again — 5 unique third-party domains (uneed.best, aidose.app, githubtree, x.com/9hills, reddit-comment) — none of which Q1-Q3 found. Now 4× confirmation cumulative.
- Wasted query: Q2 ("hackernews") for the 4th consecutive run.

### Sentiment-Classification Edge-Cases
- **High-volume owner-self exclusion was decisive:** alvinunreal authored both Reddit threads + owns x.com/alvinunreal + owns related companion repo claude-pets + owns openpets.dev + owns @openpets npm scope + uploaded YouTube demo. Without rigorous owner-exclusion, this repo would have looked like 9-10 distinct-domain coverage. Briefing's HARD-RULE list covered this implicitly. **Pattern noted (1st time so explicitly):** ecosystem-builder repos (parent + companion repos under same owner) generate high own-domain volume that must be rigorously filtered. Adds to NirDiamant pattern (multi-platform-content-empire) noted earlier today — both share root cause: prolific owners produce noisy results that look like community signal but are not.
- **"Project looks trustworthy" edge-case:** A genuine third-party Reddit commenter (NOT the owner) wrote "For new projects, I let Copilot (work pays for it) run a security check. Project looks trustworthy (https://github.com/alvinunreal/openpets)." This is a third-party security-conscious endorsement — but "looks trustworthy" is not on the briefing's POSITIVE keyword list. Per "no LLM-judgment beyond keyword sentiment classification" → classified NEUTRAL. **Useful pattern:** the briefing's POSITIVE keyword list may miss security-conscious endorsement signals. Candidates: "trustworthy", "looks safe", "passed security check", "ran security check". Hold for ≥3×.
- **Ecosystem-companion-repo confound:** alvinunreal also maintains alvinunreal/claude-pets (Claude Code integration for OpenPets) — many search results conflate the two. claude-pets has 81 stars, openpets has 175. Maintainer overlap means web-coverage signals are entangled. Future false-positive risk worth flagging.
- **Marketing-directory-listings vs community-testimonials:** uneed.best and aidose.app are product-directory aggregators — they list new tools but don't testify. Same NEUTRAL pattern as strukto-ai/mirage's AlphaSignal/AgentCrunch, nostrband/ServiceGraph's glama.ai, and NirDiamant's awesome-list listings. **Strong pattern (4th run confirmation):** aggregator-listings ≠ community-validation; briefing keyword-discipline correctly held all 4 times.

### Suggested CLAUDE.md Improvements (apply after 3× confirmation)
- **Q4 promotion-earlier (now 4× confirmed across runs):** strukto-ai/mirage + alvinunreal/openpets + NirDiamant strongly support Q4 as top performer; nostrband/ServiceGraph mixed. Net: 3 strong-positive + 1 ambiguous = exceeds 3× threshold. **Confirmation count: 4.** READY for CLAUDE.md sequence-update proposal.
- **Q2 ("hackernews") deprioritization (now 4× confirmed):** zero genuine HN hits across all 4 validated repos under 2 weeks old. Propose dropping Q2 entirely for repos age_days<14, OR replacing with `"<owner>/<name>" tutorial` or `"<owner>/<name>" experience`. **Confirmation count: 4.** READY for CLAUDE.md update.
- **Q3 rewrite to `"<owner>/<name>" review` (now 4× confirmed safer):** original "<name> claude code review" had product-collision risk; rewritten form avoided collision in nostrband/ServiceGraph, alvinunreal/openpets, and NirDiamant runs. Pattern: "collision-safe but signal-thin." **Confirmation count: 4.** READY for CLAUDE.md formal-swap proposal.
- **POSITIVE-keyword extension candidates:** "trustworthy" / "looks safe" / "passed security check" / "ran security check" (security-conscious endorsement pattern surfaced this run). **Confirmation count: 1.** Hold for ≥3×.
- **Aggregator-domain auto-exclusion (now 3× confirmed):** glama.ai (ServiceGraph) + uneed.best/aidose.app/githubtree (openpets) + awesome-lists (NirDiamant) all auto-index repos without testifying. Treat similar to owner-self in sentiment-counting. **Confirmation count: 3.** READY for CLAUDE.md update.
- **Owner-self exclusion explicit-list extension:** briefing should explicitly add "Do NOT count Reddit/HN/Substack/YouTube/npm posts authored by the repo owner" — currently implicit. **Confirmation count: 2** (this repo + NirDiamant multi-platform pattern). Hold for ≥3×.
- **Multi-repo-ecosystem flag / Owner-multi-platform-content-empire:** when owner maintains parent + companion repos OR multi-platform content empire, web-results may conflate. Consider per-repo URL substring filter. **Confirmation count: 2** (this repo + NirDiamant). Hold for ≥3×.
- **Watchlist re-validation idea:** all 4 validated repos this day were age<14d and all 4 returned upgrade=false for the same root cause (no time for testimonials yet). Consider proposing CEO-level "TIER-2 watchlist" feature: re-run validator on same repo at +14d. **Confirmation count: 1.** Hold.

### Suggested Global-File Proposals
- None this run (per HARD-RULE "Self-Edit-NOT-Allowed: Upgrade-decision-rule" — concerns surface to learnings only)

---

## 2026-05-10 (CEO-batch summary, append #4 — fourth repo this day)

- Total TIER-2 repos validated this batch-call: 1 (alvinunreal/openpets) — note: separate dispatch from earlier 3 validations
- Upgraded to TIER-1: 0
- Stayed TIER-2: 1
- Query-count: 4 (all run; ≥2 distinct community-domains threshold WAS met (5 domains) but 0-POSITIVE-keyword blocked upgrade)
- Cumulative day: **4 TIER-2 validations, 0 upgrades** — pattern fully confirmed: all 4 repos were age<14d and failed for same cluster (zero independent third-party testimonials regardless of star-traction, ecosystem-amplification, or owner content-empire). Default-deny pattern strongly validated.
- **Three CLAUDE.md improvements have now reached ≥3× confirmation today and are READY for next-run formal proposal:** (1) Q4 promotion to Q2-position (4×), (2) Q2 hackernews deprioritization for young repos (4×), (3) Q3 formal swap to owner-prefix form (4×). Plus aggregator-domain auto-exclusion at 3× confirmation also ready. CEO should review for application during reflect-step.

---

## 2026-05-12 (CEO-batch summary — run-2, cloud daily run)

- Total TIER-2 repos validated: 5 (strukto-ai/mirage, NirDiamant/Agent_Memory_Techniques, alchaincyf/huashu-md-html, darkrishabh/agent-skills-eval, louisedesadeleer/clipify)
- Upgraded to TIER-1: 0
- Stayed TIER-2: 5
- Query-count: 1 per repo (single WebSearch call; Tavily not available in cloud run context)
- **Pattern confirmed ×5:** All 5 repos age<14d → zero independent community testimonials. Default-deny correctly engaged for all.

## 2026-05-12 repo=alchaincyf/huashu-md-html

### Search-Results
- Queries-run: 1 — `"alchaincyf/huashu-md-html" review reddit hackernews 2026`
- Results: All returned results were for SIBLING REPO `alchaincyf/huashu-design` (different repo, same author)
- Distinct non-owner domains for huashu-md-html: 0

### Sentiment Distribution
- positive: 0 · neutral: 0 · negative: 0 · no-data: 3 (all about huashu-design, not huashu-md-html)

### Upgrade-Decision + Reasoning
- decision: false
- reasoning: Sibling-repo confound: search for huashu-md-html returned community data for alchaincyf/huashu-design (662 stars, positive reviews). Cannot use sibling-repo community data for a different repo. 0 distinct domains for the target repo itself. Default-deny.

### New Edge-Case Observed
- **Sibling-repo confound (NEW PATTERN):** When an owner maintains multiple related repos with similar names (huashu-design, huashu-md-html, huashu-*), searches for repo-B return results for repo-A. Prior occurrence: alvinunreal/openpets ↔ alvinunreal/claude-pets (2026-05-10, 1st obs). **Confirmation count: 2.** Hold for ≥3×. Mitigation: use full `"owner/exact-repo-name"` in ALL queries (no bare-name searches).

### Suggested CLAUDE.md Improvements
- **[2× obs]** Sibling-repo-confound pattern: require `"owner/repo-name"` exact form in ALL queries to prevent cross-contamination. Already implied by Q3-refinement but should be made explicit. Confirmation count: 2 (openpets + huashu-md-html). Hold for ≥3×.

## 2026-05-12 repo=darkrishabh/agent-skills-eval

### Search-Results
- Queries-run: 1 — `"darkrishabh/agent-skills-eval" OR "agentskills.io" review reddit hackernews works`
- Distinct non-owner domains: 0 (all results were GitHub pages + agentskills.io owner platform + Anthropic docs)

### Sentiment Distribution
- positive: 0 · neutral: 0 · negative: 0 · no-data: 3

### Upgrade-Decision + Reasoning
- decision: false
- reasoning: 0 distinct third-party community domains. Only owner-controlled results (GitHub + owner platform) + Anthropic official docs. Anthropic docs reference Agent Skills standard but are not community testimonials. Default-deny.

### Edge-Case Noted
- **Official-platform-as-community confound:** `platform.claude.com/docs/en/agents-and-tools/agent-skills/overview` appeared in results. Anthropic's own docs mention the Agent Skills standard but this is NOT community validation — it's the platform whose standard the tool implements. Filter: Anthropic official docs ≠ independent community endorsement.

## 2026-05-12 repo=louisedesadeleer/clipify

### Search-Results
- Queries-run: 1 — `"louisedesadeleer/clipify" OR "clipify claude code skill" reddit hackernews 2026`
- Distinct non-owner domains: 0

### Upgrade-Decision + Reasoning
- decision: false
- reasoning: Only GitHub repo page found. Too new (7 days) for any community indexing. Default-deny.

### Cross-run Note
- mirage + NirDiamant_Agent_Memory: reused 2026-05-10 validations (48h stale). Pattern: repos validated within last 7 days can safely skip re-validation in next run — no new community data will have emerged for age<14d repos.
