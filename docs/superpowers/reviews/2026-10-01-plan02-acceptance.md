# Plan 02 Acceptance — iOS Foundation + Canonical Domain + Parity Fixtures

Date: 2026-10-01

Status: ACCEPTED

Implementation repository:
`Centaurioun/periodontal-ios`

Frozen implementation branch:
`freeze/p02-domain-foundation-v1`

Frozen implementation HEAD:
`893723f3652bdc42a104a80a4e7f4f3c3eeacfe9`

Plan 03 contract-preparation branch:
`contract/p03-parity-contract-v1-1`

Contract-preparation HEAD:
`f5b6c83fab98961e65db44b8daff4dbed66012f4`

## Accepted implementation sequence

- Task 1 — Xcode foundation
- Task 2 — ToothID + canonical periodontal sites
- Task 3 — measurement/finding value semantics
- Task 4 — natural tooth / implant / missing + exam aggregate
- Task 5 — deterministic parity fixture catalog
- Task 5 scoped remediation — preserve transition exam identity and remove public six-slot trapping initializer

The frozen Plan 02 implementation is five commits ahead of the accepted Task 1 foundation and contains only the planned Domain/Parity/test/Xcode-project changes.

## Acceptance properties

- permanent FDI identity validated on construction and decoding;
- canonical sites are MB/B/DB/ML/L/DL;
- canonical clinical GM sign is preserved;
- CAL manual/derived source semantics preserved;
- BOP is six-site and distinct from four-wedge FMPS/FMBS;
- mobility, furcation, not-assessed, false/zero, and not-applicable semantics remain distinct;
- natural tooth, implant, and missing are type-distinct occupancy states;
- upper-palatal attached gingiva is aggregate-level not applicable;
- recession is derived and not persisted;
- duplicate ToothID exam positions are rejected on construction and decode;
- all eight Plan 01 parity fixtures exist as deterministic typed Swift data;
- F01–F07 are full-mouth 32-natural parity baselines;
- F08 is 30 natural / 1 missing / 1 implant;
- F04/F05 transition fixtures preserve exam identity and date;
- parity fixture construction contains no runtime UUID/date randomness;
- public six-slot parity helper no longer exposes a malformed-array process trap.

Reported final verification after remediation:
- focused parity tests: 12 passed;
- Domain + Parity tests: 46 passed;
- full unit tests: 48 passed;
- UI launch regression passed;
- app build passed;
- fresh reviewer: approved with no findings.

## Plan 03 contract erratum

During the Plan 02 acceptance review, an internal inconsistency was found in the old Plan 01 v1 geometry table:
the Q3/Q4 **visible left/middle/right** cells were transposed, while the coordinator ruling and q0/q1/q2 mappings were already correct.

Plan 02 code/fixtures are unaffected.

Plan 03 MUST use:
`docs/contracts/rendering-parity-contract-v1.1.md`
from the iOS contract-preparation branch.

The original v1 remains immutable for audit history.

## Downstream base rule

Plan 03 Task 1 must branch from exact contract-preparation HEAD:

`f5b6c83fab98961e65db44b8daff4dbed66012f4`

That branch is the frozen Plan 02 code plus documentation-only contract v1.1 preparation.

## Verdict

**PLAN_02_ACCEPTED**

Plan 03 may begin.
