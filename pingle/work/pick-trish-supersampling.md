Branch: pick/trish-supersampling
Status: done
Pull request: #23
Files:
- indra/newview/pipeline.cpp
- pingle/PICKS.md

## What

Pick (task T17): `RenderResolutionMultiplier` above 1 supersamples the
3D scene, up to 2x (capped at the largest texture size the GPU allows).
Only the main scene target is enlarged; the final present scales it down.
Default stays 1.0, so nothing changes until the setting is raised. Source:
snapshot branch `snapshot/trish-2026-11-19/trishstorm-develop`, commit
`f1c3f66b66f0c50c426e9561697359b70075c9e7`.
