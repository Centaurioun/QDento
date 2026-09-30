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
- Direct PD/CAL/GM editing semantics come from the accepted Plan 01 transition contract; do not invent them in SwiftUI.
- Attached gingiva is stored per applicable tooth surface; recession is derived and must not be redundantly persisted.
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
    PeriodontalSurface.swift
    PeriodontalSite.swift
    SiteMeasurement.swift
    ToothSurfaceMeasurement.swift
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
- Create: `docs/reference/qdento-ios-design-spec.md`
- Create: `docs/contracts/rendering-parity-contract-v1.md`
- Create: `docs/provenance/QDENTO_REFERENCE.md`
- Create: `docs/provenance/CLINICA_REFERENCE.md`

**Interfaces:**
- Consumes: approved spec; Plan 01 rendering contract.
- Produces: buildable/testable repository used by every later task.

- [ ] **Step 1: Preflight repository/Xcode capability and create the standard SwiftUI app**
  - Verify installed Xcode and iOS Simulator availability.
  - Verify the execution environment can create/use the intended GitHub remote.
  - If remote-repository creation is unavailable, create the local Git repository/Xcode project in a dedicated periodontal-ios directory, STOP before pushing anywhere, and ask the coordinator/user to create or authorize the remote. Never place the app inside the QDento or Clinica repositories as a fallback.
  - Product module: `PeriodontalIOS`.
  - Unit-test target: `PeriodontalIOSTests`.
  - UI-test target: `PeriodontalIOSUITests`.

- [ ] **Step 2: Set deployment target**
  - Set iOS 18.0.
  - If unavailable in installed Xcode, STOP and report rather than silently changing the target.

- [ ] **Step 3: Copy the accepted planning/evidence artifacts into the new repo**
  - Copy the approved design spec verbatim into `docs/reference/qdento-ios-design-spec.md` with source repository/branch/commit in the header.
  - Copy the accepted Plan 01 contract verbatim into `docs/contracts/rendering-parity-contract-v1.md`.
  - Record QDento and Clinica repository/ref provenance in their dedicated files.

- [ ] **Step 4: Add launch smoke test**
  - `LaunchTests.testAppLaunches()` launches the app and asserts root accessibility identifier `periodontal-root` exists.

- [ ] **Step 5: Build/test**
  - Run the Build iOS Apps recommended simulator build/test workflow.
  - Expected: app builds; unit tests pass; UI launch test passes.

- [ ] **Step 6: Commit**
  ```bash
  git add .
  git commit -m "chore: create periodontal iOS foundation"
  ```

### Task 2: Tooth identity and named-site domain

**Files:**
- Create: `PeriodontalIOS/Domain/ToothID.swift`
- Create: `PeriodontalIOS/Domain/PeriodontalSurface.swift`
- Create: `PeriodontalIOS/Domain/PeriodontalSite.swift`
- Test: `PeriodontalIOSTests/Domain/ToothIDTests.swift`
- Test: `PeriodontalIOSTests/Domain/PeriodontalSiteTests.swift`

**Interfaces:**
- Produces:
  - `struct ToothID: Hashable, Codable, Sendable`
  - `enum PeriodontalSurface: String, Codable, Sendable { case facial, oral }`
  - `enum PeriodontalSite: String, CaseIterable, Codable, Sendable { case MB, B, DB, ML, L, DL }`
  - `var PeriodontalSite.surface: PeriodontalSurface`

- [ ] **Step 1: Write failing FDI validity/order tests**
  - Assert the exact permanent FDI set: 11–18, 21–28, 31–38, 41–48; reject invalid decade/position values.
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
- Create: `PeriodontalIOS/Domain/ToothSurfaceMeasurement.swift`
- Create: `PeriodontalIOS/Domain/Mobility.swift`
- Create: `PeriodontalIOS/Domain/Furcation.swift`
- Create: `PeriodontalIOS/Domain/FullMouthWedge.swift`
- Test: `PeriodontalIOSTests/Domain/MeasurementSemanticsTests.swift`

**Interfaces:**
- Produces:
  - `enum MeasurementSource: String, Codable, Sendable { case derived, manual }`
  - `struct AttachmentLevel: Codable, Equatable, Sendable { var valueMM: Int?; var source: MeasurementSource? }`
  - `struct SiteMeasurement: Codable, Equatable, Sendable`
  - `struct ToothSurfaceMeasurement: Codable, Equatable, Sendable { var attachedGingivaMM: Int? }`
  - `var probingDepthMM: Int?`
  - `var gingivalMarginMM: Int?`
  - `var clinicalAttachmentLevel: AttachmentLevel`
  - `var bop: Bool?`
  - `enum MobilityGrade: Int, Codable { case zero, one, two, three }`
  - `enum FullMouthWedge: String, CaseIterable, Codable, Sendable { case left, up, right, down }`
  - `struct WedgeFindings: Codable, Equatable, Sendable` with explicit nullable `left/up/right/down` fields
  - `struct FullMouthScoreFindings: Codable, Equatable, Sendable { var fmps: WedgeFindings; var fmbs: WedgeFindings }`.

- [ ] **Step 1: Write failing CAL/GM and edit-contract tests**
  - Positive clinical GM adds to PD.
  - Negative clinical GM subtracts from PD.
  - Not-assessed values remain nil.
  - Manual versus derived CAL remains distinguishable.
  - The Plan 01 transition table can represent direct PD edit, direct CAL edit, and direct GM edit without inconsistent duplicate truth.

- [ ] **Step 2: Run tests and confirm failure**

- [ ] **Step 3: Implement measurement value/source model**

- [ ] **Step 4: Write surface-supplement tests**
  - Attached gingiva can be assessed only on surfaces marked applicable by the frozen contract.
  - Recession is derived from site measurements and is not encoded as an independent persisted truth.

- [ ] **Step 5: Write finding-cardinality/type-safety tests**
  - BOP is site-level.
  - FMPS/FMBS use four `FullMouthWedge` keys.
  - Mobility accepts 0–3 or nil.

- [ ] **Step 6: Implement findings**

- [ ] **Step 7: Run tests; expected PASS**

- [ ] **Step 8: Commit**

### Task 4: Natural tooth, implant, and exam aggregate

**Files:**
- Create: `PeriodontalIOS/Domain/NaturalToothRecord.swift`
- Create: `PeriodontalIOS/Domain/ImplantRecord.swift`
- Create: `PeriodontalIOS/Domain/PeriodontalExam.swift`
- Test: `PeriodontalIOSTests/Domain/RecordSeparationTests.swift`

**Interfaces:**
- Produces:
  - `struct NaturalToothRecord` with named site measurements, facial/oral surface supplements, mobility, furcation, and FMPS/FMBS findings
  - `struct ImplantRecord` with implant-specific site measurements and implant mobility
  - `enum ChartPositionRecord { case natural(NaturalToothRecord), implant(ImplantRecord), missing(ToothID) }`
  - `struct PeriodontalExam`

- [ ] **Step 1: Write failing separation tests**
  - Natural-tooth record exposes natural six-site periodontal measurements.
  - Implant record uses separate implant measurement type/namespace.
  - Missing position has no writable natural measurement collection.

- [ ] **Step 2: Run tests and confirm failure**

- [ ] **Step 3: Implement aggregate types**

- [ ] **Step 4: Write Codable round-trip test**
  - Includes nil/not-assessed, mobility, furcation, BOP, FMPS/FMBS, attached gingiva, implant, missing position.

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
