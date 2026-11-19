Branch: pick/trish-rlv-tplocal
Status: active
Pull request: pending
Files:
- indra/newview/rlvactions.cpp
- indra/newview/rlvdefines.h
- pingle/PICKS.md

## What

Pick (task T08, Firestorm ticket FIRE-37059): RLVa `@tplocal=n` without a
distance defaulted to 256 m (a whole region) instead of the usual 1.5 m, and
the distance was measured only on the ground plane (X and Y), so teleporting
straight up was not limited. The pick makes the default 1.5 m (256 m stays as
the maximum) and measures in 3D. Source: snapshot branch
`snapshot/trish-2026-11-19/fire-37059`, commits
`31b1866463ce3a09f1a735726f87ec7b053e54e2` and
`c3b01a9ef7ee88ca61a6712a9caad9123680ff4d`.
