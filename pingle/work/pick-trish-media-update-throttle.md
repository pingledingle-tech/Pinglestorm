Branch: pick/trish-media-update-throttle
Status: active
Pull request: pending
Files:
- indra/newview/llviewermedia.cpp
- pingle/PICKS.md

## What

Pick (task T21, part of the performance set with no visible change):
Media: `LLViewerMedia::updateMedia` still updates every media instance every frame, but the interest calculation, sort and priority pass run at about 4 times a second, or at once when the set of media or any media's visible, focus, loaded or playing state (or the media/music settings, window focus) changes.
Source: snapshot branch `snapshot/trish-2026-11-19/trishstorm-develop`, commit
`50ab2f54e52f97abda4c82b53582ffde34fc7663`.
