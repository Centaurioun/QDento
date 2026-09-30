[$superpowers](app://plugins~Plugin_60aea7460bd4819199fd97a9553a5e12)
[@Build iOS Apps](plugin://build-ios-apps@openai-curated-remote)

# TASK GOAL PACKET — PLAN 02 / TASK 5
# PORT DETERMINISTIC PARITY FIXTURES INTO SWIFT

## TASK TYPE

PROMPT for one dedicated Codex controller instance.

This is the final Plan 02 implementation task.

The goal is to port the eight accepted Plan 01 QDento parity fixtures into a strongly typed, deterministic Swift fixture catalog that later geometry, interaction, summary, and screenshot tests can share.

Do NOT implement geometry.
Do NOT implement SwiftUI chart UI.
Do NOT implement editing behavior/store.
Do NOT implement persistence repository.
Do NOT implement summary/risk providers.
Do NOT copy QDento assets.
Do NOT modify QDento or Clinica.

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
   only as architecture guidance; Parity and Domain files must remain SwiftUI-free.
7. Use current XcodeBuildMCP build/test workflow.

## MODEL / SUB-AGENT POLICY

You are the controller.

ALL sub-agents MUST use GPT-6 Luna only.
Never use GPT-6 Sol for a sub-agent.
No nested sub-agents.

Recommended fresh roles:

1. Fixture Implementer — strict TDD and exact source-to-canonical translation.
2. Source-Translation Reviewer — read-only; checks every F01–F08 input against Plan 01 source fixture values and frozen canonical mapping.
3. Reproducibility/Codable Reviewer — read-only.
4. Adversarial Parity Reviewer — read-only; checks BOP/FMBS/FMPS separation, sign mapping, F08 occupancy/denominator facts.
5. Final Code Quality Reviewer — read-only.

Critical/Important findings require a fresh remediation implementer and scoped re-review.

## TARGET REPOSITORY / BASE

Repository:
`Centaurioun/periodontal-ios`

Exact accepted Task 4 HEAD:
`4033d7e0a03d461609d0e8bb5f78f8ec45dc0e14`

Create a NEW isolated branch/worktree from that exact commit:

`parity/p02-t5-fixture-catalog`

Do NOT modify previous accepted branches.
Do NOT merge.
Do NOT open a PR.
Do NOT begin Plan 03.

## AUTHORITATIVE INPUTS

### In the iOS repo

Read:
- `docs/reference/qdento-ios-design-spec.md`
- `docs/contracts/rendering-parity-contract-v1.md`
- all accepted Domain files from Tasks 2–4.

### In QDento, read-only

Authoritative fixture source:

Repository:
`Centaurioun/QDento`

Synthesis branch:
`research/p01-t6-parity-contract-synthesis`

Fixture document:
`docs/periodontal-ios/research/runtime/C-parity-fixtures.md`

Fixture document blob SHA:
`f37098042105a89b269877d14fe5778cb56442bd`

Fixture capture protocol:
`docs/periodontal-ios/research/runtime/fixtures/README.md`

Frozen contract on the same synthesis branch:
`docs/periodontal-ios/contracts/2026-09-30-rendering-parity-contract-v1.md`

Do not edit those QDento documents.

## GOAL

Create a platform-native Swift parity fixture layer with:

- all eight exact fixture IDs;
- deterministic exam states;
- canonical named-site translation;
- optional transition/action metadata for F04/F05;
- typed source/evidence metadata;
- typed expectations needed by later geometry/UI/parity-summary tests;
- deterministic, Codable, Equatable fixture values;
- no reliance on current time, random UUIDs, dictionary iteration, or UI state.

## ALLOWED PRODUCT FILES

Create only:

- `PeriodontalIOS/Parity/ParityFixture.swift`
- `PeriodontalIOS/Parity/ParityFixtureCatalog.swift`

## ALLOWED TEST FILE

Create only:

- `PeriodontalIOSTests/Parity/ParityFixtureTests.swift`

Modify `PeriodontalIOS.xcodeproj/project.pbxproj` only as required to include those files.

Do not modify existing Domain types unless a reviewer proves a blocking defect in the accepted Task 4 API. If that occurs, STOP and report instead of silently changing accepted Domain code.

# REQUIRED PARITY TYPES

The exact spelling may vary slightly for clarity, but the semantics below are required.

## A. ParityFixtureID

Implement a stable typed ID enum containing exactly:

- `P01-T3-F01`
- `P01-T3-F02`
- `P01-T3-F03`
- `P01-T3-F04`
- `P01-T3-F05`
- `P01-T3-F06`
- `P01-T3-F07`
- `P01-T3-F08`

Recommended:

```swift
enum ParityFixtureID: String, CaseIterable, Codable, Sendable
```

CaseIterable order must be F01...F08.

## B. Evidence metadata

Preserve Plan 01's evidence distinction in typed form.

At minimum support:
- SOURCE_VERIFIED
- RUNTIME_VERIFIED
- SCREENSHOT_OBSERVED
- USER_OBSERVED
- UNRESOLVED
- NEW_PRODUCT_CANONICAL_MAPPING
- PARITY_LEGACY

A fixture should be able to state separately that:
- source inputs/arithmetic are source-verified;
- canonical named-site translation is a new-product mapping;
- QDento fixture-specific runtime/screenshot appearance remains unresolved.

Do not upgrade unresolved runtime evidence to verified.

## C. Transition/action metadata

F04 and F05 are not only static states.

Create a small Codable/Equatable action representation sufficient for later interaction tests.

It must support at least:
- direct CAL edit on a ToothID + PeriodontalSite with from/to values;
- attached-gingiva edit on a ToothID + PeriodontalSurface with from/to values.

Do NOT implement the actual mutation behavior here.

The fixture catalog may contain:
- initialExam;
- expectedExam;
- optional action.

For static fixtures, initialExam and expectedExam may be equal.

Equivalent structure is acceptable if it preserves the same deterministic information.

## D. Expectations

Create a typed expectation value sufficient to preserve relevant source-backed fixture results without embedding rendering code.

At minimum it must be able to record, when applicable:
- facial recession expected value;
- oral recession expected value;
- QDento legacy local display-GM six-slot values;
- QDento legacy local GM y six-slot values;
- QDento legacy local CAL y six-slot values;
- legacy BOP fraction numerator/denominator;
- legacy FMBS fraction numerator/denominator;
- legacy FMPS/HI fraction numerator/denominator;
- legacy missing-position count;
- expected natural/missing/implant occupancy at named ToothIDs.

You may use small parity-only helper value types to keep six-slot arrays/fractions structurally safe.

Do NOT put QDento packed-slot concepts into the canonical Domain module.
They are allowed only under `Parity/` as reference/test metadata.

## E. Determinism

Every fixture must use:
- fixed UUID;
- fixed examinedAt Date;
- deterministic position generation/order;
- no `UUID()`;
- no `Date()`;
- no randomized values.

Two calls that construct the same fixture must produce Equal values and stable encoded JSON when using a sorted-key JSONEncoder.

The deterministic positions sequence is fixture/test serialization order only.
Do NOT label it QDento chart display order.

# CRITICAL SOURCE → CANONICAL TRANSLATION

The source fixture document for FDI 11 uses QDento packed six-slot order.

For maxillary Q1 FDI 11, the frozen contract maps source slots:

- source site[0] → DB
- source site[1] → B
- source site[2] → MB
- source site[3] → DL
- source site[4] → L
- source site[5] → ML

This is `NEW_PRODUCT_CANONICAL_MAPPING`.

Therefore when porting any FDI 11 source arrays into canonical SiteMeasurement values, translate by that mapping.

Do NOT simply zip source array order against canonical CaseIterable order `MB,B,DB,ML,L,DL`.

That would be wrong.

## Canonical clinical GM conversion

QDento fixture document reports displayed GM as:

`displayGM = PD - CAL`

The canonical Swift Domain stores:

`clinicalGM = CAL - PD = -displayGM`

Every fixture's SiteMeasurement.gingivalMarginMM must use canonical clinical GM.

Do not store source display-GM in Domain state.

Parity-only expectation metadata may preserve source display-GM values.

## CAL source

The source fixtures explicitly provide PD and CAL arrays.

When porting those assessed CAL values into the canonical Domain, use `MeasurementSource.manual`.

Do not mark them derived merely because the arithmetic relation can be reconstructed.

# LEGACY ZERO-STATE TRANSLATION

The Plan 01 fixtures are QDento parity fixtures.

Except where the source fixture explicitly states otherwise, source defaults are numeric 0 / Boolean false, not canonical “unassessed nil”.

Therefore for the parity catalog:

For source-described natural zero-state positions:
- PD = 0
- clinical GM = 0
- CAL = 0, source = manual
- BOP = false
- FMPS four wedges = false
- FMBS four wedges = false
- applicable AG = 0

Do NOT translate source false/zero defaults into nil.

This is necessary for later QDento legacy denominator/summary parity.

For maxillary oral AG:
- remain aggregate-level not applicable, i.e. NaturalToothRecord.oralSurface = nil.

For mandibular applicable oral AG:
- source zero-state becomes attachedGingivaMM = 0.

## Mobility / furcation reconciliation

QDento fixture text lists legacy mobility/furcation zero values even where canonical anatomy differs.

For the new canonical fixture exam:
- use MobilityGrade.grade0 where the fixture explicitly states mobility 0 on a natural tooth;
- use the accepted canonical furcation defaults from NaturalToothRecord.blank/default factory unless a future fixture explicitly targets furcation.

Do NOT promote QDento legacy non-molar furcation zero slots into clinically applicable canonical furcations.

If useful, parity-only source metadata may retain the legacy source values separately.

# FULL-MOUTH BASELINE REQUIREMENT

F01–F07 assume all 32 tooth positions are natural/enabled in QDento for legacy summary denominators.

Therefore their canonical fixture exams MUST contain all 32 permanent ToothIDs as natural positions.

Use assessed zero/false parity state for all positions, then override the target FDI values for the fixture.

Do not create a one-tooth exam for F01–F07.

This is essential for:
- BOP denominator 192;
- wedge denominator 128;
- later legacy parity-summary testing.

F08 is the exception.

# EXACT FIXTURE TRANSLATIONS

## F01 — Healthy baseline

Target:
FDI 11, but all 32 positions natural.

All natural positions:
- six sites PD=0, clinicalGM=0, CAL=0/manual, BOP=false;
- applicable AG=0;
- mobility=.grade0;
- FMPS/FMBS all false;
- canonical furcation defaults.

Expected for FDI11:
- facial recession 0;
- oral recession 0;
- source displayGM slots [0,0,0,0,0,0];
- source local GM y [105,105,105,105,105,105];
- source local CAL y [105,105,105,105,105,105].

## F02 — Asymmetric contour

FDI11 source arrays:
PD [2,5,8,3,6,4]
CAL [1,3,6,5,2,4]
source displayGM [1,2,2,-2,4,0]

Translate to canonical FDI11:

- DB: PD2, CAL1/manual, clinicalGM -1
- B:  PD5, CAL3/manual, clinicalGM -2
- MB: PD8, CAL6/manual, clinicalGM -2
- DL: PD3, CAL5/manual, clinicalGM +2
- L:  PD6, CAL2/manual, clinicalGM -4
- ML: PD4, CAL4/manual, clinicalGM 0

BOP false on all six target sites.

Expected:
- facial recession 0;
- oral recession 2;
- source local GM y [108,111,111,99,117,105];
- source local CAL y [102,96,87,90,87,93].

All other positions remain assessed-zero natural baseline.

## F03 — Recession/sign case

FDI11 source:
PD [4,4,4,4,4,4]
CAL [6,3,5,4,4,4]
source displayGM [-2,1,-1,0,0,0]

Canonical FDI11:
- DB: PD4, CAL6/manual, clinicalGM +2
- B:  PD4, CAL3/manual, clinicalGM -1
- MB: PD4, CAL5/manual, clinicalGM +1
- DL/L/ML: PD4, CAL4/manual, clinicalGM 0

Expected:
- facial recession 2;
- oral recession 0;
- source local GM y [99,108,102,105,105,105];
- source local CAL y [87,96,90,93,93,93].

## F04 — Direct CAL transition

FDI11.

Initial source:
PD [4,4,4,0,0,0]
CAL [0,1,0,0,0,0]

Action:
direct CAL edit source site[1], which canonical mapping identifies as site B:
CAL 1 → 5.

Initial canonical:
- DB PD4 CAL0/manual clinicalGM -4
- B  PD4 CAL1/manual clinicalGM -3
- MB PD4 CAL0/manual clinicalGM -4
- oral sites zero-state

Expected canonical after action:
- DB PD4 CAL0/manual clinicalGM -4
- B  PD4 CAL5/manual clinicalGM +1
- MB PD4 CAL0/manual clinicalGM -4
- oral sites unchanged

Expected after:
- facial recession 1;
- oral recession 0;
- source displayGM [4,-1,4,0,0,0].

Do NOT perform the edit in behavior code.
Construct deterministic initialExam + expectedExam and action metadata.

## F05 — Attached gingiva versus recession

FDI11.

Initial:
- facial attached gingiva = 7;
- maxillary oral AG not applicable;
- source facial packed slots:
  PD [4,4,4]
  CAL [3,6,4]
- source oral slots zero.

Canonical facial:
- DB PD4 CAL3/manual clinicalGM -1
- B  PD4 CAL6/manual clinicalGM +2
- MB PD4 CAL4/manual clinicalGM 0

Initial expected facial recession = 2.
Oral recession = 0.

Action metadata:
edit facial attached gingiva 7 → 8.

ExpectedExam:
facial attached gingiva = 8;
all site measurements unchanged;
facial recession remains 2;
oral recession remains 0.

## F06 — Six-site BOP case

FDI11 source:
BOP [false,false,true,false,false,false].

Because source site[2] maps to canonical MB:
- canonical MB.bop = true;
- B, DB, ML, L, DL BOP = false.

FMBS all false.
FMPS all false.

Expected legacy BOP fraction:
- numerator 1
- denominator 192

Expected legacy FMBS fraction:
- numerator 0
- denominator 128

All 32 positions remain natural assessed-zero parity baseline except target BOP.

## F07 — FMPS/FMBS wedge case

FDI11:
FMPS source wedges [true,false,false,false]
FMBS source wedges [false,false,false,true]

Canonical neutral wedges:
- FMPS.left = true
- FMPS.up/right/down = false
- FMBS.down = true
- FMBS.left/up/right = false

All six BOP values remain false.

Expected legacy:
- FMBS numerator 1 / denominator 128
- FMPS/HI numerator 127 / denominator 128

Do NOT call that modern plaque-positive percentage.

## F08 — Natural / missing / implant

All 32 permanent positions exist.

Occupancy:
- FDI24 natural
- FDI25 missing
- FDI26 implant
- all other 29 positions natural

Total:
- 30 natural positions
- 1 missing position
- 1 implant position

For 30 natural positions:
use assessed zero/false parity baseline.

For implant FDI26:
- all six implant sites PD=0, mucosalMargin=0, BOP=false;
- implant mobility=false.

Missing FDI25 carries only ToothID.

Expected legacy denominator facts:
- BOP denominator = 180
- wedge denominator = 120
- legacy missing-position count = 2, because QDento's legacy disabled count includes missing + implant non-wisdom positions.

This legacy count is parity metadata only.
Do NOT encode “implant is missing” into canonical occupancy.

# FIXTURE CATALOG API

Implement:

`ParityFixtureCatalog.all`

or a clear equivalent returning all eight fixtures in ID order.

Also provide:

`fixture(id:)`

or equivalent deterministic lookup.

Do not rely on dictionary iteration order for `all`.

# REPRODUCIBILITY / CODABLE REQUIREMENTS

Parity fixture types themselves should be Codable, Equatable, Sendable unless there is a strong Swift limitation.

Required tests:

1. exactly eight fixture IDs;
2. exact F01...F08 order;
3. IDs unique;
4. two fresh catalog builds are Equal;
5. each fixture Codable round-trips Equal;
6. sorted-key JSON encoding of two fresh builds is byte-identical;
7. UUID/Date values are fixed and non-random;
8. each exam has unique ToothIDs;
9. F01–F07 each contain exactly 32 natural positions;
10. F08 contains exactly:
    - 30 natural
    - 1 implant
    - 1 missing.

# SOURCE-TRANSLATION TESTS

Add explicit tests so a future refactor cannot accidentally change source→canonical mapping.

At minimum:

## F02
Assert:
- source slot0 value lands at canonical DB;
- source slot1 at B;
- source slot2 at MB;
- source slot3 at DL;
- source slot4 at L;
- source slot5 at ML.

Assert all six PD/CAL/clinicalGM values listed above.

## F03
Assert both positive and negative canonical clinical GM signs.

## F04
Assert:
- action target is FDI11 / site B;
- initial CAL=1;
- expected CAL=5;
- PD stays 4;
- expected clinicalGM becomes +1.

## F05
Assert:
- maxillary oral AG is aggregate-level not applicable;
- facial AG initial7 expected8;
- recession stays2.

## F06
Assert:
- only canonical MB BOP is true on FDI11;
- FMBS remains all false;
- legacy fraction 1/192.

## F07
Assert:
- only FMPS.left true;
- only FMBS.down true;
- legacy FMPS/HI = 127/128;
- legacy FMBS = 1/128.

## F08
Assert:
- FDI24 natural;
- FDI25 missing;
- FDI26 implant;
- 30/1/1 occupancy;
- denominator facts 180 and 120;
- legacy missing count2.

# EVIDENCE STATUS

For every fixture, preserve at least this truth:

- source inputs/arithmetic: SOURCE_VERIFIED;
- canonical named-site translation: NEW_PRODUCT_CANONICAL_MAPPING where mapping is involved;
- QDento fixture-specific runtime/screenshot appearance: UNRESOLVED.

Do not create fake runtime/screenshot evidence.

# NO RENDERING CALCULATOR IN TASK 5

It is acceptable to store source-backed expected local y values as fixture expectations.

Do NOT implement the contour geometry engine in Task 5.

Plan 03 will consume these expected values to test the independent geometry engine.

Likewise, do NOT implement legacy percentage calculators in Task 5.

Store expected numerator/denominator facts only.

Plan 04 summary provider will later calculate and compare against them.

# BUILD / REGRESSION VERIFICATION

Use the verified environment unless unavailable:

- Xcode 27.0
- iPhone 18 Pro
- iOS 27.0
- deployment target iOS 18.0

Run at minimum:

1. focused ParityFixtureTests;
2. all Domain + Parity unit tests;
3. full unit-test suite;
4. existing UI launch regression;
5. app build.

No “should pass” claims.

# REVIEW FOCUS

Independent reviewers must challenge:

- packed source order accidentally zipped to canonical site CaseIterable order;
- displayGM accidentally stored as canonical clinical GM;
- QDento false/zero defaults accidentally converted to nil;
- F01–F07 accidentally reduced to one-tooth exams;
- F08 implant accidentally represented as missing;
- FMPS and FMBS conflated;
- BOP mapped to wedge state;
- F06 source site[2] mapped to anything other than canonical MB;
- F04/F05 transition state lost;
- current time/random UUIDs leaking into fixtures;
- runtime evidence falsely upgraded from UNRESOLVED;
- geometry/summary calculations implemented prematurely.

# SIX-CYCLE TASK REFINEMENT

After GREEN and before final commit, perform exactly six cumulative passes:

1. Accuracy & Fundamental Correction
2. Completeness & Gap Analysis
3. Structure & Architecture
4. Adversarial / Critical Review
5. Usability & Goal Fit
6. Final Synthesis, Regression Check & Polish

No seventh cycle.

# COMMIT

Commit accepted implementation with:

`test: add deterministic periodontal parity fixtures`

Push:

`parity/p02-t5-fixture-catalog`

Do NOT merge.
Do NOT open a PR.
Do NOT begin Plan 03.

# STOP CONDITIONS

STOP and report rather than improvising if:
- exact Task 4 base cannot be checked out;
- source fixture values conflict materially with the frozen contract;
- accepted Domain types cannot represent the fixture semantics without changing accepted Domain code;
- deterministic Codable requires a broader architecture change;
- full regression build/test cannot pass within this bounded task.

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
- focused parity-fixture test result
- all Domain + Parity test result
- full unit-test result
- UI regression-test result
- build result
- independent review verdict(s)
- source-translation verification summary
- determinism/Codable verification summary
- concerns/blockers
- verification summary
- controller rulings, if any
