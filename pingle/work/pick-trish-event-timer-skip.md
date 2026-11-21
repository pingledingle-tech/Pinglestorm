Branch: pick/trish-event-timer-skip
Status: active
Pull request: pending
Files:
- indra/llcommon/lleventtimer.cpp
- pingle/PICKS.md

## What

Pick (task T21, part of the performance set with no visible change):
Core: `LLEventTimer::updateClass` built a snapshot of every timer each frame although usually none is due; a small mutex-guarded registry of live timers now lets it check first and return early when nothing is due (the original loop runs unchanged otherwise).
Source: snapshot branch `snapshot/trish-2026-11-19/trishstorm-develop`, commit
`94043991d233a161c65048d663953272705005fb`.
