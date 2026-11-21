Branch: pick/trish-lltrunc-ll-round
Status: active
Pull request: pending
Files:
- indra/llmath/llmath.h
- pingle/PICKS.md

## What

Pick (task T21, part of the performance set with no visible change):
Math: `lltrunc` uses `std::trunc` and `ll_round` uses `lround`/`round`; the dead x87 inline-assembly variants (`BOGUS_ROUND`, `round_int`) are removed (originally from Alchemy). Note: for negative values exactly halfway (e.g. -2.5), `ll_round` now rounds away from zero (-3) where the old `floor(x + 0.5)` gave -2.
Source: snapshot branch `snapshot/trish-2026-11-19/trishstorm-develop`, commit
`bb95096a257245b9c50bf4ad8092942fc60679e4`.
