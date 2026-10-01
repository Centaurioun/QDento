# Plan 03 / Task 3 Acceptance — Four-Wedge + BOP Anchor Geometry

Date: 2026-10-01

Status: ACCEPTED

Implementation repository:
`Centaurioun/periodontal-ios`

Accepted branch:
`geometry/p03-t3-wedge-bop`

Accepted HEAD:
`7bb307ba161345890293ffc2a36f9dfd37b093c4`

Base:
`ee3497a9259059beb604f58e4f5a5054f9e6b47a`

## Independent coordinator verification

The candidate is exactly one commit ahead of the accepted Task 2 base and changes only:
- `FullMouthWedgeGeometry.swift`;
- `BOPGeometry.swift`;
- focused wedge/BOP tests;
- Xcode file registration.

Verified:
- QDento default wedge bounds are 70.25 × 30;
- center is 35.125 × 15;
- left/up/right/down polygons exactly match QDento source vertex order;
- polygon values retain neutral wedge identity;
- no anatomical meaning is assigned to wedge directions;
- custom wedge layout is honored coherently;
- BOP geometry exposes horizontal site anchors only;
- BOP site/q mapping delegates to QDentoDisplayAdapter;
- BOP source index/x delegates to ToothGeometry;
- wrong arch/surface queries return nil;
- all four chart surfaces produce 48 strictly increasing anchors;
- lower arch reversal and Q3/Q4 asymmetric site ordering remain correct;
- custom ChartLayout width propagates into BOP x;
- F06 canonical MB maps to q2 for FDI11 and remains distinct from FMBS;
- no BOP y/icon offset, BOP state, FMBS state, color, SwiftUI, or interaction logic leaked into Geometry.

Reported final verification:
- wedge tests: 4 passed;
- BOP tests: 6 passed;
- all Geometry: 25 passed;
- Domain + Parity: 47 passed;
- full unit tests: 73 passed;
- UI launch regression passed;
- app build passed;
- reviewers passed after scoped wedge-identity remediation.

## Supplementary evidence note

The Task 3 controller reported that:
`docs/periodontal-ios/research/runtime/B-full-mouth-findings.md`
was absent from its fetched QDento ref.

Coordinator verification confirms that file exists at:
`research/p01-t6-parity-contract-synthesis`.

Its contents agree with the primary source already used:
- 70.25×30 four-wedge geometry;
- left/up/right/down neutral identity;
- BOP separate from FMBS;
- BOP final pixel placement remains unresolved.

Therefore this was a task-runner ref/path resolution issue, not an implementation blocker.

## Downstream base rule

Plan 03 / Task 4 must branch from exact accepted HEAD:

`7bb307ba161345890293ffc2a36f9dfd37b093c4`

## Verdict

**ACCEPTED**

Plan 03 / Task 4 may begin.
