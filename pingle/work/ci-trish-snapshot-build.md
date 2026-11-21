Branch: ci/trish-snapshot-build
Status: active
Pull request: #33
Files:
- .github/workflows/pingle-build-windows.yml
- pingle/TASKS.md
- pingle/UPSTREAM.md

## What

Task T22: build the snapshot branch `snapshot/trish-2026-11-19/trishstorm-develop` untouched, pinned to commit `2ccd07a22994b91f3181cde4f4537cc5f21c574b`, with the existing manual Windows workflow (open configuration `ReleaseFS_open`, `--package`, artifact = packaged program folder, no FMOD, no KDU), so the project owner can try graphics, frame rate and stability on Windows. The only change is an optional workflow input `source_ref` that tells the checkout which commit to build. No snapshot code goes into `main`.

## Build

- Source: `snapshot/trish-2026-11-19/trishstorm-develop` @ `2ccd07a22994b91f3181cde4f4537cc5f21c574b`
- Run: 44904792966 (started 21 Nov 2026, 12:36 UTC), running
