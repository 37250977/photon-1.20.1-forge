# Changelog
## v2.2.7 — Forge 1.20.1 upstream sync
* Ported upstream `609a975..aac3bc1` (v2.2.6.a to v2.2.7, 31 commits), retaining Forge 1.20.1 / Java 17.
* Updated local dependency version ranges to LDLib2 `2.2.41` and KilaGraph `20.1.0.15`.
* Retained the community port's whole-effect transform (`IWholeEffectTransformer.applyAnimatedModelRotation`)
  on the model path, alongside upstream's new mesh facing modes (Unity RenderAlignment) and ZXY euler order.
* Retained the community port's whole-triangle model sorting: the sorted path now uploads the reordered
  `PhotonMesh.indices()` element buffer, and the model geometry is built on upstream's new indexed-triangle
  three-vertex-stream layout (geometry / attribute / tangent) with the model pivot applied per instance.

## Forge dependency correction
* Removed the Kotlin for Forge build dependency, repository and mandatory mod requirement.
* Uses the revised LDLib2 `2.2.39.a+forge.1.20.1` and KilaGraph `20.1.0.14` JARs without
  the erroneous Kotlin for Forge requirement. Their version numbers are unchanged; replace older JARs.

## v2.2.6.a — Forge 1.20.1 upstream sync
* Ported upstream `b288d1c..609a975` (v2.2.4 to v2.2.6.a), retaining Forge 1.20.1 / Java 17.
* Updated local dependencies to LDLib2 `2.2.39.a+forge.1.20.1` and KilaGraph `20.1.0.14`.
* Added glTF/GLB geometry loading, supplied/generated mesh tangents, and FX Pack model collection.
* Added tangent-aware shader variants and corrected object-space Transform / View Direction nodes.
* Added `FXSceneOptions` for embedded previews and fixed asset-browser creation menus.
* Retained the port's shader preview fixes, material provider editing, whole-FX rotation and test item.
* Adapted billboard tangent handedness to Forge 1.20.1's negative-Z particle normals.
* Included upstream glTF parser and mesh tangent unit tests. In-client UI scenarios remain excluded
  from production builds because the local dependency does not provide the optional test harness.

## Forge 1.20.1 backport maintenance
* Updated the bundled local LDLib2 dependency to the rebuilt 2.2.37 Forge backport.
* Reused LDLib2's native `TreeList` drag-reorder implementation. `ReorderableTreeList` remains as
  a compatibility subtype, while `FXHierarchyView` now consumes `TreeList.ReorderRequest` directly.
* The Forge editor path now shares LDLib2's HDRColor accessor/configurator, custom translucent
  particle queue classification, cached-scene rendering and resource-browser compatibility APIs.

## Upstream v2.2.7
* Added animated glTF models: rigged glb import, animation picker, per-particle animation phase and optional frame blend
* Added dynamic mesh injection, so other mods can feed geometry that changes while it is drawn
* Added optional soft particles to the texture and sprite materials
* Added mesh facing modes to the Model render mode, matching Unity's RenderAlignment
* Added editor keyboard shortcuts for the gizmo, scene view toggles and timeline transport
* Added a playback rate to FXRuntime, scaling the objects and the timeline clock together
* Improved the timeline transport with icon buttons, step and jump-to-end controls
* Fixed GPU instancing aborting the process on a GL 3.2 context
* Fixed a missing material drawing models untransformed under GPU instancing
* Fixed model particles ignoring their parent's rotation and using XYZ euler order instead of Unity's ZXY
* Fixed shader graph texture parameters dropping the wrap and filter picked on them
* Fixed timeline ruler labels being clipped at the edges
* Fixed incorrect first frame data while delayed
* Bump up ldlib2 and kilagraph

## v2.2.4
* Cached the FX listing and dropped it on resource reload
* Fixed a failed shader compile being retried every frame
* Added project icon
* Fixed sun bloom
