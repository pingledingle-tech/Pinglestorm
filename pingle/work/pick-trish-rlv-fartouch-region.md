Branch: pick/trish-rlv-fartouch-region
Status: active
Pull request: pending
Files:
- indra/newview/lltoolselect.cpp
- pingle/PICKS.md

## What

Pick (task T07, Firestorm ticket FIRE-37061): the RLVa `@fartouch` distance
check when selecting an object compared the avatar's agent position with the
object's region position, so an object across a region border wrongly became
untouchable. The pick compares both in agent space. Source: snapshot branch
`snapshot/trish-2026-11-19/fire-37061`, commit
`c3d918d23a30be1561546e0e6dede8880b266eb8`.
