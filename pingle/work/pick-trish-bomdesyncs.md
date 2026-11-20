Branch: pick/trish-bomdesyncs
Status: done
Pull request: #16 (draft, left open for the project owner to decide)
Files:
- indra/newview/llappearancemgr.cpp
- indra/newview/llcofwearables.cpp
- indra/newview/llcofwearables.h
- indra/newview/lloutfitobserver.cpp
- indra/newview/lloutfitobserver.h
- indra/newview/llpaneloutfitedit.cpp
- indra/newview/llvoavatarself.cpp
- pingle/PICKS.md

## What

Pick (task T11): bake-on-mesh bake desyncs and the outfit (COF) window going
out of sync with what the avatar is actually wearing, in several cases.
Source: snapshot branch `snapshot/trish-2026-11-19/fix-bomdesyncs`, commit
`6526d7fa766303f57230ee76754cacbdbcd3dd27`.

## Not done

Cherry-pick (`-x`) conflicts in `indra/newview/llappearancemgr.cpp`: our
`updateAppearanceFromCOF()` / `LLWearableHoldingPattern::onAllComplete()`
already contain different, partly commented-out logic for the same COF
update/callback path than the one this pick assumes. Resolving it needs a
real design decision about which code path to keep, not a mechanical merge.
Left unresolved; cherry-pick aborted and the working tree reset. The other
six files in the pick applied cleanly on their own but are not included
since the pick is one unit. Not picked; PR #16 stays open as a draft for the
project owner to look at.
