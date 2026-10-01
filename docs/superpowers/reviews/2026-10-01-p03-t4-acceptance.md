# Plan 03 / Task 4 Acceptance — Tooth Visual Provider + Prototype Assets

Date: 2026-10-01

Status: ACCEPTED

Implementation repository:
`Centaurioun/periodontal-ios`

Accepted branch:
`rendering/p03-t4-tooth-visual-provider`

Original candidate:
`87f853e2f0d382e2b9c9d610a30b384c1ac5b52c`

Accepted remediated HEAD:
`ed54a926e4f041b84589f4f9885e0db7eb1d3b9d`

## Independent coordinator verification

The Task 4 implementation remains one bounded rendering/provider tranche above the accepted Task 3 base.

Accepted properties:
- provider protocol remains semantic and independent of Qt/QPainter/source atlas indices;
- all 32 permanent FDIs map to the approved private prototype resources, tooth classes, and quadrant orientations;
- natural, missing, and implant are distinct visual states;
- missing reuses the natural body at opacity 0.1;
- implant uses separate molar/non-molar resources;
- exactly 18 approved prototype body PNGs are present;
- no periodontal/treatment/root/etc. QDento raster layers were added;
- provenance remains explicit about GPL-3.0 repository context, unresolved image-level authorship, PRIVATE DEMO ONLY use, and the later public/proprietary distribution gate;
- all 18 resources resolve from the built app bundle.

## Remediation accepted

Coordinator pre-acceptance review found that the first candidate used the raw 120/180×860 body crop as the visual canvas, while QDento's periodontal strip composes the body into a taller transparent canvas before quadrant transformation.

The accepted remediation:
`ed54a926e4f041b84589f4f9885e0db7eb1d3b9d`

now preserves:
- raw body resource sizes: 120×860 or 180×860;
- logical periodontal-strip canvas: 120/180 × 1106;
- body placement: y=140, height=860;
- intrinsic aspect ratio from the full 1106-high canvas;
- quadrant orientation on the full composed canvas;
- no raster recrop or hash change.

The remediation also removes unused speculative `view`, `bounds`, and `scale` provider inputs.

At a 332-high source display:
- 120/1106×332 ≈ 36;
- 180/1106×332 ≈ 54;

matching the QDento periodontal tooth-width classes.

Reported final verification:
- focused rendering tests: 6 passed;
- Geometry: 25 passed;
- Domain + Parity: 47 passed;
- full unit tests: 79 passed;
- UI launch regression: passed;
- app build: passed;
- exactly 18 bundle resources remain;
- PNG hashes unchanged;
- fresh source-composition and SwiftUI-boundary reviewers passed.

## Downstream base rule

Plan 03 / Task 5 must branch from exact accepted HEAD:

`ed54a926e4f041b84589f4f9885e0db7eb1d3b9d`

## Verdict

**ACCEPTED**

Plan 03 / Task 5 static chart integration may begin.
