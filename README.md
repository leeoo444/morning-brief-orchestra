# Morning Brief Orchestra — Cloud-Sync Repo

Single-Source-of-Truth for [07_Morning_Brief_Orchestra](https://github.com/leeoo444/morning-brief-orchestra). Both the Anthropic Cloud-Routine and local Manual-Triggers read+write here, so Self-Improvement-Layer accumulates across both.

## Why this repo exists

Cloud-Routines (Anthropic /schedule) cannot read local filesystem. Without Git-sync, Cloud and Local would have separate learnings stores → no cross-context learning. This repo bridges them.

## Structure

```
agents/              # Sub-Agent definitions (mirrored to ~/.claude/agents/<name>.md via symlink)
learnings/           # ACE-style learnings (mirrored to ~/.claude/agents/learnings/<name>_learnings.md)
ceo/CLAUDE.md        # CEO briefing (canonical SHARED rules — mirrored to 05_Orchestra/.../CLAUDE.md)
ceo/learnings.md     # CEO cross-cutting observations
briefs/              # Daily Morning-Briefs (mirrored to Vault/System/Morning_Brief/runs/)
```

## Workflow

### Cloud-Routine (daily 07:00 JST)

1. Cloud-CCR auto-clones this repo via `sources: [{git_repository: ...}]`
2. Reads `ceo/CLAUDE.md` + all `agents/*.md` + all `learnings/*.md`
3. Executes pipeline (gh-API + WebFetch + classify + dedup + render)
4. Writes new `briefs/YYYY-MM-DD.md`
5. APPENDS new insights to `learnings/*.md` files (helpful_count++ / new insight_id)
6. `git commit -am "Daily run YYYY-MM-DD" && git push`

### Local Manual-Trigger

```bash
cd /Users/Claude/Documents/00_Claude/cloud-sync/morning-brief-orchestra
git pull --rebase origin main           # always-pull-first
claude "execute claude code pulse daily run"   # uses files via symlinks
git add -A && git commit -m "Local manual run YYYY-MM-DD" && git push
```

### Local Sync (auto, launchd 5min)

```bash
# Just keeps the repo-clone fresh with cloud-pushed updates
cd /Users/Claude/Documents/00_Claude/cloud-sync/morning-brief-orchestra && git pull origin main
```

## Symlinks (1× setup)

Local file-locations point HERE via symlink. Setup script:

```bash
REPO=/Users/Claude/Documents/00_Claude/cloud-sync/morning-brief-orchestra
ln -sf $REPO/agents/github_pulse_scraper.md ~/.claude/agents/github_pulse_scraper.md
ln -sf $REPO/agents/news_pulse_monitor.md ~/.claude/agents/news_pulse_monitor.md
ln -sf $REPO/agents/community_validator.md ~/.claude/agents/community_validator.md
ln -sf $REPO/learnings/github_pulse_scraper_learnings.md ~/.claude/agents/learnings/github_pulse_scraper_learnings.md
ln -sf $REPO/learnings/news_pulse_monitor_learnings.md ~/.claude/agents/learnings/news_pulse_monitor_learnings.md
ln -sf $REPO/learnings/community_validator_learnings.md ~/.claude/agents/learnings/community_validator_learnings.md
ln -sf $REPO/ceo/CLAUDE.md /Users/Claude/Documents/05_Orchestra/07_Morning_Brief_Orchestra/CLAUDE.md
ln -sf $REPO/ceo/learnings.md /Users/Claude/Documents/05_Orchestra/07_Morning_Brief_Orchestra/learnings.md
# Vault briefs: per-day symlinks created at write-time, NOT folder-symlink (Vault rules)
```

## Conflict Strategy

- Cloud runs 1×/day at 07:00 JST (22:00 UTC prev day)
- Local-Manual runs ad-hoc; always `git pull --rebase` BEFORE execute
- Real merge-conflicts (rare): User decides manually

## Trigger ID

Anthropic Cloud-Routine: [trig_01TD1ajxRHcCPx1Y4zGkkFDE](https://claude.ai/code/routines/trig_01TD1ajxRHcCPx1Y4zGkkFDE)
