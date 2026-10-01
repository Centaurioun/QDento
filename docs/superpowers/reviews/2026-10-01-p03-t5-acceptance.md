# Plan 03 / Task 5 Acceptance — Static Periodontal Chart Shell

Date: 2026-10-01

Status: ACCEPTED

Implementation repository:
`Centaurioun/periodontal-ios`

Accepted branch:
`ui/p03-t5-static-chart-shell`

Accepted base:
`ed54a926e4f041b84589f4f9885e0db7eb1d3b9d`

Accepted HEAD:
`ec2108e90e9cd0a4807c0bd528318c5ad2dfb2af`

## Independent coordinator verification

The branch is exactly one commit ahead of the accepted Task 4 base.

Changed files are limited to:
- app/root wiring;
- four periodontal-chart SwiftUI feature files;
- one UI-test file;
- Xcode source registration;
- Task 5 visual evidence/review artifacts.

Accepted Domain, Parity, Geometry, Rendering/provider source, and prototype raster resources are unchanged.

Verified source-level behavior:
- only F01/F02 are exposed;
- F01 + upper arch is the default;
- one arch is shown at a time;
- visible tooth order comes from `ToothGeometry.visibleToothOrder`;
- visible site order comes from `QDentoDisplayAdapter.visibleSitesLeftToRight`;
- 16 × 70 = 1120 chart width is preserved behind horizontal scrolling;
- the label rail stays outside the horizontal ScrollView;
- row metrics are centralized;
- PD/CAL source thresholds are applied in the view;
- QDento display-GM conversion is explicit and occurs once;
- recession delegates to the accepted Domain helper;
- maxillary oral AG remains not-applicable rather than zero;
- non-natural rows fail softly;
- contour input is prepared outside the drawing body and delegates to `ContourGeometry`;
- horizontal arch reversal is not applied a second time;
- view-level source vertical placement matches the frozen formulas;
- tooth artwork is placed underneath contour strokes;
- BOP/FMPS/FMBS visual rows remain reserved and blank for Task 6;
- root and numeric accessibility identifiers exist;
- the UI flow checks exact F02 FDI11 values and arch/fixture switching.

Reported verification:
- focused static chart UI flow: 1 passed;
- Rendering: 6 passed;
- Geometry: 25 passed;
- Domain + Parity: 47 passed;
- full unit tests: 79 passed;
- full UI tests: 2 passed;
- full scheme: 81 passed;
- app build/run: passed on iPhone 18 Pro / iOS 27.0;
- landscape + portrait simulator interaction evidence recorded;
- horizontal/vertical scrolling exercised;
- scroll-indicator overlap found during visual review and fixed;
- final independent reviewers reported no blockers.

## Visual evidence limitation

The committed screenshot blobs exist in the repository and the accompanying visual-review artifact records real simulator capture/inspection. The coordinator GitHub connector can verify those binary blobs exist but cannot decode private repository PNG content into a directly inspectable image in this environment.

Therefore acceptance does not claim an additional independent pixel-level review by the coordinator. It relies on:
- the committed visual-review record;
- runtime UI assertions;
- source-level layout inspection;
- the reported simulator interaction/review pass.

This is acceptable for the Task 5 gate, but Task 6 should again capture fresh simulator evidence and preferably include a frame where the target FDI header and the visual sentinel are simultaneously visible.

## Downstream base rule

Plan 03 / Task 6 must branch from exact accepted HEAD:

`ec2108e90e9cd0a4807c0bd528318c5ad2dfb2af`

## Verdict

**ACCEPTED**

Plan 03 / Task 6 may begin.
