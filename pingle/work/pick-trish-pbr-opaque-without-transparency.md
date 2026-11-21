Branch: pick/trish-pbr-opaque-without-transparency
Status: active
Pull request: pending
Files:
- indra/newview/llvovolume.cpp
- indra/newview/pipeline.cpp
- indra/newview/app_settings/settings.xml
- pingle/PICKS.md

## What

Pick (task T20): PBR (GLTF) materials marked alpha BLEND or MASK are drawn
as opaque when they have no real transparency (base alpha 1 and no alpha in
the base texture), which saves sorting and blending. Adapted: behind our own
debug setting `PSRenderPBRBlendAsOpaque`, default off, so nothing changes by
default. Source: snapshot branch
`snapshot/trish-2026-11-19/trishstorm-develop`, commit
`cb5dd1c57b8729cf513c7d1f8275a1dc0754fab4`.
