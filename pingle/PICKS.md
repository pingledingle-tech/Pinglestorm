# Picks from other forks

A pick is a change taken from someone else's fork of Firestorm (or of another viewer). One pick, one branch, one pull request.

## How to pick

1. **Look first, do not fetch yet.** Find the commits on the source fork's GitHub page. Note the repository, branch and commit hashes.
2. **Check the licence** of the source repository and of the files the pick touches. LGPL-compatible and clearly stated: go on. Otherwise stop and tell the project owner.
3. **Check the pick stands alone.** Read the diff: does it need other commits, new libraries or settings? If yes, list them and ask before picking more than was asked.
4. **Check the base.** The source fork must be built on (nearly) the same Firestorm release as ours. If it is on a much newer or older one, expect conflicts and say so before starting.
5. Branch `pick/<source>-<topic>` from `main`, add the job file, open the draft pull request.
6. `git fetch --depth=200 <remote> <branch>`, then `git cherry-pick -x <hash>` for each commit (`-x` writes the source hash into the message). On conflict, solve it inside `// <PS>` markers and say so in the pull request.
7. Rewrite the author to the project's noreply identity (open decision 1 in `DECISIONS.md`) and keep the credit in the commit message body and in the table below.
8. Add a row below, in the same pull request.
9. Remove the remote again when done: `git remote remove <remote>`.

## Log

| Date | Source (repo @ commit) | What | Licence | Credit | Pull request | Built and tried |
|---|---|---|---|---|---|---|
| 2026-11-19 | `snapshot/trish-2026-11-19/fire-37061` @ `c3d918d23` (fork on a newer Firestorm, Oct 2026) | RLVa `@fartouch`: the distance check when selecting an object uses agent space for both positions, so objects across a region border are no longer wrongly out of reach (FIRE-37061). Applied cleanly on 7.2.5; `<PS>` markers added. | LGPL 2.1 (Firestorm headers unchanged) | Source branch and commit hash | #11 | Not built, not tried |
