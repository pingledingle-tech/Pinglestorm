Branch: pick/trish-mouselook-passon
Status: active
Pull request: pending
Files:
- indra/newview/app_settings/settings.xml
- indra/newview/llagent.cpp
- indra/newview/llagent.h
- indra/newview/llagentcamera.cpp
- indra/newview/llagentcamera.h
- indra/newview/llagentlistener.cpp
- indra/newview/llfollowcam.cpp
- indra/newview/llfollowcam.h
- indra/newview/lltoolcomp.cpp
- indra/newview/lltoolgrab.cpp
- indra/newview/llviewermenu.cpp
- indra/newview/llviewermessage.cpp
- indra/newview/llvoavatar.cpp
- indra/newview/llvoavatar.h
- pingle/PICKS.md

## What

Task T02: pick commit 449ec3de9 from the frozen source branch
`snapshot/trish-2026-11-19/trishstorm-develop`. It lets passed-on mouselook
controls (llTakeControls with pass_on TRUE) stop blocking left-clicks, HUD
touch and picking (setting FSMouseLookPassonFix), and stabilises the scripted
follow camera (setting FollowCamFollowSubjectRotation). Not built, not tried.
