[$superpowers](app://plugins~Plugin_60aea7460bd4819199fd97a9553a5e12)
[@Build iOS Apps](plugin://build-ios-apps@openai-curated-remote)

# PRE-PLAN-03-TASK-2 REMEDIATION
# CORRECT P01-T3-F02 SOURCE-LOCAL CAL-Y ORACLE

## TASK TYPE

PROMPT for one dedicated Codex remediation instance.

Do NOT begin Plan 03 / Task 2 contour geometry.

## TARGET REPOSITORY / BASE

Repository:
`Centaurioun/periodontal-ios`

Current accepted Plan 03 Task 1 HEAD:
`f25f04fb51ed72b6b3f7245da9d2574319af2e87`

Create an isolated branch/worktree from that exact commit:

`fix/p03-pregeometry-f02-cal-y`

Do not modify prior branches directly.

## AUTHORITATIVE ERRATUM

Read from:
`Centaurioun/QDento`
branch:
`origin/docs/periodontal-ios-brainstorming`

file:
`docs/superpowers/reviews/2026-10-01-p02-f02-cal-y-erratum.md`

Also inspect:
- iOS `docs/contracts/rendering-parity-contract-v1.1.md`
- `PeriodontalIOS/Parity/ParityFixtureCatalog.swift`
- `PeriodontalIOSTests/Parity/ParityFixtureTests.swift`

## SOURCE FACT

QDento formula:
`CAL_y = 105 - 3 × CAL`

F02 source CAL:
`[1, 3, 6, 5, 2, 4]`

Correct expected CAL-y:
`[102, 96, 87, 90, 99, 93]`

The existing fixture metadata incorrectly stores:
`[102, 96, 87, 90, 87, 93]`

Only slot4 is wrong.

## ALLOWED CHANGES

Modify only:
- `PeriodontalIOS/Parity/ParityFixtureCatalog.swift`
- `PeriodontalIOSTests/Parity/ParityFixtureTests.swift`

Do NOT modify:
- Domain files;
- Geometry adapter from Task 1;
- QDentoDisplayAdapter tests;
- Xcode project;
- UI;
- docs in the iOS repo.

## REQUIRED TDD / FIX

1. First update/add a focused test that derives or explicitly asserts F02's source-local CAL-y expectation from the accepted CAL values and source formula.
2. Demonstrate RED against the current candidate.
3. Correct only F02 `sourceLocalCALY.slot4` from 87 to 99.
4. Run GREEN.

Prefer a regression assertion that makes the arithmetic relationship obvious, not merely a magic literal replacement.

For example, verify:
`sourceLocalCALY.values == sourceCAL.map { 105 - 3 * $0 }`
for the exact F02 source CAL vector.

Do NOT implement the contour geometry engine here.

## REVIEW

Use:
- Superpowers `using-superpowers`
- `subagent-driven-development`
- `verification-before-completion`
- Build iOS Apps current build/test workflow

All sub-agents GPT-6 Luna only.

Use:
- fresh remediation implementer;
- fresh read-only parity/source reviewer.

Reviewer must verify:
- only F02 CAL-y oracle changed;
- canonical domain fixture data did not change;
- GM-y did not change;
- F03 and other fixtures did not change;
- Task 1 Geometry adapter files remain byte-for-byte unchanged.

## VERIFICATION

Run:
1. focused `ParityFixtureTests`;
2. focused `QDentoDisplayAdapterTests`;
3. all Geometry tests;
4. all Domain + Parity tests;
5. full unit suite;
6. existing UI launch regression;
7. app build;
8. `git diff --check`.

Confirm the exact corrected F02 value:
`[102, 96, 87, 90, 99, 93]`

Commit:

`fix: correct F02 CAL contour oracle`

Push:

`fix/p03-pregeometry-f02-cal-y`

Do NOT merge.
Do NOT begin Plan 03 Task 2.

## RETURN FORMAT

Return only:
- STATUS
- branch
- base commit
- new HEAD
- changed files
- TDD RED evidence
- corrected F02 CAL-y values
- focused parity test result
- adapter/Geometry regression result
- Domain + Parity regression result
- full unit-test result
- UI regression result
- build result
- fresh reviewer verdict
- verification summary
- concerns/blockers
