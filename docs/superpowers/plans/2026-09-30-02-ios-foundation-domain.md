# iOS Foundation and Canonical Domain Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create the native iOS repository/Xcode baseline and implement the explicit periodontal domain model and fixture infrastructure required by the parity renderer.

**Architecture:** A standard SwiftUI app with no third-party runtime dependencies. Domain types are Codable/Equatable/Sendable where appropriate, independent of SwiftUI; QDento packed arrays are not exposed as the app's canonical model.

**Tech Stack:** Swift, SwiftUI, Swift Testing, XCTest/XCUITest, Xcode, iOS 18.0 deployment target for the first demo unless the installed Xcode cannot support that target (any change requires coordinator approval).

**Spec:** `docs/superpowers/specs/2026-09-30-qdento-periodontal-ios-design.md`

## Global Constraints

- REQUIRED PLUGIN for every task: **`[@Build iOS Apps](plugin://build-ios-apps@openai-curated-remote)`**.
- Use `swiftui-ui-patterns` and `swiftui-view-refactor`.
- New repository working name: `Centaurioun/periodontal-ios`.
- SwiftUI lifecycle; Swift language.
- No external package dependencies in this plan.
- Domain must distinguish natural teeth from implants.
- Domain site enum is exactly MB/B/DB/ML/L/DL.
- Canonical clinical GM: positive = recession/apical, negative = coronal; CAL = PD + GM.
- Four QDento parity wedges use neutral enum values left/up/right/down.
- BOP is six-site; FMBS/FMPS are four-wedge findings.
- No final clinical Stage/Grade/risk engine in this plan.
- Subagents use GPT-6 Luna only.

## Review Focus

1. A tooth/implant union must not allow implant data to enter natural-tooth measurement APIs accidentally.
2. CAL/GM derived/manual semantics must not silently overwrite clinician-entered values.
3. Wedge findings and six-site findings must be type-distinct.
4. Optional/not-assessed state must survive Codable round trips.
5. FDI identity/order must not depend on array position.

---

## Target repository structure

```text
PeriodontalIOS/
  App/
    PeriodontalIOSApp.swift
    AppModel.swift
  Domain/
    ToothID.swift
    PeriodontalSite.swift
    SiteMeasurement.swift
    Mobility.swift
    Furcation.swift
    FullMouthWedge.swift
    NaturalToothRecord.swift
    ImplantRecord.swift
    PeriodontalExam.swift
  Parity/
    ParityFixture.swift
    ParityFixtureCatalog.swift
  Features/
    PlaceholderRootView.swift
PeriodontalIOSTests/
  Domain/
    ToothIDTests.swift
    PeriodontalSiteTests.swift
    MeasurementSemanticsTests.swift
    RecordSeparationTests.swift
  Parity/
    ParityFixtureTests.swift
PeriodontalIOSUITests/
  LaunchTests.swift
docs/
  reference/
  contracts/
```

### Task 1: Create repository and Xcode verification baseline

**Execution:** Sequential; first iOS implementation task.

**Files:**
- Create repository: `Centaurioun/periodontal-ios`
- Create standard Xcode project and app/test targets.
- Create: `README.md`
- Create: `docs/reference/SOURCE-AUTHORITIES.md`

**Interfaces:**
- Consumes: approved spec; Plan 01 rendering contract.
- Produces: buildable/testable repository used by every later task.

- [ ] **Step 1: Create repo and standard SwiftUI app**
  - Product module: `PeriodontalIOS`.
  - Unit-test target: `PeriodontalIOSTests`.
  - UI-test target: `PeriodontalIOSUITests`.

- [ ] **Step 2: Set deployment target**
  - Set iOS 18.0.
  - If unavailable in installed Xcode, STOP and report rather than silently changing the target.

- [ ] **Step 3: Add launch smoke test**
  - `LaunchTests.testAppLaunches()` launches the app and asserts root accessibility identifier `periodontal-root` exists.

- [ ] **Step 4: Build/test**
  - Run the Build iOS Apps recommended simulator build/test workflow.
  - Expected: app builds; unit tests pass; UI launch test passes.

- [ ] **Step 5: Commit**
  ```bash
  git add .
  git commit -m "chore: create periodontal iOS foundation"
  ```

### Task 2: Tooth identity and named-site domain

**Files:**
- Create: `PeriodontalIOS/Domain/ToothID.swift`
- Create: `PeriodontalIOS/Domain/PeriodontalSite.swift`
- Test: `PeriodontalIOSTests/Domain/ToothIDTests.swift`
- Test: `PeriodontalIOSTests/Domain/PeriodontalSiteTests.swift`

**Interfaces:**
- Produces:
  - `struct ToothID: Hashable, Codable, Sendable`
  - `enum PeriodontalSite: String, CaseIterable, Codable, Sendable { case MB, B, DB, ML, L, DL }`
  - `PeriodontalSite.surface: .facial | .oral`

- [ ] **Step 1: Write failing FDI validity/order tests**
  - Assert valid permanent FDI values used by the chart.
  - Assert arch/quadrant helpers do not depend on array position.

- [ ] **Step 2: Run tests and confirm failure**

- [ ] **Step 3: Implement `ToothID`**

- [ ] **Step 4: Write site grouping/order tests**
  - Expected canonical order: MB, B, DB, ML, L, DL.
  - Facial: MB/B/DB.
  - Oral: ML/L/DL.

- [ ] **Step 5: Implement `PeriodontalSite`**

- [ ] **Step 6: Run focused tests; expected PASS**

- [ ] **Step 7: Commit**
  ```bash
  git add PeriodontalIOS/Domain/ToothID.swift PeriodontalIOS/Domain/PeriodontalSite.swift PeriodontalIOSTests/Domain
  git commit -m "feat: add tooth and periodontal site domain"
  ```

### Task 3: Measurement semantics and findings

**Files:**
- Create: `PeriodontalIOS/Domain/SiteMeasurement.swift`
- Create: `PeriodontalIOS/Domain/Mobility.swift`
- Create: `PeriodontalIOS/Domain/Furcation.swift`
- Create: `PeriodontalIOS/Domain/FullMouthWedge.swift`
- Test: `PeriodontalIOSTests/Domain/MeasurementSemanticsTests.swift`

**Interfaces:**
- Produces:
  - `struct SiteMeasurement`
  - `var probingDepthMM: Int?`
  - `var gingivalMarginMM: Int?`
  - `var clinicalAttachmentLevelMM: Int?`
  - `var bop: Bool?`
  - `enum MobilityGrade: Int, Codable { case zero, one, two, three }`
  - `enum FullMouthWedge: String, CaseIterable, Codable { case left, up, right, down }`
  - separate FMPS/FMBS finding containers.

- [ ] **Step 1: Write failing CAL/GM tests**
  - Positive GM adds to PD.
  - Negative GM subtracts from PD.
  - Not-assessed values remain nil.
  - Manual CAL, if supported by the struct, remains distinguishable from derived CAL.

- [ ] **Step 2: Run tests and confirm failure**

- [ ] **Step 3: Implement measurement value/source model**

- [ ] **Step 4: Write finding-cardinality/type-safety tests**
  - BOP is site-level.
  - FMPS/FMBS use four `FullMouthWedge` keys.
  - Mobility accepts 0–3 or nil.

- [ ] **Step 5: Implement findings**

- [ ] **Step 6: Run tests; expected PASS**

- [ ] **Step 7: Commit**

### Task 4: Natural tooth, implant, and exam aggregate

**Files:**
- Create: `PeriodontalIOS/Domain/NaturalToothRecord.swift`
- Create: `PeriodontalIOS/Domain/ImplantRecord.swift`
- Create: `PeriodontalIOS/Domain/PeriodontalExam.swift`
- Test: `PeriodontalIOSTests/Domain/RecordSeparationTests.swift`

**Interfaces:**
- Produces:
  - `struct NaturalToothRecord`
  - `struct ImplantRecord`
  - `enum ChartPositionRecord { case natural(NaturalToothRecord), implant(ImplantRecord), missing(ToothID) }`
  - `struct PeriodontalExam`

- [ ] **Step 1: Write failing separation tests**
  - Natural-tooth record exposes natural six-site periodontal measurements.
  - Implant record uses separate implant measurement type/namespace.
  - Missing position has no writable natural measurement collection.

- [ ] **Step 2: Run tests and confirm failure**

- [ ] **Step 3: Implement aggregate types**

- [ ] **Step 4: Write Codable round-trip test**
  - Includes nil/not-assessed, mobility, furcation, BOP, FMPS/FMBS, implant, missing position.

- [ ] **Step 5: Run tests; expected PASS**

- [ ] **Step 6: Commit**

### Task 5: Port deterministic parity fixtures into Swift

**Files:**
- Create: `PeriodontalIOS/Parity/ParityFixture.swift`
- Create: `PeriodontalIOS/Parity/ParityFixtureCatalog.swift`
- Test: `PeriodontalIOSTests/Parity/ParityFixtureTests.swift`

**Interfaces:**
- Consumes: Plan 01 fixture IDs and values.
- Produces: strongly typed fixture catalog used by geometry/UI tests.

- [ ] **Step 1: Write failing fixture presence tests**
  - Assert all Plan 01 fixture IDs exist.
  - Assert each fixture references valid FDI/site values.

- [ ] **Step 2: Implement fixture catalog**

- [ ] **Step 3: Add Codable/reproducibility test**

- [ ] **Step 4: Run full unit test target; expected PASS**

- [ ] **Step 5: Commit**

## Plan acceptance gate

- Xcode build succeeds.
- Unit tests pass.
- UI launch smoke test passes.
- Domain has no SwiftUI import.
- Wedge/BOP types are not conflated.
- Natural teeth and implants are type-distinct.
- Plan 01 fixtures exist as typed Swift data.
