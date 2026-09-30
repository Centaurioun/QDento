# Current Direction and Open Questions for the QDento-Inspired iOS Periodontal App

Date: 2026-09-30

Status: Current brainstorming synthesis.

This document is intentionally shorter and more operational than the decision log. It describes what we currently believe, what evidence supports it, and what remains open before a real implementation plan is written.

## A. Current direction

### Product
Build a private native iPhone demo first.

Primary use case:
student-friendly periodontal charting with immediate visual feedback.

Primary success criterion:
a student changes measurements and can immediately understand the corresponding periodontal visual change around the correct tooth/site.

### Visual authority
QDento.

Preserve:
- chart information hierarchy;
- tooth-centered presentation;
- moving gingival/CAL contours;
- BOP marker behavior;
- compact FMPS/FMBS visual controls;
- mobility/furcation presentation;
- risk/statistics panel concept.

### Domain authority
Clinica, especially the accepted R2 clinical lineage, with a fresh authority check before implementation.

Use explicit named sites:
MB, B, DB, ML, L, DL.

Keep natural teeth and implants separate.

Use explicit null/not-assessed semantics.

Use explicit mobility and furcation records.

### Rendering authority
QDento behavior, not Clinica's current SVG geometry.

Clinica's current renderer is useful as a source example but does not match the desired QDento visual result.

### Native architecture
New Swift/SwiftUI implementation.

Do not recreate Qt framework architecture.

Keep:
- domain;
- behavior;
- geometry;
- SwiftUI views;
- persistence;
- clinical interpretation

as separable responsibilities.

## B. Important translation rules

### Six sites
Canonical domain order:
MB, B, DB, ML, L, DL.

Facial:
MB, B, DB.

Oral:
ML, L, DL.

### Gingival margin sign convention
Clinica:
CAL = PD + GM.
Positive GM = recession/apical to CEJ.

QDento source behavior:
GM = PD - CAL.

Therefore a QDento-style visual adapter may need to negate Clinica GM before applying QDento-like geometry.

This must be runtime-verified.

### Contours
QDento source analysis indicates:
- baseline about 105;
- 3 display units per mm;
- different GM/CAL y-direction formulas;
- surrounding Qt transforms by arch/surface.

Do not freeze final iOS geometry until runtime screenshots confirm the net visible result.

## C. FMPS / FMBS current decision

Keep the QDento compact four-part per-tooth visual control for the parity experiment.

Reasons:
- compact;
- understandable;
- touch-friendly;
- immediate feedback;
- useful for students;
- updates summary indices visibly.

Do not assume the four QDento positions are identical to Clinica's six site findings.

The exact wedge-to-anatomy mapping is still an evidence question.

Do not label it incorrectly merely to finish the model.

## D. BOP current decision

Preserve a direct visual BOP marker such as the QDento blood-drop presentation.

The exact artwork can change later, but the state should be visible at the correct site and update immediately.

## E. Mobility and furcation

Prefer Clinica domain semantics.

Do not reproduce QDento's mobility persistence disconnect.

Use the QDento presentation only as a visual reference.

## F. Risk and clinical interpretation

Keep the QDento risk/statistics presentation as a reference.

Do not treat QDento's PerioStatistic implementation as final clinical authority.

Use a replaceable calculation boundary.

Later calculation authority should come from the accepted Clinica clinical lineage plus any explicitly reviewed literature updates.

## G. Remaining evidence before implementation planning

The remaining research should be a narrow QDento runtime parity closure.

Source review has now resolved the FMPS/FMBS visual construction itself:
- four positions per tooth;
- index order left, up, right, down;
- separate FMPS and FMBS rows;
- separate persisted arrays.

What remains is the clinical/runtime interpretation and final screen alignment.

Required evidence:

1. One-to-one site-to-visible-position mapping for MB/B/DB and ML/L/DL.
2. Net upper/lower and facial/oral contour transforms.
3. Tooth-number-to-FMPS/FMBS group alignment across both arches.
4. Anatomical interpretation, if any, of the four left/up/right/down FMPS/FMBS wedges.
5. Runtime confirmation of BOP marker placement at named sites.
6. Deterministic screenshots using sentinel values.
7. Reference screenshots for at least:
   - healthy baseline;
   - asymmetric three-site contour;
   - recession / positive-negative margin case;
   - BOP case;
   - FMPS/FMBS selected-sector case;
   - missing/implant state.

This should be treated as a visual/rendering closure, not a second full QDento analysis.

## H. What no longer blocks the first domain design

The following earlier blockers can be retired for the new app:

- six-site clinical names: Clinica explicitly defines them;
- anonymous QDento six-slot storage: do not use it as the new canonical domain;
- QDento mobility serialization gap: do not reproduce it;
- uncertainty about which clinical engine to prefer: use the accepted Clinica clinical lineage rather than QDento risk/diagnostic code;
- QDento desktop shell dependencies: irrelevant to native iOS architecture.

## I. What remains intentionally undecided

These decisions should wait for evidence or a later phase:

- exact final FMPS/FMBS clinical data representation;
- whether the parity demo exposes both QDento-style FMPS/FMBS and six-site plaque simultaneously;
- final portrait versus landscape interaction strategy;
- whether selected-tooth enlargement or popovers are needed;
- final commercial asset replacement;
- final risk-assessment labels and formulas;
- exact persistence technology;
- final iOS deployment target;
- Android architecture.

## J. Xcode / Codex working model

Recommended workflow:

Codex app / CLI:
- main orchestration;
- repository-wide changes;
- multi-agent work;
- cross-repo analysis;
- review and verification.

Xcode:
- source of truth for build/runtime;
- simulator;
- previews;
- compiler and Instruments.

Codex in Xcode:
- focused local edits and debugging.

All operate on the same future iOS Git repository.

## K. Build iOS Apps skill usage

From the beginning:
- swiftui-ui-patterns;
- swiftui-view-refactor.

After an Xcode project and working screen exist:
- ios-debugger-agent;
- ios-simulator-browser.

After correctness:
- swiftui-performance-audit;
- ios-ettrace-performance when evidence calls for it;
- ios-memgraph-leaks when evidence calls for it.

Defer:
- App Intents;
- Liquid Glass.

## L. Stop condition for brainstorming

Do not start implementation merely because this document exists.

The next major gate is:
- finish the narrow QDento runtime visual parity closure;
- update/freeze the portable contract;
- review the written design specification;
- only then invoke Superpowers writing-plans.

Until that happens, this branch remains a research/brainstorming branch.


## M. Second-pass evidence refinements

The second brainstorming review tightened several earlier statements:

- FMPS/FMBS visual geometry is no longer fully unresolved. QDento source verifies left/up/right/down triangles by `index % 4`. Only anatomical naming remains unresolved.
- FMPS, FMBS, and BOP are serialized periodontal fields in QDento.
- BOP uses six site-level positions; FMBS uses four per tooth. They are distinct source concepts.
- Mobility exists in QDento `PerioStatus` but is omitted from the periodontal JSON serializer. The future app should not reproduce this gap.
- QDento's date-change behavior does not reload date-specific tooth status and therefore should not be treated as a parity requirement.
- QDento same-day record behavior is a legacy lifecycle behavior, not a design authority for the new app.
- The remaining parity closure is primarily runtime orientation/anatomical mapping plus deterministic screenshots.

Full evidence and rationale:
`docs/periodontal-ios/2026-09-30-second-brainstorming-review.md`


## N. Rule for unresolved QDento semantics

Not every undocumented QDento detail must be reverse-engineered before the new application can proceed.

Classify each unknown as:

1. **Parity-critical evidence** — must be runtime/source verified because it changes the QDento visual behavior we want to reproduce.
2. **New-product clinical/product decision** — define explicitly for the new app, then validate with a periodontist / clinical colleague where appropriate.
3. **Deferred** — postpone because it does not block the next milestone.

The four FMPS/FMBS wedges are currently:
- visually source-verified as left / up / right / down;
- not yet anatomically named from QDento source;
- eligible for a new clinically coherent mapping after expert review;
- safe to keep as neutral wedge identities in the first parity prototype.

Unknown original-author intention must not be confused with required product truth.

## O. Codex orchestration model

The project should use separate Codex instances for separate deliverables.

Preferred unit:
**Task Goal Packet**

Each packet contains:
- Goal
- Sources to read
- Required Superpowers skills
- Required iOS plugin, when applicable
- Scope
- Allowed writes
- Forbidden writes
- Acceptance evidence
- Output artifacts
- Stop condition

Use:
`[@Build iOS Apps](plugin://build-ios-apps@openai-curated-remote)`

for Swift/SwiftUI/Xcode/simulator tasks.

Do not give the whole iOS project to one Codex instance.

Use parallel subagents only for genuinely independent tasks; otherwise preserve sequential dependency gates.

Research agents may write dedicated evidence notes if explicitly assigned a documentation path.

Implementation agents later should work in isolated branches/worktrees.

Review agents remain read-only unless a remediation task gives explicit write authority.
