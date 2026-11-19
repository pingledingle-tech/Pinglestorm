# Decisions

Newest first. Each entry says what, why and what was turned down. "Proposed" means the project owner has not said yes yet.

## Open decisions (waiting for the project owner)

1. **Credit and authors on picks.** Proposed: rewrite the commit author to the project noreply identity and credit the source by repository and commit hash (in the message body and in `PICKS.md`). Why: the privacy check blocks other people's email addresses, and PinglyMotion's rule is no account names of private persons. Turned down: keeping the original author, which fails the privacy check; exempting picked commits from the check, which weakens it. Alternative: keep the original author and exempt picks. Decide before the first pick.
2. ~~Real fork or one clean commit.~~ **Decided 19 Nov 2026: real fork** (see below). Old text: Cherry-picking from other forks only works well when our repository shares history with Firestorm. A repository filled with one snapshot commit (as PinglyMotion was) has no shared history: every pick must then be applied as a patch (`git format-patch` and `git am`, or by hand), and `-x` hashes still work as notes only. Proposed: a real GitHub fork of the official repository, with a shallow-friendly workflow. This needs the empty `Pinglestorm` repository to be deleted and recreated as a fork (only the project owner can do that). Decide before any code is imported.
3. **Standing permission to merge technical jobs.** Not given. Every merge needs a yes for now.

## Decided

**19 Nov 2026 – FMOD allowed in local builds only.** Old rule: never FMOD, KDU or any other non-free library. New rule: FMOD is allowed in local builds on the owner's own PC (the owner downloads the FMOD Studio SDK with their own FMOD account). FMOD is never committed to the repository, never used in CI, and builds with FMOD are never shared or published. KDU stays forbidden everywhere (not obtainable or licensable). Other non-free libraries stay forbidden. CI and shared builds stay on the `_open` configurations (`ReleaseFS_open`). Reason: the owner wants sound in the viewer on their own PC for learning and testing. Source: approved by the owner in chat, 19 Nov 2026.

**19 Nov 2026 – Real fork of Firestorm, base 7.2.5.** `Pinglestorm` is a GitHub fork of the official Firestorm repository, based on the release branch `Firestorm_7.2.5` (latest official release when chosen). `main` is created from that branch and holds our files on top. Reason: shared history makes `git cherry-pick` from other forks work. Rejected: one snapshot commit (picks would have to be patches). Source: the project owner, 19 Nov 2026.

**Firestorm's own GitHub workflows removed from `main`.** Reason: its `build.yaml` runs on every pull request and every push to `main`, and its other build workflows build with FMOD and KDU (not allowed in CI or in shared builds under our licence rule, see the 19 Nov 2026 entry on FMOD) and run nightly. Removed instead of disabled so that nobody can turn them on by accident. When upstream changes one of these files, merging shows a modify/delete conflict: resolve it by removing the file again. Our own build workflow comes later, as its own job. Source: this setup job.

**Start with rules, import code later.** First the rules, the privacy check and the work board (copied from PinglyMotion, in `pingle/`); then the base; then the first build; then the first pick. Reason: the rules do not depend on the choices above, and a first build tells us how heavy the viewer is before any workflow is added. Not copied now: PinglyMotion's nightly fuzzing, CodeQL, sanitizer and dependency-review workflows (they fit a small Qt program, not a viewer of this size; each can be added as its own job later), and the permanent test page (nothing to test yet).
