Branch: pick/trish-animation-data-lookup
Status: done
Pull request: #28
Files:
- indra/llcharacter/llcharacter.cpp
- indra/llcharacter/llcharacter.h
- indra/llcharacter/llmotioncontroller.cpp
- indra/newview/llhudeffectlookat.cpp
- pingle/PICKS.md

## What

Pick (task T21, part of the performance set with no visible change):
Animation: `LLCharacter` animation data is looked up with `std::string_view` and a transparent map comparator, so per-frame motion code no longer builds a heap string for each lookup; the motion purge returns early when nothing can be purged; and with look-at animation disabled the head rotation motion is stopped only once instead of every frame.
Source: snapshot branch `snapshot/trish-2026-11-19/trishstorm-develop`, commit
`16e0be77629d3d6d7950a353e50d2338dff67b6b`.
