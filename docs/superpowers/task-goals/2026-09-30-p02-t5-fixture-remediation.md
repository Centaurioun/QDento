[$superpowers](app://plugins~Plugin_60aea7460bd4819199fd97a9553a5e12)
[@Build iOS Apps](plugin://build-ios-apps@openai-curated-remote)

# PLAN 02 / TASK 5 — COORDINATOR REMEDIATION

Resume the SAME Task 5 Codex/controller context if possible.

Do NOT begin Plan 03.

Target repository:
`Centaurioun/periodontal-ios`

Existing branch:
`parity/p02-t5-fixture-catalog`

Current candidate HEAD:
`6eb610370ce1202d9e80fabe6e8e6d3f567d5b9d`

Coordinator review source:
`Centaurioun/QDento`
`docs/superpowers/reviews/2026-09-30-p02-t5-fixture-preacceptance-review.md`
on `origin/docs/periodontal-ios-brainstorming`.

Read that review first.

## Finding A — IMPORTANT — transition fixture identity

F04 and F05 represent edits to the SAME periodontal exam.

Currently their `initialExam` and `expectedExam` use different fixed UUIDs.

Fix this.

Required invariant for F04 and F05:
- `initialExam.id == expectedExam.id`
- `initialExam.examinedAt == expectedExam.examinedAt`

The expected state should differ only in the intended fixture edit:
- F04: target canonical B CAL/clinical-GM consequence;
- F05: facial attached gingiva 7 → 8.

Do not change exam identity as a side effect of an edit.

Add focused tests proving:
1. F04 initial/expected ID and date are identical;
2. F05 initial/expected ID and date are identical;
3. F04 non-target positions are identical;
4. F04 target tooth differs only where the direct CAL fixture expects;
5. F05 non-target positions are identical;
6. F05 target tooth differs only in facial AG;
7. deterministic Codable guarantees remain intact.

Do not implement behavior/editing logic.

## Finding B — MINOR API SAFETY — ParitySixSlotValues

Current public initializer accepts `[Int]` and traps with `precondition` if count != 6.

Replace the public trapping API with a safe API.

Preferred design:
- expose an explicit six-scalar initializer:
  `init(_ slot0: Int, _ slot1: Int, _ slot2: Int, _ slot3: Int, _ slot4: Int, _ slot5: Int)`
  or labeled equivalent;
- if an array conversion helper is useful, make it failable/throwing or non-public;
- no caller-controlled malformed array should be able to process-crash through the public API.

Update catalog uses accordingly.

Add a test demonstrating that the public API no longer has a malformed-array trapping path. Do this through API shape/validated initializer tests; do not deliberately crash the test process.

## SCOPE

Allowed changes only:
- `PeriodontalIOS/Parity/ParityFixture.swift`
- `PeriodontalIOS/Parity/ParityFixtureCatalog.swift`
- `PeriodontalIOSTests/Parity/ParityFixtureTests.swift`
- Xcode project only if genuinely required (it should not be).

Do NOT change accepted Domain types.
Do NOT implement geometry/UI/summary/editing.
Do NOT change source fixture values or canonical mapping.

## WORKFLOW

Use:
- Superpowers `using-superpowers`
- `subagent-driven-development`
- `verification-before-completion`
- Build iOS Apps current build/test workflow

All sub-agents GPT-6 Luna only, never Sol.

Use:
1. fresh remediation implementer;
2. fresh read-only scoped reviewer after the fix.

## VERIFICATION

Run:
- focused `ParityFixtureTests`;
- all Domain + Parity tests;
- full unit suite;
- existing UI launch regression;
- app build;
- `git diff --check`.

Confirm:
- deterministic sorted-key JSON still byte-identical;
- source-translation tests unchanged/passing;
- F04/F05 identity/time invariant now passes;
- no accepted Domain files changed.

Commit the remediation separately with a clear message such as:

`fix: preserve parity transition fixture identity`

Push the SAME branch:
`parity/p02-t5-fixture-catalog`

Do NOT merge.
Do NOT open a PR.
Do NOT begin Plan 03.

Return only:
- STATUS
- new HEAD
- changed files
- F04 identity/date verification
- F05 identity/date verification
- six-slot API change
- focused parity test result
- Domain + Parity test result
- full unit-test result
- UI regression result
- build result
- fresh reviewer verdict
- git diff/status/push verification
- concerns/blockers
