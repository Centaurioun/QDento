[$superpowers](app://plugins~Plugin_60aea7460bd4819199fd97a9553a5e12)
[@Build iOS Apps](plugin://build-ios-apps@openai-curated-remote)

# TASK GOAL PACKET — PLAN 02 / TASK 4
# NATURAL TOOTH + IMPLANT + CHART POSITION + EXAM AGGREGATE

## TASK TYPE

PROMPT for one dedicated Codex controller instance.

This is a bounded DOMAIN AGGREGATE implementation task.

It combines the already accepted identity, site-measurement, surface, mobility, furcation, and FMPS/FMBS value types into stable natural-tooth, implant, chart-position, and dated-exam aggregates.

Do NOT implement SwiftUI.
Do NOT implement editing actions/store.
Do NOT implement geometry.
Do NOT implement persistence repository/files.
Do NOT implement summary/risk metrics.
Do NOT implement Stage/Grade.
Do NOT implement patient/backend/authentication.

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
   only as architecture guidance; all Task 4 product code remains UI-independent Domain code.
7. Use the current XcodeBuildMCP/build-test workflow.

## MODEL / SUB-AGENT POLICY

You are the controller.

All sub-agents MUST use GPT-6 Luna only.
Never use GPT-6 Sol for a sub-agent.
No nested sub-agents.

Recommended fresh roles:

1. Aggregate Implementer — strict TDD, minimal implementation.
2. Natural/Implant Separation Reviewer — read-only.
3. Codable/Invariants Reviewer — read-only.
4. Clinical Domain Boundary Reviewer — read-only; checks AG/furcation/null applicability rules and scope.
5. Final Code Quality Reviewer — read-only.

Reviewer agents must not edit.
Critical/Important findings require a fresh remediation implementer and a fresh scoped re-review.

## TARGET REPOSITORY / BASE

Repository:
`Centaurioun/periodontal-ios`

Exact accepted Task 3 HEAD:
`90a82e87bb43c23298e01380539ee31841d0578e`

Create a NEW isolated branch/worktree from that exact commit:

`domain/p02-t4-records-exam`

Do NOT work directly on any previous branch.
Do NOT merge.
Do NOT open a PR.
Do NOT begin Task 5.

## AUTHORITATIVE INPUTS

Read first in the iOS repo:

- `docs/reference/qdento-ios-design-spec.md`
- `docs/contracts/rendering-parity-contract-v1.md`
- `docs/reference/SOURCE-AUTHORITIES.md`
- all accepted `PeriodontalIOS/Domain/` files from Tasks 2–3

Clinica domain reference when needed:

Repository:
`Centaurioun/clinica-dental`

Reference lineage:
`integration/pux2-r2-clinical-modernized`

Relevant files:
- `lib/periodontal/types.ts`
- `lib/periodontal/defaults.ts`

Clinica is semantic/domain reference only.
Do NOT copy its React/store architecture.

## GOAL

Implement:

1. fixed six-site measurement containers so a tooth/implant record cannot silently omit one canonical site;
2. NaturalToothRecord;
3. ImplantSiteMeasurement + ImplantRecord;
4. contextual attached-gingiva surface applicability;
5. explicit furcation defaults without permanently forbidding anatomical variants;
6. ChartPositionRecord separating natural / implant / missing;
7. a minimal dated PeriodontalExam aggregate with duplicate-position protection;
8. aggregate Codable round-trip proving nil/false/zero/notApplicable separation survives serialization.

## ALLOWED PRODUCT FILES

Create only:

- `PeriodontalIOS/Domain/NaturalToothRecord.swift`
- `PeriodontalIOS/Domain/ImplantRecord.swift`
- `PeriodontalIOS/Domain/PeriodontalExam.swift`

## ALLOWED TEST FILES

Create only:

- `PeriodontalIOSTests/Domain/RecordSeparationTests.swift`
- `PeriodontalIOSTests/Domain/PeriodontalExamRoundTripTests.swift`

Modify `PeriodontalIOS.xcodeproj/project.pbxproj` only as required to include these files.

Do not modify app UI/AppModel.

# REQUIRED DOMAIN DESIGN

## A. Fixed six-site natural measurement container

Do NOT store canonical natural-tooth sites as an unconstrained dictionary that can silently omit MB/B/DB/ML/L/DL.

Implement a fixed value-semantic container in `NaturalToothRecord.swift`, for example:

```swift
struct PeriodontalSiteMeasurements: Codable, Equatable, Sendable {
    var mb: SiteMeasurement
    var b: SiteMeasurement
    var db: SiteMeasurement
    var ml: SiteMeasurement
    var l: SiteMeasurement
    var dl: SiteMeasurement
}
```

Exact names may vary slightly if the result is clearer, but all six canonical sites MUST be structurally present.

Provide a tested subscript/accessor keyed by `PeriodontalSite` so later behavior code can read/replace a site without QDento packed indexes.

If Codable keys are customized, prefer the stable clinical abbreviations:
- MB
- B
- DB
- ML
- L
- DL

Do not encode chart order or q0/q1/q2 here.

## B. Surface measurements / attached gingiva

NaturalToothRecord must model attached-gingiva applicability contextually using ToothID/arch.

Recommended aggregate representation:

```swift
var facialSurface: ToothSurfaceMeasurement
var oralSurface: ToothSurfaceMeasurement?
```

Meaning:
- facialSurface is applicable for all first-demo permanent natural teeth;
- oralSurface == nil means NOT APPLICABLE at the aggregate level;
- oralSurface != nil with `attachedGingivaMM == nil` means APPLICABLE BUT NOT ASSESSED.

Required frozen applicability:
- maxillary facial/buccal: applicable;
- maxillary oral/palatal: NOT APPLICABLE;
- mandibular facial/buccal: applicable;
- mandibular oral/lingual: applicable.

This distinction MUST survive direct construction and Codable decoding.

Therefore:
- a maxillary NaturalToothRecord carrying a meaningful/present oralSurface is invalid;
- a mandibular NaturalToothRecord missing oralSurface is invalid.

Use a validated initializer/custom decoder or an equivalent robust invariant.

Do not persist a recession property.

## C. Null-aware recession derived at aggregate level

Task 4 now has all three site measurements for a surface and may define the recession derivation.

Use the frozen clinical formula:

`recession = max(0, max(canonical clinical GM of the three surface sites))`

NEW PRODUCT NULL-AWARE RULE:

Return `nil` unless ALL THREE gingival-margin values on that surface are assessed.

Reason:
the domain distinguishes unassessed from zero; returning a numeric surface maximum from an incomplete three-site set could falsely imply a complete surface assessment.

When all three are present:
- facial uses MB/B/DB;
- oral uses ML/L/DL;
- return max(0, maximum clinical GM).

For maxillary oral/palatal AG applicability, note:
AG is not applicable there, but recession from ML/L/DL periodontal site measurements is still a valid periodontal derived concept. Do NOT conflate AG applicability with whether oral periodontal sites exist.

Implement recession as a PURE computed/helper value, never persisted.

Test the incomplete-site nil behavior explicitly.

## D. Furcation container and defaults

Implement a fixed four-site furcation container in `NaturalToothRecord.swift`, for example:

```swift
struct FurcationFindings: Codable, Equatable, Sendable {
    var b: FurcationAssessment
    var m: FurcationAssessment
    var d: FurcationAssessment
    var l: FurcationAssessment
}
```

Provide a tested accessor keyed by `FurcationSite`.

Provide a default factory based on ToothID using the accepted Clinica reference defaults:

Maxillary molars:
- 16,17,18,26,27,28
- B = notAssessed
- M = notAssessed
- D = notAssessed
- L = notApplicable

Mandibular molars:
- 36,37,38,46,47,48
- B = notAssessed
- L = notAssessed
- M = notApplicable
- D = notApplicable

Other permanent teeth:
- B/M/D/L = notApplicable

IMPORTANT:
These are DEFAULTS, not a hard biological impossibility rule.

Do NOT make NaturalToothRecord decoding reject an explicitly supplied non-default furcation assessment solely because tooth morphology can vary anatomically.

The purpose is to seed sensible defaults while preserving explicit clinician-observed exceptions later.

## E. NaturalToothRecord

Implement a value-semantic type equivalent to:

```swift
struct NaturalToothRecord: Codable, Equatable, Sendable {
    var toothID: ToothID
    var sites: PeriodontalSiteMeasurements
    var facialSurface: ToothSurfaceMeasurement
    var oralSurface: ToothSurfaceMeasurement?
    var mobility: MobilityGrade?
    var furcation: FurcationFindings
    var fullMouthScores: FullMouthScoreFindings
}
```

Property names may vary slightly only for clarity.

Semantics:
- NaturalToothRecord represents a PRESENT natural tooth.
- Do NOT add `present: Bool`; missing state belongs to ChartPositionRecord.
- mobility nil = not assessed;
- mobility grade0 = assessed zero;
- BOP remains inside six SiteMeasurements;
- FMPS/FMBS remain independent four-wedge values;
- AG applicability invariant comes from ToothID/arch;
- no implant-specific fields.

A convenience `blank(for toothID: ToothID)` factory is allowed and useful if it remains deterministic and fully tested.

If added:
- all six site measurements start not assessed;
- mobility starts nil;
- FMPS/FMBS wedges start nil;
- furcation uses the default factory above;
- surface supplements follow AG applicability.

Do not make this blank factory a clinical diagnosis/default-positive source.

## F. ImplantSiteMeasurement

Implants MUST use their own measurement type.

Implement in `ImplantRecord.swift`:

```swift
struct ImplantSiteMeasurement: Codable, Equatable, Sendable {
    var probingDepthMM: Int?
    var mucosalMarginMM: Int?
    var bop: Bool?
}
```

Do NOT give implants:
- natural-tooth CAL;
- natural gingivalMarginMM;
- attached gingiva;
- natural furcation;
- MobilityGrade.

For first-demo scope, do NOT add plaque/suppuration here merely because Clinica contains them; those are later clinical-domain expansion items unless a later approved task adds them.

Mucosal-margin sign/range editing behavior is NOT being defined in Task 4.
Store the optional raw integer only.

## G. Fixed six-site implant container

As with natural teeth, do not use an unconstrained dictionary.

Implement an explicit six-site container for ImplantSiteMeasurement with MB/B/DB/ML/L/DL and a tested accessor keyed by `PeriodontalSite`.

## H. ImplantRecord

Implement:

```swift
struct ImplantRecord: Codable, Equatable, Sendable {
    var toothID: ToothID
    var sites: ImplantSiteMeasurements
    var mobility: Bool?
}
```

Implant mobility:
- nil = not assessed;
- false = assessed non-mobile;
- true = assessed mobile.

Do NOT model implant mobility with MobilityGrade.

Do not add natural-tooth FMPS/FMBS or furcation into ImplantRecord in this task.

## I. ChartPositionRecord

Implement exactly three semantic occupancy states:

```swift
enum ChartPositionRecord: Codable, Equatable, Sendable {
    case natural(NaturalToothRecord)
    case implant(ImplantRecord)
    case missing(ToothID)
}
```

Provide:

`var toothID: ToothID`

The enum itself prevents a missing position from exposing writable natural SiteMeasurement state.

Do NOT implement:
- “present bool + implant bool” combinations;
- natural+implant collisions inside one chart position;
- QDento disabled-bit semantics as canonical occupancy.

Natural, implant, and missing are mutually exclusive by construction.

## J. PeriodontalExam

Implement a minimal dated snapshot aggregate, for example:

```swift
struct PeriodontalExam: Codable, Equatable, Sendable {
    var id: UUID
    var examinedAt: Date
    var positions: [ChartPositionRecord]
}
```

Foundation may be used for UUID/Date/Codable.

This first aggregate intentionally does NOT yet contain:
- patient ID;
- provider ID;
- exam status;
- Stage/Grade;
- radiographic data;
- progression;
- risk modifiers;
- history;
- notes;
- backend identifiers.

Those belong to later explicitly approved tasks.

### Position invariant

Each ToothID may occur at most once in `positions`.

Reject duplicate ToothID positions:
- in direct construction;
- in Codable decoding.

Do not silently keep first/last duplicate.

A partial positions array is allowed in Task 4 because deterministic fixtures and unit tests may construct bounded exam subsets.

Do NOT require exactly 32 positions yet.

A later full-mouth factory/fixture may provide all 32.

### Order invariant

Do NOT treat `positions` array order as clinical/chart order.

Identity is ToothID.

If you add `record(for toothID: ToothID)`, it must search/key by ToothID, not index.

Do not silently sort into QDento visual order.

# TDD REQUIREMENTS

## Phase A — Natural tooth separation

Write RED tests first covering at minimum:

1. NaturalToothRecord structurally contains all six named site measurements.
2. Site accessor returns/replaces only the requested site.
3. A maxillary tooth requires:
   - facial surface present;
   - oral AG surface absent/not applicable.
4. A mandibular tooth requires both facial and oral surface supplements.
5. Maxillary record with oral surface is rejected.
6. Mandibular record without oral surface is rejected.
7. AG nil inside an applicable surface remains distinct from surface not-applicable.
8. mobility nil differs from grade0.
9. fullMouthScores preserve independent FMPS/FMBS state.
10. furcation default for maxillary molar.
11. furcation default for mandibular molar.
12. furcation default for non-molar.
13. explicit non-default furcation state can still be represented.
14. recession:
    - all facial GM assessed → correct max(0,max) value;
    - negative-only surface → 0;
    - any missing GM among the three → nil;
    - oral recession uses ML/L/DL independently from AG applicability.

Run RED before implementation.

## Phase B — Implant separation

Write RED tests first covering:

1. Implant record has a separate six-site ImplantSiteMeasurement container.
2. Implant site has PD + mucosal margin + BOP.
3. Implant site has no CAL API.
4. Implant record has Bool? mobility semantics.
5. nil/false/true implant mobility round-trips distinctly.
6. implant site BOP nil/false/true round-trips distinctly.
7. natural and implant records cannot be confused by type.

Run RED before implementation.

## Phase C — Chart position + exam aggregate

Write RED tests first covering:

1. ChartPositionRecord natural/implant/missing are distinct.
2. computed toothID returns the underlying ToothID for all three cases.
3. missing case exposes no natural measurement collection.
4. PeriodontalExam can contain natural + implant + missing at different ToothIDs.
5. duplicate ToothID direct construction is rejected.
6. duplicate ToothID encoded input is rejected during decode.
7. partial exam positions are accepted.
8. record(for:) returns by ToothID identity rather than array index/order.
9. aggregate Codable round-trip preserves:
   - a natural site with PD/GM/CAL;
   - BOP false versus nil;
   - mobility grade0 versus nil;
   - furcation notAssessed/notApplicable/grade0;
   - FMPS/FMBS nil/false/true combinations;
   - attached gingiva measured and nil;
   - implant PD/mucosalMargin/BOP;
   - implant mobility false;
   - missing tooth state.
10. no `recession` field is persisted in encoded natural tooth/exam JSON.

Then implement the minimum code.

# CODABLE / INVARIANT REQUIREMENTS

Any invariant enforced during direct initialization must also be enforced during decoding.

Specifically test malformed encoded data for:
- maxillary natural tooth with oralSurface present;
- mandibular natural tooth with oralSurface absent;
- duplicate ToothID positions in PeriodontalExam.

Do not rely only on “well-formed app-generated JSON.”

# RANGE POLICY

Do not clamp numeric domain values in Task 4.

UI/input range enforcement belongs later.

Task 4 aggregate validation is about semantic structure, not numeric-entry behavior.

# BUILD / REGRESSION VERIFICATION

Use the verified environment unless unavailable:

- Xcode 27.0
- iPhone 18 Pro
- iOS 27.0
- deployment target iOS 18.0

Run at minimum:

1. focused Task 4 tests;
2. all Domain tests;
3. full unit-test suite;
4. existing UI launch regression;
5. app build.

No “should pass” claims.

# REVIEW FOCUS

Independent reviewers must challenge:

## Natural/implant separation
- Can an implant accidentally carry CAL/furcation/natural mobility?
- Can a missing position expose natural measurement state?
- Can a chart position encode simultaneous natural+implant occupancy?

## Surface applicability
- Is maxillary oral AG truly not applicable rather than nil-unassessed?
- Is lower oral AG applicable-but-unassessed representable?
- Is recession independent from AG applicability?

## Furcation
- Are Clinica morphology mappings defaults rather than permanent impossibility constraints?
- Is grade0 distinct from notAssessed/notApplicable?

## Exam invariants
- Can duplicate ToothID positions enter through decoding?
- Does any logic depend on array position instead of ToothID?

## Scope
- Did Task 4 accidentally add Stage/Grade, persistence service, SwiftUI editor, geometry, or patient/backend scope?

# SIX-CYCLE TASK REFINEMENT

After GREEN and before final commit, perform exactly six cumulative passes:

1. Accuracy & Fundamental Correction
2. Completeness & Gap Analysis
3. Structure & Architecture
4. Adversarial / Critical Review
5. Usability & Goal Fit
6. Final Synthesis, Regression Check & Polish

Adversarially attempt malformed aggregate decode and natural/implant/missing collisions.

No seventh cycle.

# COMMIT

Commit accepted implementation with:

`feat: add periodontal tooth implant and exam domain`

Push:

`domain/p02-t4-records-exam`

Do NOT merge.
Do NOT open a PR.
Do NOT begin Task 5.

# STOP CONDITIONS

STOP and report rather than improvising if:
- exact Task 3 base cannot be checked out;
- surface-applicability invariant conflicts with accepted contract;
- aggregate Codable cannot preserve invariants without a broader architecture decision;
- a Clinica reference suggests a material first-demo semantic conflict not resolved by this packet;
- full regression build/test cannot pass within the bounded task.

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
- focused Task 4 test result
- all Domain test result
- full unit-test result
- UI regression-test result
- build result
- independent review verdict(s)
- concerns/blockers
- verification summary
- controller rulings, if any
