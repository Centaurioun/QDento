[$superpowers](app://plugins~Plugin_60aea7460bd4819199fd97a9553a5e12)
[@Build iOS Apps](plugin://build-ios-apps@openai-curated-remote)

# TASK GOAL PACKET — PLAN 02 / TASK 1
# CREATE THE REAL iOS REPOSITORY + XCODE VERIFICATION BASELINE

## TASK TYPE

PROMPT for one dedicated Codex controller instance.

This is the FIRST Swift/iOS implementation task after the accepted Plan 01 evidence freeze.

Do NOT implement periodontal domain types yet.
Do NOT implement geometry yet.
Do NOT build the periodontal chart yet.
Do NOT import QDento application code.
Do NOT copy QDento image assets in this task.

Your job is only to create a clean, real, native iOS repository/Xcode baseline with source-authority documentation and a verified launch/test baseline.

## REQUIRED WORKFLOW

Before implementation:

1. Invoke Superpowers `using-superpowers`.
2. Invoke `using-git-worktrees`.
3. Invoke `subagent-driven-development` and use it for THIS bounded Task 1.
4. Use `verification-before-completion` before every completion claim.
5. Use the Build iOS Apps plugin and read/use at minimum:
   - `swiftui-ui-patterns`
   - `swiftui-view-refactor`
   - the relevant Xcode/simulator build/verification guidance exposed by the plugin.
6. Once a runnable app exists, use the plugin's simulator/debug tooling where appropriate to verify launch.

## MODEL / SUB-AGENT POLICY

You are the controller.

Use fresh sub-agents for implementation/review according to `subagent-driven-development` where supported.

ALL sub-agents MUST:
- use GPT-6 Luna only;
- never use GPT-6 Sol;
- receive only the bounded Task 1 brief/context they need;
- not spawn nested sub-agents.

Recommended internal roles:
- Implementer — creates the repository/Xcode foundation and tests.
- Task Reviewer — read-only spec + code-quality review of the Task 1 diff.
- If useful, a read-only Xcode/Simulator Verification reviewer may independently inspect build/run evidence.

The controller may use the user-selected main model for orchestration.

## AUTHORITATIVE INPUTS

### Plan 01 frozen evidence authority

Repository:
`Centaurioun/QDento`

Frozen branch:
`freeze/p01-rendering-parity-contract-v1`

Current freeze HEAD after acceptance documentation:
`c3ece449a9db1b513a1fe1a89176595293cbec7f`

Frozen contract:
`docs/periodontal-ios/contracts/2026-09-30-rendering-parity-contract-v1.md`

Frozen contract blob SHA:
`ff663f2eeef01d9bdb71ca8964ff31eee836124c`

Plan 01 acceptance:
`docs/superpowers/reviews/2026-09-30-plan01-acceptance.md`

### Approved design / implementation plan authority

Read from QDento:
- `docs/superpowers/specs/2026-09-30-qdento-periodontal-ios-design.md`
- `docs/superpowers/plans/2026-09-30-periodontal-ios-master-roadmap.md`
- `docs/superpowers/plans/2026-09-30-02-ios-foundation-domain.md`
- `docs/superpowers/reviews/2026-09-30-periodontal-ios-plan-review.md`

If plan text conflicts with the approved spec, the spec wins.
If either conflicts with the frozen Plan 01 rendering/edit contract on a Plan 01 concern, the frozen contract wins for that concern.

## TARGET REPOSITORY

Intended GitHub repository:
`Centaurioun/periodontal-ios`

Visibility:
**private**

Intended local repository:
`/Users/yusuf/Repos/periodontal-ios`

Do NOT create the iOS app inside QDento.
Do NOT create it inside Clinica.
Do NOT modify either source repository.

### Remote creation rule

First check whether `Centaurioun/periodontal-ios` already exists.

- If it exists, inspect it before doing anything and do not overwrite unrelated content.
- If it does not exist and the available environment is authorized/capable of creating the GitHub repository, create it as PRIVATE.
- If remote repository creation is unavailable, create the dedicated local Git repository/Xcode project at `/Users/yusuf/Repos/periodontal-ios`, but STOP before pushing anywhere and return `BLOCKED_REMOTE_CREATION`.
- Never use QDento or Clinica as a fallback destination.

### Main / feature branch rule

Do not develop Task 1 directly on `main`.

If repository bootstrapping requires an initial `main` commit, keep it minimal (repository bootstrap only), push it, then create an isolated branch/worktree:

`foundation/p02-t1-xcode-baseline`

All actual Task 1 Xcode/project/docs/test work must happen on that isolated feature branch/worktree.

Do not merge Task 1 to main.
Do not open a PR unless explicitly asked later.

## XCODE / PROJECT REQUIREMENTS

Create a standard native SwiftUI iOS application.

Required:
- App/product module: `PeriodontalIOS`
- Unit-test target: `PeriodontalIOSTests`
- UI-test target: `PeriodontalIOSUITests`
- SwiftUI lifecycle
- Swift language
- no third-party package/runtime dependencies
- iOS deployment target: **18.0**

If the installed Xcode cannot support iOS 18.0:
STOP and report the exact Xcode/toolchain limitation.
Do not silently select another deployment target.

Do not introduce XcodeGen, Tuist, CocoaPods, or other project-generation/dependency tooling unless the approved Build iOS Apps workflow explicitly requires it and the controller records a ruling before use. Prefer a standard Xcode project.

## REQUIRED INITIAL APP STRUCTURE

At minimum create:

```text
PeriodontalIOS/
  App/
    PeriodontalIOSApp.swift
    AppModel.swift
  Features/
    PlaceholderRootView.swift

PeriodontalIOSTests/

PeriodontalIOSUITests/
  LaunchTests.swift

docs/
  reference/
    SOURCE-AUTHORITIES.md
    qdento-ios-design-spec.md
  contracts/
    rendering-parity-contract-v1.md
  provenance/
    QDENTO_REFERENCE.md
    CLINICA_REFERENCE.md

README.md
```

Keep the initial UI deliberately minimal.

The root visible SwiftUI surface must expose accessibility identifier:

`periodontal-root`

Do not add periodontal chart features yet.

## COPY / PROVENANCE REQUIREMENTS

### Copy approved design spec

Copy the approved design spec content into:

`docs/reference/qdento-ios-design-spec.md`

Add a short provenance header that states:
- source repo: `Centaurioun/QDento`
- source frozen branch/ref
- original path
- source commit/ref used
- copied for implementation reference

Do not alter the actual spec body.

### Copy frozen rendering contract

Copy the frozen Plan 01 contract verbatim into:

`docs/contracts/rendering-parity-contract-v1.md`

Verify the copied contract body corresponds to source blob SHA:

`ff663f2eeef01d9bdb71ca8964ff31eee836124c`

If you add provenance metadata, place it outside/above the copied body in a clearly marked wrapper/header and preserve the contract body byte-for-byte in a delimited section.

### SOURCE-AUTHORITIES.md

Record the authority split:

QDento:
- visual/interaction reference;
- frozen Plan 01 rendering/edit contract;
- not final clinical authority.

Clinica:
- canonical periodontal domain/clinical reference;
- site naming MB/B/DB/ML/L/DL;
- natural tooth/implant separation;
- accepted R2 clinical lineage for later integration;
- NOT being copied into the first Xcode baseline.

New iOS repo:
- native SwiftUI/state/persistence/rendering/test authority.

### QDENTO_REFERENCE.md

At minimum record:
- QDento repo/ref used;
- GPL-3.0 repository context;
- private-demo/reference nature;
- no QDento code/assets copied in Task 1;
- public/proprietary distribution remains separately gated.

### CLINICA_REFERENCE.md

At minimum record:
- repo: `Centaurioun/clinica-dental`
- current accepted reference lineage used by the project:
  `integration/pux2-r2-clinical-modernized`
- accepted frozen Clinical implementation SHA:
  `4a9b1133d9b2c5756b73aa5ac84394502f807209`
- Clinical source HEAD:
  `49236d941a2118d0233c4d44a86dbc9dc5a66006`
- integrated fix/reference HEAD:
  `f201035b561f440e5e1b35a22c7dbe03de6e84e7`
- note that a fresh authority check is required before later clinical-engine integration.

Do not copy Clinica source into this task.

## TDD / VERIFICATION REQUIREMENT

Before implementation of the launch marker, write the UI launch test first.

Required UI test:

`PeriodontalIOSUITests/LaunchTests.swift`

Test behavior:
1. launch app;
2. locate the root element with accessibility identifier `periodontal-root`;
3. assert it exists.

Demonstrate RED before adding/finalizing the root accessibility identifier where practical in the new scaffold.
Then implement the minimum root view and demonstrate GREEN.

Do not write meaningless tests that assert only process launch without checking the required root marker.

## BUILD / TEST VERIFICATION

Use the Build iOS Apps plugin's recommended current Xcode/simulator workflow.

You must record:
- Xcode version;
- selected simulator/device;
- exact build command/workflow;
- exact unit/UI test command/workflow;
- exit status;
- test counts/results where available.

At minimum verify:
- project opens/is recognized by Xcode tooling;
- app target builds;
- unit-test target is runnable;
- UI launch test passes;
- app launches in simulator and root marker is present.

No “should build” claims.

If simulator execution is impossible because of a concrete local environment issue, report the exact blocker and do not claim Task 1 DONE.

## README REQUIREMENTS

README should state:
- purpose: native periodontal iPhone demo;
- current status: foundation only;
- authority split pointer;
- build/test entry points;
- no claim of clinical-production readiness;
- no claim of App Store/distribution readiness.

Keep it concise.

## SCOPE / FORBIDDEN WORK

Do NOT yet implement:
- ToothID
- PeriodontalSite
- PD/CAL/GM domain model
- BOP
- FMPS/FMBS
- mobility
- furcation
- tooth visuals
- contour geometry
- persistence
- risk/summary engine
- Clinica Stage/Grade
- actual patient data
- backend/auth/cloud
- App Intents
- Liquid Glass

Task 1 is foundation only.

## SUBAGENT-DRIVEN REVIEW GATE

After the implementer completes:

1. Build the review package from the true Task 1 base to final HEAD.
2. Dispatch a fresh read-only task reviewer on GPT-6 Luna.
3. Require BOTH:
   - spec compliance verdict;
   - code/project quality verdict.
4. If Critical/Important findings exist, follow the Superpowers fix loop.
5. Do not move to Plan 02 / Task 2 from this Codex instance.

A clean implementer self-review is not enough.

## SIX-CYCLE TASK REFINEMENT

Before final completion, perform exactly these six cumulative review passes over the finished Task 1 artifact:

1. Accuracy & Fundamental Correction
2. Completeness & Gap Analysis
3. Structure & Architecture
4. Adversarial / Critical Review
5. Usability & Goal Fit
6. Final Synthesis, Regression Check & Polish

Specific adversarial questions:
- Did any QDento/Clinica source code or asset accidentally enter the new repo?
- Did project generation introduce an undeclared third-party dependency?
- Is the iOS deployment target exactly 18.0?
- Is `periodontal-root` actually verified by UI test?
- Are source-authority/provenance copies traceable to the frozen Plan 01 branch/contract?
- Was any periodontal feature implemented prematurely?
- Is Task 1 work isolated from main?

No seventh cycle.

## ALLOWED WRITES

Only the new `Centaurioun/periodontal-ios` repository / its Task 1 feature branch.

QDento and Clinica are read-only references for this task.

## COMMIT POLICY

Prefer small coherent commits if project creation naturally separates bootstrap/docs/test work, but Task 1 must end in one reviewable branch.

At minimum the final functional Task 1 commit history must include a clear foundation commit, e.g.:

`chore: create periodontal iOS foundation`

Do not squash away useful red/green/review evidence if the Superpowers workflow records it in commits/ledger.

## PUSH POLICY

The user has authorized creation/use of the private repository and isolated feature branches for this project.

Push:
`foundation/p02-t1-xcode-baseline`

Do NOT merge to main.
Do NOT open a PR yet.

## STOP CONDITIONS

STOP and report instead of improvising if:
- the target GitHub repo exists with unrelated/conflicting content;
- private repo creation is unavailable/unauthorized;
- installed Xcode cannot support iOS 18.0;
- no iOS Simulator/runtime capable of running the baseline is available;
- Build iOS Apps guidance conflicts materially with the approved spec/plan;
- project creation would require an unapproved third-party generator/dependency;
- tests/build fail and the failure cannot be resolved within the bounded Task 1 fix loop.

## RETURN FORMAT

Return only:

- STATUS: DONE / DONE_WITH_CONCERNS / BLOCKED
- repository
- local path
- feature branch
- starting/base commit
- final HEAD
- Xcode version
- simulator/device used
- Build iOS Apps skills actually used
- sub-agent roles/models actually used
- files/artifacts created
- build result
- unit-test result
- UI-test result
- simulator launch result
- review verdict
- concerns/blockers
- verification summary
- any controller rulings made
