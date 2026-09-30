# Plan 02 / Task 5 Coordinator Review — Pre-Acceptance Findings

Date: 2026-09-30

Status: REMEDIATION_REQUIRED

Implementation candidate:
- repo: `Centaurioun/periodontal-ios`
- branch: `parity/p02-t5-fixture-catalog`
- candidate HEAD: `6eb610370ce1202d9e80fabe6e8e6d3f567d5b9d`

## Independent verification

The candidate is exactly one commit ahead of Task 4 and changes only the intended Parity source/test files plus Xcode project references.

The source-to-canonical fixture translation is otherwise consistent with the frozen Plan 01 contract.

## Finding 1 — F04/F05 expected exam identity changes across an edit transition

Severity: IMPORTANT

`ParityFixtureCatalog` creates F04/F05 initial and expected exams using different fixed UUIDs.

These fixtures represent edits to an existing periodontal exam:
- F04 = direct CAL edit;
- F05 = attached-gingiva edit.

The edit action must not create a new exam identity.

If later behavior tests apply the fixture action to `initialExam` and compare the resulting exam to `expectedExam`, the current catalog would fail even when the domain edit is correct, because the IDs differ.

Required correction:
- for transition fixtures, initial and expected exams must preserve the same exam `id` and `examinedAt`;
- only the intended edited domain values should differ;
- add regression tests proving identity/time preservation and proving the expected diff is scoped to the intended edit.

This is a fixture-oracle correctness issue and must be fixed before Plan 02 acceptance.

## Finding 2 — public six-slot initializer can process-crash on malformed input

Severity: MINOR but worth fixing before freeze

`ParitySixSlotValues.init(_ values: [Int])` is public and uses `precondition(values.count == 6)`.

A malformed caller can crash the process. This is unnecessary for a small test/reference value type.

Preferred correction:
- replace the public trapping array initializer with a non-trapping API;
- either a six-scalar initializer, a throwing/failable validated array initializer, or make the trapping helper private/internal and expose a safe public initializer;
- preserve Codable/Equatable determinism.

Do not broaden scope.

## Verdict

**REMEDIATION_REQUIRED**

Do not start Plan 03 until both findings are resolved, full regression verification passes, and a fresh read-only reviewer approves the scoped fix.
