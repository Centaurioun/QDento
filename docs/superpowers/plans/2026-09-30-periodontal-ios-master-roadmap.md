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
- static upper/lower chart shell using deterministic fixture data.

Depends on Plans 01–02.

### Plan 04 — Interactive Parity, Persistence, and Summary Shell
Path:
`docs/superpowers/plans/2026-09-30-04-interaction-persistence-summary.md`

Produces:
- measurement editing + live contours;
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
- independent review results;
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
