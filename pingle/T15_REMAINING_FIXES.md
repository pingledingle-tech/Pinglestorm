# T15: remaining small fixes in the snapshot

A list for the project owner to choose from. No pick has been made and no code has been changed. Nothing here is built or tried.

Method: each source branch was fetched with a bounded depth (`--depth=15`), and its own commit(s) were dry-run with `git cherry-pick -n` on top of `main` (7.2.5 plus our picks), then reset. Nothing was committed. "Applies" means "no conflict in the dry run", not "works". Sizes are for the commit(s) on that branch. All sources are the frozen copy under `snapshot/trish-2026-11-19/`; licence as for the earlier picks (LGPL 2.1, Firestorm headers unchanged). Each pick would still follow `pingle/PICKS.md` (one branch, one pull request).

Not on this list: the already picked fixes, the version/backup/base branches (`7.2.5*`, `7.2.6*`, `7.3.0*`, `Trish-MainBranch*`, `master`, `perfs`) and `trishstorm-develop` (the large stack, handled in T02, T03, T10 and T16 to T21).

## A. Applies cleanly on 7.2.5 (15 rows)

| # | Source branch | What it fixes for a user | Size | Risk or dependency |
|---|---|---|---|---|
| 1 | `FIRE-31254` | "Clear temporary derendered objects" now also clears derendered avatars (FIRE-31254), also when clearing on teleport. | 1 file, +4 | Low. |
| 2 | `fire-34069` | Local mesh import (DAE) no longer fails when the file name or path has a special character (FIRE-34069). | 1 file, +13 -3 | Low. |
| 3 | `fire-32374` | When a group script is closing, the notification says which script it is (FIRE-32374). Two commits (the second only drops a `const`). | 3 files, +35 -1 | Adds a notification text in English only. |
| 4 | `fire-34952` | Teleport history shows which timezone the time is in (FIRE-34952). | 5 files, +11 -8 | Also changes the default of the teleport-history timezone setting from UTC to Second Life Time, so the visible behaviour changes for everyone. |
| 5 | `fire-35212` | Area search: owner, creator and last-owner filters also match legacy name, account name and display name, not only the name shown in the list (FIRE-35212). | 1 file, +29 -6 | Low. Uses RLVa `@shownames` check, so it stays hidden under that restriction. |
| 6 | `fire-36992` | Area search menus ignore the build-mode selection modes (FIRE-36992). | 1 file, +4 -2 | Low. Touches the same file as row 5, so pick one after the other. |
| 7 | `fire-35825` | The animation overrider (AO) no longer gets stuck in a sit/jump/landing/stand-up animation after a region change (FIRE-35825). | 1 file, +14 | Low. |
| 8 | `fire-36930` | Ctrl+C on a no-copy inventory item can be pasted as a link, like the context menu allows (FIRE-36930). | 2 files, +8 -1 | Depends: it adds a virtual `isItemLinkable()` that returns false; the real implementation is in the inventory copy/paste work (row 12). Alone it changes nothing. |
| 9 | `fire-37092` | Fixes the order in which settings groups are searched, so an old "ignores" entry no longer hides a real setting such as the online-status toggle (FIRE-37092). | 12 files, +31 -13 | Touches core `LLUI::getControlControlGroup`, which is used everywhere; 10 of the files are one-line notification text edits in other languages. Medium. |
| 10 | `fix-includepbr-debugalpha` | The "debug alpha" view also shows transparent PBR (GLTF) materials. | 1 file, +140 -11 | Developer view only. Touches the alpha draw pool (same area as the PBR opaque pick), so test together. Medium. |
| 11 | `fs-ctrl-drag` | The existing Ctrl+drag build option also works on HUD items. | 6 files, +334 -1 | Adds new files `fstoolhudmove.*`, a new setting and a preferences checkbox, and edits `CMakeLists.txt`. Medium. |
| 12 | `fs-improve-copypaste` | Inventory copy and paste: handles mixed content, no more "copy as link", new "paste as link" shortcut, with new settings and a notification. | 9 files, +297 -4 | Medium. Changes visible behaviour of inventory. Needed by row 8 to do anything. |
| 13 | `nearbychatbarstylingfixes` | Nearby chat bar: styling fixes and full-height text cursor. Two commits. | 3 files, +91 -20 | Low to medium (visible, chat bar layout). |
| 14 | `fire36492` (first commit only) | Nearby chat bar text is no longer cut off with large fonts. | 1 file, +12 | The second commit on that branch (moves two properties into the XML widget definition) conflicts, see section B. Overlaps with row 13 in the same file. |
| 15 | `feature-camerabookmarks` | NOT a small fix: camera links, camera bookmarks and camera tours (new window, menu, chat commands, URL type). Four commits. | 22 files, +5232 -86 | Large new feature (about 5000 lines in 4 new files), edits `chatbar_as_cmdline.cpp`, `settings.xml`, three skins and the URL parser. Applies, but belongs with the owner's feature choices, not with small fixes. High. |

## B. Does not apply cleanly on 7.2.5 (needs hand work)

| Source branch | What it fixes for a user | Size | Why it conflicts |
|---|---|---|---|
| `fire-35802` | Area search "return" can return objects separately (FIRE-35802). | 4 files, +41 -7 | `llviewermenu.h` conflicts: the commit sits next to a declaration (`save_object_back_to_contents`) that 7.2.5 does not have. Probably a small hand merge; may also depend on that other change. |
| `fire-36600` | Profile window gets its read-only mode back when updated after having been used for your own profile. | 1 file, +15 -5 | Conflict in `llpanelprofile.cpp`: surrounding code differs on 7.2.5 (the display-name button lines). Small hand merge. |
| `fire36029` | Two settings to deselect the chat input after sending (mouselook, and nearby chat bar only). Three commits. | 3 commits, up to 6 files each | Every commit conflicts in `settings.xml`, preferences XML and the nearby chat code; depends on other nearby chat changes missing in 7.2.5. Not worth it unless wanted. |
| `fire36492` (second commit) | Moves chat bar widget properties into the XML definition. | 2 files, +5 -5 | Conflict in `fs_nearby_chat_control.xml`. Only a tidy-up, no user change. |

## C. Already in Firestorm 7.2.5: nothing to pick

The dry run ended without conflict but produced no change, because the change is already in the base. Checked in the files on `main`.

| Source branch | What it is |
|---|---|
| `fire36758` | Hover height window: the +/- buttons' step now reads the live keyboard state (Alt = x10, Ctrl/Shift = x0.1). Already in 7.2.5. |
| `fix-fire32688-patch` | Object cache: a one-word fix so the high-memory limit writes `SceneLoadHighMemoryBound`, not the low one. Already in 7.2.5. |
| `fix-region-title-btns-init` | Group titles window enables its newer region buttons on first load. Already in 7.2.5. |

## Summary

Cleanest and smallest, by user value: rows 1 (clear derendered avatars), 2 (local mesh special characters), 5 and 6 (area search), 7 (AO stuck). Row 4 changes a default. Row 9 touches core code. row 8 is useless without row 12. row 15 is a feature, not a fix.
