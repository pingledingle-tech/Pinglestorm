Branch: pick/trish-rlv-flycam
Status: done
Pull request: #13
Files:
- indra/newview/llviewerjoystick.*
- indra/newview/llviewermenu.cpp
- pingle/PICKS.md

## What

Pick (task T09, Firestorm ticket FIRE-37066): the joystick/3D-mouse flycam
ignored RLVa camera restrictions. The pick blocks flycam (and greys its menu
item) under `@setcam` and `@setcam_unlock`, leaves flycam when such a
restriction appears, and keeps the RLVa camera distance and field-of-view
limits while flycam is on. Source: snapshot branch
`snapshot/trish-2026-11-19/fire-37066`, commit
`06224c827b005b98a4bb95be42ea0ada4692c4de`.
