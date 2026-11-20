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

## Not yet verified

- The Aurora protocol (challenge/response, message format) has only been read from PinglyMotion's own documentation of it, not from Aurora's source code directly.
- Not confirmed that Firestorm 7.2.5 has a clean, reusable way to set joint rotations locally (assumption: the same mechanism used to preview BVH files before upload).

## Build steps (none started)

1. Token file + listener alone, new code under `pingle/`. Test: the file appears/disappears correctly when turned on/off.
2. Handshake + HMAC authentication. Test: a small test script (not PinglyMotion) connects and passes auth.
3. Pose message → actual movement. Find/reuse the viewer's existing local posing. Test: send one pose message by hand, see a joint turn locally.
4. Connect PinglyMotion (needs the small PinglyMotion-side change above). Test: a real live pose from PinglyMotion.
5. UI: a Build-menu option plus a status indicator.
6. Blender add-on + auto-install.

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
