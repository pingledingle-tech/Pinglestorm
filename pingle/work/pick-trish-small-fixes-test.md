Branch: test/combined-picks-2026-11-19
Status: done
Pull request: #14
Files:
- indra/newview/llpreviewscript.cpp
- indra/newview/llpreviewscript.h
- indra/newview/skins/default/xui/*/notifications.xml
- indra/newview/vjlocalmeshimportdae.cpp
- indra/newview/app_settings/settings.xml
- indra/newview/llpanelteleporthistory.cpp
- indra/newview/skins/default/xui/*/panel_teleport_history_item.xml
- indra/newview/fsareasearch.cpp
- indra/newview/aoengine.cpp
- indra/llui/llfolderview.cpp
- indra/llui/llfolderviewmodel.h
- indra/llui/llui.cpp
- indra/newview/llviewercontrol.cpp
- indra/newview/llavataractions.cpp
- pingle/PICKS.md
- pingle/TASKS.md

## What

Task T15, test branch only (pull request #14, never merged): nine small,
standalone fixes from the snapshot of the friend's fork, one commit each:
FIRE-32374, FIRE-34069, FIRE-34952, FIRE-35212, FIRE-35825, FIRE-36930,
FIRE-36992, FIRE-37092, FIRE-31254. Source branches are
`snapshot/trish-2026-11-19/<name>`; the hashes are in `pingle/PICKS.md`.
Skipped (conflict or not standalone): FIRE-35802, FIRE-36600, the nearby chat
bar fixes. Already in 7.2.5: three small fixes. Details in `pingle/TASKS.md`
(T15). Not built, not tried.
