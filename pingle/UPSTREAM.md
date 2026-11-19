# Upstream and branches

## Base

- Upstream: `https://github.com/FirestormViewer/phoenix-firestorm` (official Firestorm source, LGPL 2.1).
- Build variables (needed to build): `https://github.com/FirestormViewer/fs-build-variables`.
- **Base version: Firestorm 7.2.5** (branch `Firestorm_7.2.5`, tag `Firestorm_Release_7.2.5.81788`, commit `4ac201fd`). Rule: use an official release branch or tag, not `master` (Firestorm's own request for anyone who uses its code beyond personal use).
- Pinglestorm is a real GitHub fork of the upstream repository, so it shares history with Firestorm and with other forks of it. Checkouts are shallow (`--depth 50`); deepen only with a bounded `git fetch --depth=N`.

## Branches

| Branch | Holds |
|---|---|
| `main` | The release base plus our changes. Protected: pull requests only. |
| `Firestorm_7.2.5` and the other release branches | Come with the fork. Untouched copies of Firestorm's releases. Never edited by hand. |
| `pick/<source>-<topic>` | One pick from another fork (see `PICKS.md`). |
| `feat/…`, `fix/…`, `docs/…` | Our own jobs. |

## Remotes

```
git remote add upstream https://github.com/FirestormViewer/phoenix-firestorm
git remote add <friend> https://github.com/<friend>/<repo>     # only while picking
```

Fetch with a depth, never the whole history of a fork: `git fetch --depth=200 <friend> <branch>`.

## Moving to a newer Firestorm release

Later, as its own job, not now: fetch the new release branch, merge it into a branch off `main`, and settle conflicts. The `// <PS>` markers show what is ours. Log it in `DECISIONS.md`.

## Building

- **Windows, open build:** `.github/workflows/pingle-build-windows.yml`, started by hand from the Actions tab ("Run workflow") on `main`. It builds the untouched base with `ReleaseFS_open` (no FMOD, no KDU) on a GitHub-hosted `windows-2022` runner and keeps the result for 7 days as the artifact `pinglestorm-windows-base`.
- **Not yet known:** whether it builds, how long it takes, and where the finished program ends up (the artifact paths are a first guess). The first run answers that; fix this section then.
- Firestorm's own build page (`doc/building_windows.md`) is the reference for building on your own computer.

## Files of ours that live in Firestorm's folders

`.github/workflows/pingle-*.yml`. The pull request template is `pingle/pull_request_template.md` (Firestorm has its own in `.github/`, which we leave alone): copy its text into the description of every pull request.
