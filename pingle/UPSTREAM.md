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

Not set up yet. Plan, in this order (see `DECISIONS.md`):

1. Build the untouched base once, to learn whether it builds at all (Windows, `ReleaseFS_open`, Visual Studio 2022, Python 3 and autobuild per Firestorm's own build page).
2. Only then decide where builds run (own computer or GitHub Actions) and add a workflow.

## Files of ours that live in Firestorm's folders

`.github/workflows/pingle-*.yml`. The pull request template is `pingle/pull_request_template.md` (Firestorm has its own in `.github/`, which we leave alone): copy its text into the description of every pull request.
