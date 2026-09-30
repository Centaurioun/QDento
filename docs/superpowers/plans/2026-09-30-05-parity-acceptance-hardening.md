# Parity Acceptance and Architecture Hardening Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Prove the first iOS demo reproduces the intended QDento visual/behavioral contract, then harden only the architecture/performance areas justified by evidence.

**Architecture:** Acceptance is evidence-first: deterministic fixtures, simulator/device captures, independent reviewers, then targeted refactoring/profiling. No clinical feature expansion occurs in this plan.

**Tech Stack:** Xcode, iOS Simulator/device, Swift Testing, XCUITest, Build iOS Apps simulator/debug/performance/memory skills.

**Spec:** `docs/superpowers/specs/2026-09-30-qdento-periodontal-ios-design.md`

## Global Constraints

- REQUIRED PLUGIN: **`[@Build iOS Apps](plugin://build-ios-apps@openai-curated-remote)`**.
- Use `ios-debugger-agent` and `ios-simulator-browser`.
- Use `swiftui-performance-audit` after parity correctness.
- Use `ios-ettrace-performance` only for observed performance problems.
- Use `ios-memgraph-leaks` only for observed/suspected memory problems.
- Do not add clinical Stage/Grade/risk scope.
- Do not redesign the UI during parity acceptance.
- Reviewers are read-only.
- Subagents use GPT-6 Luna only.

## Review Focus

1. Pixel similarity must not hide a clinically wrong site mapping.
2. Asymmetric fixtures must verify both arches and both surfaces.
3. Save/reopen parity must be part of acceptance, not only live-screen parity.
4. Performance work must not change geometry/behavior without regression proof.
5. Touch acceptance must be based on realistic simulator/device scale, not enlarged desktop preview.

---

### Task 1: Build automated parity scenario suite

**Files:**
- Create: `PeriodontalIOSUITests/ParityFixtureFlowTests.swift`
- Create: `docs/parity/first-demo-parity-checklist.md`

**Interfaces:**
- Consumes: Plan 01 fixture catalog + implemented app.
- Produces: repeatable UI paths for each fixture.

- [ ] **Step 1: Add one UI flow per fixture**
- [ ] **Step 2: Assert named tooth/site states using accessibility IDs**
- [ ] **Step 3: Assert save/reopen for at least one asymmetric fixture**
- [ ] **Step 4: Run full unit/UI suite**
- [ ] **Step 5: Commit**

### Task 2: Capture QDento ↔ iOS parity evidence

**Execution:** Independent evidence agent; no source edits except docs/screenshots.

**Files:**
- Create: `docs/parity/evidence/README.md`
- Add: approved iOS simulator screenshots.
- Reference: QDento source/runtime screenshots according to provenance rules.

**Interfaces:**
- Produces: side-by-side evidence index.

- [ ] **Step 1: Capture identical fixture states**
- [ ] **Step 2: Compare tooth, site, contour direction/magnitude, BOP, wedge, tooth-state**
- [ ] **Step 3: Record differences as ACCEPTED_VARIANCE / DEFECT / UNKNOWN**
- [ ] **Step 4: Commit evidence**

### Task 3: Parallel independent reviews

**Execution:** 5 independent read-only reviewer Codex instances in parallel.

**Artifacts:**
- `docs/reviews/first-demo/domain-review.md`
- `docs/reviews/first-demo/geometry-review.md`
- `docs/reviews/first-demo/swiftui-review.md`
- `docs/reviews/first-demo/persistence-review.md`
- `docs/reviews/first-demo/parity-adversarial-review.md`

**Reviewer focus:**
- domain separation;
- site/orientation/GM geometry;
- SwiftUI state/view structure;
- persistence round-trip/lifecycle;
- adversarial parity/educational behavior.

- [ ] **Step 1: Dispatch all 5 reviewers with isolated contexts**
- [ ] **Step 2: Reviewers inspect code/tests/evidence read-only**
- [ ] **Step 3: Coordinator deduplicates findings**
- [ ] **Step 4: Classify BLOCKING / NONBLOCKING / FUTURE**

### Task 4: Remediate blocking review findings

**Execution:** New implementation agents, one per independent blocking domain; may run in parallel if files do not overlap.

**Files:** Determined by accepted review findings.

- [ ] **Step 1: Create one remediation task per root cause**
- [ ] **Step 2: Add/adjust regression test before fix**
- [ ] **Step 3: Implement minimal fix**
- [ ] **Step 4: Re-run focused + full suite**
- [ ] **Step 5: Fresh reviewer verifies remediation**
- [ ] **Step 6: Commit each remediation separately**

### Task 5: SwiftUI performance audit

**Execution:** Sequential after blocking correctness findings are closed.

**Files:**
- Create: `docs/reviews/first-demo/performance-audit.md`
- Modify product code only if audit identifies a measured/code-evident issue.

- [ ] **Step 1: Run `swiftui-performance-audit`**
- [ ] **Step 2: Identify broad invalidation, giant-view, or expensive-body issues**
- [ ] **Step 3: If no substantive issue, document and do not refactor**
- [ ] **Step 4: If issue exists, create separate remediation task with tests/measurement**
- [ ] **Step 5: Use `ios-ettrace-performance` only if a concrete slow flow remains**

### Task 6: Memory/ownership check

**Execution:** Only after functional acceptance; conditional.

**Files:**
- Create: `docs/reviews/first-demo/memory-check.md`

- [ ] **Step 1: Run repeated open/edit/save/reopen flow**
- [ ] **Step 2: If memory growth/retain suspicion exists, use `ios-memgraph-leaks`**
- [ ] **Step 3: Remediate only evidence-backed leaks**
- [ ] **Step 4: Record result**

### Task 7: Touch usability acceptance

**Files:**
- Create: `docs/parity/touch-usability-review.md`

- [ ] **Step 1: Test compact BOP and four-wedge controls at realistic iPhone scale**
- [ ] **Step 2: Record mis-tap/ambiguity observations**
- [ ] **Step 3: If controls are usable, keep parity UI unchanged**
- [ ] **Step 4: If not, create a separate bounded design experiment for selected-tooth enlargement; do not silently redesign during this plan**

### Task 8: Freeze first-demo acceptance report

**Files:**
- Create: `docs/releases/first-demo-acceptance.md`

**Interfaces:**
- Produces: formal gate for the later Clinica clinical-integration spec.

- [ ] **Step 1: Record repository/branch/HEAD**
- [ ] **Step 2: Record full build/test evidence**
- [ ] **Step 3: Record parity fixture results**
- [ ] **Step 4: Record reviewer verdicts and remaining nonblocking items**
- [ ] **Step 5: State ACCEPT / ACCEPT_WITH_NONBLOCKING_FINDINGS / NOT_ACCEPTED**
- [ ] **Step 6: Commit acceptance report**

## Plan acceptance gate

Do not start Clinica clinical integration until:
- all blocking parity defects are closed;
- full build/unit/UI verification passes;
- deterministic fixture evidence exists;
- touch usability is acceptable for the first demo or separately scoped for redesign;
- acceptance report is frozen.
