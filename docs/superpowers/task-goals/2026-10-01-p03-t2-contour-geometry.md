[$superpowers](app://plugins~Plugin_60aea7460bd4819199fd97a9553a5e12)
[@Build iOS Apps](plugin://build-ios-apps@openai-curated-remote)

# TASK GOAL PACKET — PLAN 03 / TASK 2
# PURE QDENTO CONTOUR GEOMETRY

## TASK TYPE

PROMPT for one dedicated Codex controller instance.

This task implements the pure, UI-independent contour coordinate engine consumed later by the static SwiftUI chart.

It owns:
- four chart-surface identities;
- 16-tooth / 48-measurement-point chart mapping;
- source-local and final-visible x coordinates;
- QDento baseline/vertical equations;
- pure complete-input contour path construction;
- F02/F03 geometry regression against frozen parity oracles.

It does NOT own SwiftUI rendering.

## REQUIRED WORKFLOW

Before implementation:

1. Invoke Superpowers `using-superpowers`.
2. Invoke `using-git-worktrees`.
3. Invoke `subagent-driven-development`.
4. Use `verification-before-completion`.
5. Use Build iOS Apps.
6. Read/use `swiftui-ui-patterns` and `swiftui-view-refactor` only as downstream architecture guidance; Geometry remains SwiftUI-free.
7. Use current XcodeBuildMCP build/test workflow.

## MODEL / SUB-AGENT POLICY

You are the controller.

ALL sub-agents MUST use GPT-6 Luna only.
Never use GPT-6 Sol for a sub-agent.
No nested sub-agents.

Recommended fresh roles:
1. Contour Geometry Implementer — strict TDD.
2. Source-Equation/48-Point Reviewer — read-only.
3. Arch/Transform Reviewer — read-only.
4. Fixture Oracle Reviewer — read-only; explicitly checks corrected F02 CAL-y.
5. Final Geometry Quality Reviewer — read-only.

Critical/Important findings require fresh remediation + fresh scoped re-review.

## TARGET REPOSITORY / BASE

Repository:
`Centaurioun/periodontal-ios`

Exact accepted downstream base:
`b90cbe9d18bfb376cbc0441932364024e078b7eb`

Create NEW isolated worktree/branch:

`geometry/p03-t2-contour-engine`

Do NOT work on prior branches.
Do NOT merge.
Do NOT open a PR.
Do NOT begin Plan 03 Task 3.

## AUTHORITATIVE INPUTS

Read first in the iOS repo:
- `docs/contracts/rendering-parity-contract-v1.1.md`
- `PeriodontalIOS/Geometry/QDentoDisplayAdapter.swift`
- accepted Domain types;
- accepted corrected Parity fixture catalog/tests.

Read-only QDento evidence if needed:
- `src/View/Graphics/PerioChartItem.cpp`
- `docs/periodontal-ios/research/runtime/A-contour-site-orientation.md` from QDento synthesis evidence.

Do not reinterpret the already-frozen clinical site mapping.

## FROZEN SOURCE GEOMETRY

QDento source establishes:

- logical chart width = `1120`
- logical chart height = `166`
- baseline y = `105`
- vertical coefficient = `3`
- measurement point count per chart surface = `48`
- 16 tooth groups per arch
- 3 points per tooth group
- even spacing

For source-local point index `j = 0...47`:

`x_local = (j + 0.5) × 1120 / 48`

Exact spacing:

`1120 / 48 = 23.333333333333...`

Representative:
- j0 = 11.666666...
- j47 = 1108.333333...

QDento source equations:

`displayGM = -clinicalGM`

`GM_y = 105 + 3 × displayGM`

equivalently:

`GM_y = 105 - 3 × clinicalGM`

and:

`CAL_y = 105 - 3 × CAL`

Do not clamp coordinates.

## SOURCE CHART TOOTH GROUP ORDER

Source-local 16-tooth group order:

Maxilla:
`18,17,16,15,14,13,12,11,21,22,23,24,25,26,27,28`

Mandible:
`38,37,36,35,34,33,32,31,41,42,43,44,45,46,47,48`

Each source group consumes local indexes:
`group×3 + q0/q1/q2`.

## FINAL VISIBLE HORIZONTAL TRANSFORM

Maxillary facial/oral:
- no net horizontal reversal;
- `x_visible = x_local`.

Mandibular facial/oral:
- net horizontal reversal;
- `x_visible = 1120 - x_local`.

Therefore source local indices are visually encountered:
- maxilla: `0...47`;
- mandible: `47...0`.

Final visible tooth order must consequently be:

Maxilla left→right:
`18,17,16,15,14,13,12,11,21,22,23,24,25,26,27,28`

Mandible left→right:
`48,47,46,45,44,43,42,41,31,32,33,34,35,36,37,38`

This is chart geometry/presentation order, not canonical ToothID identity order.

Do NOT put this ordering into ToothID/Domain.

## CHART SURFACE TYPE

Create `PeriodontalIOS/Geometry/ChartSurface.swift`.

Implement exactly four geometry surfaces, names may be:

- maxillaryFacial
- maxillaryOral
- mandibularFacial
- mandibularOral

or semantically equivalent.

Required helpers:
- `arch: DentalArch`
- `periodontalSurface: PeriodontalSurface`
- `isHorizontallyReversed: Bool`

Do NOT reproduce QDento's misleading mandibular `mandLing`/`mandBuccal` enum-name anomaly in public API.

Use semantic names.

## TOOTH GEOMETRY

Create `PeriodontalIOS/Geometry/ToothGeometry.swift`.

Provide pure helpers equivalent to:

- source group index 0...15 for a ToothID on a compatible ChartSurface;
- source local point index 0...47 for ToothID + QDentoTripletSlot;
- source-local x for point index;
- final-visible x after surface transform;
- final visible tooth order for a ChartSurface.

Arch mismatch must be safe:
- maxillary tooth requested on mandibular chart → nil/error, not trap;
- mandibular tooth requested on maxillary chart → nil/error.

Do not depend on array positions stored in Domain.

Derive from ToothID/explicit geometry lookup.

## GEOMETRY COORDINATE TYPES

Create `PeriodontalIOS/Geometry/ContourGeometry.swift`.

Do NOT import SwiftUI.

Prefer a small platform-independent point type rather than exposing `CGPoint` unless a compelling Build iOS Apps reason exists.

Recommended:

```swift
struct ContourPoint: Equatable, Sendable {
    let x: Double
    let y: Double
}
```

For path metadata, use a typed point/vertex representation that can preserve:
- ToothID
- PeriodontalSite
- QDentoTripletSlot or sourceLocalIndex
- final x/y

without leaking those reference fields into Domain.

## COMPLETE-INPUT BOUNDARY

Do NOT invent visual semantics for canonical `nil = not assessed` in this task.

Contour geometry should accept a complete assessed geometry input.

Recommended small Geometry-only input:

```swift
struct ContourSample: Equatable, Sendable {
    let toothID: ToothID
    let site: PeriodontalSite
    let clinicalGM: Int
    let cal: Int
}
```

or an equivalent typed structure.

The path constructor must validate complete input for the requested chart surface:
- exactly 16 matching-arch teeth;
- exactly 3 matching-surface sites per tooth;
- no duplicate ToothID+site;
- no wrong-surface site;
- no missing required site.

On malformed/incomplete input:
- return a typed error/throw;
- do NOT treat missing values as zero;
- do NOT silently skip points;
- do NOT process-crash.

This is a geometry API rule, not a clinical data policy.

Later chart/adaptation code will decide how unassessed domain values are presented.

## CONTOUR PATHS

Represent the two paths separately:

- GM contour
- CAL contour

A complete surface must produce exactly 48 measurement vertices for each.

Prefer a result equivalent to:

```swift
struct ContourPath: Equatable, Sendable {
    let measurementPoints: [ContourVertex] // exactly 48, final visible L→R
    let baselineStart: ContourPoint        // (0,105)
    let baselineEnd: ContourPoint          // (1120,105)
}
```

and a pair/result holding GM + CAL.

The 48 measurement points should be exposed in final visible left→right order, so x is monotonically increasing.

Preserve source local index / slot as metadata on vertices so later parity debugging can trace back to QDento.

Do NOT include a repeated closing point. SwiftUI rendering later can close/outline as needed.

## Y EQUATIONS

Implement pure functions or equivalent tested logic:

```text
gmY(clinicalGM) = 105 - 3×clinicalGM
calY(cal)        = 105 - 3×cal
```

Use Double for coordinates.

Do not introduce color/stroke concepts.

## ALLOWED PRODUCT FILES

Create only:
- `PeriodontalIOS/Geometry/ChartSurface.swift`
- `PeriodontalIOS/Geometry/ToothGeometry.swift`
- `PeriodontalIOS/Geometry/ContourGeometry.swift`

## ALLOWED TEST FILE

Create only:
- `PeriodontalIOSTests/Geometry/ContourGeometryTests.swift`

Modify Xcode project only to register these files.

Do NOT modify QDentoDisplayAdapter unless a blocking defect is proven. If so, STOP and report rather than silently expanding scope.

Do NOT modify Domain/Parity files.

## REQUIRED TDD — PHASE A: CONSTANTS / X GEOMETRY

Write RED tests first.

At minimum assert:

1. width = 1120;
2. height = 166;
3. baseline = 105;
4. scale = 3;
5. 48 points;
6. spacing = 1120/48;
7. local x point0 = 1120/96;
8. local x point47 = 1120 - 1120/96;
9. all 48 local x values are evenly spaced;
10. source point index rejects <0/>47 if a public index API exists rather than trapping.

## REQUIRED TDD — PHASE B: TOOTH / SLOT MAPPING

Test exact source group order.

Representative source groups:
- maxillary FDI18 group0;
- FDI11 group7;
- FDI21 group8;
- FDI28 group15;
- mandibular FDI38 group0;
- FDI31 group7;
- FDI41 group8;
- FDI48 group15.

For each representative:
- q0 local index = group×3;
- q1 = group×3+1;
- q2 = group×3+2.

Arch mismatch returns nil/error.

Test final visible tooth order exactly:
- maxillary 18...11,21...28;
- mandibular 48...41,31...38.

## REQUIRED TDD — PHASE C: TRANSFORMED X

Maxilla:
`x_visible == x_local`.

Mandible:
`x_visible == 1120 - x_local`.

Explicitly test one lower tooth asymmetric triplet, e.g. FDI31:
- q2 must be visually left of q1;
- q1 left of q0.

Also assert all final path x coordinates are strictly increasing left→right after path ordering.

## REQUIRED TDD — PHASE D: Y EQUATIONS

Test:
- clinicalGM 0 → GM y105;
- +2 → y99;
- -2 → y111;
- CAL0 → y105;
- CAL6 → y87;
- CAL2 → y99.

The CAL2→99 test is mandatory regression protection for the corrected F02 oracle.

## REQUIRED TDD — PHASE E: F01/F02/F03 FIXTURE ORACLES

Build complete geometry samples from the accepted fixture exam values; do not hardcode a separate contradictory domain state.

### F01 FDI11
Verify target tooth's relevant GM/CAL vertices are y105.

### F02 FDI11

Source expectation metadata must now be:

display GM:
`[1,2,2,-2,4,0]`

GM y:
`[108,111,111,99,117,105]`

CAL y:
`[102,96,87,90,99,93]`

The geometry engine consumes canonical clinical GM/CAL but must reproduce those QDento-local y expectations when traced by source q slot.

Explicitly verify all six.

### F03 FDI11

GM y:
`[99,108,102,105,105,105]`

CAL y:
`[87,96,90,93,93,93]`

Explicitly verify all six.

## REQUIRED TDD — PHASE F: ALL 48 VERTICES

Using F01 full-mouth baseline:

For each of the four ChartSurface values:
- produce exactly 48 GM vertices;
- exactly 48 CAL vertices;
- all y =105;
- first visible x = 1120/96;
- last visible x = 1120 - 1120/96;
- x strictly increases;
- each visible vertex maps to the correct ToothID/site according to QDentoDisplayAdapter + surface transform.

Explicit first/last semantic checks:

### Maxillary facial
leftmost:
- FDI18
- visible site according to adapter = DB

rightmost:
- FDI28
- visible site = DB

Why both can be DB is quadrant-dependent source mapping; do not assume all leftmost/rightmost triplet labels are symmetric by name.

The reviewer must derive the exact expected first/last sites from the adapter rather than trusting this prose if any conflict appears.

### Mandibular facial
derive and assert from:
- source group order;
- horizontal reversal;
- v1.1 adapter.

The tests should make a whole-arch mirror bug impossible to miss.

## MALFORMED INPUT TESTS

At minimum:
- duplicate ToothID+site → error;
- missing one required site → error;
- wrong arch tooth → error;
- wrong surface site → error;
- extra unexpected sample → error.

No preconditions/fatalError for caller data.

## ARCHITECTURE CONSTRAINTS

- No SwiftUI import in Geometry.
- No Canvas/Shape/Path.
- No QGraphics naming.
- No color/stroke.
- No pixel/device scale; these are logical QDento units.
- Do not use actual tooth raster widths (36/54) to alter contour spacing.
- Do not use 70-unit tooth scene slots to change contour spacing.
- Do not implement alternative `even=false` QDento path.
- Do not persist geometry.
- Do not mutate PeriodontalExam.
- Do not implement nil-display policy.
- Do not implement BOP/wedge geometry; Task 3 owns those.
- Do not copy assets; Task 4 owns visual provider/assets.

## REVIEW FOCUS

Independent reviewers must challenge:

1. Is 48-point spacing actually even and independent of tooth artwork width?
2. Did lower horizontal transform reverse the WHOLE arch, not merely q order?
3. Is final mandibular tooth order 48...41,31...38?
4. Are q-slot/site mappings delegated to the accepted adapter rather than re-invented inconsistently?
5. Are clinical GM signs converted exactly once?
6. Does CAL=2 yield y99?
7. Does F02 reproduce corrected CAL-y [102,96,87,90,99,93]?
8. Can incomplete/malformed samples accidentally become baseline-zero points?
9. Are output vertices final visible left→right with strictly increasing x?
10. Did SwiftUI/CoreGraphics rendering concerns leak into pure geometry?

## SIX-CYCLE TASK REFINEMENT

After GREEN and before commit, perform exactly six cumulative passes:

1. Accuracy & Fundamental Correction
2. Completeness & Gap Analysis
3. Structure & Architecture
4. Adversarial / Critical Review
5. Usability & Goal Fit
6. Final Synthesis, Regression Check & Polish

No seventh cycle.

## COMPUTER / DEVICE HUB

Do NOT use Computer or Device Hub for acceptance of this task.

There is no rendered visual surface yet.

XcodeBuildMCP/unit tests are the correct evidence.

## BUILD / REGRESSION VERIFICATION

Verified environment:
- Xcode 27.0
- iPhone 18 Pro
- iOS 27.0
- deployment target iOS 18.0

Run:
1. focused `ContourGeometryTests`;
2. all Geometry tests including QDentoDisplayAdapter;
3. Domain + Parity tests;
4. full unit suite;
5. existing UI launch regression;
6. app build;
7. `git diff --check`.

## COMMIT

Commit accepted implementation with:

`feat: add pure periodontal contour geometry`

Push:
`geometry/p03-t2-contour-engine`

Do NOT merge.
Do NOT open a PR.
Do NOT begin Task 3.

## STOP CONDITIONS

STOP and report rather than improvising if:
- exact base cannot be checked out;
- corrected F02 oracle is missing;
- contract v1.1 or Task 1 adapter contradicts the source geometry;
- 48-point mapping cannot be represented without changing accepted Domain/Parity code;
- nil/unassessed rendering requires a product decision;
- full regression cannot pass.

## RETURN FORMAT

Return only:
- STATUS: DONE / DONE_WITH_CONCERNS / BLOCKED
- repository
- local worktree path
- feature branch
- base commit
- final HEAD
- files changed
- Build iOS Apps skills used
- sub-agent roles/models used
- TDD RED evidence
- constants/x-spacing verification
- tooth/source-index mapping verification
- lower-arch transform verification
- F01/F02/F03 oracle verification
- focused contour-test result
- all Geometry test result
- Domain + Parity regression result
- full unit-test result
- UI regression-test result
- build result
- independent review verdict(s)
- concerns/blockers
- verification summary
- controller rulings, if any
