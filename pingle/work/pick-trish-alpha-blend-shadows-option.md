Branch: pick/trish-alpha-blend-shadows-option
Status: done
Pull request: #24
Files:
- indra/newview/app_settings/settings.xml
- indra/newview/app_settings/settings_text.xml
- indra/newview/lldrawpoolavatar.cpp
- indra/newview/pipeline.cpp
- pingle/PICKS.md

## What

Pick (task T19): new setting `RenderAlphaBlendShadows` lets alpha-blended
(transparent) objects stop casting sun shadows, and `RenderAvatarShadowDetail`
can leave out avatar alpha shadow passes. Adapted so both keep today's
behaviour by default (`RenderAlphaBlendShadows` defaults to on here; the
source turns it off). Source: snapshot branch
`snapshot/trish-2026-11-19/trishstorm-develop`, commit
`8039997a4b5ab967b10945b47b53c9c227a69f86`.
