# Proposals — Global-File Change Suggestions

Agents (CEO + 3 Sub-Agents in `07_Morning_Brief_Orchestra`) write proposals here when they observe patterns that would benefit GLOBAL files. They MUST NOT directly edit these files:

- `~/.claude/CLAUDE.md` — Global User-CLAUDE.md
- `/Users/Claude/Documents/CLAUDE.md` — Project-wide CLAUDE.md
- `~/.claude/projects/-Users-Claude-Documents/memory/*.md` — Global Memory entries
- `~/.claude/projects/-Users-Claude-Documents/memory/MEMORY.md` — Memory index

## Workflow

1. **Agent observes pattern ≥ 2× confirmation** in own `learnings.md`
2. **Agent surfaces suggestion** in its learnings.md "Suggested Global-File Proposals" section
3. **CEO reviews** at end of daily-run (Step 13) — if learning qualifies, CEO writes Proposal-Doc to this folder
4. **Dashboard 7th Section** reads proposals folder + surfaces them to User
5. **User reviews** the proposal manually
6. **User decides:**
   - Apply → User edits the global file manually + renames proposal `<slug>.applied.md`
   - Reject → User renames `<slug>.rejected.md` (with optional 1-line reason)
   - Defer → leave in pending state, will keep showing in Dashboard

## File Naming

```
YYYY-MM-DD_<slug>.md                 (pending — Dashboard surfaces)
YYYY-MM-DD_<slug>.applied.md         (User-applied, archived state)
YYYY-MM-DD_<slug>.rejected.md        (User-rejected, archived state)
```

## Proposal Template

```markdown
---
title: <short title>
target_file: ~/.claude/CLAUDE.md | Documents/CLAUDE.md | memory/<file>.md
proposed_at: YYYY-MM-DD
proposed_by: Morning_Brief_Orchestra (CEO | github_scraper | news_monitor | community_validator)
status: pending_user_review
confidence: low | medium | high
---

## Problem Observed

<what pattern triggered this proposal>

## Evidence

- Run YYYY-MM-DD: <observation>
- Run YYYY-MM-DD: <observation>
- Run YYYY-MM-DD: <observation>

## Proposed Change

<exact edit / addition>

## Scope of Impact

<what would change in user's setup; who else affected>

## Why This Should Be Global vs. Local

<reason it shouldn't stay in own learnings.md or own CLAUDE.md>
```

## Why HARD-RULE-No-Auto-Edit on Global Files

Global files are User-controlled context. Auto-edits would:

- Corrupt User's curated context across all projects + sessions
- Trigger silent behavioral drift across all Claude-instances (not just Pulse-Orchestra)
- Violate User-Decision 2026-05-10: "Wenn etwas in die globalMD soll, muss [Agent] mir ein Dokument schreiben"

Proposal-Doc Pattern preserves User-Sovereignty over global context.
