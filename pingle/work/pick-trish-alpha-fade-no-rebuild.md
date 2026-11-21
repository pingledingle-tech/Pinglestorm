Branch: pick/trish-alpha-fade-no-rebuild
Status: active
Pull request: pending
Files:
- indra/newview/llvovolume.cpp
- indra/newview/llviewerregion.cpp
- pingle/PICKS.md

## What

Pick (task T21, part of the performance set with no visible change):
Objects: `LLVOVolume::setTEColor` rebuilt the geometry on every alpha change, so smooth scripted fades rebuilt the object every step; it now rebuilds only when alpha crosses a render class (invisible, blended, nearly opaque, opaque) and otherwise uses the vertex colour update. The same source commit also makes object creation from the object cache keep to the per-frame time budget after the count throttle runs out (`LLViewerRegion::createVisibleObjects`).
Source: snapshot branch `snapshot/trish-2026-11-19/trishstorm-develop`, commit
`8d165c90f6296c1f71466ddd276ac7d31cdcc87d`.
