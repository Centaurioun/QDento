# Geometry Engine and Static Chart Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Implement the pure periodontal geometry engine and a static QDento-faithful SwiftUI chart shell driven by parity fixtures.

**Architecture:** Geometry is a pure module with no SwiftUI dependency; views consume geometry outputs. The QDento display sign/transform behavior is adapted explicitly from the canonical Clinica-style domain rather than hidden in view code.

**Tech Stack:** Swift, SwiftUI Canvas/Shape selected by implementation evidence, Swift Testing, Xcode/iOS Simulator.

**Spec:** `docs/superpowers/specs/2026-09-30-qdento-periodontal-ios-design.md`

## Global Constraints

- REQUIRED PLUGIN: **`[@Build iOS Apps](plugin://build-ios-apps@openai-curated-remote)`**.
- Use `swiftui-ui-patterns` and `swiftui-view-refactor`.
- Geometry must be unit-testable without launching SwiftUI.
- Canonical domain GM sign must remain Clinica-style.
- QDento display adaptation occurs in a named adapter/geometry function.
- Do not use QGraphics concepts in API naming.
- Do not copy the Clinica SVG renderer as the parity renderer.
- Neutral wedge geometry remains left/up/right/down.
- Subagents use GPT-6 Luna only.

## Initial mobile layout decision for parity work

For the first static/parity shell:
- use one arch at a time, matching the QDento mental model;
- validate parity first in landscape because the reference chart is wide, but do not hard-lock the app to landscape during foundation work;
- preserve clinically useful tooth/control scale rather than squeezing all 16 teeth into an unreadable width;
- use horizontal scrolling when the logical arch width exceeds the viewport;
- do not add pinch-to-zoom as part of this plan;
- portrait optimization and final orientation policy remain later evidence-based UX decisions.

## Review Focus

1. Positive/negative GM conversion may invert contour movement.
2. Maxillary palatal and mandibular lingual transforms may mirror the wrong site order.
3. Tooth-type widths can shift interproximal points away from the intended tooth gaps.
4. A static chart can visually resemble QDento while binding geometry to the wrong FDI.
5. SwiftUI redraw performance should not be optimized before correctness, but view boundaries must avoid one giant body.

---

## Target files

```text
PeriodontalIOS/
  Geometry/
    ChartSurface.swift
    QDentoDisplayAdapter.swift
    ContourGeometry.swift
    ToothGeometry.swift
    FullMouthWedgeGeometry.swift
    BOPGeometry.swift
  Rendering/
    ToothVisualDescriptor.swift
    ToothVisualProviding.swift
    PrototypeToothVisualProvider.swift
  Features/PeriodontalChart/
    PeriodontalChartScreen.swift
    ArchChartView.swift
    ToothChartColumn.swift
    PeriodontalContourView.swift
    ToothGraphicView.swift
    FullMouthWedgeView.swift
    BOPMarkerView.swift
PeriodontalIOSTests/
  Geometry/
    QDentoDisplayAdapterTests.swift
    ContourGeometryTests.swift
    WedgeGeometryTests.swift
    BOPGeometryTests.swift
PeriodontalIOSUITests/
  StaticChartSnapshotFlowTests.swift
```

### Task 1: QDento display adapter

**Files:**
- Create: `PeriodontalIOS/Geometry/QDentoDisplayAdapter.swift`
- Test: `PeriodontalIOSTests/Geometry/QDentoDisplayAdapterTests.swift`

**Interfaces:**
- Produces:
  - `static func qdentoDisplayGM(fromClinicalGM mm: Int) -> Int`
  - chart-surface orientation helpers defined from Plan 01 contract.

- [ ] **Step 1: Verify the Plan 01 contract contains an explicit clinical-GM → QDento-display-GM rule**
  - If it does not, STOP: Plan 01 was not ready for Plan 03.

- [ ] **Step 2: Write failing sign-bridge tests**
  - Use the exact mapping frozen in the Plan 01 contract; for the currently expected inversion, +2 clinical GM maps to -2 QDento-style display GM.
  - -2 maps to +2.
  - 0 maps to 0.

- [ ] **Step 3: Run and confirm failure**

- [ ] **Step 4: Implement minimal adapter from frozen contract**

- [ ] **Step 5: Run tests; expected PASS**

- [ ] **Step 6: Commit**

### Task 2: Pure contour geometry

**Files:**
- Create: `PeriodontalIOS/Geometry/ChartSurface.swift`
- Create: `PeriodontalIOS/Geometry/ToothGeometry.swift`
- Create: `PeriodontalIOS/Geometry/ContourGeometry.swift`
- Test: `PeriodontalIOSTests/Geometry/ContourGeometryTests.swift`

**Interfaces:**
- Produces:
  - `struct ContourPoint: Equatable`
  - `struct ContourPath: Equatable`
  - `func contour(for:surface:layout:) -> ContourPath`

- [ ] **Step 1: Write baseline/scale failing tests**
  - QDento local baseline = 105.
  - QDento local scale = 3 per mm.

- [ ] **Step 2: Write three-site x-position tests from Plan 01 contract**

- [ ] **Step 3: Write arch/surface orientation tests**
  - One test per maxillary facial, maxillary oral, mandibular facial, mandibular oral.
  - Use asymmetric fixture so mirrored mappings cannot accidentally pass.

- [ ] **Step 4: Run tests and confirm failure**

- [ ] **Step 5: Implement pure geometry**

- [ ] **Step 6: Run focused tests; expected PASS**

- [ ] **Step 7: Commit**

### Task 3: Four-wedge and BOP geometry primitives

**Execution:** May run in parallel with Task 2 after Task 1 and the Plan 01 contract are frozen.

**Files:**
- Create: `PeriodontalIOS/Geometry/FullMouthWedgeGeometry.swift`
- Create: `PeriodontalIOS/Geometry/BOPGeometry.swift`
- Test: `PeriodontalIOSTests/Geometry/WedgeGeometryTests.swift`
- Test: `PeriodontalIOSTests/Geometry/BOPGeometryTests.swift`

**Interfaces:**
- Produces:
  - wedge paths for left/up/right/down;
  - site anchor points for BOP markers.

- [ ] **Step 1: Write four-wedge geometry tests**
  - All four paths meet at center.
  - Each path occupies expected side of rectangle.
  - No anatomical labels appear in geometry API.

- [ ] **Step 2: Write BOP anchor tests from Plan 01 mapping**

- [ ] **Step 3: Implement primitives**

- [ ] **Step 4: Run tests; expected PASS**

- [ ] **Step 5: Commit**

### Task 4: Tooth visual rendering/provider layer

**Execution:** May run in parallel with Tasks 2–3 after Plan 01 contract and Plan 02 tooth-state types are frozen.

**Files:**
- Create: `PeriodontalIOS/Rendering/ToothVisualDescriptor.swift`
- Create: `PeriodontalIOS/Rendering/ToothVisualProviding.swift`
- Create: `PeriodontalIOS/Rendering/PrototypeToothVisualProvider.swift`
- Create: `PeriodontalIOS/Features/PeriodontalChart/ToothGraphicView.swift`
- Test: `PeriodontalIOSTests/Rendering/ToothVisualProviderTests.swift`
- Create/Update: `docs/provenance/QDENTO_REFERENCE.md`

**Interfaces:**
- Consumes: Plan 01 tooth visual/asset contract + Plan 02 chart-position state.
- Produces: replaceable visual descriptor/provider independent of QDento raster filenames.

- [ ] **Step 1: Write failing visual-state mapping tests**
  - Natural/present, missing, and implant fixture states produce distinct descriptors.
  - FDI/tooth type mapping matches the frozen parity contract.

- [ ] **Step 2: Implement descriptor/provider protocol**
  - No Qt concepts in the public API.
  - Raster/prototype resource choice remains behind the provider.

- [ ] **Step 3: Add only the minimum approved prototype visual assets needed for the first fixtures**
  - Record source/provenance for every copied or transformed file.
  - Do not import unrelated QDento treatment layers.

- [ ] **Step 4: Implement `ToothGraphicView`**

- [ ] **Step 5: Run rendering tests/build**

- [ ] **Step 6: Commit**

### Task 5: Static arch/chart shell

**Execution:** Sequential after Tasks 2–4.

**Files:**
- Create: `PeriodontalIOS/Features/PeriodontalChart/PeriodontalChartScreen.swift`
- Create: `PeriodontalIOS/Features/PeriodontalChart/ArchChartView.swift`
- Create: `PeriodontalIOS/Features/PeriodontalChart/ToothChartColumn.swift`
- Create: `PeriodontalIOS/Features/PeriodontalChart/PeriodontalContourView.swift`
- Modify: `PeriodontalIOS/App/AppModel.swift`
- Modify: root view file created in Plan 02.

**Interfaces:**
- Consumes: fixture catalog + pure geometry + tooth visual provider.
- Produces: read-only chart screen.

- [ ] **Step 1: Add static fixture selector in app model for DEBUG/demo only**

- [ ] **Step 2: Build chart as focused subviews**
  - No business/geometry calculations in `body`.
  - Upper/lower arch structure follows approved visual contract.
  - One arch is visible at a time.
  - Include tooth graphic, six-site rows, attached-gingiva row where applicable, and derived recession row.
  - Use a horizontally scrollable logical chart width when needed; landscape is the first parity-validation orientation but not a permanent lock.

- [ ] **Step 3: Add accessibility identifiers**
  - `arch-upper`, `arch-lower`, `tooth-<FDI>`, `contour-<surface>`.

- [ ] **Step 4: Build/run in simulator**
  - Expected: fixture data renders both arches without interaction.

- [ ] **Step 5: Capture simulator screenshots for two fixtures**
  - healthy baseline;
  - asymmetric contour.

- [ ] **Step 6: Commit**

### Task 6: Static wedge and BOP views

**Execution:** May run in parallel with Task 5 only if shared chart interfaces were frozen in a prior commit; otherwise sequential.

**Files:**
- Create: `PeriodontalIOS/Features/PeriodontalChart/FullMouthWedgeView.swift`
- Create: `PeriodontalIOS/Features/PeriodontalChart/BOPMarkerView.swift`

**Interfaces:**
- Consumes: geometry primitives; fixture state.
- Produces: read-only visual controls.

- [ ] **Step 1: Render selected/unselected FMPS and FMBS colors**

- [ ] **Step 2: Render BOP marker at site anchor**

- [ ] **Step 3: Verify in simulator against Plan 01 screenshots**

- [ ] **Step 4: Commit**

## Plan acceptance gate

- All geometry tests pass.
- Asymmetric orientation fixtures pass.
- Static chart builds/runs.
- QDento/Clinica sign translation is explicit and tested.
- Views contain no QDento packed-index business logic.
- Tooth visuals are replaceable behind the provider abstraction and all prototype assets have provenance entries.
- Attached-gingiva/recession rows appear according to the frozen contract.
- Simulator screenshots show correct tooth/site/contour relationships before interaction work starts.
