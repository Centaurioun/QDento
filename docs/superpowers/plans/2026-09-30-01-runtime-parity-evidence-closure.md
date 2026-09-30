# Runtime Parity Evidence Closure Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Close the narrow QDento visual/orientation evidence needed to implement the iOS renderer without repeating the broad QDento audit.

**Architecture:** Run five independent evidence lanes in parallel against QDento source/runtime/screenshots, then a synthesizer freezes a rendering/parity contract. No QDento application code or Clinica code is modified.

**Tech Stack:** QDento C++/Qt source, available local QDento runtime/screenshot evidence, Markdown, Git/GitHub.

**Spec:** `docs/superpowers/specs/2026-09-30-qdento-periodontal-ios-design.md`

## Global Constraints

- QDento is read-only.
- Clinica is read-only.
- No Swift/iOS product code.
- Do not repeat the full dependency audit.
- Do not invent anatomical names for four FMPS/FMBS wedges.
- Neutral wedge IDs are `left/up/right/down`.
- If runtime execution is unavailable, distinguish source-verified, screenshot-observed, user-observed, and unresolved evidence.
- Research subagents may write only their assigned `docs/periodontal-ios/research/runtime/` file.
- Subagents use GPT-6 Luna only.
- Public web research is not a substitute for repository/runtime evidence.

## Review Focus

1. Named-site ordering could be mirrored by quadrant while still looking plausible.
2. QDento local contour coordinates could be confused with final transformed screen coordinates.
3. Lower-arch tooth order could make wedge groups appear aligned while binding to the wrong FDI tooth.
4. User screenshots could show a state without proving the source index that generated it.
5. Clinical wedge naming must not be inferred from geometry alone.

---

### Task 1: Contour and six-site runtime mapping

**Execution:** Independent research Codex instance; may run in parallel with Tasks 2–3.

**Files:**
- Create: `docs/periodontal-ios/research/runtime/A-contour-site-orientation.md`

**Interfaces:**
- Consumes: approved spec; QDento `PerioView.cpp/.h`, `PerioChartItem.cpp`, `PerioScene.cpp`; Clinica named-site order only as semantic reference.
- Produces: source/runtime mapping table for 4 chart positions and 6 named sites, plus explicit unresolved list.

- [ ] **Step 1: Record source mapping for all 192 measurement indices**
  - Trace `chartIndex[index].position` and `chartIndex[index].index`.
  - Record maxillary buccal/palatal and mandibular buccal/lingual construction order.
  - Verification: every measurement index range used by the four surfaces is accounted for.

- [ ] **Step 2: Record pure QDento local geometry**
  - Capture baseline `105`, coefficient `3`, x-spacing rules, and tooth-type widths from source.
  - Verification: cite exact file/symbol locations.

- [ ] **Step 3: Capture sentinel visual evidence**
  - Prefer one tooth per arch with distinguishable values such as 1/4/8 across the three sites on each surface.
  - If local QDento runtime is usable, capture screenshots directly.
  - If not, use existing screenshots/user-supplied evidence and clearly mark limitations.
  - Verification: each visual claim states evidence class.

- [ ] **Step 4: Produce mapping table**
  - Columns: FDI/tooth index, domain site, QDento measurement index, chart position, local chart index, final visible left/middle/right position, evidence status.
  - Expected: no clinically named mapping marked verified without source/runtime support.

- [ ] **Step 6: Commit evidence**
  ```bash
  git add docs/periodontal-ios/research/runtime/A-contour-site-orientation.md
  git commit -m "docs: map QDento contour site orientation"
  ```

### Task 2: FMPS/FMBS/BOP alignment

**Execution:** Independent research Codex instance; may run in parallel with Tasks 1 and 3.

**Files:**
- Create: `docs/periodontal-ios/research/runtime/B-full-mouth-findings.md`

**Interfaces:**
- Consumes: QDento `PerioGraphicsButton.cpp/.h`, `PerioView.cpp`, `PerioPresenter.cpp`, `PerioStatus.h`, `Parser.cpp`.
- Produces: tooth-to-wedge alignment, BOP placement evidence, persistence/cardinality table.

- [ ] **Step 1: Verify four-wedge source geometry**
  - Record `index % 4`: 0 left, 1 up, 2 right, 3 down.
  - Record bounding rectangle and selected colors.
  - Verification: source citations for each constant.

- [ ] **Step 2: Map 128 wedge indices to tooth groups**
  - Upper 0–63 and lower 64–127.
  - Resolve tooth-number/group alignment from actual display order and source.
  - Verification: 32 tooth groups accounted for with four indices each.

- [ ] **Step 3: Verify BOP placement**
  - Map six-site BOP controls to the same named-site table used by Task 1 where evidence permits.
  - Verify icon/state and presenter update flow.
  - Verification: BOP remains distinct from FMBS.

- [ ] **Step 4: Freeze QDento parity-summary update semantics for BOP/FMBS/FMPS**
  - Record the exact source formula/denominator for the visible BOP and FMBS statistics.
  - Record the exact legacy FMPS/HI meaning, including whether it counts true or false values.
  - Record how disabled/missing teeth affect denominators.
  - Mark these formulas PARITY_LEGACY, not final clinical authority.

- [ ] **Step 5: Capture runtime/screenshot cases**
  - One FMPS wedge, one FMBS wedge, and one BOP site active on a clearly identified tooth.
  - Verification: screenshot label includes FDI, control type, source index when known.

- [ ] **Step 5: Commit evidence**
  ```bash
  git add docs/periodontal-ios/research/runtime/B-full-mouth-findings.md
  git commit -m "docs: map QDento full-mouth finding controls"
  ```

### Task 3: Deterministic QDento parity fixtures

**Execution:** Independent research Codex instance; may run in parallel with Tasks 1–2.

**Files:**
- Create: `docs/periodontal-ios/research/runtime/C-parity-fixtures.md`
- Create: `docs/periodontal-ios/research/runtime/fixtures/README.md`

**Interfaces:**
- Consumes: approved spec; known QDento measurement semantics; screenshots/runtime available.
- Produces: fixture definitions later mirrored in Swift tests/UI.

- [ ] **Step 1: Define eight fixtures**
  - healthy baseline;
  - asymmetric 3-site contour;
  - recession/GM sign case;
  - direct CAL-edit transition case;
  - attached-gingiva/recession surface case;
  - BOP case;
  - FMPS/FMBS wedge case;
  - missing/implant tooth-visual case.

- [ ] **Step 2: Give each fixture explicit input values**
  - Tooth/FDI, site values, findings, tooth state, expected visible relationships.
  - Do not include final clinical risk outputs unless source behavior is explicitly observed.

- [ ] **Step 3: Define screenshot capture protocol**
  - Required crop/context, file naming, arch visibility, zoom, value table.
  - Verification: another agent can reproduce the fixture from the document alone.

- [ ] **Step 4: Record evidence status**
  - For each expected result use SOURCE_VERIFIED / RUNTIME_VERIFIED / SCREENSHOT_OBSERVED / USER_OBSERVED / UNRESOLVED.

- [ ] **Step 5: Commit fixtures**
  ```bash
  git add docs/periodontal-ios/research/runtime/C-parity-fixtures.md docs/periodontal-ios/research/runtime/fixtures/README.md
  git commit -m "docs: define QDento parity fixtures"
  ```

### Task 4: PD/CAL/GM editing plus attached-gingiva/recession semantics

**Execution:** Independent research Codex instance; may run in parallel with Tasks 1–3 and 5.

**Files:**
- Create: `docs/periodontal-ios/research/runtime/D-measurement-edit-semantics.md`

**Interfaces:**
- Consumes: QDento `PerioPresenter.cpp`, `PerioView.cpp/.ui`, `PerioStatus.h`, `Parser.cpp`; Clinica site-measurement sign convention as semantic reference.
- Produces: explicit transition/range table for PD, CAL, GM, attached gingiva, and derived recession.

- [ ] **Step 1: Record source edit handlers and ranges**
  - PD: QDento range and `pdChanged` behavior.
  - CAL: QDento range and `calChanged` behavior.
  - GM: QDento range and `gmChanged` behavior.
  - Record any special constraint/cap logic.

- [ ] **Step 2: Derive the clinical-sign translation table**
  - Separate canonical Clinica-style clinical GM from QDento display GM.
  - For each direct edit path, state which values are held constant and which are recomputed.

- [ ] **Step 3: Record attached-gingiva semantics**
  - Map persisted `AG[64]` to tooth/surface positions.
  - Record applicability/disabled surfaces.

- [ ] **Step 4: Record recession semantics**
  - Verify the source derivation across three sites on a surface.
  - State whether recession is persisted or derived.

- [ ] **Step 5: Define parity-critical versus legacy-only edge behavior**
  - Do not silently copy odd legacy constraints.
  - Classify each as PARITY_REQUIRED / NEW_PRODUCT_RULE / DEFERRED.

- [ ] **Step 6: Commit evidence**
  ```bash
  git add docs/periodontal-ios/research/runtime/D-measurement-edit-semantics.md
  git commit -m "docs: map QDento periodontal edit semantics"
  ```

### Task 5: Tooth rendering, assets, and provenance closure

**Execution:** Independent research Codex instance; may run in parallel with Tasks 1–4.

**Files:**
- Create: `docs/periodontal-ios/research/runtime/E-tooth-rendering-assets.md`

**Interfaces:**
- Consumes: QDento `ToothPainter.cpp`, `SpriteSheets.cpp/.h`, `PerioScene.cpp`, QRC/resource manifests, existing QDento provenance notes.
- Produces: minimum visual-state contract and prototype asset/provenance manifest for Plan 03.

- [ ] **Step 1: Map the tooth visual composition used by the periodontal screen**
  - Base tooth/root layers.
  - Periodontal layer.
  - Missing/extracted state.
  - Implant state.
  - Any state that the first demo fixture actually needs.

- [ ] **Step 2: Map tooth-index → sprite/type/width behavior**
  - Record permanent tooth type mapping and any arch/quadrant mirroring.

- [ ] **Step 3: Identify the minimum prototype asset set**
  - Do not copy unrelated QDento dental-treatment layers just because the generic painter can render them.

- [ ] **Step 4: Record provenance status per candidate asset**
  - Source path.
  - QDento license context.
  - Whether direct prototype reuse is needed.
  - Whether production replacement is expected.

- [ ] **Step 5: Define replaceable tooth-visual contract**
  - State information required by iOS rendering independent of raster filenames.

- [ ] **Step 6: Commit evidence**
  ```bash
  git add docs/periodontal-ios/research/runtime/E-tooth-rendering-assets.md
  git commit -m "docs: close QDento tooth rendering asset evidence"
  ```

### Task 6: Synthesize Rendering/Parity Contract v1

**Execution:** Sequential coordinator/synthesizer after Tasks 1–5; read all five reports and independently spot-check source.

**Files:**
- Create: `docs/periodontal-ios/contracts/2026-09-30-rendering-parity-contract-v1.md`
- Modify: `docs/periodontal-ios/README.md`

**Interfaces:**
- Consumes: Tasks 1–5 evidence.
- Produces: frozen input contract for Plans 02–05.

- [ ] **Step 1: Reconcile contradictions**
  - Never resolve by majority vote; use strongest evidence.
  - Keep unresolved items explicit.

- [ ] **Step 2: Freeze the minimum renderer contract**
  - named-site semantics;
  - QDento index mapping where verified;
  - GM display adapter requirements;
  - local contour equations;
  - final orientation rules where verified;
  - four-wedge neutral IDs and geometry;
  - BOP visual placement;
  - QDento parity-summary formulas/update semantics for BOP/FMBS/FMPS;
  - PD/CAL/GM edit transition table;
  - attached-gingiva applicability and persisted mapping;
  - derived recession rule;
  - tooth visual state/provider requirements and prototype asset provenance;
  - fixture IDs.

- [ ] **Step 3: Classify remaining unknowns**
  - BLOCKS_GEOMETRY;
  - BLOCKS_PARITY_TEST;
  - NONBLOCKING_PRODUCT_DECISION;
  - DEFERRED.

- [ ] **Step 4: Verify no QDento/Clinica source edits**
  - Run/record `git status` in each source repo.
  - Expected: no research-induced source changes.

- [ ] **Step 5: Commit contract**
  ```bash
  git add docs/periodontal-ios/contracts/2026-09-30-rendering-parity-contract-v1.md docs/periodontal-ios/README.md
  git commit -m "docs: freeze periodontal rendering parity contract"
  ```

## Plan acceptance gate

Proceed to Plan 02 only when:
- contract file exists;
- no BLOCKS_GEOMETRY item remains;
- any unresolved wedge anatomical naming is explicitly NONBLOCKING_PRODUCT_DECISION;
- deterministic fixtures are reproducible from written inputs;
- PD/CAL/GM edit transitions are explicit enough to implement without agent guesswork;
- prototype tooth-visual provenance is documented.
