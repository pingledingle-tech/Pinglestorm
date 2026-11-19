# Rules for AI sessions working on Pinglestorm

Pinglestorm (`pingledingle-tech/Pinglestorm`) is the project owner's own version of the Firestorm Viewer for Second Life. It is built from an official Firestorm release, plus changes the project owner picks from other people's forks and changes of our own. The goal is to learn and to be able to make our own features.

Several Claude sessions may work here at the same time. The rules are the ones from PinglyMotion (`pingledingle-tech/PinglyMotion`), cut down to what a viewer fork needs. Where this file says nothing, PinglyMotion's `CLAUDE.md` is the model.

All our own files live in `pingle/` (plus this file and `.github/*pingle*`), so that updating from upstream does not conflict with them. Do not move them into Firestorm's folders.

## Before you start

0. After cloning, run `pingle/tools/setup_clone.sh` (commit author and a pre-push privacy check).
1. Pull the newest `main`. Read `pingle/work/README.md`, `pingle/UPSTREAM.md` and `pingle/DECISIONS.md`. Run `python3 pingle/tools/work.py list`.
2. If another active job already claims a file you need, do not start. Tell the user which job and file, and wait.
3. Create your branch and the job file `pingle/work/<branch with "/" replaced by "-">.md` (`Branch:`, `Status: active`, `Pull request:`, `Files:`, then a short "What"). Commit it first, push, and open a **draft** pull request at once, with the text of `pingle/pull_request_template.md` as its description.
4. A plan is not a start: start a job only when the project owner says "start" or names the job. Background jobs never ask the owner questions; if blocked they stop and report.

## While you work

- Work only on your own branch. Never push to `main`.
- One feature, fix or pick per branch. Stay inside the files you listed; add a file to `Files:` first if you need another.
- **Touch Firestorm's files as little as possible.** Prefer new files. Where you must change a Firestorm file, wrap each change in `// <PS> ... // </PS>` markers (XML: `<!-- <PS> -->`), so a merge from upstream shows at once what is ours.
- **Picks from other forks:** follow `pingle/PICKS.md` (one pick, one branch; `git cherry-pick -x`; log it; check the licence first).
- **Docs in the same pull request:** if your change makes `pingle/UPSTREAM.md`, `pingle/DECISIONS.md` or `pingle/PICKS.md` wrong, fix it in the same pull request.

## Licence and distribution

- Firestorm's code is LGPL 2.1. Keep every licence header and notice. Never remove or rewrite another author's copyright line.
- **FMOD** is allowed in builds. The owner downloads the FMOD SDK with their own FMOD account. The SDK is never committed to the repository and never used in CI. FMOD builds may be shared, free of charge only (non-commercial: no monetisation, sponsorship or promotion), and must credit FMOD as the FMOD EULA requires (attribution, e.g. in the About window/credits).
- **KDU stays forbidden everywhere** (not obtainable or licensable). Other non-free libraries stay forbidden.
- CI always uses the `_open` build configurations (`ReleaseFS_open`).
- Before a pick: read the licence of the source fork. No licence, or a licence that is not LGPL-compatible: do not pick; tell the user.
- The viewer is for personal use for now. Before it is given to anyone else, it needs its own name and branding (no Firestorm name or logo), only code from official release branches as the base, and a check against Linden Lab's Third-Party Viewer Policy. Ask the project owner first.

## Builds and checks

- A pull request runs only the quick checks (privacy, work board). A full viewer build is heavy: do not start one unless the job needs it, and never on every push. How builds are done: `pingle/UPSTREAM.md`.
- Do not claim a change works until it has been built and tried. "Not built" is an honest status.

## When you are done

1. In your job file set `Status: done`, in your last commit.
2. Bring your branch up to date with `main`. Run `python3 pingle/tools/work.py check`.
3. Mark the pull request ready: `gh pr ready <number>`, or where GraphQL is blocked `gh api -X POST repos/pingledingle-tech/Pinglestorm/pulls/<number>/ccr/ready_for_review`.
4. Wait until all required checks are green.
5. Merge with squash: `gh api -X PUT repos/pingledingle-tech/Pinglestorm/pulls/<number>/merge -f merge_method=squash`. There is **no standing permission** yet: every merge needs a yes from the project owner in this session. When a change shows on screen, he tests the Windows build first, one plain step at a time.
6. After the merge: `git switch main && git pull && git branch -D <branch>`.

## Commits

- Author: `pingledingle-tech <351915610+pingledingle-tech@users.noreply.github.com>`; `setup_clone.sh` sets it.
- No personal names or email addresses, and no `Co-Authored-By` or `Claude-Session` lines. Commit messages, issues and pull requests in English.

## Privacy: no personal data anywhere

The repository is public. Never write the name of the project owner or of any other private person (not even a first name or nickname), no email addresses (except GitHub noreply), no Second Life or other account or avatar names, no personal paths (`C:\Users\<you>`), no machine names. Write "the project owner". Check screenshots and logs before attaching them. Authors of upstream projects may be named for credit and licences. Details and the reasons: PinglyMotion's `CLAUDE.md`, section "Privacy". The check is `pingle/tools/check_privacy.py`; it is a safety net, not a replacement for the rule.

## Never

- Never force-push to `main`, delete someone else's branch or rewrite another job's commits.
- Never merge unless all required checks are green and the project owner has said yes.
- Never change repository settings, rules or required checks. Ask the user.
