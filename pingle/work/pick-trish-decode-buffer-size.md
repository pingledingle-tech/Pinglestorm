Branch: pick/trish-decode-buffer-size
Status: active
Pull request: pending
Files:
- indra/llimage/llimageworker.cpp
- pingle/PICKS.md

## What

Pick (task T21, part of the performance set with no visible change):
Textures: when a JPEG2000 texture is decoded at a reduced resolution (discard level above 0), the output buffer is allocated at that reduced size instead of full size first (a 4096x4096 texture decoded at discard 3 briefly took 64 MB to hold 1 MB).
Source: snapshot branch `snapshot/trish-2026-11-19/trishstorm-develop`, commit
`2dc81fb260dafab099c02bd249158122d7ea3096`.
