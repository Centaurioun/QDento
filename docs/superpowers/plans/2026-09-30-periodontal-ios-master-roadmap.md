# Periodontal iOS Demo — Master Implementation Roadmap

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Deliver a native iPhone periodontal-charting demo that preserves the QDento visual/interaction behavior selected in the approved spec while using a Clinica-informed domain model and clean Swift/SwiftUI architecture.

**Architecture:** QDento is the visual/interaction reference; Clinica is the domain/clinical-reference source; the new iOS app owns native SwiftUI state, pure geometry, persistence, accessibility, and verification. Work is split into independently reviewable plans so that evidence, domain, rendering, interaction, and acceptance are frozen in dependency order rather than built by one monolithic agent.

**Tech Stack:** Swift, SwiftUI, Swift Testing for unit tests, XCTest/XCUITest for UI tests, Codable + FileManager actor for demo persistence, Xcode/iOS Simulator, Git/GitHub, QDento C++/Qt source as reference, Clinica TypeScript/React source as domain/clinical reference.

**Spec:** `docs/superpowers/specs/2026-09-30-qdento-periodontal-ios-design.md`

## Global Constraints

- First milestone is a private iPhone evaluation demo, not a production App Store release.
- QDento is authoritative for preferred periodontal chart visual/interaction behavior.
- Clinica is authoritative for explicit sites `MB/B/DB/ML/L/DL`, structured periodontal domain semantics, natural-tooth/implant separation, and the accepted modernized clinical lineage.
- Do not perform a literal Qt/C++ architecture translation.
- Do not reproduce QDento mobility serialization omission, same-day lifecycle ambiguity, stale tooth-state date behavior, old DB format, or Qt shell architecture.
- Keep QDento FMPS/FMBS four-wedge findings distinct from QDento six-site BOP and from Clinica six-site plaque unless a later explicit clinical mapping says otherwise.
- Keep canonical clinical GM semantics separate from QDento-compatible display geometry.
- Do not invent anatomical labels for QDento's left/up/right/down wedges; neutral wedge IDs are allowed until expert review.
- Any task touching Swift, SwiftUI, Xcode, simulator verification, iOS rendering, performance, or memory MUST include **`[@Build iOS Apps](plugin://build-ios-apps@openai-curated-remote)`**.
- Use `swiftui-ui-patterns` and `swiftui-view-refactor` from the start of iOS implementation.
- Do not use App Intents or Liquid Glass in the first demo.
- Subagents must use the user's permitted GPT-6 Luna route only; do not route subagents to GPT-6 Sol.
- Research agents may write only assigned documentation paths.
- Implementation agents later work in isolated branches/worktrees and only in their assigned scope.
- Reviewer agents are read-only unless a separate remediation task explicitly authorizes writes.
- No dependent phase begins on an agent's assertion alone; the coordinator verifies artifacts, diffs, tests/builds, and acceptance evidence.

## Review Focus

1. **Site/orientation mismatch:** a chart can look plausible while MB/DB or facial/oral points are swapped; geometry tests and parity fixtures must pin named-site mapping.
2. **GM sign error:** Clinica clinical GM and QDento display GM use opposite sign conventions; tests must prove the translation boundary for positive and negative margins.
3. **Finding conflation:** BOP (6 sites), FMBS (4 wedges), FMPS (4 wedges), and six-site plaque must remain distinct unless a later explicit mapping is approved.
4. **State round-trip loss:** mobility, furcation, wedge findings, BOP, tooth/implant/missing state, and measurement values must survive save/reopen.
5. **Touch/visual mismatch:** compact controls may render correctly but be hard to tap; simulator/device acceptance must verify usable hit targets without prematurely redesigning the chart.

---

## Plan decomposition

The approved spec spans multiple independent subsystems. Per Superpowers `writing-plans`, execution is split into five plans rather than one oversized transcript.

### Plan 01 — Runtime Parity Evidence Closure
Path:
`docs/superpowers/plans/2026-09-30-01-runtime-parity-evidence-closure.md`

Produces:
- source/runtime evidence for contour/site orientation;
- source/runtime evidence for FMPS/FMBS/BOP alignment;
- source/runtime evidence for PD/CAL/GM edit transitions plus attached-gingiva/recession behavior;
- tooth-rendering/asset/provenance closure;
- deterministic QDento reference fixtures;
- frozen rendering/parity contract.

No Swift product code.

### Plan 02 — iOS Foundation and Canonical Domain
Path:
`docs/superpowers/plans/2026-09-30-02-ios-foundation-domain.md`

Produces:
- new `Centaurioun/periodontal-ios` repository;
- standard native Xcode SwiftUI app;
- build/test baseline;
- explicit periodontal domain model;
- parity fixture types.

Depends on Plan 01 contract.

### Plan 03 — Geometry Engine and Static Chart
Path:
`docs/superpowers/plans/2026-09-30-03-geometry-static-chart.md`

Produces:
- pure QDento-faithful contour geometry;
- explicit GM display adapter;
- wedge and BOP geometry primitives;
- tooth visual rendering/provider layer with prototype provenance;
- static upper/lower chart shell using deterministic fixture data.

Depends on Plans 01–02.

### Plan 04 — Interactive Parity, Persistence, and Summary Shell
Path:
`docs/superpowers/plans/2026-09-30-04-interaction-persistence-summary.md`

Produces:
- PD/CAL/GM editing + live contours;
- attached-gingiva entry + derived recession;
- BOP;
- FMPS/FMBS;
- mobility/furcation;
- tooth/implant/missing-state interaction;
- local save/reopen;
- separated summary/risk presentation shell.

Depends on Plan 03 interfaces.

### Plan 05 — Parity Acceptance and Architecture Hardening
Path:
`docs/superpowers/plans/2026-09-30-05-parity-acceptance-hardening.md`

Produces:
- QDento ↔ iOS deterministic parity evidence;
- simulator/device interaction evidence;
- seven independent review results, including provenance and accessibility/touch;
- targeted refactor/performance/memory hardening only where evidence requires it;
- first-demo acceptance/freeze report.

Depends on Plan 04.

## Deliberately separate future plan

Clinica Stage/Grade/risk/longitudinal integration is NOT part of these five first-demo plans.

After first-demo parity is accepted, write a new spec/plan for:
- current accepted Clinica authority check;
- clinical-engine integration;
- literature-backed updates;
- longitudinal workflow.

## Dependency graph

```text
Plan 01 evidence closure
        ↓
Plan 02 foundation + domain
        ↓
Plan 03 geometry + static chart
        ↓
Plan 04 interactive parity + persistence + summary
        ↓
Plan 05 parity acceptance + hardening
        ↓
NEW SPEC/PLAN: Clinica clinical integration
```

Inside a plan, independent tasks may run in parallel only after their shared inputs are frozen.

## Codex orchestration model

Each numbered task is executed by a separate Codex instance using a **Task Goal Packet** derived from that task.

Task Goal Packet fields:

```text
Goal
Source documents
Required Superpowers skills
Required plugin(s)
Repository / branch / worktree
Scope
Allowed writes
Forbidden writes
Interfaces consumed
Interfaces produced
Verification required
Artifact/report required
Stop condition
```

Do not hand the entire master roadmap to one implementation agent as a single mission.

## Commit and review policy

- One coherent task = one implementation branch/worktree and one or more small commits.
- Each nontrivial task gets a fresh independent reviewer before the next dependent task begins.
- Reviewer is read-only.
- Remediation uses a new task/agent.
- After each plan, run a plan-level integration verification and freeze an evidence report.
- Merge to the main iOS branch only after plan-level acceptance.

## Execution order recommendation

Use **subagent-driven execution** because:
- tasks have strong interface dependencies;
- clinical/rendering mapping mistakes are easy to hide visually;
- independent review is worth the cost;
- the user explicitly prefers multiple isolated Codex instances.

The user has already selected the multi-agent/separate-instance execution style; no execution-mode choice is required later unless they change it.

## Master stop condition

Do not begin Clinica clinical integration until Plan 05 has accepted the first-demo parity baseline.


## Branch/worktree and online-research policy

### Branch/worktree rule

Execution happens in isolated branches/worktrees.

Research lanes:
- one branch per lane when they write documentation;
- each branch starts from the accepted planning/evidence base;
- only the assigned documentation path may be written.

Implementation tasks:
- one task branch/worktree per Codex instance;
- branch names should use the plan/task identifier, for example `feat/p02-t3-measurement-semantics`;
- a dependent task starts from the accepted commit produced by its prerequisite task or from the integration branch that already contains that accepted work.

Reviewers:
- read-only against the candidate branch;
- no hidden fixes;
- remediation receives a fresh branch/task.

### Online-research rule

Do not use general web research to decide QDento implementation facts that can be read from source/runtime evidence.

Web/current documentation is appropriate only when:
- an Apple/Xcode/Swift API needs current verification;
- Build iOS Apps guidance directs the agent to current platform documentation;
- a later clinical-literature task explicitly requires evidence outside Clinica/QDento.

Any outside-source conclusion must be labeled separately from repository/runtime evidence.

## Improved 6-Cycle Iterative Refinement applied to this plan set

### Cycle 1 — Accuracy & Fundamental Correction
- Kept QDento visual authority separate from Clinica domain/clinical authority.
- Prevented the plan from treating legacy QDento lifecycle bugs as parity requirements.
- Required the GM sign bridge to be explicit and testable.
- Kept BOP, FMBS, FMPS, and six-site plaque as distinct concepts.

### Cycle 2 — Completeness & Gap Analysis
- Added repository/Xcode foundation, provenance/source-document handoff, deterministic fixtures, persistence, summary-shell, parity acceptance, touch review, and reviewer remediation.
- Added agent write-rights, branch/worktree isolation, and online-research policy.

### Cycle 3 — Structure & Architecture
- Split the project into five independently reviewable plans instead of one oversized plan.
- Ordered domain before geometry, geometry before interaction, and interaction before parity acceptance.
- Kept later Clinica clinical integration in a separate future spec/plan.

### Cycle 4 — Adversarial / Critical Review
- Added asymmetric geometry tests to catch mirrored-but-plausible site errors.
- Added round-trip tests for mobility/furcation/wedge/BOP state.
- Added reviewer roles for domain, geometry, SwiftUI, persistence, and adversarial parity.
- Prevented performance/refactor work from running before correctness.

### Cycle 5 — Usability & Goal Fit
- Preserved the student-facing QDento interaction as the first-demo goal.
- Kept compact controls while requiring realistic touch acceptance.
- Avoided premature portrait redesign and clinical feature expansion.

### Cycle 6 — Final Synthesis, Regression Check & Polish
- Checked task dependencies and interfaces across all five plans.
- Ensured every Swift/iOS task carries the Build iOS Apps requirement.
- Ensured research-only Plan 01 does not require the iOS plugin unnecessarily.
- Ensured no unresolved placeholder tokens are required for execution; remaining unknowns are expressed as evidence gates or explicit later product decisions.


## Plan self-review against the approved spec

### Spec coverage

- Product intent / student audience → Master roadmap + Plans 03–05.
- Authority split → all plans' global constraints.
- No literal Qt translation → Plans 02–03.
- Named six-site model → Plan 02.
- GM/CAL sign bridge → Plans 02–03.
- Dynamic contour → Plan 03.
- FMPS/FMBS four-wedge control → Plans 01, 03, 04.
- Six-site BOP → Plans 01, 03, 04.
- Mobility/furcation → Plans 02, 04.
- Natural tooth / implant separation → Plan 02 + Plan 04 guards.
- Summary/risk separation → Plan 04.
- Legacy behaviors excluded → Plans 02 and 04.
- Unknown-QDento-semantics rule → Plan 01 gates.
- Touch-first strategy → Plans 04–05.
- First-demo persistence → Plan 04.
- Xcode/Codex hybrid workflow → Master + Plan 02 onward.
- Build iOS Apps plugin policy → Plans 02–05.
- Multi-agent/task-packet architecture → Master + Plan 05 reviewers.
- Parity acceptance → Plan 05.
- Later Clinica clinical integration → intentionally deferred to a new future spec/plan after first-demo acceptance.

### Step scan

Every implementation/research task has:
- one bounded deliverable;
- explicit files/artifacts;
- consumed/produced interfaces;
- checkable steps;
- verification or acceptance evidence;
- a commit/freeze point.

### Type/interface consistency

Critical cross-plan interfaces were checked:
- `ToothID`
- `PeriodontalSite`
- `PeriodontalSurface`
- `SiteMeasurement`
- `FullMouthWedge`
- `FullMouthScoreFindings`
- `PeriodontalExam`
- `PeriodontalExamStore`
- geometry contracts from Plan 01/03
- summary-provider boundary.

Later Task Goal Packets must copy these names exactly from the accepted plan revision.

### Review Focus coverage

The five master Review Focus risks are covered by:
1. site/orientation → Plan 03 asymmetric geometry tests;
2. GM sign → Plan 03 display-adapter tests;
3. finding conflation → Plan 02 type tests + Plan 04 interaction tests;
4. round-trip loss → Plan 04 JSON + UI round-trip tests;
5. touch mismatch → Plan 05 touch-usability acceptance.

### Proportion

The approved spec is intentionally implemented by five smaller plans. No individual plan attempts to transcribe the entire application. Function bodies are omitted except where the spec fixes an exact formula/contract.


## Independent plan review — revision 2

A fresh pre-execution review was performed after the first plan set was written.

### Material corrections

1. **PD/CAL/GM editing parity added**
   - QDento source proves all three values are editable.
   - Plan 01 now freezes the exact transition rules before Swift implementation.
   - Plan 04 must test direct PD, CAL, and GM editing separately.

2. **Attached gingiva + recession restored to scope**
   - QDento persists `AG[64]`.
   - Recession is a read-only derived surface summary.
   - The original plan omitted both even though they are visible in the selected workspace.

3. **Tooth rendering/asset layer added**
   - The original plan described tooth states but did not give the central tooth visuals their own rendering/provenance task.
   - Plan 01 now closes source/asset evidence; Plan 03 implements a replaceable native tooth-visual layer.

4. **Orientation lock softened**
   - Landscape is the first parity-validation environment, not a permanent product lock.
   - The Xcode project must not prematurely hard-disable portrait unless later usability evidence justifies it.

5. **Review fan-out increased**
   - Final review expands from five to seven independent reviewers:
     domain, geometry, SwiftUI, persistence, parity-adversarial, accessibility/touch, and provenance/licensing.

### Six-cycle re-review

#### Cycle 1 — Accuracy & Fundamental Correction
- Re-checked QDento source instead of trusting the previous plan summary.
- Corrected the false simplification that CAL could be treated as derived/display-only.
- Restored source-visible AG/recession behavior.

#### Cycle 2 — Completeness & Gap Analysis
- Added tooth visual rendering/provenance.
- Added AG/recession to persistence and parity fixtures.
- Added direct edit semantics for PD/CAL/GM.

#### Cycle 3 — Structure & Architecture
- Kept edit semantics in Behavior rather than SwiftUI.
- Kept tooth visual state/provider separate from periodontal geometry and domain.
- Kept derived recession out of redundant persistence.

#### Cycle 4 — Adversarial / Critical Review
- Challenged the landscape-first choice and prevented it from becoming an irreversible orientation lock.
- Challenged visual parity plans that could succeed without a real tooth-rendering layer.
- Challenged the assumption that Clinica-style CAL derivation alone reproduces QDento interaction.

#### Cycle 5 — Usability & Goal Fit
- Preserved compact QDento-style controls.
- Kept the central tooth image and live numeric/contour relationship as the student-facing priority.
- Deferred final touch-entry optimization until an actual iPhone prototype can be tested.

#### Cycle 6 — Final Synthesis / Regression Check
- Confirmed the hybrid authority model is unchanged.
- Confirmed no final Stage/Grade/risk engine was accidentally pulled into the first demo.
- Confirmed the new tasks close omissions rather than expanding into unrelated product scope.
