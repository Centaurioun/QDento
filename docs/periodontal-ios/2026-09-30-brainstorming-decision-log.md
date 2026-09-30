# QDento Periodontal iOS Brainstorming Decision Log

Date: 2026-09-30

Status: Brainstorming record, not implementation plan.

Purpose: Preserve the full reasoning trail behind the future iOS direction so later Codex sessions do not have to reconstruct the discussion from chat history.

This document deliberately records:
- decisions we currently prefer;
- earlier ideas we rejected or revised;
- why those decisions changed;
- which repository is authoritative for which concern;
- known contradictions between QDento and Clinica that must be bridged explicitly;
- what remains unresolved.

## 1. Product intent

The long-term goal is no longer to create a reduced desktop copy of QDento.

The intended first product is a private iPhone demo that reproduces the useful educational qualities of QDento's Periodontal Measurement workspace.

The most important audience for the first concept is dental / periodontology students. The value of the interface is not only faster data entry. Its educational value comes from making measurements visibly change the periodontal chart around the teeth.

The first demo should answer a practical question:

Can the QDento periodontal charting experience become a clear and useful iPhone experience while preserving the visual relationships that make the original screen easy to understand?

Possible later directions:
- production iOS application;
- Android implementation;
- integration of selected Clinica clinical/domain behavior;
- longitudinal examination support;
- contemporary Stage/Grade and decision support;
- production-grade persistence and distribution.

Those later goals must not distort the first parity-oriented experiment.

## 2. What must not be lost from QDento

The current QDento Periodontal Measurement workspace remains the visual and interaction reference.

The following elements are considered important to preserve or reproduce closely in the first demo:

- upper and lower arch organization;
- individual tooth representation;
- FDI tooth identity and ordering;
- six periodontal measurement positions per tooth;
- visible PD, CAL, and GM relationships;
- dynamic periodontal contours that move when values change;
- the fact that the middle site is visually aligned with the body of the tooth and the adjacent sites represent interproximal positions;
- BOP shown directly on the chart, including the visible blood-drop style indicator when active;
- mobility presentation;
- furcation presentation;
- FMPS and FMBS controls;
- the compact four-part triangular / wedge visual control used for FMPS and FMBS;
- automatic changes in the summary indices when FMPS, FMBS, or BOP states change;
- tooth-status rendering such as missing teeth and implants where relevant;
- the lower Periodontal Risk Assessment / statistics presentation;
- immediate visual feedback rather than forcing a student to interpret a separate table.

The reason for retaining these elements is functional and educational, not nostalgia for Qt.

## 3. Initial idea: literal Swift conversion

Early in the discussion, the tentative idea was to convert the QDento periodontal code more or less directly to Swift, then evaluate whether it works on iPhone.

This was revised.

### Why literal translation was rejected as the preferred architecture

QDento uses implementation mechanisms such as:
- Qt widgets;
- QObject ownership;
- signals and slots;
- generated UI classes;
- QGraphicsScene and QGraphicsItem;
- QPainter-specific rendering;
- QDento tab/document shell;
- QDento database adapters.

These mechanisms do not have clean one-to-one native SwiftUI equivalents.

A literal translation would risk reproducing old framework structure rather than reproducing the user-visible behavior.

It could also preserve historical defects or awkward data structures such as packed arrays purely because they exist in QDento.

### Current decision

Do not translate QDento architecture literally.

Instead:
- preserve observable behavior;
- preserve clinically meaningful visual geometry;
- preserve the selected interaction model;
- reimplement those results using native Swift / SwiftUI concepts.

The user-facing experience can remain QDento-like even when the internal architecture is completely different.

This is the most important distinction in the project:
visual and behavioral parity does not require architectural parity.

## 4. Source-of-truth split

A single repository should not be treated as authoritative for everything.

### QDento authority

QDento is the primary source for:
- the visual concept of the periodontal chart;
- the relationship between numeric values and live contours;
- tooth presentation;
- upper/lower layout behavior;
- FMPS/FMBS compact visual controls;
- BOP visual presentation;
- the existing risk/statistics visual presentation;
- the current appearance and interaction flow that the user prefers.

### Clinica authority

Clinica is the preferred source for:
- explicit periodontal site naming;
- structured per-tooth / per-site domain modeling;
- separation of natural teeth and implants;
- explicit mobility and furcation data;
- null / not-assessed semantics;
- newer accepted Stage/Grade/extent clinical logic;
- radiographic attribution rules;
- direct-progression comparability rules;
- distinct-position tooth-loss semantics;
- same-FDI reoccupation safeguards;
- clinician suggestion / confirmation separation;
- modernized clinical validation and persistence concepts.

### Future iOS authority

The new iOS application should own:
- native SwiftUI state flow;
- native rendering architecture;
- local persistence implementation;
- final mobile touch interaction;
- final accessibility behavior;
- final device-responsive layout;
- tests and simulator parity fixtures;
- eventual distributable asset set.

The iOS application should not inherit framework-specific constraints simply because either QDento or Clinica uses them.

## 5. Clinica repository reference used during brainstorming

Repository:
Centaurioun/clinica-dental

The current main branch observed during this brainstorming pass was:
da006f311780037d5fdd1f6d17beaba1184503ca

The important newer clinical lineage inspected was:
integration/pux2-r2-clinical-modernized

At the time of inspection, this branch was 16 commits ahead of main.

The integration report on that branch records:
- canonical main base da006f311780037d5fdd1f6d17beaba1184503ca;
- Clinical source HEAD 49236d941a2118d0233c4d44a86dbc9dc5a66006;
- frozen reviewed Clinical implementation 4a9b1133d9b2c5756b73aa5ac84394502f807209;
- integrated product tree / fix HEAD f201035b561f440e5e1b35a22c7dbe03de6e84e7.

These identifiers are historical anchors for this brainstorming record, not permanent promises that the branch will remain the final authority forever. A later implementation phase should perform a fresh authority check.

## 6. Six-site periodontal model: decision changed

The earlier QDento forensic report could verify six slots per tooth but could not safely assign all six clinical names from QDento source alone.

Clinica removes that ambiguity for the new application's domain model.

Clinica explicitly defines the canonical site order as:

MB, B, DB, ML, L, DL

Clinica also groups them as:

Facial / buccal:
- MB
- B
- DB

Oral / lingual:
- ML
- L
- DL

The chart UI and screenshots are consistent with this explicit model.

### Current decision

The future iOS domain model should use explicit named sites rather than anonymous packed-array offsets.

The new app should not expose or depend on QDento-style pd[192], cal[192], or similar packed storage as its canonical public model.

### Upper / lower visual layout

Clinica's chart deliberately displays the two surfaces around the tooth anatomy:

Upper arch:
- facial sites above the tooth row;
- oral sites below the tooth row.

Lower arch:
- oral sites above the tooth row;
- facial sites below the tooth row.

This mirrors the screenshots supplied during brainstorming and is a useful explicit semantic reference.

## 7. Important sign-convention conflict: GM and CAL

This is a critical bridge issue and must not be silently normalized away.

Clinica's canonical comment states:
- positive gingival margin means recession / apical to CEJ;
- negative means coronal to CEJ;
- CAL = PD + GM.

Therefore:
Clinica GM = CAL - PD.

The QDento analysis reported:
- GM is displayed as PD - CAL.

Therefore:
QDento GM = PD - CAL = -(Clinica GM), if both CAL and PD represent the same underlying measurements.

### Consequence

The future iOS domain can follow the Clinica sign convention, but a QDento-faithful renderer may need an explicit translation layer.

Conceptually:
QDento-style GM for rendering = negative of Clinica-style GM.

This mapping should be verified against QDento runtime screenshots before it is frozen as a rendering contract.

This is exactly why the project should separate domain semantics from visual rendering semantics.

## 8. Contour rendering: Clinica renderer is not the visual authority

This decision became much clearer after inspecting Clinica.

Clinica's current web contour implementation uses a simple display mapping:
- baseline 20;
- 1 display pixel per millimeter;
- path geometry generated as a compact SVG.

QDento's source analysis found a different mapping:
- local baseline 105;
- scale factor 3;
- GM y approximately 105 + 3 * GM;
- CAL y approximately 105 - 3 * CAL before the surrounding Qt transforms are applied.

The user explicitly prefers the QDento appearance and found the Clinica version less satisfactory.

### Current decision

Do not port Clinica's current contour renderer to iOS as the visual reference.

Use:
- Clinica for named data semantics;
- QDento for visual contour behavior;
- a new native geometry engine for iOS.

The remaining work is to verify QDento's net upper/lower and facial/oral transform behavior at runtime so the native renderer reproduces the final visible result rather than merely copying intermediate Qt coordinates.

## 9. FMPS and FMBS: decision changed after visual review

An earlier idea was to discard QDento's four-sector FMPS/FMBS representation and use Clinica's six-site plaque / BOP representation exclusively.

That idea was revised after closer review of the QDento screen.

### Why the earlier idea was rejected

The QDento four-part triangular control is compact and touch-friendly.

For a student-facing mobile interface it can be easier to understand and operate than a wide grid of tiny site-level controls.

The screenshot also demonstrates useful immediate feedback:
- tapping one part changes its visual fill;
- FMPS/FMBS summary values update;
- the student can see the distribution directly in the chart.

### Current decision

Preserve the QDento-style compact FMPS/FMBS four-part visual control in the first parity-oriented iOS experiment.

Do not discard it simply because Clinica stores plaque / bleeding at six named sites.

However, do not force an invented equivalence between:
- QDento's four FMPS/FMBS slots;
- Clinica's six named periodontal sites.

The exact anatomical meaning of the four QDento wedge positions remains a specific runtime / source-mapping question.

Until that is verified, treat the four visual positions as distinct display findings rather than falsely labeling them with clinical names.

The final model may choose to maintain both:
- a clinically explicit site-level finding model;
- a compact QDento-like display control model.

That bridge must be designed deliberately rather than inferred.

## 10. FMPS percentage semantics also require care

The QDento forensic analysis indicated that its FMPS-derived hygiene index is based on the percentage of false FMPS entries rather than simply the percentage of positive plaque findings.

The supplied screenshot showing a high FMPS value after only a small number of selected sectors is consistent with the idea that the displayed index is not necessarily a direct positive-plaque percentage.

### Current decision

Do not assume QDento FMPS percentage and Clinica plaque percentage mean the same thing.

For parity:
- reproduce QDento's visible behavior only when it is intentionally desired.

For future clinical/statistical semantics:
- define the metric explicitly;
- avoid misleading labels;
- prefer the clinically intended denominator and sign convention.

## 11. BOP: preserve the QDento visual behavior

A newly noticed QDento behavior is the blood-drop-like marker that appears when BOP is selected.

This was judged useful, clear, and educational.

### Current decision

The first iOS parity demo should preserve a direct visual BOP indicator around the relevant site/tooth location.

The mobile implementation does not have to copy the exact raster/icon implementation.

It should preserve:
- site association;
- immediate appearance/disappearance;
- clear active state;
- summary metric updates.

Clinica can remain the source for clean boolean / null semantics.

## 12. Touch interaction: mobile changes may be invisible

The iPhone interface will be touch-first.

QDento's controls can remain visually compact while the native iOS hit targets are larger than the visible shapes.

For example:
- an FMPS triangle can remain visually small;
- its effective touch target can be expanded;
- BOP markers can remain compact;
- tooth/site selection can have invisible padding;
- focused state can provide stronger visual feedback.

### Current decision

Do not redesign the chart solely to satisfy touch size.

First try:
- QDento-faithful appearance;
- larger invisible hit areas;
- native selection feedback.

If real simulator testing shows that touch accuracy is poor, then test a second interaction mode such as a selected-tooth enlarged control.

Do not pre-commit to popovers or a new mobile UI before the parity prototype is evaluated.

## 13. Mobility: do not copy QDento's persistence gap

QDento displays mobility, but the forensic analysis found that mobility is sourced from date-specific dental status and is not included in the periodontal serializer.

Clinica models mobility cleanly as part of a periodontal tooth record:
0, 1, 2, 3, or null.

### Current decision

The future application should not reproduce the QDento persistence omission as a requirement.

Use an explicit periodontal mobility concept in the new domain model.

The exact persistence ownership can later be finalized, but the new app should not intentionally inherit the legacy disconnect.

## 14. Furcation

Clinica provides an explicit furcation model with:
- site identity;
- grades 0 through 3;
- null for applicable but not assessed;
- not-applicable for sites without an applicable root division.

QDento's compact visual presentation remains useful as a UI reference.

### Current decision

Prefer Clinica semantics for the domain, while preserving a compact QDento-like presentation where helpful.

## 15. Natural tooth and implant separation

This is one of the strongest Clinica improvements and should be retained.

Clinica separately models:
- natural tooth periodontal sites;
- implant sites;
- implant mucosal margin;
- implant mobility;
- implant descriptive metrics.

Implant measurements are excluded from natural-tooth periodontitis Stage/Grade/extent logic.

### Current decision

The iOS application should keep natural-tooth and implant data as separate domain entities even if they share chart presentation components.

Do not treat an implant as merely a tooth with a different sprite.

## 16. Modern clinical calculation lineage

QDento's PerioStatistic behavior remains useful for understanding the old risk/statistics panel but should not automatically become the final clinical authority.

The accepted Clinica R2 clinical lineage contains more deliberate safeguards.

Important retained rules observed in the integration branch include:

### Radiographic attribution
Radiographic findings are only eligible for classification logic when attribution is explicitly consistent with periodontitis.

Raw findings remain stored separately.

### Direct progression
Observed loss and actual observation interval are retained.

No silent annualization or extrapolation.

Direct evidence requires:
- an eligible source;
- explicit comparability;
- an approximately five-year interval, bounded to 4.99 through 5.01 years.

### Stage IV
Eligible Stage-IV complexity or five distinct eligible periodontitis-related tooth-loss positions can raise the stage without imposing a Stage-III severity floor.

### Distinct tooth-loss positions
Duplicate history rows are preserved, but clinical thresholds count distinct FDI positions.

### Same-FDI reoccupation
A natural tooth returning to a position with prior tooth-loss history cannot be assumed automatically; explicit confirmation is required.

### Natural / implant separation
Implant findings do not enter natural-tooth Stage/Grade/extent calculations.

### Suggestion versus clinician confirmation
Algorithm suggestion and clinician-confirmed classification remain separate concepts.

### Current decision

When the future iOS project reaches clinical decision support, start from the accepted Clinica clinical lineage rather than porting QDento's higher-level diagnosis/risk code.

Before implementation, perform a fresh authority check to confirm whether an even newer accepted Clinica branch supersedes this R2 branch.

## 17. Risk Assessment: preserve presentation, replaceable calculation

The user likes the lower QDento risk/statistics panel and radar/hexagon presentation.

The QDento analysis identified known issues and legacy assumptions in the underlying calculations, including label/calculation mismatches and other edge cases.

### Current decision

Preserve the QDento-style risk/statistics presentation as a visual reference.

Architect it behind a replaceable boundary.

Conceptually:
clinical/risk inputs -> calculator -> result model -> presentation.

This allows:
- initial visual parity;
- later replacement with Clinica/literature-based rules;
- stable UI while clinical logic evolves.

Do not entangle the chart view directly with a single legacy risk formula.

## 18. QDento assets and licensing

QDento is GPL-licensed.

The existing Clinica project already preserves explicit QDento provenance for limited presentation reuse.

Individual image authorship / provenance is not fully established simply by the repository license.

### Current decision

For early private research/demo work:
- QDento assets can remain reference material;
- provenance must remain documented.

For future proprietary or public distribution:
- do not assume the current raster assets can be retained unchanged;
- prefer independently recreated / licensed tooth graphics where necessary;
- keep the geometry and behavior independent from a specific sprite sheet.

Asset replacement must not force a rewrite of the clinical/domain model.

## 19. QDento reduction is no longer the main path

A previous line of thinking was to remove unrelated QDento modules until only Periodontal Measurement remained.

That analysis was useful for discovering dependencies.

It is no longer the preferred product path.

### Why this changed

The actual goal is a native mobile application, not a smaller Qt desktop program.

Removing Calendar, Finance, patient-management screens, and other unrelated QDento modules would consume time without materially improving the iOS implementation.

### Current decision

Keep QDento as a reference implementation.

Do not spend the primary project effort slimming QDento.

Use its source and runtime to answer specific parity questions.

## 20. New iOS architecture direction

The future iOS application should separate at least these responsibilities:

### Domain
- exam;
- tooth / implant identity;
- six named periodontal sites;
- PD;
- GM;
- CAL;
- BOP;
- plaque;
- suppuration;
- mobility;
- furcation;
- presence / loss state;
- risk/evidence inputs.

### Behavior
- editing relationships;
- derived CAL behavior;
- validation;
- state transitions;
- clinical calculation interfaces.

### Geometry / rendering
- QDento-faithful site positioning;
- contour point generation;
- arch transforms;
- tooth alignment;
- BOP marker placement;
- FMPS/FMBS visual sectors;
- missing / implant display state.

### SwiftUI feature layer
- chart layout;
- focus/selection;
- touch interaction;
- accessibility;
- adaptive screen behavior.

### Persistence
- native save/reopen contract;
- not QDento's existing database implementation.

### Clinical interpretation
- replaceable calculator interfaces;
- later Clinica/literature-backed logic.

## 21. Xcode and Codex workflow

The project should use a hybrid development workflow rather than choosing one interface exclusively.

### Xcode
Xcode should become the authoritative build/runtime environment for:
- real iOS project structure;
- simulator;
- SwiftUI previews where useful;
- compiler diagnostics;
- signing;
- Instruments;
- device/runtime validation.

### Codex app / CLI
Use as the main engineering/orchestration surface for:
- repository-wide work;
- QDento + Clinica cross-repository research;
- multi-agent tasks;
- branch/worktree coordination;
- broad refactors;
- code review;
- test orchestration;
- documentation and evidence synthesis.

### Codex inside Xcode
Use for:
- local Swift edits;
- focused view/debug iterations;
- compiler-error assistance;
- simulator-adjacent changes.

### Current decision

There is one codebase and one Git repository for the future iOS project.

Using Codex from multiple surfaces does not mean creating multiple products.

## 22. Build iOS Apps skills

The OpenAI Build iOS Apps skill package was reviewed.

### Use early
- swiftui-ui-patterns
- swiftui-view-refactor

These support:
- correct state ownership;
- small focused views;
- native SwiftUI data flow;
- avoiding giant chart files;
- keeping business/geometry logic out of view body.

### Use once a real app exists
- ios-debugger-agent
- ios-simulator-browser

These are useful for:
- real simulator checks;
- screenshot evidence;
- interactive parity validation.

### Use after correctness exists
- swiftui-performance-audit
- ios-ettrace-performance
- ios-memgraph-leaks

These should diagnose actual performance / memory evidence rather than drive premature optimization.

### Defer
- ios-app-intents
- swiftui-liquid-glass

They do not serve the first parity objective.

## 23. Parity versus mobile optimization

The first iOS milestone should not redesign the chart into a different periodontal product.

### First
Prove visual and behavioral parity:
- correct tooth/site association;
- correct contour movement;
- correct BOP/FMPS/FMBS state;
- correct indices;
- usable touch behavior;
- recognizable QDento information hierarchy.

### Later
Optimize specifically for phone usage:
- portrait alternatives;
- selected-tooth detail;
- enlarged controls;
- paging;
- landscape strategy;
- responsive density.

This sequencing prevents a mobile redesign from obscuring whether the QDento educational concept itself works.

## 24. Remaining QDento runtime evidence

After the Clinica review, most earlier conceptual blockers are no longer blockers for the new domain model.

The most important remaining QDento work is narrow:

1. Verify the final visible mapping of MB/B/DB and ML/L/DL onto QDento's contour points.
2. Verify upper/lower and facial/oral net transforms at runtime.
3. Verify the exact four FMPS/FMBS wedge positions and their intended meaning.
4. Record BOP marker positioning and toggling behavior.
5. Capture deterministic reference screenshots with sentinel values.
6. Record enough visual or geometric measurements to make the Swift renderer testable.

This is a rendering/orientation parity closure, not another broad repository audit.

## 25. Things deliberately deferred

These do not need to block the first parity demo:

- final App Store strategy;
- Android;
- backend;
- authentication;
- cloud sync;
- production patient-management suite;
- full longitudinal history UI;
- final proprietary asset replacement;
- final clinical validation of every risk formula;
- advanced exports;
- App Intents;
- Liquid Glass;
- exhaustive performance optimization.

## 26. Corrections to earlier research notes

The earlier portable-contract report contained a four-slot FMPS/FMBS table but accidentally listed an i*4+4 entry even though a four-slot tooth range should only use offsets 0 through 3.

This is a documentation error and must not be propagated into a future contract.

Any frozen portable contract should correct the four-slot indexing table before it becomes implementation authority.

## 27. Current concise architecture statement

The current preferred combination is:

QDento visual and interaction reference
+
Clinica structured periodontal domain and accepted clinical lineage
+
new native Swift / SwiftUI architecture
+
new native persistence
+
replaceable clinical/risk engine
+
runtime parity tests against QDento.

This is the current brainstorming direction.

It is not yet an approved implementation plan.

## 28. Why this direction is preferred

It preserves the exact part the user values:
- the clear educational QDento chart;
- the moving periodontal contours;
- the tooth-centered visual mapping;
- compact BOP/FMPS/FMBS feedback;
- risk/statistics visualization.

At the same time it avoids carrying forward:
- Qt framework architecture;
- anonymous packed arrays as the public domain;
- known QDento persistence gaps;
- outdated higher-level clinical assumptions;
- unnecessary desktop application shell dependencies.

It also keeps future changes affordable:
- Clinica clinical logic can evolve;
- assets can be replaced;
- risk formulas can be updated;
- mobile UX can be optimized later;
- the core visual contract can remain stable.

## 29. Six-cycle refinement summary for this document

This record was refined cumulatively through six review passes.

Cycle 1 — Accuracy and fundamental correction
- separated verified source facts from design choices;
- preserved the QDento/Clinica authority split;
- included the important GM sign-convention conflict.

Cycle 2 — Completeness and gap analysis
- added FMPS/FMBS decision history;
- added BOP visual behavior;
- added Xcode/Codex workflow;
- added provenance and deferred-scope boundaries.

Cycle 3 — Structure and architecture
- separated domain, behavior, rendering, UI, persistence, and clinical interpretation;
- removed accidental coupling between Clinica's renderer and Clinica's domain.

Cycle 4 — Adversarial review
- challenged the idea that six-site plaque and four-sector FMPS are automatically equivalent;
- retained the four-sector mapping as unresolved rather than inventing labels;
- retained runtime contour orientation as an evidence gap.

Cycle 5 — Usability and goal fit
- centered the student / touch-first use case;
- preserved QDento's compact visual controls;
- deferred redesign until parity is evaluated.

Cycle 6 — Final synthesis and regression check
- checked current decisions against earlier reversals;
- retained important rejected approaches and the reasons they were rejected;
- kept this document explicitly at brainstorming status rather than silently turning it into an implementation plan.


## 30. Second brainstorming refinement

A second independent brainstorming / six-cycle pass was completed against the written notes and QDento source.

Canonical review record:
`docs/periodontal-ios/2026-09-30-second-brainstorming-review.md`

Key refinements incorporated into the project direction:

- QDento FMPS/FMBS four-part geometry is now source-verified as index modulo four = left, up, right, down.
- The visual wedge order is resolved, but anatomical labeling of those four wedges remains unresolved and must not be invented.
- FMPS and FMBS are independently serialized QDento periodontal fields, not presentation-only artifacts.
- QDento BOP is a separate six-site persisted finding and must not be conflated with four-part FMBS.
- Mobility persistence mismatch is directly verified: mobility exists in `PerioStatus` but is omitted by the periodontal serializer.
- QDento date changes do not reload date-specific tooth status; this legacy behavior should not become an iOS parity requirement.
- QDento same-day new/existing exam behavior is treated as legacy lifecycle behavior rather than a new-product requirement.
- The user-observed central/interproximal contour relationship remains a required visual goal, but exact anatomical labels still require runtime sentinel verification.
- The remaining QDento research scope is now limited to runtime visual/orientation mapping and deterministic parity fixtures rather than another broad audit.

This second pass does not alter the core hybrid-authority decision:
QDento for preferred visual/interaction behavior, Clinica for structured periodontal domain and accepted clinical logic, and a new native Swift/SwiftUI implementation for the future mobile product.
