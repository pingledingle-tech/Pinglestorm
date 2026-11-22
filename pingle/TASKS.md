# Task list

This is the task list the project owner approved on 19 Nov 2026. Nothing in it is started. A task starts only when the project owner says "start" or names the task. Every pick from another fork follows `pingle/PICKS.md`.

The source is a frozen copy of another fork, stored in this repository as branches named `snapshot/trish-2026-11-19/<branch>`. Tasks refer to the source only by those branch names.


## Step 0

- [x] T01 Try the packaged Windows base build: the project owner downloads the build, starts it, logs in and walks around. Everything else waits for this.
  - Source: none (our own base build)
  - Status: Done 21 Nov 2026. The first (unpackaged) build failed to start on 19 Nov ("Missing Files", no `--package` step). The packaged build (run 44580296192) was downloaded and tried on Windows by the project owner, who reported "it works" (his words; no further detail given).


## Step 1: Mouselook and RLV (first priority)

- [ ] T02 Mouselook: let clicks and HUD touches through when a script has taken controls with pass-on. The same commit also holds a fix for scripted follow camera.
  - Source: commit 449ec3de9 on `snapshot/trish-2026-11-19/trishstorm-develop`, setting `FSMouseLookPassonFix`, 14 files, about 186 lines
  - Status: Not started. Not checked whether it applies on Firestorm 7.2.5. Not built, not tried.

- [ ] T03 Mouselook: do not draw navmesh in mouselook unless the pathfinding window was open. First find the change and see if it can be separated.
  - Source: inside a larger combined commit on `snapshot/trish-2026-11-19/trishstorm-develop`
  - Status: Not started. Not checked whether it applies on Firestorm 7.2.5. Not built, not tried.

- [ ] T04 RLV fix: give-to-#RLV subfolder gets wrong name and place.
  - Source: `snapshot/trish-2026-11-19/fix-rlvgivefolder` (3 files, about 142 lines)
  - Status: Not started. Not checked whether it applies on Firestorm 7.2.5. Not built, not tried.

- [ ] T05 RLV fix: SHOW_LOC can be bypassed through "Set Location".
  - Source: `snapshot/trish-2026-11-19/fix-showlocrlva` (2 files, about 33 lines)
  - Status: Not started. Not checked whether it applies on Firestorm 7.2.5. Not built, not tried.

- [ ] T06 RLV fix: SHOW_LOC can be bypassed at a new login.
  - Source: `snapshot/trish-2026-11-19/fire-36682` (1 file, about 8 lines)
  - Status: Not started. Not checked whether it applies on Firestorm 7.2.5. Not built, not tried.

- [ ] T07 RLV fix: fartouch distance in agent space.
  - Source: `snapshot/trish-2026-11-19/fire-37061` (1 file, about 3 lines)
  - Status: Not started. Not checked whether it applies on Firestorm 7.2.5. Not built, not tried.

- [ ] T08 RLV fix: tplocal on XYZ axis instead of the 256 m default.
  - Source: `snapshot/trish-2026-11-19/fire-37059` (2 files, about 10 lines)
  - Status: Not started. Not checked whether it applies on Firestorm 7.2.5. Not built, not tried.

- [ ] T09 RLV fix: RLVa in flycam.
  - Source: `snapshot/trish-2026-11-19/fire-37066` (3 files, about 114 lines)
  - Status: Not started. Not checked whether it applies on Firestorm 7.2.5. Not built, not tried.

- [ ] T10 RLV additions (new commands): @showareasearch, @showfavwear, @showlookat, @sendlookat, @showcontacts, @showsearch, @worldsounds, @soundothers, @soundself, and the login privacy guard. Investigation only until the project owner decides: find out whether they can be separated from the large stack.
  - Source: inside the large stack on `snapshot/trish-2026-11-19/trishstorm-develop`
  - Status: Not started. Not checked whether it applies on Firestorm 7.2.5. Not built, not tried.


## Step 2: Other small fixes

- [ ] T11 Bake-on-mesh bake and outfit window out of sync.
  - Source: `snapshot/trish-2026-11-19/fix-bomdesyncs` (7 files, about 180 lines)
  - Status: Not started. Not checked whether it applies on Firestorm 7.2.5. Not built, not tried.

- [ ] T12 Adjustment window remembers its position.
  - Source: `snapshot/trish-2026-11-19/fire-36531`
  - Status: Not started. Not checked whether it applies on Firestorm 7.2.5. Not built, not tried.

- [ ] T13 Omnifilter allows empty names.
  - Source: `snapshot/trish-2026-11-19/fire-36860`
  - Status: Not started. Not checked whether it applies on Firestorm 7.2.5. Not built, not tried.

- [ ] T14 Trash icon that matches the right-click menu.
  - Source: `snapshot/trish-2026-11-19/unwear-trashicon`
  - Status: Not started. Not checked whether it applies on Firestorm 7.2.5. Not built, not tried.

- [ ] T15 Go through the remaining ~20 small fix branches in the snapshot and list them for the project owner to choose from. A list only, no picks.
  - Source: the other `snapshot/trish-2026-11-19/*` branches
  - Status: List done (see `pingle/T15_REMAINING_FIXES.md`): 15 branches apply cleanly in a dry run on 7.2.5, 4 conflict, 3 are already in 7.2.5. No picks made. Not built, not tried.


## Step 3: Graphics and performance commits that applied cleanly in a dry run

All on `snapshot/trish-2026-11-19/trishstorm-develop`. The first step of each task is to find the exact commit hash.

- [ ] T16 Colour grading with 3D LUT.
  - Source: `snapshot/trish-2026-11-19/trishstorm-develop`, commit hash to be found
  - Status: Applied without conflict in a dry run on Firestorm 7.2.5. Not built, not tried.

- [ ] T17 Supersampling up to 2x.
  - Source: `snapshot/trish-2026-11-19/trishstorm-develop`, commit hash to be found
  - Status: Applied without conflict in a dry run on Firestorm 7.2.5. Not built, not tried.

- [ ] T18 Cheaper screen-space reflection marching.
  - Source: `snapshot/trish-2026-11-19/trishstorm-develop`, commit hash to be found
  - Status: Applied without conflict in a dry run on Firestorm 7.2.5. Not built, not tried.

- [ ] T19 Option: transparent objects cast no shadow (off by default).
  - Source: `snapshot/trish-2026-11-19/trishstorm-develop`, commit hash to be found
  - Status: Applied without conflict in a dry run on Firestorm 7.2.5. Not built, not tried.

- [ ] T20 PBR blend/mask without true transparency.
  - Source: `snapshot/trish-2026-11-19/trishstorm-develop`, commit hash to be found
  - Status: Applied without conflict in a dry run on Firestorm 7.2.5. Not built, not tried.

- [ ] T21 Performance set with no visible change: less allocation in texture decoding, animation data lookup without allocation, less work for media, skip timer snapshot, minimap colours cached, scripted alpha fades without geometry rebuild, fix for hero probe list changed while read, simplify lltrunc and ll_round. One pick per branch still applies.
  - Source: `snapshot/trish-2026-11-19/trishstorm-develop`, commit hashes to be found
  - Status: Applied without conflict in a dry run on Firestorm 7.2.5. Not built, not tried.


## Step 4: The large graphics and performance stack

- [ ] T22 Build the snapshot branch untouched (open configuration, pinned to one commit) so the project owner can try graphics, frame rate and stability on their own PC. Nothing goes into `main`.
  - Source: `snapshot/trish-2026-11-19/trishstorm-develop`
  - Status: Not started. Not checked whether it applies on Firestorm 7.2.5. Not built, not tried.

- [ ] T23 Decide the route after T22. (A) Move the base to Firestorm 7.3.0 when it is official and take the whole stack from there in its own branch (recommended: fewest conflicts, the stack is built on that base). (B) An experiment branch on 7.2.5 now. (C) Leave the stack. The project owner decides.
  - Source: none (decision)
  - Status: Not started. Waiting for T22.

- [ ] T24 Move the base to Firestorm 7.3.0 when it is an official release.
  - Source: official Firestorm release
  - Status: Not started. Waiting for T22, T23 and the official release.

- [ ] T25 Take in the whole rendering and performance stack in its own branch. It is a chain of several hundred commits and cannot be picked feature by feature. Everything new stays off by default.
  - Source: `snapshot/trish-2026-11-19/trishstorm-develop`
  - Status: Not started. Waiting for T22 and T23. Not built, not tried.


## Step 5: The rest (after the graphics route is chosen; they touch the same files)

- [ ] T26 Ctrl+drag for HUD attachments.
  - Source: `snapshot/trish-2026-11-19/fs-ctrl-drag` (6 files, about 334 lines)
  - Status: Not started. Not checked whether it applies on Firestorm 7.2.5. Not built, not tried.

- [ ] T27 Better copy and paste in inventory.
  - Source: `snapshot/trish-2026-11-19/fs-improve-copypaste` (9 files, about 297 lines)
  - Status: Not started. Not checked whether it applies on Firestorm 7.2.5. Not built, not tried.

- [ ] T28 Camera bookmarks and camera tours (about 5,200 lines).
  - Source: in the large stack on `snapshot/trish-2026-11-19/trishstorm-develop`
  - Status: Not started. Not checked whether it applies on Firestorm 7.2.5. Not built, not tried.

- [ ] T29 Undo/redo in building.
  - Source: in the large stack on `snapshot/trish-2026-11-19/trishstorm-develop`
  - Status: Not started. Not checked whether it applies on Firestorm 7.2.5. Not built, not tried.

- [ ] T30 Script profiler.
  - Source: in the large stack on `snapshot/trish-2026-11-19/trishstorm-develop`
  - Status: Not started. Not checked whether it applies on Firestorm 7.2.5. Not built, not tried.


## Ideas to explore (not yet scheduled)

- [ ] T31 Ground sit: look into how the viewer plays the built-in ground-sit animation (sitting directly on the ground, no object to sit on), and whether an AO (Animation Overrider) can already override it the same way it overrides a normal object sit, or whether ground sit needs its own viewer-side hook. Investigation only; no branch, pick or build started yet.
  - Source: none yet (our own investigation; check whether Trish's or i-doll's forks touch this)
  - Status: Not started.


## Dependencies

- T01 comes before every other task.
- T24 and T25 wait for T22 and T23.
- Step 5 (T26 to T30) waits for T23.

## Rules for every task

- One pick, one branch, one pull request. Follow `pingle/PICKS.md`, including the licence check before a pick.
- Changes in Firestorm's files go inside `// <PS> ... // </PS>` markers (XML: `<!-- <PS> -->`). Our own files live in `pingle/`.
- A viewer build takes about 47 minutes. Do not build on every push.
- Visible changes are tried by the project owner on Windows before the merge, one plain step at a time.
- No merge without the project owner's yes and green required checks.
- Never claim a change works until it has been built and tried. "Not built" is an honest status.
- Changes from the source to audio-thread code are checked for FMOD-only code before a pick. FMOD code is never picked.
- Never FMOD, KDU or other non-free libraries. Open build only.
