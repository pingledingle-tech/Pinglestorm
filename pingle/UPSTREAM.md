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

- **Windows, open build:** `.github/workflows/pingle-build-windows.yml`, started by hand from the Actions tab ("Run workflow"). It builds with `ReleaseFS_open` (no FMOD, no KDU) and Firestorm's own packaging step (`--package`) on a GitHub-hosted `windows-2022` runner. It keeps two artifacts for 7 days: `pinglestorm-windows-base` (the complete program folder: download, extract everything into one new folder, start `FirestormOS-Pinglestorm.exe`) and `pinglestorm-windows-logs` (the build logs, under 1 MB).
- **What was wrong with the first artifact.** The first run (run id 44541790579, on `main`, 19 Nov 2026) built without `--package`. Then only the build-time copy step of `viewer_manifest.py` runs (`--actions=copy`), which copies the binaries and a few data folders but leaves out everything marked "only when packaging": `app_settings/*.xml` (including `settings_files.xml`), the shaders and `skins/`. It is meant for running from Visual Studio with the source folder as working folder. The project owner started that exe on Windows and got "Missing Files" (`LLAppViewer::initConfiguration` cannot read `app_settings/settings_files.xml`).
- **The fix (verified from the build log, not tried on Windows).** With `--package`, `viewer_manifest.py` runs with `copy package`, stages the whole program into `build-*/newview/Release/` and builds an NSIS installer from it (NSIS is preinstalled on the runner, `C:\Program Files (x86)\NSIS`). The workflow copies that folder without build leftovers (Firestorm's own exclude list: `*.pdb`, `*.map`, `*.bat`, `*.exp`, `*.lib`, `*.nsi`, `*.tar.xz`, `firestorm-bin.*`, the installer) and a check step fails the run if the program, `app_settings/settings_files.xml`, the shaders, `skins/` or the main libraries are missing, or if any FMOD or KDU library is present. The installer is built but not uploaded (one download is enough, and installing would register the program in Windows).
- **Packaged run: built, packaged and tried on Windows (21 Nov 2026): the project owner reports that it works.** Run id 44580296192 on branch `ci/windows-package-build` (19 Nov 2026, 17:43 to 18:39 UTC, about 55 minutes, of which about 11 minutes is the installer). The program folder holds 24,738 files, 790 MB unpacked (`llplugin/` 536 MB, `skins/` 92 MB, `fonts/` 42 MB, `app_settings/` 40 MB); the artifact `pinglestorm-windows-base` is about 349 MB as a zip. The file count matches the files `viewer_manifest.py` listed, so nothing else from the build folder is in it. The artifact itself could not be downloaded from the build session, so its contents are known from the check step in the log only.
- **Settings folder.** The program name in the code is fixed (`APP_NAME = "Firestorm"` in `indra/llcommon/indra_constants.h`), so this build uses the same settings folder as an installed 64-bit Firestorm: `%APPDATA%\Firestorm_x64`. Take a copy of that folder before the first start if an installed Firestorm is in use. A separate folder needs our own name (see `CLAUDE.md`, licence and distribution), which is its own job.
- Firestorm's own build page (`doc/building_windows.md`) is the reference for building on your own computer.

## Files of ours that live in Firestorm's folders

`.github/workflows/pingle-*.yml`. The pull request template is `pingle/pull_request_template.md` (Firestorm has its own in `.github/`, which we leave alone): copy its text into the description of every pull request.
