[$superpowers](app://plugins~Plugin_60aea7460bd4819199fd97a9553a5e12)
[@Build iOS Apps](plugin://build-ios-apps@openai-curated-remote)

# TASK GOAL PACKET — PLAN 03 / TASK 3
# FOUR-WEDGE + BOP SITE-ANCHOR GEOMETRY

## TASK TYPE

PROMPT for one dedicated Codex controller instance.

This is a bounded PURE GEOMETRY task.

It implements:
1. QDento-faithful four-wedge local polygons for FMPS/FMBS;
2. BOP site horizontal anchors tied to the accepted tooth/site/q-slot geometry.

It does NOT implement SwiftUI rendering, colors, icons, touch interaction, or final BOP pixel-y placement.

## REQUIRED WORKFLOW

Before implementation:

1. Invoke Superpowers `using-superpowers`.
2. Invoke `using-git-worktrees`.
3. Invoke `subagent-driven-development`.
4. Use `verification-before-completion`.
5. Use Build iOS Apps.
6. Read/use `swiftui-ui-patterns` and `swiftui-view-refactor` only as downstream architecture guidance; Geometry remains SwiftUI-free.
7. Use the current XcodeBuildMCP build/test workflow.

## MODEL / SUB-AGENT POLICY

You are the controller.

ALL sub-agents MUST use GPT-6 Luna only.
Never use GPT-6 Sol for a sub-agent.
No nested sub-agents.

Recommended fresh roles:
1. Wedge Geometry Implementer — strict TDD.
2. BOP Anchor Implementer or focused implementer lane if the controller can keep writes serialized.
3. QDento Source Geometry Reviewer — read-only.
4. Site/Anchor Mapping Reviewer — read-only.
5. Final Geometry Quality Reviewer — read-only.

You remain the sole integration writer.
Do not allow parallel agents to write the same files.

Critical/Important findings require a fresh remediation implementer and fresh scoped re-review.

## TARGET REPOSITORY / BASE

Repository:
`Centaurioun/periodontal-ios`

Exact accepted Task 2 HEAD:
`ee3497a9259059beb604f58e4f5a5054f9e6b47a`

Create a NEW isolated worktree/branch:

`geometry/p03-t3-wedge-bop`

Do NOT work on prior accepted branches.
Do NOT merge.
Do NOT open a PR.
Do NOT begin Plan 03 Task 4.

## AUTHORITATIVE INPUTS

Read first in the iOS repo:
- `docs/contracts/rendering-parity-contract-v1.1.md`
- `PeriodontalIOS/Domain/FullMouthWedge.swift`
- `PeriodontalIOS/Geometry/QDentoDisplayAdapter.swift`
- `PeriodontalIOS/Geometry/ChartSurface.swift`
- `PeriodontalIOS/Geometry/ToothGeometry.swift`
- `PeriodontalIOS/Geometry/ContourGeometry.swift`
- accepted F06/F07 parity fixtures/tests.

Read-only QDento source/evidence:
- `src/View/Graphics/PerioGraphicsButton.cpp`
- `src/View/Graphics/PerioGraphicsButton.h`
- `src/View/Widgets/PerioView.cpp` — surface/BOP and full-mouth construction
- `docs/periodontal-ios/research/runtime/B-full-mouth-findings.md`

Do not invent anatomy or pixel placement that the source does not establish.

# PART A — FOUR-WEDGE GEOMETRY

## Frozen QDento source geometry

QDento `PerioGraphicsButton` uses a rectangle:

- width = `70.25`
- height = `30`

Corners:
- topLeft = (0,0)
- bottomLeft = (0,height)
- topRight = (width,0)
- bottomRight = (width,height)

Center:
- (width/2, height/2)

Neutral wedge identity from `index % 4`:

- 0 = left
- 1 = up
- 2 = right
- 3 = down

Triangles:

left:
`topLeft → center → bottomLeft`

up:
`topLeft → center → topRight`

right:
`topRight → center → bottomRight`

down:
`bottomLeft → center → bottomRight`

This is SOURCE_VERIFIED.

Do NOT assign mesial/distal/facial/oral/buccal/lingual/palatal meanings.

## Wedge API

Create:
`PeriodontalIOS/Geometry/FullMouthWedgeGeometry.swift`

Keep it SwiftUI/CoreGraphics-free.

Use a small platform-independent local point type, for example:

```swift
struct LocalGeometryPoint: Equatable, Sendable {
    let x: Double
    let y: Double
}
```

If an equivalent generic geometry point already exists and can be reused without confusing contour semantics, reuse it only if the naming remains clear. Do not rename accepted contour APIs merely for aesthetic cleanup.

Provide a layout, e.g.:

```swift
struct FullMouthWedgeLayout: Equatable, Sendable {
    let width: Double
    let height: Double
    static let qdento = ... // 70.25 × 30
}
```

Provide a polygon value that preserves:
- wedge identity;
- exactly three vertices.

Provide a pure function equivalent to:

```swift
FullMouthWedgeGeometry.polygon(
    for wedge: FullMouthWedge,
    layout: FullMouthWedgeLayout = .qdento
)
```

The custom layout, if supported, must be honored coherently just as Task 2 now does.

Do not add color/selected state here.

## Wedge TDD

RED first.

Required tests:
1. QDento default width = 70.25;
2. default height = 30;
3. center = (35.125,15);
4. each wedge has exactly three vertices;
5. every wedge includes the center;
6. left vertices are exactly TL/center/BL;
7. up exactly TL/center/TR;
8. right exactly TR/center/BR;
9. down exactly BL/center/BR;
10. every point lies within the rectangle;
11. `FullMouthWedge.allCases` order remains left/up/right/down;
12. a custom layout, e.g. 140.5×60, scales/repositions corners and center coherently;
13. no anatomy-specific enum/type/string is introduced by Geometry.

No polygon-closing duplicate point.

# PART B — BOP SITE HORIZONTAL ANCHORS

## Evidence boundary

QDento source verifies:
- BOP is six independent site slots per tooth;
- the same six packed slots are aligned with the periodontal measurement rows;
- canonical site association is supplied by the accepted NEW_PRODUCT_CANONICAL_MAPPING;
- upper/lower q orientation is already frozen in `QDentoDisplayAdapter`;
- horizontal measurement x geometry is already frozen in `ToothGeometry`.

QDento source/evidence does NOT independently freeze a final BOP icon pixel-y or icon-offset constant.

Therefore Task 3 MUST model the source-backed HORIZONTAL SITE ANCHOR only.

Do NOT invent:
- BOP marker y coordinate;
- icon width/height;
- pixel offset from contour;
- drop-tip offset;
- vertical row spacing.

Those belong to the later rendering/static chart layer where actual UI evidence can be inspected.

## BOP API

Create:
`PeriodontalIOS/Geometry/BOPGeometry.swift`

Recommended value:

```swift
struct BOPSiteAnchor: Equatable, Sendable {
    let toothID: ToothID
    let site: PeriodontalSite
    let sourceSlot: QDentoTripletSlot
    let sourceLocalIndex: Int
    let x: Double
}
```

Equivalent is fine.

Provide a safe single-site function equivalent to:

```swift
static func anchor(
    for toothID: ToothID,
    site: PeriodontalSite,
    on surface: ChartSurface,
    layout: ChartLayout = .qdento
) -> BOPSiteAnchor?
```

Rules:
- tooth arch must match chart surface;
- site surface must match chart surface;
- derive q slot using `QDentoDisplayAdapter.sourceSlot`;
- derive local index/x using accepted `ToothGeometry`;
- wrong arch/surface returns nil;
- no trap.

Provide a complete surface function equivalent to:

```swift
static func anchors(
    on surface: ChartSurface,
    layout: ChartLayout = .qdento
) -> [BOPSiteAnchor]
```

It should return exactly 48 anchors in FINAL visible left→right order.

Do not accept or encode BOP bool state in Geometry. State belongs to Domain/view composition.

## BOP TDD

RED first.

Required tests:

### Single-site mapping
For representative teeth Q1–Q4, verify:
- returned q slot matches accepted adapter;
- sourceLocalIndex matches ToothGeometry;
- x equals ToothGeometry visible x.

Wrong surface:
- FDI11 + ML on maxillaryFacial → nil.

Wrong arch:
- FDI11 on mandibularFacial → nil.

### Full surface anchors
For each of the four ChartSurface values:
- exactly 48 anchors;
- x strictly increasing;
- first/last x equal the same measurement centers as contour geometry;
- all ToothID/site/sourceSlot/sourceLocalIndex metadata matches accepted adapter + ToothGeometry.

### Maxillary asymmetry
FDI11 facial:
- DB/B/MB must map q0/q1/q2 and increasing left→right x.

FDI21 facial:
- MB/B/DB must map q0/q1/q2.

### Mandibular asymmetry
FDI31 facial:
- source q0=DB, q1=B, q2=MB;
- visible left→right anchor sites must be MB/B/DB because q2/q1/q0 are visible left→right.

FDI41 facial:
- source q0=MB, q1=B, q2=DB;
- visible sites must be DB/B/MB.

Equivalent oral checks required for at least one lower quadrant.

### Custom ChartLayout
Use width 560:
- BOP anchor x values must use the supplied 560-wide coordinate system;
- first x = 560/96;
- last x = 560 - 560/96;
- lower reflection uses 560, not hard-coded 1120.

### F06 parity bridge
Using F06:
- canonical MB is the only true BOP site on FDI11;
- BOPGeometry anchor for FDI11/MB on maxillaryFacial must correspond to q2/source index for that tooth;
- Geometry does not inspect or conflate FMBS.

This is a mapping regression test, not a rendering test.

# ALLOWED PRODUCT FILES

Create only:
- `PeriodontalIOS/Geometry/FullMouthWedgeGeometry.swift`
- `PeriodontalIOS/Geometry/BOPGeometry.swift`

## ALLOWED TEST FILES

Create only:
- `PeriodontalIOSTests/Geometry/WedgeGeometryTests.swift`
- `PeriodontalIOSTests/Geometry/BOPGeometryTests.swift`

Modify Xcode project only as required to register these files.

Do NOT modify accepted Task 1/2 Geometry, Domain, or Parity files unless a blocking defect is proven. If so, STOP and report.

# IMPORTANT BOUNDARIES

Do NOT implement:
- SwiftUI;
- Path/Shape/Canvas;
- colors;
- selected/unselected appearance;
- hover/disabled visuals;
- touch targets;
- BOP icon assets;
- BOP icon y/offset;
- FMPS/FMBS percentage calculations;
- BOP percentage calculations;
- anatomy names for wedge directions;
- interaction/toggle behavior.

Colors remain known source evidence but belong to the rendering-view task:
- FMPS selected RGB 204/228/247;
- FMBS selected RGB 255/146/148;
- disabled RGB 236/236/236.

Do not implement them here.

# REVIEW FOCUS

Independent reviewers must challenge:

1. Does each wedge exactly use the source triangle vertices?
2. Does custom wedge layout avoid hidden 70.25/30 constants?
3. Did any anatomical label get assigned to left/up/right/down?
4. Does BOP anchor geometry delegate site/q mapping to QDentoDisplayAdapter?
5. Does BOP x delegate to ToothGeometry rather than duplicate whole-arch mapping?
6. Does lower whole-arch reversal remain correct?
7. Does custom ChartLayout flow through BOP x?
8. Was a BOP y/icon offset invented without evidence?
9. Did BOP bool state or FMBS state leak into geometry?
10. Does F06 still identify MB only and remain distinct from FMBS?

# SIX-CYCLE TASK REFINEMENT

After GREEN and before commit, perform exactly six cumulative passes:

1. Accuracy & Fundamental Correction
2. Completeness & Gap Analysis
3. Structure & Architecture
4. Adversarial / Critical Review
5. Usability & Goal Fit
6. Final Synthesis, Regression Check & Polish

No seventh cycle.

# COMPUTER / DEVICE HUB

Do NOT use Computer or Device Hub for acceptance of this task.

There is still no rendered UI surface.

# BUILD / REGRESSION VERIFICATION

Use verified environment:
- Xcode 27.0
- iPhone 18 Pro
- iOS 27.0
- deployment target iOS 18.0

Run:
1. focused WedgeGeometryTests;
2. focused BOPGeometryTests;
3. all Geometry tests;
4. Domain + Parity regression;
5. full unit suite;
6. existing UI launch regression;
7. app build;
8. `git diff --check`.

# COMMIT

Commit accepted implementation with:

`feat: add periodontal wedge and BOP geometry`

Push:
`geometry/p03-t3-wedge-bop`

Do NOT merge.
Do NOT open a PR.
Do NOT begin Task 4.

# STOP CONDITIONS

STOP and report rather than improvising if:
- exact Task 2 base cannot be checked out;
- QDento source contradicts the frozen 70.25×30 wedge geometry;
- BOP final y/icon placement appears necessary to satisfy tests (it is not part of this task);
- accepted adapter/ToothGeometry must be changed to implement anchors;
- full regression cannot pass.

# RETURN FORMAT

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
- wedge geometry verification
- BOP anchor verification
- lower-arch mapping verification
- custom-layout verification
- F06 parity bridge verification
- focused wedge-test result
- focused BOP-test result
- all Geometry test result
- Domain + Parity regression result
- full unit-test result
- UI regression-test result
- build result
- independent review verdict(s)
- concerns/blockers
- verification summary
- controller rulings, if any
