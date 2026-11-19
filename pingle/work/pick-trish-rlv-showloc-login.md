Branch: pick/trish-rlv-showloc-login
Status: active
Pull request: pending
Files:
- indra/newview/llinspectremoteobject.cpp
- pingle/PICKS.md

## What

Pick (task T06, Firestorm ticket FIRE-36682): the remote object inspector
(opened from an object's name in chat) could show the region SLurl under the
RLVa restriction `@showloc` when the chat arrived before the restriction was
set, for example right after login. The pick checks the active restriction
when the inspector is shown. Source: snapshot branch
`snapshot/trish-2026-11-19/fire-36682`, commit
`eb1d70690ebd592fa568cf76fc1df5c6867aaa01`.
