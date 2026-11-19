Branch: pick/trish-rlv-givefolder
Status: active
Pull request: pending
Files:
- indra/newview/llviewermessage.cpp
- indra/newview/rlvinventory.*
- pingle/PICKS.md

## What

Pick (task T04): fix for "Give to #RLV" offers from scripts, where the new
subfolder could get the wrong name (the full path) or land in the wrong place
in #RLV. Source: snapshot branch `snapshot/trish-2026-11-19/fix-rlvgivefolder`,
commit `98dabbdf28be2b3a81b6bf76ddb6b1a9414b6150` (Firestorm tickets
FIRE-35486 and FIRE-35704).
