Branch: ci/windows-package-build
Status: done
Pull request: #4
Files:
- .github/workflows/pingle-build-windows.yml
- pingle/UPSTREAM.md
- pingle/DECISIONS.md

## What

The Windows build artifact was only the raw build output folder and stopped at start with "Missing Files". Make the manual Windows workflow package the viewer (Firestorm's own `--package` step) and upload a complete, runnable program folder instead. Open build only (`ReleaseFS_open`), no viewer code changes.
