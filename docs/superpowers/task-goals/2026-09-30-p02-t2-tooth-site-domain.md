[$superpowers](app://plugins~Plugin_60aea7460bd4819199fd97a9553a5e12)
[@Build iOS Apps](plugin://build-ios-apps@openai-curated-remote)

# TASK GOAL PACKET — PLAN 02 / TASK 2
# TOOTH IDENTITY + CANONICAL PERIODONTAL SITE DOMAIN

## TASK TYPE

PROMPT for one dedicated Codex controller instance.

This is a bounded DOMAIN implementation task.

Do NOT implement measurement semantics yet.
Do NOT implement PD/CAL/GM.
Do NOT implement BOP/FMPS/FMBS.
Do NOT implement UI/chart geometry.
Do NOT implement persistence.
Do NOT modify QDento or Clinica.

## REQUIRED WORKFLOW

Before implementation:

1. Invoke Superpowers `using-superpowers`.
2. Invoke `using-git-worktrees`.
3. Invoke `subagent-driven-development` for this bounded task.
4. Use `verification-before-completion` before completion claims.
5. Use the Build iOS Apps plugin.
6. Read/use `swiftui-ui-patterns` and `swiftui-view-refactor` for architectural consistency, while keeping this task's Domain files free of SwiftUI.

## MODEL / SUB-AGENT POLICY

You are the controller.

Use fresh GPT-6 Luna-only sub-agents where supported.

Recommended sequence:
- Implementer — writes tests first, then minimal domain implementation.
- Spec Compliance Reviewer — fresh, read-only GPT-6 Luna.
- Code/Domain Quality Reviewer — fresh, read-only GPT-6 Luna.

Reviewers MUST NOT edit.
If a reviewer finds a Critical/Important issue, dispatch a fresh remediation implementer and repeat the scoped review.

No sub-agent may use GPT-6 Sol.
No nested sub-agents.

## TARGET REPOSITORY / BASE

Repository:
`Centaurioun/periodontal-ios`

Accepted Task 1 branch:
`foundation/p02-t1-xcode-baseline`

Exact accepted Task 1 HEAD:
`6a7d1fbbd1fbc384ef662b0775d146bb97579c06`

Create a NEW isolated worktree/branch from that exact commit:

`domain/p02-t2-tooth-site`

Do NOT work directly on:
- `main`
- `foundation/p02-t1-xcode-baseline`

Do NOT merge either branch.

## AUTHORITATIVE INPUTS IN THE iOS REPO

Read first:

- `docs/reference/qdento-ios-design-spec.md`
- `docs/contracts/rendering-parity-contract-v1.md`
- `docs/reference/SOURCE-AUTHORITIES.md`
- `docs/provenance/CLINICA_REFERENCE.md`
- `docs/provenance/QDENTO_REFERENCE.md`

For this task, the frozen rendering contract is authoritative for:
- permanent FDI identity;
- QDento adapter distinction;
- canonical named-site model;
- facial/oral grouping.

Do not re-open Plan 01 research.

## GOAL

Implement the smallest robust canonical identity layer needed by all later periodontal domain/geometry work:

1. validated permanent-tooth FDI identity;
2. arch/quadrant helpers derived from FDI identity, never from array position;
3. canonical periodontal surface enum;
4. canonical six-site enum and surface grouping.

The public domain must use explicit tooth/site identity and must NOT expose QDento packed-array positions as canonical identity.

## ALLOWED PRODUCT FILES

Create only:

- `PeriodontalIOS/Domain/ToothID.swift`
- `PeriodontalIOS/Domain/PeriodontalSurface.swift`
- `PeriodontalIOS/Domain/PeriodontalSite.swift`

## ALLOWED TEST FILES

Create only:

- `PeriodontalIOSTests/Domain/ToothIDTests.swift`
- `PeriodontalIOSTests/Domain/PeriodontalSiteTests.swift`

The Xcode project file may be modified ONLY as required to include these new source/test files in the existing targets.

Do not modify the root UI or app model for this domain task.

## REQUIRED PUBLIC INTERFACES

### ToothID

Implement a validated value type:

`struct ToothID: Hashable, Codable, Sendable`

It must represent only permanent FDI teeth in this first-demo scope.

Valid exact FDI set:

- 11...18
- 21...28
- 31...38
- 41...48

Values outside that exact set are invalid.

Provide an explicit validated construction API. A failable initializer is acceptable and preferred unless a stronger equivalent is justified.

Provide arch and quadrant helpers derived entirely from the FDI value.

You may define small supporting enums such as `DentalArch` and `FDIQuadrant` in `ToothID.swift`; do not create unrelated new files/types.

Required semantic mapping:

- Q1 / 11...18 = maxillary
- Q2 / 21...28 = maxillary
- Q3 / 31...38 = mandibular
- Q4 / 41...48 = mandibular

### Critical Codable invariant

A decoded `ToothID` MUST be validated through the same FDI invariant.

Synthesized Codable that can reconstruct an invalid stored integer is NOT acceptable.

Add a test proving that decoding an invalid FDI value fails.

The encoded representation may be a single integer unless an existing repository convention strongly justifies otherwise.

### Ordering boundary

Do NOT make QDento visual/chart order part of `ToothID`.

Identity and chart presentation order are different concepts.

Do NOT embed these arrays as canonical ToothID order:
- QDento maxillary display order;
- QDento mandibular display order.

Those belong to later adapter/geometry layers.

If you expose an `allPermanent` collection, its semantics must be explicit and must not be mislabeled as chart order.

## PeriodontalSurface

Implement exactly:

```swift
enum PeriodontalSurface: String, Codable, Sendable {
    case facial
    case oral
}
```

It may conform to additional value-semantic protocols such as `Hashable` or `CaseIterable` if useful, but do not add extra surface cases.

Do not split oral into palatal/lingual in the canonical enum. Arch-specific human terminology can be presentation-derived later.

## PeriodontalSite

Implement the approved canonical six sites:

- MB
- B
- DB
- ML
- L
- DL

Required canonical iteration order:

`MB, B, DB, ML, L, DL`

Required grouping:

- facial = MB, B, DB
- oral = ML, L, DL

Implement:

`var surface: PeriodontalSurface`

The canonical site type must not contain:
- QDento packed offsets;
- screen-left/screen-right;
- quadrant mirroring;
- QDento q0/q1/q2.

Those are later adapter/geometry concerns.

Follow the approved plan's public semantic identifiers. Preserve exact raw clinical abbreviations for Codable/raw representation.

## TDD REQUIREMENTS

### ToothID tests first

Before production implementation, write failing tests covering at minimum:

1. every valid permanent FDI value is accepted;
2. representative invalid values are rejected:
   - 0
   - negative value
   - 10
   - 19
   - 20
   - 29
   - 30
   - 39
   - 40
   - 49
   - obvious non-FDI value such as 99;
3. quadrant derivation for all four quadrants;
4. arch derivation:
   - Q1/Q2 → maxillary
   - Q3/Q4 → mandibular;
5. equality/hash identity is based on tooth identity, not construction history;
6. Codable round-trip succeeds for valid values;
7. decoding an invalid value FAILS rather than creating an invalid `ToothID`;
8. no test or API relies on array index to determine arch/quadrant.

Run the focused test target and demonstrate RED before implementation.

### Site tests first

Write failing tests covering at minimum:

1. exact `CaseIterable` order:
   `MB, B, DB, ML, L, DL`;
2. facial sites are exactly MB/B/DB;
3. oral sites are exactly ML/L/DL;
4. each site's `surface` is correct;
5. raw/Codable round-trip preserves the clinical abbreviation;
6. unknown site raw/decoded value fails.

Demonstrate RED before final implementation where practical.

## IMPLEMENTATION CONSTRAINTS

- Domain files must NOT import SwiftUI.
- Prefer `Foundation` only where Codable needs it; avoid unnecessary dependencies.
- No global mutable state.
- No singleton.
- No packed integer offset helpers.
- No UI strings beyond the exact clinical raw abbreviations.
- No premature tooth morphology/type logic.
- No temporary-tooth support.
- No implant record type yet.
- No convenience APIs for future tasks unless directly required by current tests.

Keep the implementation intentionally small.

## BUILD / TEST VERIFICATION

Use the currently verified environment from Task 1 unless unavailable:

- Xcode 27.0
- iPhone 18 Pro
- iOS 27.0

Deployment target remains:
- iOS 18.0

Run at minimum:

1. focused domain unit tests;
2. full unit test target;
3. existing UI launch test as regression;
4. app build.

Use Build iOS Apps/Xcode tooling to verify.

No “should pass” claims.

## REVIEW PACKAGE

Before independent review, provide reviewers the true diff:

Base:
`6a7d1fbbd1fbc384ef662b0775d146bb97579c06`

Head:
your Task 2 candidate HEAD.

The reviewers must check especially:
- invalid FDI decode cannot bypass validation;
- chart order is not embedded in ToothID;
- all six sites and only six sites exist;
- surface grouping is correct;
- no QDento q-index leaks into canonical domain;
- no SwiftUI dependency entered Domain.

## SIX-CYCLE TASK REFINEMENT

After implementation and initial tests, apply exactly six cumulative passes:

1. Accuracy & Fundamental Correction
2. Completeness & Gap Analysis
3. Structure & Architecture
4. Adversarial / Critical Review
5. Usability & Goal Fit
6. Final Synthesis, Regression Check & Polish

Adversarial questions:
- Can malformed persisted data create ToothID(99)?
- Did any “helpful” order API accidentally encode QDento visual order?
- Can a site belong to the wrong surface?
- Are the raw abbreviations stable through Codable?
- Did Task 2 accidentally implement Task 3 concepts?
- Does full Task 1 launch behavior still work?

No seventh cycle.

## COMMIT

Commit the accepted Task 2 implementation with:

`feat: add tooth and periodontal site domain`

Push branch:

`domain/p02-t2-tooth-site`

Do NOT merge.
Do NOT open a PR.
Do NOT begin Task 3.

## STOP CONDITIONS

STOP and report rather than improvising if:
- accepted Task 1 HEAD cannot be checked out;
- the Xcode project cannot include the new files without broader unrelated project churn;
- Build iOS Apps guidance materially conflicts with the approved contract;
- tests reveal an unresolved semantic conflict with the frozen contract;
- full regression build/test cannot pass within this bounded task.

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
- focused unit-test result
- full unit-test result
- UI regression-test result
- build result
- independent review verdict(s)
- concerns/blockers
- verification summary
- controller rulings, if any
