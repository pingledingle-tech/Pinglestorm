# Plan: local live-pose link in Pinglestorm

Status: **planned, not started.** Starts only when the project owner says "start" or names the job.

## Goal

A local, network-based link in Pinglestorm (like Aurora's LiveMesh) that lets PinglyMotion and Blender show a pose on the user's own avatar in real time, with no upload. For the project owner's own use regardless of outcome; proposed to Firestorm (FS) upstream once built and tried.

## Security method (unchanged whether or not FS accepts it)

- Listens only on `127.0.0.1` — never the external network.
- A random 32-byte key is written to a token file when the feature is turned on (readable only by the user's own account), removed when it is turned off.
- Mutual HMAC-SHA256 challenge/response: the key itself is never sent over the connection, only proof that both sides can read the token file.
- One client at a time. Off by default.
- Same protocol PinglyMotion and the Blender add-on already speak with Aurora (documented, open) — so they work without client-side changes.

## Two separate pipelines

1. **PinglyMotion** — no installation needed, just a "Live link" option in Options. Needs one small change in the PinglyMotion repository: it must also look for the token file in Pinglestorm's data folder (today it only looks for Aurora's).
2. **Blender** — its own add-on, auto-installed from Pinglestorm, the same pattern Aurora uses. Comes after pipeline 1 works.

## How it works technically (grounded in Firestorm 7.2.5 source, not built or tried yet)

**Posing the avatar locally: a real, existing mechanism, found in the 7.2.5 code.**

- `LLMotion` (`indra/llcharacter/llmotion.h`) is the base class every animation in the viewer is built on — built-in gestures, uploaded `.anim`/BVH files, all of them.
- `LLJointState::setRotation()` / `setPosition()` (`indra/llcharacter/lljointstate.h`) is the actual per-joint call a motion makes, every frame, to pose a joint. This is the low-level hook the pose link would use: a new, small `LLMotion` subclass (name to be decided, e.g. `LLPoseStreamMotion`) that on each frame reads the latest pose received over the link and calls `setRotation`/`setPosition` on the joints it covers.
- `LLMotionController::registerMotion()` (`indra/llcharacter/llmotioncontroller.h`) registers a new motion type under a UUID; `LLVOAvatar::startMotion()` then plays it.
- Crucially, this can run on the user's **own live avatar**, locally, with no network/upload step: `indra/newview/llfloaterbvhpreview.cpp` already does this today (`LLFloaterBvhPreview`, `mUseOwnAvatar`) — when that flag is set, `mAnimPreview->getPreviewAvatar(this)` returns the real avatar (`gAgentAvatarp`) instead of a dummy one, and the BVH preview plays on it, visible only to the user, before any upload. The pose link's own motion would do the same: attach to `gAgentAvatarp`, never call anything that sends the pose to the simulator.
- Open point, not yet checked: how cheaply a custom `LLMotion` can be driven from network data arriving on its own thread. The socket I/O must happen off the main/render thread; applying the pose (the `setRotation` calls) must happen on the main thread, so the plan needs a small thread-safe queue between the two, not shared state touched from both.

**The link itself.**

- A new, small module under `pingle/` (own files, not Firestorm's), owning: the listener socket, the token file, the HMAC handshake, and the queue that hands poses to the `LLMotion` above.
- Threading: socket accept/read loop on a worker thread (standard C++ `std::thread`, or the viewer's own thread-pool primitives if that fits better — to be decided when this step starts); only the decoded pose is pushed onto a lock-protected queue the main loop drains once per frame.
- A `settings.xml` entry (default off) turns the listener on or off; a small Build-menu item and a status indicator (same idea as Aurora's) toggle it and show the connection state, reusing the existing menu/floater patterns Firestorm already has for similar toggles.
- Message framing and JSON: needs checking what the codebase already links (likely `LLSD` for the viewer's own wire formats, or a JSON library already vendored for something else) rather than adding a new JSON dependency; to be confirmed when step 2 below starts.

## Not yet verified

- The Aurora protocol (challenge/response, message format) has only been read from PinglyMotion's own documentation of it, not from Aurora's Rust source code directly.
- The local-posing mechanism above is grounded in real classes and a real existing use (`LLFloaterBvhPreview`'s own-avatar preview), but nobody has written or built the new `LLMotion` subclass yet — "found a working example to copy the pattern from" is not the same as "built and tried".
- Threading approach for the socket (own thread vs. the viewer's existing thread-pool) is not decided.
- Which JSON/length-framing approach fits the codebase best is not decided.

## Roadmap

**Phase 0 — groundwork (no visible change, can run before "start")**
- Read Aurora's actual protocol source (not just PinglyMotion's docs of it) to confirm the wire format by hand, not by description.
- Decide the threading approach for the socket and the JSON/framing approach.

**Phase 1 — the link alone (steps 1–3 below)**
1. Token file + listener alone, new code under `pingle/`. Test: the file appears/disappears correctly when turned on/off.
2. Handshake + HMAC authentication. Test: a small test script (not PinglyMotion) connects and passes auth.
3. Pose message → actual movement, via the new `LLMotion` subclass on `gAgentAvatarp`. Test: send one pose message by hand, see a joint turn locally.

*Decision point:* only after phase 1 actually works end to end does it make sense to invest in the client-side and UI work below — this is the point to also raise the idea with Firestorm (see below), since the security-sensitive part (the listener) already exists and can be shown, not just described.

**Phase 2 — PinglyMotion pipeline (step 4)**
4. Connect PinglyMotion (needs the small PinglyMotion-side change: it must also look for the token file in Pinglestorm's data folder, not just Aurora's). Test: a real live pose from PinglyMotion.

**Phase 3 — polish (step 5)**
5. UI: a Build-menu option plus a status indicator.

**Phase 4 — Blender pipeline (step 6)**
6. Blender add-on + auto-install, the same pattern Aurora uses. Only after phase 2 and 3 are solid.

**Phase 5 — upstream proposal**
- Put the idea, the working phase-1 prototype, and the security model to Firestorm (JIRA issue, see below), ideally before phase 4, so feedback can still change the design.

## Proposal to Firestorm (after it is built and tried here)

Based on what Firestorm's own `CONTRIBUTING.md` actually says:

- A JIRA issue first (`jira.firestormviewer.org`, format `FIRE-12345`).
- A pull request against a release branch (e.g. `Firestorm_7.2.5`), not `master`.
- Changes to LL/Firestorm files wrapped in `<FS>`/`<FS:initials>` tags (not Pinglestorm's own `<PS>` tags — those are only for our fork).
- Run `.clang-format`/`.editorconfig`/`.pre-commit-config.yaml` before submitting.
- Whether Firestorm's own project requires a CLA is not confirmed — unchecked, needs asking directly.
- Recommendation: since this opens a local network listener (a new attack surface), the idea and its security model should be put to the FS developers early — ideally before pipeline 2 is built — rather than waiting until everything is finished.

## Git/job flow in Pinglestorm (once started)

Follows the normal `pingle/work/` process: its own branch and job file per build step above, a draft pull request from the start. As of 20 Nov 2026, `pingle/**` is broadly claimed by the open test pull request #14 (combined test build of seven Trish picks, never to be merged) — an actual code job file here should wait for that to close, or be cleared separately.
