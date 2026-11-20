# Plan: local live-pose link in Pinglestorm

Status: **planned, not started.** Starts only when the project owner says "start" or names the job.

## Background, for a developer who has not seen this before

- **Pinglestorm** is the project owner's own fork of the Firestorm viewer for Second Life (base: Firestorm 7.2.5), built to learn, and to be able to add features of our own. See `CLAUDE.md` for the rules this repository works under.
- **PinglyMotion** (`pingledingle-tech/PinglyMotion`) is a separate, free, open-source animation editor for Second Life, based on QAvimator. You pose a Bento skeleton (fingers, face, wings, tail, with limits and mirroring), build up a timeline of keyframes, and export a `.anim`/BVH file for upload.
- **The problem this plan solves:** PinglyMotion already has a "live link" feature — while you pose, your own avatar in a viewer shows the pose immediately, with nothing uploaded, so you can see it on your actual body/shape before exporting anything. Today that only works with **Aurora**, a separate Second Life viewer written in Rust, through a feature of Aurora's called **LiveMesh** (the same local link its Blender add-on uses). Pinglestorm/Firestorm has no equivalent today, so the project owner cannot get this live preview without switching viewers.
- **The goal:** bring the same live-preview capability into Pinglestorm, so PinglyMotion — and later, Blender's mesh/3D tools — can drive a live, local-only pose on the project owner's own avatar inside Pinglestorm itself.
- **Why this is a security-sensitive feature, not just a UI feature:** it means Pinglestorm opens a listener that an outside program connects to and sends it information. That is a new kind of surface a viewer does not normally have (viewers hold the user's login session), which is why this plan treats the security method as a first-class part of the design, not an afterthought, and why it plans to ask Firestorm's own maintainers before assuming they would want it upstream.

## Integration methods considered, and why this one was picked

Several general ways exist to connect two programs running on the same computer. These were discussed and weighed against each other before picking one:

| Method | What it is | Why picked / not picked |
|---|---|---|
| **Local TCP socket with a token (chosen)** | One program listens on `127.0.0.1`, the other connects and exchanges messages. | Portable across operating systems and languages, simple to implement and to test, and — decisively — **Aurora, the Blender add-on, and PinglyMotion already speak exactly this kind of link today**. Reusing their open, documented protocol means PinglyMotion and Blender work with Pinglestorm with no changes on their side. |
| HTTP server with JSON | A small embedded web server inside the viewer. | Easier to poke at by hand (a browser, `curl`), but heavier to implement correctly and a slightly bigger exposed surface (HTTP parsing, routing) for no real benefit here, since nothing needs a browser UI. |
| Named pipe / Unix domain socket | A file-like channel, readable only by the owning user by default. | Safer by default than a plain TCP socket (no port to probe at all), but Windows and Linux implement it differently, which adds platform-specific code for a project that already needs to build and ship on both. The TCP-plus-token approach gets most of the same safety (loopback-only, authenticated) without that split. |
| Shared memory | Direct memory mapping between the two processes. | Fastest option, but tightly coupled (both sides must agree on exact memory layout and lifetime) and fragile to get right. Large overkill for sending a pose a few times a second. |
| A plugin or script inside the viewer (e.g. Lua) | Code running inside the viewer's own sandbox. | Safe by construction, but limited to whatever the viewer's scripting layer exposes — it could not, by itself, let an entirely separate program like PinglyMotion drive the avatar; it only moves where logic runs inside the viewer. |

The TCP-socket-with-token-file approach is also exactly what Aurora already ships and what its own maintainers decided was secure enough for this use case — which is itself a point in its favour when the idea is eventually taken to Firestorm's maintainers: there is precedent, not just a theory.

## Security method (unchanged whether or not Firestorm accepts it upstream)

- Listens only on `127.0.0.1` — never the external network.
- A random 32-byte key is written to a token file when the feature is turned on (readable only by the user's own account), removed when it is turned off.
- Mutual HMAC-SHA256 challenge/response: the key itself is never sent over the connection, only proof that both sides can read the token file. The viewer also proves itself to the client, so the client does not hand poses to an impostor on the port.
- One client at a time. Off by default.
- Same protocol PinglyMotion and the Blender add-on already speak with Aurora (documented, open) — so they work without client-side changes (beyond the one small token-folder change described below).

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

## Roadmap, with how each step is built and tested

**Phase 0 — groundwork (no visible change, can run before "start")**
- Read Aurora's actual protocol source (`crates/aurora-viewer/src/blender_live/` in `pingledingle-tech/Aurora-Viewer-Fork`, files `protocol.rs`, `token.rs`, `transport.rs`) to confirm the wire format by hand, not by description. *Test: write down the exact byte layout of the greeting, the hello, and one pose message, and compare it against PinglyMotion's own `docs/AURORA_LINK.md` — any mismatch is resolved before phase 1 starts.*
- Decide the threading approach for the socket and the JSON/framing approach, after checking what the codebase already links.

**Phase 1 — the link alone**
1. Token file + listener alone, new code under `pingle/`. *Build: a class that, when a settings flag turns on, opens a TCP listener on `127.0.0.1` on a fixed or configurable port, writes the token file, and removes it on shutdown or when turned off. Test: toggle the setting on and off by hand (e.g. from the debug console or a temporary menu item) and check the token file appears and disappears with the right permissions — no client involved yet.*
2. Handshake + HMAC authentication. *Build: the greeting/hello exchange and the challenge/response. Test: a small, separate command-line test script (Python or similar, not PinglyMotion) connects, reads the token file, and completes the handshake — confirm it is accepted with a correct key and rejected with a wrong one.*
3. Pose message → actual movement, via the new `LLMotion` subclass on `gAgentAvatarp`. *Build: the pose message parsing and the `LLMotion` subclass that applies it. Test: extend the same test script to send one hand-written pose message (e.g. rotate one arm) and confirm, by eye on the Windows build, that the joint actually turns on the project owner's own avatar, locally.*

*Decision point:* only after phase 1 actually works end to end, confirmed on Windows, does it make sense to invest in the client-side and UI work below — this is also the point to raise the idea with Firestorm (see below), since the security-sensitive part (the listener) already exists and can be shown working, not just described.

**Phase 2 — PinglyMotion pipeline**
4. Connect PinglyMotion. *Build: the small PinglyMotion-side change — it must also look for the token file in Pinglestorm's data folder, not just Aurora's. Test: with Pinglestorm's listener turned on, enable PinglyMotion's existing "Live link" option, confirm it reports "connected" instead of "waiting", and pose a joint in PinglyMotion to see it move on the avatar in Pinglestorm.*

**Phase 3 — polish**
5. UI: a Build-menu option plus a status indicator. *Build: the same toggle and status pattern Aurora uses. Test: turn the feature on and off purely from the menu, with no console/debug step, and confirm the status indicator matches PinglyMotion's own connection state.*

**Phase 4 — Blender pipeline**
6. Blender add-on + auto-install, the same pattern Aurora uses. *Build: ship the add-on inside Pinglestorm and a one-click installer into Blender's add-ons folder. Test: install from a clean Blender, enable the live link there too, and pose from Blender while Pinglestorm shows it.* Only after phase 2 and 3 are solid.

**Phase 5 — upstream proposal**
- Put the idea, the working phase-1 prototype, and the security model to Firestorm (JIRA issue, see below), ideally before phase 4, so feedback can still change the design.

## Proposal to Firestorm (after it is built and tried here)

Based on what Firestorm's own `CONTRIBUTING.md` actually says:

- A JIRA issue first (`jira.firestormviewer.org`, format `FIRE-12345`).
- A pull request against a release branch (e.g. `Firestorm_7.2.5`), not `master`.
- Changes to LL/Firestorm files wrapped in `<FS>`/`<FS:initials>` tags (not Pinglestorm's own `<PS>` tags — those are only for our fork).
- Run `.clang-format`/`.editorconfig`/`.pre-commit-config.yaml` before submitting.
- Whether Firestorm's own project requires a CLA is not confirmed — unchecked, needs asking directly.
- Recommendation: since this opens a local network listener (a new attack surface), the idea and its security model should be put to the FS developers early — ideally before phase 4 is built — rather than waiting until everything is finished.

## Git/job flow in Pinglestorm (once started)

Follows the normal `pingle/work/` process: its own branch and job file per build step above, a draft pull request from the start. As of 20 Nov 2026, `pingle/**` is broadly claimed by the open test pull request #14 (combined test build of seven Trish picks, never to be merged) — an actual code job file here should wait for that to close, or be cleared separately.
