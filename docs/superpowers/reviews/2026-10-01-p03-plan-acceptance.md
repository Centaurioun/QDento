# Plan 03 Acceptance and Freeze — Geometry Engine + Static Chart

Date: 2026-10-01

Status: ACCEPTED_AND_FROZEN

Implementation repository:
`Centaurioun/periodontal-ios`

Frozen branch:
`freeze/p03-static-chart-v1`

Frozen HEAD:
`394bf5133cb452729baa37e55e62db473a7d02b6`

Plan 03 started from the accepted Plan 02 baseline and incrementally froze:
1. QDento display/site adapter;
2. pure contour geometry;
3. FMPS/FMBS wedge geometry and BOP horizontal anchors;
4. replaceable tooth visual provider + private-demo prototype artwork;
5. static upper/lower periodontal chart shell;
6. static FMPS/FMBS and BOP findings visuals.

## Acceptance gate

### Geometry
Accepted:
- explicit clinical-GM ↔ QDento-display-GM adapter;
- 1120×166 contour local geometry;
- baseline 105;
- 3 units/mm;
- 48 measurement vertices;
- source tooth/q-slot mapping;
- whole-arch mandibular horizontal reversal;
- custom ChartLayout consistency;
- F01/F02/F03 deterministic geometry oracles;
- distinct BOP six-site anchors;
- distinct neutral four-wedge geometry.

### Tooth visuals
Accepted for PRIVATE DEMO:
- replaceable semantic provider boundary;
- natural/missing/implant separation;
- 16 natural + 2 implant extracted prototype raster bodies;
- QDento-like missing opacity;
- source-backed tooth classes/orientations;
- 1106-high periodontal-strip logical canvas and y=140 body placement;
- explicit GPL/private-demo provenance and public-distribution gate.

### Static chart
Accepted:
- one arch at a time;
- 1120 logical chart width with horizontal scrolling;
- source-like row hierarchy;
- 70-point tooth slots;
- named-site numeric rows tied to accepted visible-site order;
- PD/CAL threshold presentation;
- explicit QDento-sign GM display;
- derived recession;
- maxillary oral AG not-applicable;
- 1120×332 tooth/contour scene;
- source-derived view-level contour y placement;
- F01/F02 static simulator evidence.

### Static findings
Accepted:
- FMPS and FMBS remain independent;
- neutral left/up/right/down wedge identities;
- source selected colors;
- explicit 70.25→70 per-slot mobile adaptation;
- BOP remains independent from FMBS;
- BOP x from accepted six-site geometry;
- native/vector BOP marker; no QDento BOP raster;
- NEW_PRODUCT center-in-row BOP y rule;
- F06/F07 simulator/UI-test sentinel evidence.

## Regression state reported at final Plan 03 HEAD

At Task 6 final verification:
- 79 unit tests passed;
- 3 UI tests passed;
- 82 complete-scheme tests passed;
- app build and simulator launch passed;
- Task 5 chart flow remained green;
- real landscape simulator inspection/capture completed;
- git diff check passed.

## Static-baseline decisions frozen for Plan 04

Plan 04 MUST consume rather than reinterpret:
- canonical sites and QDentoDisplayAdapter mappings;
- clinical GM sign convention;
- direct display conversion;
- contour geometry/transforms;
- 70-point tooth grid;
- 1120×332 central scene;
- tooth provider/asset mapping;
- FMPS/FMBS neutral wedge identities;
- BOP six-site identity and x anchors;
- maxillary oral AG not-applicable;
- recession derived/read-only;
- F01–F08 deterministic fixtures.

Plan 04 may add interaction/state/persistence but must not silently rewrite these frozen interfaces.

## Remaining nonblocking/deferred items

1. Task 6 lacked a captured pre-implementation RED run — process finding only.
2. Four-wedge anatomical interpretation remains intentionally unresolved; left/up/right/down stay neutral.
3. QDento prototype raster use remains PRIVATE DEMO ONLY; public/proprietary distribution still requires separate asset/licensing/replacement review.
4. Exact BOP portable pixel-y was not source-frozen; Plan 03 uses documented NEW_PRODUCT row-centering.
5. 70.25 source wedge width is adapted to the 70-point mobile grid as a documented NEW_PRODUCT rule.
6. Full QDento runtime fixture-by-fixture acceptance remains a Plan 05 parity-hardening responsibility; Plan 03 acceptance does not claim pixel-perfect or final parity certification.

## Freeze rule

All Plan 04 tasks branch from the frozen Plan 03 HEAD:
`394bf5133cb452729baa37e55e62db473a7d02b6`

Do not base Plan 04 on an earlier Task 5/6 branch tip.

## Verdict

**PLAN 03 ACCEPTED_AND_FROZEN**

Plan 04 — Interactive Parity, Persistence, and Summary Shell — may begin.
