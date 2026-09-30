[$superpowers](app://plugins~Plugin_60aea7460bd4819199fd97a9553a5e12)
[@Build iOS Apps](plugin://build-ios-apps@openai-curated-remote)

# TASK GOAL PACKET — PLAN 03 / TASK 1
# QDENTO DISPLAY + SITE-ORIENTATION ADAPTER

## TASK TYPE

PROMPT for one dedicated Codex controller instance.

This is the first Plan 03 geometry-layer task.

It implements ONLY the pure adapter between:
- canonical periodontal domain semantics;
- QDento parity display GM sign;
- QDento q0/q1/q2 triplet order;
- final visible left/middle/right site order.

Do NOT implement contour x/y geometry yet.
Do NOT implement SwiftUI.
Do NOT implement Canvas/Shape.
Do NOT copy assets.
Do NOT implement chart layout.
Do NOT implement BOP/wedge geometry.
Do NOT modify accepted Domain or Parity semantics.

## REQUIRED WORKFLOW

Before implementation:

1. Invoke Superpowers `using-superpowers`.
2. Invoke `using-git-worktrees`.
3. Invoke `subagent-driven-development`.
4. Use `verification-before-completion`.
5. Use Build iOS Apps.
6. Read/use:
   - `swiftui-ui-patterns`
   - `swiftui-view-refactor`
   as architecture guidance only.
7. Use XcodeBuildMCP/current build-test workflow.

## MODEL / SUB-AGENT POLICY

You are the controller.

All sub-agents MUST use GPT-6 Luna only.
Never use GPT-6 Sol for any sub-agent.
No nested sub-agents.

Recommended fresh roles:
1. Adapter Implementer — strict TDD.
2. Geometry/Quadrant Mapping Reviewer — read-only.
3. Contract/Erratum Reviewer — read-only; specifically checks Q3/Q4 v1.1 correction.
4. Final Code Quality Reviewer — read-only.

Any Critical/Important finding requires a fresh remediation implementer and scoped re-review.

## TARGET REPOSITORY / BASE

Repository:
`Centaurioun/periodontal-ios`

Plan 02 frozen code:
`freeze/p02-domain-foundation-v1`
at
`893723f3652bdc42a104a80a4e7f4f3c3eeacfe9`

Plan 03 contract-preparation branch:
`contract/p03-parity-contract-v1-1`

Exact Task 1 base:
`f5b6c83fab98961e65db44b8daff4dbed66012f4`

Create a NEW isolated worktree/branch from that exact commit:

`geometry/p03-t1-qdento-display-adapter`

Do NOT work directly on main, the Plan 02 freeze branch, or the contract-preparation branch.

Do NOT merge.
Do NOT open a PR.
Do NOT start Task 2.

## AUTHORITATIVE INPUTS

Read first:

- `docs/contracts/rendering-parity-contract-v1.1.md`
- `docs/reference/SOURCE-AUTHORITIES.md`
- accepted Domain files:
  - `ToothID.swift`
  - `PeriodontalSurface.swift`
  - `PeriodontalSite.swift`
- accepted Parity fixture types/catalog

IMPORTANT:
For Plan 03 geometry, contract v1.1 supersedes the old v1 visible-order table.

Do NOT use v1's erroneous Q3/Q4 visible-left/middle/right cells.

## V1.1 CORRECTED VISIBLE ANATOMY

Canonical facial site set:
`MB, B, DB`

Canonical oral site set:
`ML, L, DL`

Correct visible left/middle/right:

- Q1 facial: DB / B / MB
- Q1 oral: DL / L / ML
- Q2 facial: MB / B / DB
- Q2 oral: ML / L / DL
- Q3 facial: MB / B / DB
- Q3 oral: ML / L / DL
- Q4 facial: DB / B / MB
- Q4 oral: DL / L / ML

This is the NEW_PRODUCT_CANONICAL_MAPPING.

## SOURCE q0/q1/q2 MAPPING

The q-slot anatomical mappings remain:

Q1:
- facial q0=DB, q1=B, q2=MB
- oral q0=DL, q1=L, q2=ML

Q2:
- facial q0=MB, q1=B, q2=DB
- oral q0=ML, q1=L, q2=DL

Q3:
- facial q0=DB, q1=B, q2=MB
- oral q0=DL, q1=L, q2=ML

Q4:
- facial q0=MB, q1=B, q2=DB
- oral q0=ML, q1=L, q2=DL

QDento final visible q order:
- maxilla: q0, q1, q2 = left, middle, right
- mandible: q2, q1, q0 = left, middle, right

The adapter must preserve BOTH concepts:
- source q-slot identity;
- final visible site order.

Do not collapse them into one ambiguous array.

## DISPLAY GM RULE

Canonical Domain:
`clinicalGM = CAL - PD`

QDento parity display:
`displayGM = PD - CAL = -clinicalGM`

Required pure adapter:

```swift
QDentoDisplayAdapter.qdentoDisplayGM(fromClinicalGM:)
```

Examples:
- clinical +2 → display -2
- clinical -2 → display +2
- 0 → 0

No range clamping or mutation logic in Task 1.

## ALLOWED PRODUCT FILE

Create only:

`PeriodontalIOS/Geometry/QDentoDisplayAdapter.swift`

## ALLOWED TEST FILE

Create only:

`PeriodontalIOSTests/Geometry/QDentoDisplayAdapterTests.swift`

Modify Xcode project only as required to include these files.

Do NOT modify accepted Domain/Parity source.

## REQUIRED GEOMETRY-ADAPTER TYPES

Inside the Geometry adapter file, define a small geometry/reference-only q-slot type, for example:

```swift
enum QDentoTripletSlot: Int, CaseIterable, Sendable {
    case q0 = 0
    case q1 = 1
    case q2 = 2
}
```

Codable is optional unless tests/use make it useful.

This type belongs to Geometry/reference adapter scope, NOT Domain.

Do not add q0/q1/q2 to `PeriodontalSite`.

## REQUIRED ADAPTER API

Implement pure functions equivalent to:

### 1. Display sign bridge

```swift
static func qdentoDisplayGM(fromClinicalGM mm: Int) -> Int
```

### 2. q-slot → canonical site

```swift
static func canonicalSite(
    for slot: QDentoTripletSlot,
    toothID: ToothID,
    surface: PeriodontalSurface
) -> PeriodontalSite
```

### 3. canonical site → q-slot

Use a SAFE optional result for wrong-surface input:

```swift
static func sourceSlot(
    for site: PeriodontalSite,
    toothID: ToothID,
    surface: PeriodontalSurface
) -> QDentoTripletSlot?
```

Examples:
- facial query with `.ml` → nil
- oral query with `.db` → nil

Do not trap.

### 4. q slots in final visible left→right order

```swift
static func visibleSlotsLeftToRight(for toothID: ToothID) -> [QDentoTripletSlot]
```

Required:
- maxillary → [q0, q1, q2]
- mandibular → [q2, q1, q0]

### 5. canonical sites in final visible left→right order

```swift
static func visibleSitesLeftToRight(
    for toothID: ToothID,
    surface: PeriodontalSurface
) -> [PeriodontalSite]
```

This must be derived consistently from q-slot mapping + visible-slot order, not maintained as an unrelated duplicated lookup table unless tests prove the duplication cannot drift.

## REQUIRED TDD — RED FIRST

Write failing tests before implementation.

### A. GM sign bridge

Test:
- +2 → -2
- -2 → +2
- 0 → 0
- representative values from F02/F03 preserve exact inversion.

### B. All quadrant/surface q mappings

Use one representative ToothID per quadrant:
- Q1: 11
- Q2: 21
- Q3: 31 or 36
- Q4: 41 or 46

Test all 8 quadrant/surface combinations exactly.

### C. Corrected visible left→right mapping

Explicitly assert:

Q1:
- facial DB/B/MB
- oral DL/L/ML

Q2:
- facial MB/B/DB
- oral ML/L/DL

Q3:
- facial MB/B/DB
- oral ML/L/DL

Q4:
- facial DB/B/MB
- oral DL/L/ML

The Q3/Q4 assertions are regression tests for the v1.1 erratum.

### D. Visible q-slot order

Assert:
- Q1/Q2 maxillary = q0/q1/q2
- Q3/Q4 mandibular = q2/q1/q0

### E. Bidirectional mapping

For every:
- quadrant representative;
- surface;
- site on that surface;

assert:
`site -> slot -> site`
round-trips exactly.

For wrong-surface site queries, assert nil instead of crash.

### F. Fixture bridge check

Use F02 or F03:
- FDI11 source q0 must map to DB;
- q1 → B;
- q2 → MB;
- parity source display-GM values remain separate from canonical clinical GM values.

Do NOT modify fixture catalog.

## ARCHITECTURE CONSTRAINTS

- Geometry file must NOT import SwiftUI.
- Prefer Foundation only if actually needed; pure Swift is sufficient.
- No CGPoint/CoreGraphics yet unless unavoidable; Task 2 owns contour geometry.
- No baseline=105 or scale=3 implementation in Task 1; Task 2 owns those equations.
- No tooth width/layout.
- No chart x positions.
- No QGraphics/Qt naming except explicit `QDento` reference adapter types.
- No state, Observable, singleton, or mutable global configuration.
- No UI strings.

## ERRATUM SAFETY CHECK

The independent reviewer MUST compare:
1. v1.1 corrected visible-order cells;
2. q0/q1/q2 mapping;
3. maxillary/mandibular visible q direction;

and prove they are mutually consistent.

For Q3, specifically:
- q mapping facial = q0 DB / q1 B / q2 MB;
- mandibular visible slots = q2/q1/q0;
- therefore visible sites = MB/B/DB.

For Q4:
- q mapping facial = q0 MB / q1 B / q2 DB;
- mandibular visible slots = q2/q1/q0;
- therefore visible sites = DB/B/MB.

Equivalent oral proof required.

## BUILD / REGRESSION VERIFICATION

Use verified environment unless unavailable:

- Xcode 27.0
- iPhone 18 Pro
- iOS 27.0
- deployment target iOS 18.0

Run:
1. focused `QDentoDisplayAdapterTests`;
2. all Geometry tests;
3. all Domain + Parity tests;
4. full unit-test suite;
5. existing UI launch regression;
6. app build.

No “should pass” claims.

## SIX-CYCLE TASK REFINEMENT

After GREEN and before commit, perform exactly six cumulative passes:

1. Accuracy & Fundamental Correction
2. Completeness & Gap Analysis
3. Structure & Architecture
4. Adversarial / Critical Review
5. Usability & Goal Fit
6. Final Synthesis, Regression Check & Polish

Adversarially attempt to catch:
- clinical/display GM sign inversion error;
- Q3/Q4 visible-order regression;
- q-slot/site drift;
- wrong-surface site trap;
- q-index leakage into Domain;
- accidental Task 2 contour geometry implementation.

No seventh cycle.

## COMPUTER / DEVICE HUB

Do NOT require `[@Computer]` or `[@Device Hub]` for this task.

This task has no meaningful visual UI acceptance surface.

Simulator/build verification through Build iOS Apps/XcodeBuildMCP is sufficient.

## COMMIT

Commit accepted implementation with:

`feat: add QDento display geometry adapter`

Push:

`geometry/p03-t1-qdento-display-adapter`

Do NOT merge.
Do NOT open a PR.
Do NOT begin Task 2.

## STOP CONDITIONS

STOP and report rather than improvising if:
- exact base cannot be checked out;
- v1.1 contract is missing;
- v1.1 q mappings and visible-order rules still conflict;
- accepted Domain/Parity code would need modification;
- full regression build/test cannot pass.

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
- focused adapter-test result
- all Geometry test result
- Domain + Parity regression result
- full unit-test result
- UI regression-test result
- build result
- Q1/Q2/Q3/Q4 mapping verification summary
- v1.1 erratum verification summary
- independent review verdict(s)
- concerns/blockers
- verification summary
- controller rulings, if any
