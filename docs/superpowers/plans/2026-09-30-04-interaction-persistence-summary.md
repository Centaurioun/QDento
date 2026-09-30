# Interactive Parity, Persistence, and Summary Shell Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Turn the static periodontal chart into a touch-usable parity demo with live measurement updates, QDento-like findings, local save/reopen, and a replaceable summary/risk presentation boundary.

**Architecture:** Feature editing mutates the canonical exam model; geometry derives from the model; persistence sits behind a protocol; descriptive metrics/classification/risk presentation are separated. Independent finding controls may be implemented in parallel after editing interfaces are frozen.

**Tech Stack:** Swift, SwiftUI, Observation, Swift Testing, XCTest/XCUITest, Codable JSON file persistence, Xcode/iOS Simulator.

**Spec:** `docs/superpowers/specs/2026-09-30-qdento-periodontal-ios-design.md`

## Global Constraints

- REQUIRED PLUGIN: **`[@Build iOS Apps](plugin://build-ios-apps@openai-curated-remote)`**.
- Use `swiftui-ui-patterns`, `swiftui-view-refactor`.
- After a runnable interactive screen exists, use `ios-debugger-agent` and `ios-simulator-browser` for verification.
- No backend/cloud/authentication.
- Persistence implementation is demo-local Codable JSON behind a protocol.
- BOP, FMBS, FMPS remain distinct.
- PD, CAL, and GM are separately editable according to the frozen Plan 01 transition table.
- Attached gingiva is editable where applicable; recession is derived/read-only.
- Four wedges keep neutral IDs.
- Do not implement final Clinica Stage/Grade/risk clinical engine.
- Summary/risk UI must consume protocols/results, not legacy formulas directly.
- Subagents use GPT-6 Luna only.

## Review Focus

1. Editing one site must not mutate a neighboring site due to index/order bugs.
2. GM edits must update derived CAL without double-applying the sign adapter.
3. Save/reopen must preserve every finding, including null/not-assessed and four-wedge states.
4. Expanded hit targets must not cause neighboring wedge/site taps to toggle the wrong control.
5. Summary values must update from model changes without coupling the chart to a future clinical engine.

---

### Task 1: Editable examination store and measurement editor

**Files:**
- Create: `PeriodontalIOS/Features/PeriodontalChart/PeriodontalExamModel.swift`
- Create: `PeriodontalIOS/Behavior/MeasurementEditingRules.swift`
- Create: `PeriodontalIOS/Features/PeriodontalChart/MeasurementRowView.swift`
- Create: `PeriodontalIOS/Features/PeriodontalChart/SurfaceSupplementRowView.swift`
- Test: `PeriodontalIOSTests/Behavior/MeasurementEditingRulesTests.swift`
- UI Test: `PeriodontalIOSUITests/MeasurementEditingFlowTests.swift`

**Interfaces:**
- Produces:
  - `@Observable final class PeriodontalExamModel`
  - edit methods keyed by `ToothID` + `PeriodontalSite`, not packed indices.

- [ ] **Step 1: Write failing editing-rule tests from the frozen transition table**
  - Direct PD edit affects target site only and recomputes/preserves dependent values exactly as specified.
  - Direct CAL edit is supported and affects target site only.
  - Direct clinical GM edit is supported and maps correctly to QDento-compatible display semantics.
  - Derived CAL = PD + clinical GM whenever the selected transition mode says CAL is derived.
  - nil/not-assessed semantics are preserved.
  - QDento legacy range/cap behavior is reproduced only where Plan 01 classifies it PARITY_REQUIRED.

- [ ] **Step 2: Implement editing rules**

- [ ] **Step 3: Bind focused measurement row view**
  - Keep the numeric-input control separate from domain editing rules so the touch-entry UI can change later without changing clinical semantics.

- [ ] **Step 4: Bind attached-gingiva / derived-recession surface row**
  - Attached gingiva is editable only on applicable surfaces.
  - Recession is displayed from the domain-derived value and is not directly editable.

- [ ] **Step 5: Write UI test**
  - Change one site value.
  - Assert corresponding contour accessibility value/state changes.
  - Assert adjacent site remains unchanged.

- [ ] **Step 6: Build/run simulator and commit**

### Task 2: BOP interaction

**Execution:** Parallel with Tasks 3–4 after Task 1 model interface is frozen.

**Files:**
- Modify: `BOPMarkerView.swift`
- Test: `PeriodontalIOSTests/Behavior/BOPInteractionTests.swift`
- UI Test: `PeriodontalIOSUITests/BOPInteractionFlowTests.swift`

**Interfaces:**
- Consumes: exam model site edit API.
- Produces: six-site BOP toggle UI.

- [ ] **Step 1: Write failing site-target test**
- [ ] **Step 2: Implement tap/hit-area behavior**
- [ ] **Step 3: Verify blood-drop-style active marker**
- [ ] **Step 4: Run unit/UI tests**
- [ ] **Step 5: Commit**

### Task 3: FMPS/FMBS four-wedge interaction

**Execution:** Parallel with Tasks 2 and 4.

**Files:**
- Modify: `FullMouthWedgeView.swift`
- Create: `PeriodontalIOS/Features/PeriodontalChart/FullMouthScoreControl.swift`
- Test: `PeriodontalIOSTests/Behavior/FullMouthScoreInteractionTests.swift`
- UI Test: `PeriodontalIOSUITests/FullMouthScoreFlowTests.swift`

**Interfaces:**
- Consumes: neutral wedge IDs + exam model.
- Produces: separate FMPS and FMBS toggle APIs.

- [ ] **Step 1: Write failing four-wedge independence tests**
  - Toggling left does not toggle up/right/down.
  - FMPS state does not toggle FMBS state.

- [ ] **Step 2: Implement enlarged native hit areas while retaining compact visible geometry**

- [ ] **Step 3: Add accessibility IDs**
  - `fmps-<FDI>-left`, etc.
  - `fmbs-<FDI>-left`, etc.

- [ ] **Step 4: Run UI test tapping each wedge**

- [ ] **Step 5: Commit**

### Task 4: Mobility, furcation, and tooth-position state

**Execution:** Parallel with Tasks 2–3.

**Files:**
- Create: `PeriodontalIOS/Features/PeriodontalChart/MobilityControl.swift`
- Create: `PeriodontalIOS/Features/PeriodontalChart/FurcationControl.swift`
- Create: `PeriodontalIOS/Features/PeriodontalChart/ToothStateView.swift`
- Test: `PeriodontalIOSTests/Behavior/ToothStateInteractionTests.swift`

**Interfaces:**
- Produces: explicit mobility/furcation/tooth-state editing without QDento lifecycle bugs.

- [ ] **Step 1: Write failing mobility 0–3/nil tests**
- [ ] **Step 2: Write furcation applicable/not-assessed tests**
- [ ] **Step 3: Implement controls**
- [ ] **Step 4: Verify implant/missing state cannot accept invalid natural-tooth edits**
- [ ] **Step 5: Commit**

### Task 5: Descriptive metrics and summary/risk presentation contracts

**Execution:** Can run in parallel with Task 6 after Tasks 1–4 model interfaces are frozen.

**Files:**
- Create: `PeriodontalIOS/Summary/PeriodontalMetrics.swift`
- Create: `PeriodontalIOS/Summary/PeriodontalSummaryProviding.swift`
- Create: `PeriodontalIOS/Summary/ParitySummaryProvider.swift`
- Create: `PeriodontalIOS/Features/PeriodontalChart/RiskSummaryView.swift`
- Test: `PeriodontalIOSTests/Summary/PeriodontalMetricsTests.swift`

**Interfaces:**
- Produces:
  - descriptive metric result types;
  - replaceable summary provider protocol;
  - presentation model separate from future clinical classification engine.

- [ ] **Step 1: Write failing assessed-site metric tests**
  - BOP percentage uses assessed six-site BOP denominator.
  - Do not equate QDento legacy HI with Clinica plaque percentage.

- [ ] **Step 2: Implement descriptive metrics**

- [ ] **Step 3: Define summary provider protocol**
  - No Stage/Grade implementation in parity provider unless explicitly fixture-driven.

- [ ] **Step 4: Implement QDento-inspired presentation shell**

- [ ] **Step 5: Run tests/build and commit**

### Task 6: Local exam persistence

**Execution:** Parallel with Task 5.

**Files:**
- Create: `PeriodontalIOS/Persistence/PeriodontalExamStore.swift`
- Create: `PeriodontalIOS/Persistence/JSONPeriodontalExamStore.swift`
- Create: `PeriodontalIOS/Persistence/ExamStorageLocation.swift`
- Test: `PeriodontalIOSTests/Persistence/JSONPeriodontalExamStoreTests.swift`

**Interfaces:**
- Produces:
  - `protocol PeriodontalExamStore`
  - actor-backed JSON implementation.

- [ ] **Step 1: Write failing save/load round-trip test**
  - Include PD/GM/CAL, BOP, FMPS/FMBS, attached gingiva, mobility, furcation, implant, missing, nil/not-assessed.

- [ ] **Step 2: Write failing update-existing-exam test**
  - Explicit exam ID; do not copy QDento same-day ambiguity.

- [ ] **Step 3: Implement storage location abstraction**
  - Production/demo store uses Application Support.
  - Tests inject a temporary directory.

- [ ] **Step 4: Implement atomic JSON store**

- [ ] **Step 5: Run persistence tests**

- [ ] **Step 6: Commit**

### Task 7: Integrate save/reopen + live summary

**Execution:** Sequential after Tasks 2–6.

**Files:**
- Modify: app model and chart screen.
- UI Test: `PeriodontalIOSUITests/SaveReopenFlowTests.swift`

**Interfaces:**
- Consumes: exam model, persistence protocol, summary provider.
- Produces: complete interactive demo flow.

- [ ] **Step 1: Write UI round-trip scenario**
  - Directly edit PD, CAL, and GM in the reference flow.
  - Edit attached gingiva on an applicable surface and verify derived recession display.
  - Toggle BOP and one FMPS/FMBS wedge.
  - Set mobility/furcation.
  - Save.
  - Relaunch/reopen.
  - Assert all state restored.

- [ ] **Step 2: Wire persistence**

- [ ] **Step 3: Wire live descriptive summary**

- [ ] **Step 4: Run unit + UI tests**

- [ ] **Step 5: Capture simulator evidence using `ios-simulator-browser`**

- [ ] **Step 6: Commit**

## Plan acceptance gate

- Live PD/CAL/GM edits follow the frozen transition table and move only intended contour points.
- Attached gingiva persists and recession remains derived/read-only.
- BOP and wedge controls are independently tappable.
- Mobility/furcation persist.
- Save/reopen round-trip is proven by UI test.
- Summary presentation is replaceable and does not embed final clinical classification logic.
- Simulator screenshots show the intended student-facing interactions.
