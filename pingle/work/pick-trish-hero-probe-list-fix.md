Branch: pick/trish-hero-probe-list-fix
Status: done
Pull request: #26
Files:
- indra/newview/llheroprobemanager.cpp
- pingle/PICKS.md

## What

Pick (task T21, part of the performance set with no visible change):
Mirrors: `LLHeroProbeManager::update` removed dead or no longer eligible mirror objects from `mHeroVOList` while looping over it, which could skip an entry or read past the end; stale entries are now collected and removed after the loop.
Source: snapshot branch `snapshot/trish-2026-11-19/trishstorm-develop`, commit
`d66f6a826d8ed9fbde23cde4c55d05e2de58b71f`.
