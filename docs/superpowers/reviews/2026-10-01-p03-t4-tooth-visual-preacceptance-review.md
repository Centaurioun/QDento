# Plan 03 / Task 4 Coordinator Pre-Acceptance Review

Date: 2026-10-01

Status: REMEDIATION_REQUIRED

Candidate:
- repo: `Centaurioun/periodontal-ios`
- branch: `rendering/p03-t4-tooth-visual-provider`
- HEAD: `87f853e2f0d382e2b9c9d610a30b384c1ac5b52c`

## Independent verification

The candidate is exactly one commit ahead of the accepted Task 3 base and stays within the intended rendering/provider/resource/provenance scope.

Verified:
- 16 natural + 2 implant prototype resource outputs are present in the diff;
- all-32 FDI → prototype resource mapping matches the frozen table;
- natural/missing/implant remain semantically distinct;
- missing reuses natural art at opacity 0.1;
- implant uses molar vs non-molar resources correctly;
- Q1/Q2/Q3/Q4 orientation mapping matches the source-backed contract;
- provider protocol does not expose Qt/source atlas indices;
- provenance remains explicitly private-demo/GPL-context/public-distribution-gated;
- no periodontal/treatment overlay assets were added.

The Final Code Quality Reviewer's broader rendering concern is partly out of scope, but independent coordinator review found one concrete Task-4-owned parity defect that must be corrected before static-chart integration.

## Finding 1 — IMPORTANT — raw crop aspect ratio is not QDento periodontal-strip layout

The current descriptor uses the raw extracted body crop size:
- non-molar: 120×860;
- molar: 180×860;

and `ToothGraphicView` applies `.aspectRatio(descriptor.intrinsicAspectRatio, .fit)` directly to that raw crop.

QDento does NOT present the 860-high crop directly.

Source composition is:

1. `getTooth()` produces the 120/180 × 860 body;
2. `getToothPixmap()` creates a 1140-high transparent canvas and translates the body by y=140;
3. `getBuccalLingual()` creates the periodontal-strip pixmap at height **1106**, applies the quadrant transform to that full canvas, and draws the composed tooth pixmap;
4. `ToothGraphicsItem::showLingual(true)` displays that result at height **332**, with tooth pixel width **36** for frontal/premolar and **54** for molars, centered inside a 70-wide tooth slot.

Therefore the current direct raw-crop aspect fit:
- uses the wrong intrinsic aspect ratio;
- loses the source top/bottom transparent layout;
- applies quadrant transforms around the wrong visual canvas;
- will make tooth width/vertical placement visibly diverge when Task 5 builds the real chart.

This is not merely a future chart-shell concern; `ToothGraphicView` already owns how descriptor artwork is composed.

## Required correction

Keep the existing 18 crop files. Do NOT extract more rasters.

Add explicit provider-owned layout metadata separating:
- resource pixel size;
- periodontal-strip canvas size;
- body placement inside that canvas.

Recommended equivalent:

```swift
struct ToothVisualLayout: Equatable, Sendable {
    let canvasPixelSize: CGSize
    let bodyFrame: CGRect
}
```

For every current prototype body:
- canvas width = body resource width (120 or 180);
- canvas height = **1106**;
- body frame = x0, y140, width=resource width, height=860.

The descriptor's intrinsic display aspect ratio must come from the full canvas, not the raw body crop.

`ToothGraphicView` must:
1. compose the body into that transparent canvas using the descriptor's body frame;
2. apply opacity to the body layer;
3. apply the descriptor orientation to the composed canvas, not just to an 860-high raw image;
4. preserve the full-canvas aspect ratio.

No new source raster is required.

Add tests proving:
- non-molar prototype canvas = 120×1106 with body frame (0,140,120,860);
- molar canvas = 180×1106 with body frame (0,140,180,860);
- missing and implant use the same correct canvas semantics for their resource width;
- intrinsic aspect ratio comes from canvas size;
- the body frame is wholly contained inside the 1106-high canvas.

A concise source-parity check may also establish that a 332-high rendered canvas yields approximately the QDento source widths of 36 (120-wide class) and 54 (180-wide class), within floating-point tolerance.

## Finding 2 — MINOR — unused/misleading provider inputs

Current `PrototypeToothVisualProvider` has an overload:

`descriptor(for:view:bounds:scale:)`

but `bounds` and `scale` are ignored.

It also stores `ToothVisualView.buccal/lingual`, although the copied QDento periodontal body strip is the combined `getBuccalLingual()` artwork rather than two independently selected body resources.

This creates API surface that implies behavior the implementation does not provide.

Required correction:
- remove unused `bounds` and `scale` parameters;
- remove `ToothVisualView` / descriptor `view` unless the implementation actually needs a source-backed distinction;
- keep the public provider protocol minimal: `descriptor(for position: ChartPositionRecord)`.

Do not add speculative future inputs.

## Scope

Allowed source changes:
- `PeriodontalIOS/Rendering/ToothVisualDescriptor.swift`
- `PeriodontalIOS/Rendering/PrototypeToothVisualProvider.swift`
- `PeriodontalIOS/Features/PeriodontalChart/ToothGraphicView.swift`
- `PeriodontalIOSTests/Rendering/ToothVisualProviderTests.swift`

Update provenance only if needed to document the 1106/140 composition metadata.

Do NOT:
- add or change raster assets;
- modify Domain/Parity/Geometry;
- add chart shell;
- add overlays/BOP/wedges;
- add interaction.

## Verdict

**REMEDIATION_REQUIRED**

Do not begin Task 5 static-chart integration until these rendering-boundary issues are corrected and independently re-reviewed.
