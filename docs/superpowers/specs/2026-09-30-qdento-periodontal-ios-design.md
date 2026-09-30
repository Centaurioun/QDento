# QDento-Inspired Periodontal iOS Demo — Architectural Design Specification

Date: 2026-09-30

Status: Approved by the user on 2026-09-30.

This document is the formal design artifact produced after two brainstorming passes and two six-cycle refinement rounds.

It is NOT yet an implementation plan.

Implementation must not begin until this specification is reviewed and approved and a separate Superpowers implementation plan is written.

## 1. Goal

Build a native iPhone periodontal-charting demo that preserves the parts of QDento's Periodontal Measurement workspace that are visually clear and educational, while using a cleaner periodontal domain and modern clinical logic derived from Clinica where appropriate.

The first milestone is a private evaluation demo, not a production App Store release.

The first question to answer is:

> Can the QDento periodontal-chart interaction be reproduced on iPhone in a way that remains clear, touch-usable, and educational for students?

## 2. Primary user

The initial concept is especially intended for dental / periodontology students.

The interface should make periodontal measurements easier to understand by showing their visible relationship to the tooth and surrounding contour, rather than only presenting numeric entry fields.

## 3. Architectural authority split

### QDento is authoritative for

- preferred periodontal chart visual concept;
- tooth-centered presentation;
- dynamic contour behavior;
- upper/lower layout relationships;
- BOP direct visual feedback;
- compact FMPS/FMBS four-wedge control;
- mobility/furcation presentation inspiration;
- existing risk/statistics visual concept;
- parity reference screenshots and runtime behavior.

### Clinica is authoritative for

- explicit named sites: MB, B, DB, ML, L, DL;
- structured per-tooth and per-site periodontal data;
- separation of natural teeth and implants;
- explicit null / not-assessed states;
- cleaner mobility and furcation semantics;
- accepted modernized periodontal clinical lineage;
- suggestion versus clinician-confirmation separation;
- radiographic attribution safeguards;
- direct progression comparability;
- tooth-loss history/distinct-position safeguards;
- natural-tooth versus implant clinical separation.

### The new iOS application is authoritative for

- native Swift/SwiftUI state flow;
- native geometry implementation;
- native persistence;
- final touch behavior;
- accessibility;
- device-responsive presentation;
- test architecture;
- simulator parity verification;
- eventual production asset set.

## 4. Explicitly rejected architecture

Do not do a literal Qt/C++ architecture translation.

Do not recreate these concepts merely because QDento uses them:

- QWidget hierarchy;
- QObject ownership;
- signals/slots;
- generated .ui architecture;
- QGraphicsScene;
- QGraphicsItem;
- QPainter application plumbing;
- QDento tab/document shell;
- QDento database adapters;
- QDento lifecycle quirks.

Behavioral parity does not require architectural parity.

## 5. Canonical periodontal site model

Use explicit site names:

- MB
- B
- DB
- ML
- L
- DL

Group them as:

Facial/buccal:
- MB
- B
- DB

Oral:
- ML
- L
- DL

The domain model must not expose anonymous packed-array offsets as its public contract.

## 6. Gingival margin / CAL convention bridge

Clinica convention:

- positive margin = recession / apical to CEJ;
- negative margin = coronal to CEJ;
- CAL = PD + GM.

QDento source behavior:

- displayed GM = PD - CAL;
- GM editing drives CAL = PD - GM.

Therefore the native app must keep clinical domain semantics separate from QDento-compatible rendering semantics.

The rendering bridge may need a sign conversion.

This must be verified with parity fixtures before the rendering contract is frozen.

## 7. Dynamic contour requirement

The moving contour is a non-negotiable educational feature.

Source-verified QDento local geometry includes:

- baseline y = 105;
- scale = 3 display units per mm;
- GM local y = 105 + 3 * GM;
- CAL local y = 105 - 3 * CAL.

QDento applies additional transforms for:
- maxillary buccal;
- maxillary palatal;
- mandibular buccal;
- mandibular lingual.

The final native renderer must reproduce the clinically meaningful visible result, not Qt's implementation mechanism.

A pure geometry layer must be testable independently from SwiftUI.

## 8. FMPS / FMBS requirement

Preserve the QDento-style compact four-wedge visual control in the first parity demo.

Source-verified visual identities:

- left;
- up;
- right;
- down.

Source-verified properties:

- 4 findings per tooth;
- FMPS and FMBS are separate;
- both are persisted in QDento;
- FMPS selected color approximately RGB 204/228/247;
- FMBS selected color approximately RGB 255/146/148.

The QDento source does not explicitly name the anatomical meaning of the four wedge directions.

Therefore:

- do not invent QDento-author intent;
- keep neutral wedge identities until the clinical mapping is frozen;
- record a later expert-review decision for mesial/distal/facial/oral semantics;
- keep this decision independent from the six-site periodontal measurement model.

The first prototype may preserve the four-wedge control even if the final production data model also stores six-site plaque/BOP observations.

## 9. BOP requirement

BOP is a six-site finding in QDento and is distinct from FMBS.

Preserve:

- correct site association;
- direct active/inactive visual feedback;
- blood-drop-style visual concept;
- immediate summary metric update.

The exact legacy icon does not have to remain the production asset.

## 10. Mobility and furcation

Do not reproduce QDento's mobility serialization gap.

Use explicit periodontal mobility semantics from Clinica:
- 0;
- 1;
- 2;
- 3;
- null/not assessed.

Use explicit furcation semantics from Clinica.

QDento remains a compact presentation reference.

## 11. Tooth / implant separation

Natural teeth and implants must be distinct domain entities.

Implants must not be modeled as ordinary teeth with a different image.

The UI may reuse rendering components where appropriate.

Natural-tooth Stage/Grade/extent logic must not consume implant findings.

## 12. Risk/statistics architecture

Preserve the visual idea of the QDento summary/risk region where useful.

Do not build one monolithic legacy risk engine.

Separate:

1. descriptive periodontal metrics;
2. periodontitis classification;
3. risk/prognostic assessment;
4. presentation.

QDento's existing PerioStatistic code is a parity/reference source, not final clinical authority.

The later clinical engine should derive from the accepted Clinica clinical lineage and reviewed literature updates.

## 13. Legacy behavior that is NOT a parity requirement

The new app does not need to reproduce:

- QDento mobility persistence omission;
- QDento same-day new/existing exam ambiguity;
- stale tooth state after date changes;
- Qt navigation/window behavior;
- old database schema details;
- packed array persistence format.

## 14. Rule for unknown QDento semantics

Every unknown is classified into:

### Parity-critical evidence

Recover with source/runtime evidence when a wrong answer would change the visible QDento behavior we want.

### New-product decision

Where QDento is silent, define a clinically coherent new rule and label it as a new rule.

Expert review can be used before freezing clinical meaning.

### Deferred

Postpone anything that does not block the next milestone.

Unknown author intent alone is not a reason to stop the project.

## 15. Touch-first strategy

First experiment:

- preserve QDento-like compact visuals;
- enlarge native hit regions invisibly where possible;
- provide clear focus/selection feedback.

Do not redesign the entire chart pre-emptively.

If simulator/device testing shows poor touch accuracy:
- test a selected-tooth enlarged interaction mode;
- consider a temporary detail surface/popover only as a second experiment.

## 16. First-demo scope

### In scope

- one native iOS app;
- one demo examination context;
- upper and lower arch;
- six-site PD/GM/CAL with explicit parity editing behavior for PD, CAL, and GM;
- per-surface attached-gingiva entry where clinically/applicably supported by the selected QDento screen;
- derived recession surface summaries;
- dynamic contours;
- BOP;
- FMPS/FMBS compact controls;
- mobility;
- furcation;
- basic tooth/implant/missing state with a dedicated tooth-visual rendering layer;
- local save/reopen;
- summary/risk presentation shell;
- deterministic parity fixtures.

### Out of scope for first demo

- backend;
- authentication;
- cloud sync;
- appointment calendar;
- billing;
- full patient-management suite;
- Android;
- App Intents;
- Liquid Glass;
- production App Store packaging;
- final production asset replacement;
- final longitudinal history UX;
- final clinical validation of every risk formula.

## 17. Native responsibility boundaries

### Domain
Exam, tooth, implant, site, measurements, findings, mobility, furcation, identity.

### Behavior
Editing relationships, derived values, validation, state transitions.

### Geometry
QDento-faithful contour coordinates, surface orientation, tooth/site placement, BOP and wedge geometry.

### SwiftUI feature layer
Touch, focus, selection, chart layout, adaptive presentation, accessibility.

### Persistence
Native local persistence behind a narrow interface.

### Clinical interpretation
Replaceable metrics/classification/risk interfaces.

## 18. Xcode and Codex working model

Use one future iOS Git repository.

Xcode is the build/runtime authority.

Codex app/CLI is the primary orchestration surface.

Codex inside Xcode may be used for focused local work and debugging.

No separate product fork is created merely because different Codex surfaces are used.

## 19. Required iOS plugin policy

Any Codex task that touches Swift, SwiftUI, Xcode, simulator verification, rendering, performance, or memory must include:

**`[@Build iOS Apps](plugin://build-ios-apps@openai-curated-remote)`**

Use early:
- swiftui-ui-patterns;
- swiftui-view-refactor.

Use after a runnable screen exists:
- ios-debugger-agent;
- ios-simulator-browser.

Use after correctness:
- swiftui-performance-audit;
- ios-ettrace-performance when needed;
- ios-memgraph-leaks when needed.

Defer:
- ios-app-intents;
- swiftui-liquid-glass.

## 20. Multi-agent execution architecture

Do not give the entire product to one Codex instance.

Use a coordinator plus separate task-focused Codex instances.

Each instance receives a **Task Goal Packet**.

The packet contains:

- Goal
- Source documents
- Required skills/plugins
- Scope
- Allowed writes
- Forbidden writes
- Interfaces consumed
- Interfaces produced
- Verification commands/evidence
- Required artifact/report
- Stop condition

This is preferable to a bare “goal” because agents need explicit boundaries, and preferable to one massive project prompt because each context remains focused.

### Research subagents

May write only to assigned documentation paths when explicitly authorized.

### Implementation subagents

Later work in isolated branches/worktrees and write only their assigned implementation/test files.

### Review subagents

Read-only by default.

A separate remediation task is required before a reviewer can edit product code.

## 21. Parallel and sequential work rules

Parallelize only independent tasks.

Potential early parallel research:
- contour/orientation runtime closure;
- FMPS/FMBS/BOP runtime capture;
- parity-fixture definition;
- iOS architecture translation review.

Required sequential gates:
- synthesis/freeze after research;
- Xcode project scaffold before feature implementation;
- domain/interface freeze before dependent parallel modules;
- integration before whole-app parity review.

After interfaces are stable, independent feature modules may run in parallel.

## 22. Planned project phases

This is a phase map, not yet the detailed Superpowers implementation plan.

### Phase 0 — Narrow runtime parity closure

Goal:
close only the remaining QDento visual evidence needed for implementation.

Parallel lanes may investigate:
- contour/site orientation;
- FMPS/FMBS/BOP runtime alignment;
- deterministic reference fixtures.

Then one synthesizer freezes the portable rendering contract.

### Phase 1 — New iOS repository and Xcode foundation

Create the real iOS project and verification baseline.

No clinical feature breadth yet.

### Phase 2 — Canonical domain + parity fixtures

Implement named periodontal entities and pure test fixtures.

### Phase 3 — Native geometry engine

Implement pure contour/tooth/site geometry without coupling to the screen.

### Phase 4 — Static periodontal chart shell

Reproduce upper/lower arch information hierarchy with fixture data.

### Phase 5 — Core interactive parity

Add measurement editing and live contour updates.

Then add BOP, FMPS/FMBS, mobility, furcation, and basic tooth state through separately reviewable tasks.

### Phase 6 — Persistence + summary presentation

Add local save/reopen and selected QDento-like summary/risk presentation boundaries.

Where interfaces are independent, these may be developed in parallel.

### Phase 7 — QDento ↔ iOS parity acceptance

Use simulator/device evidence and deterministic fixtures.

Run multiple independent reviewers.

### Phase 8 — Architecture hardening

Refactor only after parity behavior is proven.

Run SwiftUI performance and memory checks when justified.

### Phase 9 — Clinica clinical integration

Separate later phase.

Bring accepted modern clinical logic into the already-proven mobile chart without replacing the visual contract unintentionally.

## 23. Quality gates

Every implementation task must end with:

- focused tests;
- build/run verification where applicable;
- evidence in the task report;
- independent review for nontrivial tasks;
- no silent expansion of scope.

A dependent task must not start merely because an agent says “done.”

The coordinator must verify the artifact/branch/diff and relevant tests.

## 24. Recommended planning/execution approach

Preferred:
phase-based separate Codex instances with parallel subagents inside phases where work is independent.

Rejected:
one giant implementation mission.

Rejected:
unbounded micro-agent fragmentation before contracts are stable.

This balances:
- safety;
- speed;
- context isolation;
- reviewability;
- rollback;
- interface stability.

## 25. Open decisions that do not block this spec

The following remain intentionally open:

- final anatomical names for the four FMPS/FMBS wedges;
- final portrait/landscape interaction;
- whether an enlarged selected-tooth control is necessary;
- final production tooth assets;
- exact local persistence technology;
- final deployment target;
- final risk labels/formulas;
- Android.

They should be resolved at the phase where evidence becomes available.

## 26. Evidence still required before Swift implementation

The remaining evidence closure should be narrow:

1. final visible six-site contour mapping;
2. upper/lower and facial/oral net contour orientation;
3. tooth-number-to-wedge group alignment;
4. BOP visual placement;
5. deterministic reference screenshots.

The anatomical naming of FMPS/FMBS wedges may be finalized later through clinical expert review and need not block the first neutral-wedge parity renderer.

## 27. Success condition for the first demo

A knowledgeable user should be able to enter reference periodontal data and observe the intended QDento-like relationships:

- correct tooth;
- correct site;
- correct contour response;
- clear BOP state;
- compact FMPS/FMBS interaction;
- mobility/furcation display;
- correct basic tooth/implant state;
- save and reopen;
- recognizable summary/risk presentation shell.

The implementation must achieve this without requiring Qt runtime architecture.

## 28. Six-cycle refinement outcome

### Cycle 1 — Accuracy
Separated QDento source facts, Clinica clinical/domain facts, user preferences, and new-product decisions.

### Cycle 2 — Completeness
Added unresolved-author-intent policy, touch strategy, multi-agent rights, iOS plugin policy, lifecycle exclusions, and quality gates.

### Cycle 3 — Architecture
Separated domain, behavior, geometry, SwiftUI, persistence, and clinical interpretation.

### Cycle 4 — Adversarial review
Rejected hidden assumptions that four QDento wedges are automatically six-site observations, that QDento lifecycle bugs are parity requirements, or that one giant Codex mission is safer.

### Cycle 5 — Goal fit
Kept the student-facing visual feedback and touch use case central while deferring nonessential production features.

### Cycle 6 — Final synthesis
Converted the brainstorming conclusions into a reviewable design with clear source authorities, phase boundaries, agent permissions, and explicit non-blocking unknowns.

## 29. Plan-review amendments

The approved design was reviewed again before execution. The review found three source-supported scope corrections that are consistent with the original user intent and do not change the core architecture.

### 29.1 PD / CAL / GM are all editable parity inputs

QDento source connects all three controls to presenter edit handlers:

- PD → `pdChanged`;
- CAL → `calChanged`;
- GM → `gmChanged`.

The first iOS parity demo must therefore not assume that CAL is display-only.

The implementation must freeze an explicit edit-transition contract before coding:
- what is held constant when PD changes;
- what is held constant when CAL changes;
- what is recalculated when GM changes;
- how the Clinica clinical GM sign convention maps to the QDento-style display/edit behavior;
- which legacy range/constraint behaviors are parity-critical and which become explicit new-product rules.

### 29.2 Attached gingiva and recession are part of the selected QDento workspace

QDento exposes:
- two attached-gingiva values per tooth/surface pair through the persisted `AG[64]` array;
- read-only recession values derived from the three sites on each surface.

These were missing from the first plan draft.

The first demo should include them unless the runtime evidence phase finds that a particular surface is explicitly not applicable.

The new domain must not store derived recession redundantly when it can be computed from canonical site measurements.

### 29.3 Tooth visual rendering requires its own plan boundary

The central tooth image is essential to the educational value of the QDento screen.

The implementation therefore needs:
- an explicit tooth visual descriptor/state;
- a rendering/provider abstraction;
- documented prototype asset provenance;
- separate missing/implant handling;
- the ability to replace QDento-derived prototype artwork later without rewriting domain or geometry.

### 29.4 Prototype asset/provenance rule

QDento image reuse for the private parity demo must remain explicitly marked as prototype/reference use.

The new iOS repository must keep a provenance manifest for every QDento-derived or QDento-referenced visual asset.

Future public/proprietary distribution requires a separate asset/code licensing review and, where needed, independent replacement artwork.

### 29.5 Mobile orientation remains experimental

Landscape is a useful first parity-validation environment because the QDento chart is wide, but the product must not hard-lock its final orientation during the foundation phase.

The first implementation may validate parity in landscape while keeping portrait optimization and final orientation policy as later evidence-based UX decisions.

## 30. Approval gate

This specification has been approved by the user.

After approval:
1. invoke Superpowers writing-plans;
2. create the detailed implementation plan;
3. split the plan into separate Task Goal Packets;
4. assign sequential versus parallel Codex execution;
5. only then begin implementation.
