# Plan 02 / Task 4 Acceptance — Tooth / Implant / Exam Aggregate

Date: 2026-09-30

Status: ACCEPTED

Implementation repository:
`Centaurioun/periodontal-ios`

Accepted branch:
`domain/p02-t4-records-exam`

Accepted base:
`90a82e87bb43c23298e01380539ee31841d0578e`

Accepted HEAD:
`4033d7e0a03d461609d0e8bb5f78f8ec45dc0e14`

## Independent coordinator verification

The Task 4 branch is exactly one commit ahead of the accepted Task 3 base.

Changed files are limited to:
- `NaturalToothRecord.swift`
- `ImplantRecord.swift`
- `PeriodontalExam.swift`
- two focused Task 4 test files;
- required Xcode project references.

Verified aggregate properties:
- natural, implant, and missing are mutually exclusive chart-position states;
- NaturalToothRecord structurally contains all six named sites;
- ImplantRecord uses its own six-site measurement type with no natural CAL/furcation/mobility-grade fields;
- maxillary oral attached gingiva is aggregate-level not applicable;
- mandibular oral attached gingiva can be applicable but unassessed;
- those applicability invariants are enforced during direct construction and decode;
- recession is null-aware, surface-specific, derived, and not persisted;
- furcation morphology mapping is used as a default factory rather than a hard rejection rule;
- duplicate ToothID positions are rejected both at direct PeriodontalExam construction and decoding;
- exam lookup is by ToothID identity rather than array position;
- partial exam position sets are allowed;
- no Stage/Grade, persistence service, SwiftUI, geometry, or backend scope was introduced.

Reported execution evidence:
- focused Task 4 tests: 12 passed;
- all Domain tests: 34 passed;
- full unit tests: 36 passed;
- UI launch regression passed in the full suite;
- simulator app build succeeded on Xcode 27.0 / iPhone 18 Pro / iOS 27.0;
- independent reviewers approved after scoped remediation.

## Downstream base rule

Plan 02 / Task 5 must branch from exact accepted HEAD:

`4033d7e0a03d461609d0e8bb5f78f8ec45dc0e14`

## Verdict

**ACCEPTED**

Plan 02 / Task 5 may begin.
