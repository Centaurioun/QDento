# Plan 03 / Task 1 Acceptance + Pre-Geometry Fixture Correction

Date: 2026-10-01

Status: ACCEPTED

Implementation repository:
`Centaurioun/periodontal-ios`

Accepted Task 1 branch:
`geometry/p03-t1-qdento-display-adapter`

Task 1 HEAD:
`f25f04fb51ed72b6b3f7245da9d2574319af2e87`

Accepted pre-geometry correction branch:
`fix/p03-pregeometry-f02-cal-y`

Current downstream base:
`b90cbe9d18bfb376cbc0441932364024e078b7eb`

## Task 1 coordinator verification

Verified:
- Task 1 is exactly one commit ahead of its contract-preparation base;
- changed implementation files are only QDentoDisplayAdapter, its focused tests, and Xcode file registration;
- canonical clinical GM → QDento display GM is explicit sign inversion;
- q0/q1/q2 remains Geometry/reference-only and does not leak into Domain;
- all quadrant/surface source-slot mappings match contract v1.1;
- visible q order is q0/q1/q2 for maxilla and q2/q1/q0 for mandible;
- Q3/Q4 visible-site regression tests match the accepted v1.1 erratum;
- wrong-surface reverse lookup returns nil rather than trapping.

The review note that negating Swift `Int.min` would overflow is not a blocker for this clinical adapter. It is outside the accepted periodontal measurement input domain and remains part of deferred malformed-import numeric policy; no saturation/clamping behavior is invented here.

## Pre-geometry F02 correction

Before Task 2, the coordinator independently recomputed F02 CAL-y from QDento source:

`CAL_y = 105 - 3 × CAL`

For F02 source CAL:
`[1,3,6,5,2,4]`

the correct expected CAL-y is:
`[102,96,87,90,99,93]`.

The prior oracle incorrectly recorded slot4 as 87.

The remediation commit:
`b90cbe9d18bfb376cbc0441932364024e078b7eb`

changes only:
- `ParityFixtureCatalog.swift`
- `ParityFixtureTests.swift`

and records the corrected value plus arithmetic regression test.

No Domain semantics, named-site mapping, GM-y expectation, F03 value, or Task 1 adapter changed.

Reported final verification:
- parity tests: 13 passed;
- Geometry adapter tests: 7 passed;
- Domain + Parity: 47 passed;
- full unit tests: 55 passed;
- UI launch: passed;
- build: passed;
- fresh reviewer: passed.

## Downstream base rule

Plan 03 / Task 2 must branch from exact HEAD:

`b90cbe9d18bfb376cbc0441932364024e078b7eb`

## Verdict

**ACCEPTED**

Plan 03 / Task 2 may begin.
