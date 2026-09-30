[$superpowers](app://plugins~Plugin_60aea7460bd4819199fd97a9553a5e12)
[@Build iOS Apps](plugin://build-ios-apps@openai-curated-remote)

# TASK GOAL PACKET — PLAN 02 / TASK 3
# MEASUREMENT SEMANTICS + MOBILITY/FURCATION + FMPS/FMBS DOMAIN

## TASK TYPE

PROMPT for one dedicated Codex controller instance.

This is a bounded DOMAIN implementation task.

Implement value semantics and finding types only.

Do NOT implement:
- natural-tooth aggregate;
- implant aggregate;
- PeriodontalExam;
- SwiftUI chart/editing UI;
- interactive edit actions/store;
- QDento geometry;
- persistence repository;
- summary/risk calculations;
- Clinica Stage/Grade/assessment logic.

Those belong to later tasks.

## REQUIRED WORKFLOW

Before implementation:

1. Invoke Superpowers `using-superpowers`.
2. Invoke `using-git-worktrees`.
3. Invoke `subagent-driven-development` for this bounded task.
4. Use `verification-before-completion` before every completion claim.
5. Use the Build iOS Apps plugin.
6. Read/use:
   - `swiftui-ui-patterns`
   - `swiftui-view-refactor`
   for architectural consistency, but keep Domain code free of SwiftUI.
7. Use XcodeBuildMCP/current plugin build-test workflow for verification.

## MODEL / SUB-AGENT POLICY

You are the controller.

All sub-agents MUST use GPT-6 Luna only.
Never use GPT-6 Sol for a sub-agent.
No nested sub-agents.

Recommended fresh roles:
1. Implementer — strict TDD, minimal domain implementation.
2. Measurement Semantics Reviewer — read-only, checks CAL/GM/null/source semantics.
3. Findings/Furcation Reviewer — read-only, checks mobility/furcation/wedge type safety.
4. Final Domain Quality Reviewer — read-only, checks scope and Codable invariants.

Reviewer agents must not edit.
Critical/Important findings require a fresh remediation implementer and scoped re-review.

## TARGET REPOSITORY / BASE

Repository:
`Centaurioun/periodontal-ios`

Exact accepted Task 2 HEAD:
`e82dca6d0a512be5f53de0b3298732497f2b98c3`

Create a NEW isolated branch/worktree from that exact commit:

`domain/p02-t3-measurement-findings`

Do NOT modify:
- `main`
- `foundation/p02-t1-xcode-baseline`
- `domain/p02-t2-tooth-site`

Do NOT merge.
Do NOT open a PR.
Do NOT begin Task 4.

## AUTHORITATIVE INPUTS

Read first in the iOS repo:

- `docs/reference/qdento-ios-design-spec.md`
- `docs/contracts/rendering-parity-contract-v1.md`
- `docs/reference/SOURCE-AUTHORITIES.md`
- existing Task 2 Domain files

For clinical/domain reference only, inspect the accepted Clinica R2 lineage if needed:
`Centaurioun/clinica-dental`
branch:
`integration/pux2-r2-clinical-modernized`

Relevant Clinica semantics:
- null = not assessed;
- false/zero are observed values;
- clinical GM positive = recession/apical;
- clinical GM negative = coronal;
- CAL = PD + clinical GM;
- CAL source = derived/manual/null;
- natural-tooth mobility = 0/1/2/3/null;
- furcation sites = B/M/D/L;
- furcation value = grade 0/1/2/3, null for applicable-but-unassessed, or not-applicable.

Do NOT copy Clinica application architecture.

The frozen Plan 01 contract remains authoritative for QDento parity-specific differences.

## GOAL

Implement the smallest robust value/domain layer for:

1. canonical site measurement values;
2. CAL source semantics;
3. surface-level attached gingiva storage;
4. mobility grade;
5. furcation site/grade/assessment semantics;
6. neutral four-wedge identity;
7. separate nullable FMPS and FMBS wedge findings.

This task must preserve the distinction between:
- canonical clinical GM;
- QDento display GM;
- site-level BOP;
- four-wedge FMPS;
- four-wedge FMBS;
- not assessed;
- false/zero observed values;
- not applicable furcation state.

## ALLOWED PRODUCT FILES

Create only:

- `PeriodontalIOS/Domain/SiteMeasurement.swift`
- `PeriodontalIOS/Domain/ToothSurfaceMeasurement.swift`
- `PeriodontalIOS/Domain/Mobility.swift`
- `PeriodontalIOS/Domain/Furcation.swift`
- `PeriodontalIOS/Domain/FullMouthWedge.swift`

## ALLOWED TEST FILES

Create only:

- `PeriodontalIOSTests/Domain/MeasurementSemanticsTests.swift`
- `PeriodontalIOSTests/Domain/FindingSemanticsTests.swift`

The Xcode project file may change only as required to include these source/test files.

Do not modify app UI/AppModel.

## REQUIRED MEASUREMENT INTERFACES

### MeasurementSource

Implement:

```swift
enum MeasurementSource: String, Codable, Sendable {
    case derived
    case manual
}
```

Additional value-semantic conformances such as Equatable/Hashable are fine.

### AttachmentLevel

Implement a value type equivalent to:

```swift
struct AttachmentLevel: Codable, Equatable, Sendable {
    var valueMM: Int?
    var source: MeasurementSource?
}
```

Required invariant:

- `valueMM == nil` MUST imply `source == nil`.
- A non-nil CAL may have source `.derived` or `.manual`.
- Do not require a source for malformed legacy input by silently inventing one.
- Prefer a validated initializer/custom Codable if needed so invalid `nil value + non-nil source` cannot survive decode.

Add a test proving invalid encoded state is rejected or normalized according to one explicit documented rule. Prefer rejection.

### SiteMeasurement

Implement:

```swift
struct SiteMeasurement: Codable, Equatable, Sendable {
    var probingDepthMM: Int?
    var gingivalMarginMM: Int?   // canonical CLINICAL sign
    var clinicalAttachmentLevel: AttachmentLevel
    var bop: Bool?
}
```

This task's `gingivalMarginMM` is canonical clinical GM:
- positive = recession/apical;
- negative = coronal.

Do NOT store QDento `displayGM = PD - CAL` here.

Provide a pure derivation helper for derived CAL:

`PD + clinicalGM`

Behavior:
- if PD or clinical GM is nil → derived CAL is nil;
- otherwise return their arithmetic sum.

This helper must NOT mutate the measurement.
Interactive edit transitions belong to Plan 04.

Do NOT add plaque, suppuration, attribution, radiographic data, or Stage/Grade fields in this task.

### Range policy

Do not build UI range-clamping behavior into these raw domain structs.

The frozen contract's parity editing limits and safe direct-GM rule belong to the later Behavior/editing task.

These value types must preserve integer data/null semantics without silently clamping.

Tests may use values inside the accepted demo range, but do not create hidden setters that mutate or clamp input.

## TOOTH SURFACE SUPPLEMENT

Implement:

```swift
struct ToothSurfaceMeasurement: Codable, Equatable, Sendable {
    var attachedGingivaMM: Int?
}
```

Important semantic boundary:

- `attachedGingivaMM == nil` means the value is not assessed/present in THIS applicable surface supplement.
- It does NOT mean `not applicable`.

Do NOT add a `notApplicable` flag inside this struct.

Why:
Task 4 has ToothID/arch context and will model whether a surface supplement exists/is applicable. In particular:
- upper facial/buccal AG applicable;
- upper oral/palatal AG not applicable;
- lower facial/buccal applicable;
- lower oral/lingual applicable.

Do NOT encode upper-palatal applicability in this context-free value type.

### Recession rule

Do NOT add a persisted `recessionMM` property.

Recession remains derived from the three canonical clinical GM values on a surface.

You may add a small PURE helper for recession only if it can do so without inventing missing-site completeness semantics.

If the null/incomplete three-site case would require a new product decision, leave the actual recession helper for the later aggregate/behavior task and test only that this Task 3 type does not persist recession.

Do not guess.

## MOBILITY

Implement:

```swift
enum MobilityGrade: Int, Codable, CaseIterable, Sendable {
    case zero = 0
    case one = 1
    case two = 2
    case three = 3
}
```

Mobility “not assessed” is represented later by `MobilityGrade?`.

Required tests:
- raw/Codable values 0...3 round-trip;
- invalid decoded grade such as 4 fails;
- zero is a valid assessed finding and must not be confused with nil.

Do not create implant mobility here; implant semantics belong to Task 4.

## FURCATION

Preserve Clinica's three-way semantic distinction:

1. assessed grade 0...3;
2. applicable but not assessed;
3. not applicable.

Implement canonical furcation sites exactly:
- B
- M
- D
- L

Recommended types:

```swift
enum FurcationSite: String, Codable, CaseIterable, Sendable {
    case b = "B"
    case m = "M"
    case d = "D"
    case l = "L"
}

enum FurcationGrade: Int, Codable, CaseIterable, Sendable {
    case zero = 0
    case one = 1
    case two = 2
    case three = 3
}

enum FurcationAssessment: Codable, Equatable, Sendable {
    case notAssessed
    case notApplicable
    case grade(FurcationGrade)
}
```

Equivalent representation is allowed ONLY if it preserves all three states without ambiguity.

Do not encode “not assessed” as grade zero.
Do not encode “not applicable” as nil if the chosen representation would make it indistinguishable from unassessed.

Do not decide which furcation sites apply to which tooth morphology yet. Task 4 aggregate/tooth-type context will do that.

## FMPS / FMBS FOUR-WEDGE DOMAIN

Implement neutral QDento-parity wedge identity exactly:

```swift
enum FullMouthWedge: String, Codable, CaseIterable, Sendable {
    case left
    case up
    case right
    case down
}
```

Exact `CaseIterable` order:
`left, up, right, down`

Do NOT assign:
- mesial
- distal
- facial
- oral
- buccal
- lingual
- palatal

to these wedges.

The anatomy remains a later nonblocking product/periodontist decision.

### WedgeFindings

Implement a value type with explicit nullable Boolean fields:

```swift
struct WedgeFindings: Codable, Equatable, Sendable {
    var left: Bool?
    var up: Bool?
    var right: Bool?
    var down: Bool?
}
```

Semantics:
- nil = not assessed;
- false = assessed negative/unselected;
- true = assessed positive/selected.

Do NOT collapse nil and false.

A small subscript keyed by `FullMouthWedge` is allowed if useful and fully tested, but explicit fields remain the storage contract.

### FullMouthScoreFindings

Implement:

```swift
struct FullMouthScoreFindings: Codable, Equatable, Sendable {
    var fmps: WedgeFindings
    var fmbs: WedgeFindings
}
```

FMPS and FMBS MUST remain independent values.

Do NOT add legacy QDento percentage calculations here.
Those belong to the later parity-summary provider.

## BOP BOUNDARY

BOP remains a property of `SiteMeasurement`.

Do NOT create a four-wedge BOP representation.

Do NOT conflate BOP and FMBS.

Required test:
- `bop == nil`, `false`, and `true` round-trip distinctly.

## TDD — REQUIRED RED/GREEN ORDER

### Phase A — Measurement semantics

Write failing tests first for:

1. positive clinical GM:
   PD 4 + GM +2 → derived CAL 6;
2. negative clinical GM:
   PD 4 + GM -2 → derived CAL 2;
3. missing PD → derived CAL nil;
4. missing GM → derived CAL nil;
5. AttachmentLevel nil value + nil source accepted;
6. derived CAL source preserved;
7. manual CAL source preserved;
8. invalid nil-value/non-nil-source encoded state rejected;
9. SiteMeasurement BOP nil/false/true remain distinct through Codable;
10. clinical GM remains canonical sign; no QDento display-GM property exists in this type.

Run and record RED before implementation.

### Phase B — Surface supplement + findings

Write failing tests before implementation for:

1. attached gingiva nil and measured values round-trip;
2. no persisted recession property/field is emitted;
3. mobility grades 0...3 round-trip;
4. invalid mobility grade 4 decode fails;
5. furcation sites are exactly B/M/D/L;
6. furcation grades are exactly 0...3;
7. furcation assessment distinguishes:
   - notAssessed
   - notApplicable
   - grade zero;
8. FullMouthWedge exact order left/up/right/down;
9. WedgeFindings keeps nil/false/true distinct per wedge;
10. FMPS state does not alter FMBS state;
11. FMBS state does not alter FMPS state;
12. Codable round-trip preserves all of the above.

Run and record RED, then implement minimum code.

## IMPORTANT SCOPE DISTINCTION — EDIT TRANSITIONS

The frozen Plan 01 contract already defines future interactive edit behavior:

- direct PD edit keeps CAL and recomputes GM;
- direct CAL edit keeps PD and recomputes GM;
- direct QDento-sign GM edit uses the safe new-product rule.

DO NOT implement those mutation transitions in Task 3.

Task 3 only provides the value types and pure CAL derivation primitive required by the later Behavior layer.

Do not add `setPD`, `setCAL`, `setGM`, editor/store objects, or Observable state.

## BUILD / REGRESSION VERIFICATION

Use the verified environment unless unavailable:

- Xcode 27.0
- iPhone 18 Pro
- iOS 27.0
- deployment target remains iOS 18.0

Run at minimum:

1. focused Task 3 domain tests;
2. all Domain tests including Task 2;
3. full unit test suite;
4. existing UI launch regression test;
5. app build.

No “should pass” claims.

## REVIEW FOCUS

Independent reviewers must explicitly challenge:

### Measurement semantics
- Did canonical clinical GM accidentally become QDento display GM?
- Can invalid AttachmentLevel nil/source combination survive construction/decode?
- Is manual versus derived CAL preserved?
- Did mutation/edit logic leak into Task 3?

### Findings
- Is mobility zero distinct from unassessed nil?
- Can furcation distinguish notAssessed / notApplicable / grade zero?
- Are wedge IDs still neutral?
- Can nil/false/true wedge states survive Codable?
- Are FMPS and FMBS independent?
- Was BOP accidentally modeled as four wedges?

### Architecture
- Are Domain files SwiftUI-free?
- Did Task 3 add Task 4 aggregate/tooth morphology logic prematurely?
- Did any packed QDento index enter canonical domain?

## SIX-CYCLE TASK REFINEMENT

After GREEN and before final commit, perform exactly six cumulative passes:

1. Accuracy & Fundamental Correction
2. Completeness & Gap Analysis
3. Structure & Architecture
4. Adversarial / Critical Review
5. Usability & Goal Fit
6. Final Synthesis, Regression Check & Polish

No seventh cycle.

Adversarially test malformed Codable states, especially:
- invalid mobility raw value;
- invalid furcation raw/assessment state;
- invalid wedge raw value;
- AttachmentLevel(value=nil, source=manual/derived).

Do not add features merely to make the six cycles look productive.

## ALLOWED XCODE PROJECT CHANGE

Modify `PeriodontalIOS.xcodeproj/project.pbxproj` only as required to add the new Task 3 source/test files to the existing targets.

No unrelated project-setting changes.

## COMMIT

Commit accepted implementation with:

`feat: add periodontal measurement and finding domain`

Push:

`domain/p02-t3-measurement-findings`

Do NOT merge.
Do NOT open a PR.
Do NOT begin Task 4.

## STOP CONDITIONS

STOP and report rather than improvising if:
- exact Task 2 base cannot be checked out;
- frozen contract and Clinica reference create a material semantic conflict not resolved above;
- preserving AttachmentLevel invariants requires a broader architectural decision;
- Xcode project changes require unrelated churn;
- full regression build/test cannot pass within the bounded task.

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
- focused Task 3 test result
- all Domain test result
- full unit-test result
- UI regression-test result
- build result
- independent review verdict(s)
- concerns/blockers
- verification summary
- controller rulings, if any
