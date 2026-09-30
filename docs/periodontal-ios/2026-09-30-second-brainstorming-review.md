# Second Brainstorming Review — QDento-Inspired Periodontal iOS Direction

Date: 2026-09-30

Status: Second-pass architectural brainstorming review. Not an implementation plan.

Branch:
`docs/periodontal-ios-brainstorming`

QDento source anchor reviewed:
`c5e7f00c55b442b9e6e1c2c6196598f8fac13bb1`

Purpose:
Challenge the first brainstorming synthesis against the actual QDento source and the previously inspected Clinica clinical/domain lineage. This review is intended to remove accidental certainty, close cheap source-level gaps, and narrow the evidence that truly remains before a formal design spec and implementation plan.

## 1. Review method

The first brainstorming documents were treated as hypotheses rather than authority.

The review re-checked relevant QDento source, including:

- `src/View/Graphics/PerioGraphicsButton.cpp/.h`
- `src/View/Widgets/PerioView.cpp/.h`
- `src/View/Graphics/PerioChartItem.cpp`
- `src/View/Graphics/PerioScene.cpp`
- `src/Presenter/PerioPresenter.cpp/.h`
- `src/Model/Dental/PerioStatus.h`
- `src/Model/Dental/PerioStatistic.cpp`
- `src/Model/Parser.cpp`

The earlier Clinica findings remain relevant, especially:

- explicit sites `MB, B, DB, ML, L, DL`;
- facial `MB/B/DB` versus oral `ML/L/DL`;
- separate natural-tooth and implant records;
- explicit mobility/furcation semantics;
- accepted R2 clinical decision-support lineage.

This review did not modify QDento application code.

## 2. Overall conclusion after the second pass

The central architecture decision remains valid:

> Preserve QDento's preferred visual/interaction behavior, use Clinica as the cleaner periodontal domain/clinical reference, and implement the future application natively in Swift/SwiftUI.

The second pass did not reveal a reason to return to literal Qt-to-Swift translation.

It did, however, refine several details:

1. The four-part FMPS/FMBS visual geometry is more source-verifiable than previously documented.
2. Its anatomical meaning is still not source-labeled and must not be invented.
3. Mobility persistence failure is directly verified in QDento serialization.
4. FMPS/FMBS and BOP are persisted in QDento, unlike mobility.
5. BOP is a full six-site finding, not the same data structure as FMBS.
6. The new iOS model should not collapse BOP, FMBS, FMPS, and Clinica plaque into one undifferentiated boolean concept.
7. The remaining runtime investigation can be narrower than previously planned.

## 3. FMPS/FMBS geometry — newly source-verified

QDento stores:

- `FMBS[128]`
- `FMPS[128]`

This is exactly four positions per tooth across 32 tooth positions.

`PerioView::getUIbyTooth()` groups them as:

`toothIndex * 4 + 0...3`

`PerioGraphicsButton` explicitly defines the visual geometry by `index % 4`:

- 0 = left triangle;
- 1 = upper triangle;
- 2 = right triangle;
- 3 = lower triangle.

The graphics item rectangle is:

- width: 70.25;
- height: 30.

Each state fills one triangular path bounded by the rectangle corner(s) and center.

Selected colors are also explicit:

- Plaque / FMPS: RGB 204, 228, 247;
- Bleeding / FMBS: RGB 255, 146, 148.

### What is now resolved

The first iOS parity renderer can reproduce the exact four-part geometric idea without guessing how QDento constructs it.

The visual/toggle order is source-verified as:

`left → up → right → down`

### What is NOT resolved

The QDento source examined here does not name those four positions using anatomical surface terms.

Therefore the following must remain separate concepts:

- visual wedge identity: left / up / right / down;
- clinical surface/site identity: not yet proven from QDento source.

Do not rename the wedges mesial/distal/buccal/lingual merely because the geometry looks suggestive.

A runtime/source corroboration pass may establish the intended anatomical interpretation later.

## 4. Upper/lower FMPS/FMBS ordering — partially resolved

`PerioView::initializeFullMouth()` creates:

Upper:
- indices 0 through 63;
- four consecutive indices grouped at one x-position;
- x-position advances once per group of four.

Lower:
- indices 64 through 127;
- groups are laid out with x-position moving in the opposite horizontal direction after the initial lower group.

This supports the existing conclusion that mandibular presentation reverses spatial direction relative to the maxillary initialization.

What remains useful to verify at runtime is the final tooth-number-to-group alignment, not the basic four-triangle construction.

## 5. FMPS and FMBS are true persisted periodontal fields

`Parser::write(const PerioStatus&)` serializes:

- `FMBS`;
- `FMPS`;
- `BOP`;
- `PD`;
- `CAL`;
- `AG`;
- `Furc`;
- disabled state;
- smoking;
- bone loss;
- restoration;
- systemic state.

Therefore the QDento FMPS/FMBS controls are not merely presentation artifacts.

They are explicit periodontal examination data in QDento.

This matters for the future design:

The iOS project should decide deliberately whether these four-per-tooth findings survive as first-class parity data, rather than assuming they can be reconstructed losslessly from Clinica's six-site plaque/BOP model.

## 6. Mobility persistence gap — now directly verified

`PerioStatus` contains:

`mobility[32]`

The presenter populates it from date-specific tooth status when a periodontal document is opened.

However `Parser::write(const PerioStatus&)` does not serialize mobility.

Thus the earlier mobility concern is not merely an inference from missing UI wiring.

It is a direct source-level mismatch between the periodontal in-memory structure and the periodontal serialized payload.

### Design consequence

Do not copy this behavior into the new application.

Use an explicit persisted mobility field in the new periodontal domain.

Clinica's `0 | 1 | 2 | 3 | null` model remains the preferred semantic reference.

## 7. BOP is distinct from FMBS

QDento has:

- `bop[192]`: six positions per tooth;
- `FMBS[128]`: four positions per tooth.

The BOP UI uses one checkable `PerioButton` per periodontal measurement position and displays `icon_BOP.png`.

Clicking BOP calls:

`PerioPresenter::bopChanged(index, checked)`

which:
- updates the six-site `bop[]` field;
- recalculates periodontal statistics;
- marks the document edited.

FMBS uses a separate four-position graphics control and calls:

`FMBSChanged(index, value)`.

### Design consequence

BOP and FMBS must not be collapsed merely because both concern bleeding.

For the parity contract they are separate source concepts with different cardinality and different UI.

Any later clinical simplification must be explicit and clinically reviewed.

## 8. FMPS is also distinct from Clinica plaque

QDento's FMPS:
- four findings per tooth;
- persisted independently;
- displayed by four-part graphics;
- used in QDento's `HI` statistic.

Clinica's plaque:
- one nullable finding at each of six named periodontal sites;
- assessed-site denominator semantics.

The two models are not proven to be lossless equivalents.

### Refined decision

For the first parity-oriented app, do not force one into the other.

A safe future domain design may carry:
- explicit six-site observations used by modern Clinica-style charting;
- a separate four-part full-mouth scoring observation when preserving QDento parity.

Whether both remain in the final production model is a later clinical/product decision.

## 9. QDento FMPS index semantics remain legacy-specific

`PerioStatistic` defines:

`HI = calculatePercent(FMPS, countExisting=false)`

while:
- `BI` counts true FMBS findings;
- `BOP` counts true BOP findings.

This confirms the previous warning that the displayed FMPS-related index is not simply the same thing as a positive-plaque percentage.

### Refined decision

When reproducing the old statistics panel:
- preserve legacy meaning only if intentionally demonstrating QDento parity;
- label the statistic carefully;
- do not silently equate it with Clinica's plaque percentage.

When building final clinical metrics:
- use explicitly defined modern semantics.

## 10. BOP blood-drop presentation

The source confirms every six-site BOP control is assigned:

`:/icons/icon_BOP.png`

The user's screenshot confirms the icon is visually presented as a blood-drop-like marker at active sites.

### Refined decision

The iOS parity behavior should preserve:
- a visually immediate bleeding indicator;
- six-site association;
- state-driven appearance;
- live summary update.

The exact old icon file is not a mandatory production asset. A native/recreated symbol can later replace it while retaining the behavior.

## 11. QDento contour geometry — source facts remain valid

`PerioChartItem.cpp` confirms:

- baseline `y_pos = 105`;
- scale `y_coef = 3`;
- GM point y = `gm * 3 + 105`;
- CAL point y = `cal * -3 + 105`.

The item contains 48 measurement points per rendered surface.

The x-position logic uses tooth type widths and inter-point offsets in the non-even mode.

The surrounding `PerioView::initializeTeethScenes()` then applies different transforms to:

- maxillary buccal;
- maxillary palatal;
- mandibular buccal;
- mandibular lingual.

### Important distinction

Those local formulas are source-verified.

The final user-visible direction after all transforms is not fully frozen until runtime visual parity cases are captured.

Therefore both facts must coexist:
- geometry equations are known;
- net visible orientation remains a runtime validation target.

## 12. The GM sign-convention bridge remains important

QDento presenter behavior:
- displayed GM derives from `PD - CAL`;
- changing GM recalculates CAL using `CAL = PD - GM`.

Clinica domain behavior:
- positive margin = recession/apical to CEJ;
- `CAL = PD + GM`.

This remains a real sign-convention difference.

### Refined decision

Do not make the renderer own clinical sign semantics implicitly.

Create an explicit translation boundary between:
- canonical clinical margin semantics;
- QDento-compatible display geometry.

Runtime fixtures must include positive and negative margin/recession examples so the sign bridge is tested, not assumed.

## 13. Date behavior remains a QDento source weakness, but need not govern iOS

`PerioPresenter::dateChanged()` updates:
- patient info date;
- periodontal status date;
- edited state.

It does not reload `m_toothStatus`.

The existing source therefore supports the earlier concern that changing the periodontal date can leave date-specific tooth rendering/status stale.

### Refined decision

Do not make QDento's date mutation behavior part of the parity requirement.

The new app should define examination date and tooth-state snapshot semantics explicitly.

This is a domain/persistence decision, not a visual-parity feature.

## 14. Same-day new-record behavior is legacy behavior, not a design authority

QDento's constructor first requests the periodontal status for the patient/current date when no row id is supplied.

Only when the loaded record date differs from today does it clear row id and offer previous-result carry-forward.

This means same-day behavior can collapse the distinction between “new” and “existing today” in a way that is specific to the legacy application.

### Refined decision

The new iOS app does not need to preserve this behavior.

The future exam lifecycle should use explicit exam identity and explicit re-evaluation/new-exam actions.

Clinica's dated snapshot model is a better conceptual reference.

## 15. The user-observed “middle point on tooth, side points interproximal” behavior

The user explicitly values the visual interpretation that:
- the central measurement point visually crosses/aligns with the tooth body;
- the side points represent the interproximal areas between adjacent teeth.

This is an important product requirement.

However, the exact QDento source-to-clinical-label correspondence still needs runtime confirmation before the visual positions are assigned canonical clinical names.

### Refined documentation rule

Record this as:
- USER-OBSERVED / DESIRED VISUAL BEHAVIOR;
- not yet as QDENTO-SOURCE-VERIFIED anatomical labeling.

The iOS design should preserve the visual relationship while the mapping is being frozen.

## 16. Three architecture options re-evaluated

### Option A — literal QDento port
Rejected as the preferred architecture.

Pros:
- superficially direct lineage.

Cons:
- imports Qt-shaped concepts;
- packed arrays;
- legacy lifecycle behavior;
- known persistence mismatch;
- harder Clinica integration.

### Option B — Clinica-only mobile version
Also rejected as the first parity target.

Pros:
- cleaner domain;
- modern clinical behavior.

Cons:
- loses the QDento chart interaction the user specifically prefers;
- Clinica's existing renderer is not visually equivalent.

### Option C — hybrid authority model
Still preferred.

QDento:
- visual and interaction reference.

Clinica:
- explicit domain and accepted clinical logic reference.

iOS:
- new native architecture and persistence.

This option remains the best fit for the actual product goal.

## 17. Touch-first refinement

The first pass correctly proposed larger invisible hit regions around compact QDento-like controls.

The second pass adds one caution:

Four wedge controls occupying one approximately tooth-width rectangle may still be too small or ambiguous on some iPhone layouts when the entire arch is visible.

Therefore the first implementation should not hard-code one interaction mode as final.

### Preferred experiment sequence

1. Preserve compact four-wedge visual control.
2. Give each wedge a native enlarged hit region where geometry permits.
3. Test on simulator/device at realistic chart scale.
4. Only if error rate is high, test a selected-tooth enlarged control or transient detail interaction.

The second interaction mode is a fallback experiment, not a current requirement.

## 18. Risk/statistics panel refinement

The QDento panel remains useful as:
- visual reference;
- educational summary concept;
- parity reference.

But it contains mixed categories:
- descriptive percentages;
- histogram/distribution data;
- Stage-like diagnosis;
- risk hexagon;
- legacy modifiers.

### Refined decision

Do not design one monolithic “risk engine.”

Future architecture should distinguish at least:
- descriptive metrics;
- periodontitis classification;
- risk/prognostic assessment;
- presentation.

This prevents later replacement of one clinical algorithm from destabilizing all summary UI.

## 19. Licensing/provenance remains a production gate, not a parity-design blocker

The second review found no reason to change the existing stance.

Keep:
- GPL provenance;
- source references;
- asset manifest.

For a private parity experiment, QDento assets may be used as clearly documented reference/prototype material as appropriate.

Before proprietary/public distribution, asset/code reuse must be reviewed and replacement may be required.

Do not let temporary prototype asset reuse become an undocumented permanent dependency.

## 20. What is now genuinely left before formal design freeze

The second pass narrows the remaining QDento evidence closure to:

1. Runtime/screenshot confirmation of final visible six-site orientation.
2. Sentinel-value confirmation of which displayed QDento point corresponds to each clinically named site.
3. Runtime confirmation of maxillary versus mandibular, buccal/facial versus palatal/lingual net contour direction.
4. Tooth-number-to-FMPS/FMBS group alignment across both arches.
5. Anatomical interpretation, if any, of the four left/up/right/down FMPS/FMBS wedges.
6. Reference screenshot fixtures for BOP, FMPS, FMBS, recession/GM, missing tooth, and implant states.
7. Optional measurement of touch feasibility once an iPhone parity prototype exists.

Items 1–6 are evidence closure.

Item 7 belongs after implementation begins and must not block the design spec unnecessarily.

## 21. What should NOT be researched again broadly

Do not repeat:
- the entire QDento dependency audit;
- generic QDento removal analysis;
- generic Clinica periodontal architecture analysis;
- accepted Clinica R2 clinical-rule review;
- Qt-to-Swift feasibility discussion.

These questions are sufficiently understood for the next stage.

## 22. Improved Six-Cycle Iterative Refinement — second pass

### Cycle 1 — Accuracy & Fundamental Correction
Corrections:
- FMPS/FMBS visual order changed from “unresolved” to source-verified left/up/right/down.
- mobility persistence changed from an inferred gap to a source-verified serializer omission.
- BOP and FMBS explicitly separated as 192-site versus 128-surface structures.
- user visual observations were separated from source-verified anatomical claims.

### Cycle 2 — Completeness & Gap Analysis
Added:
- exact FMPS/FMBS button dimensions and colors;
- persisted-versus-nonpersisted field distinction;
- date-change stale-tooth-status concern;
- same-day new-record legacy behavior;
- distinction between legacy HI and plaque percentage.

### Cycle 3 — Structure & Architecture
Refined:
- four-part full-mouth score observations remain separate from six-site findings until a deliberate bridge exists;
- risk presentation was decomposed into metrics, classification, risk assessment, and UI;
- explicit domain-to-renderer sign translation retained.

### Cycle 4 — Adversarial / Critical Review
Attacked:
- the assumption that four wedge directions have anatomical labels;
- the assumption that Clinica's plaque field can replace QDento FMPS losslessly;
- the assumption that visual parity requires legacy lifecycle bugs;
- the assumption that source-local contour equations alone prove final screen direction.

No unsupported anatomical wedge labels were promoted.

### Cycle 5 — Usability & Goal Fit
Kept the student goal central:
- compact controls remain;
- touch enlargement remains invisible where possible;
- fallback enlarged interaction is only tested if compact control proves inaccurate;
- dynamic chart feedback remains non-negotiable.

### Cycle 6 — Final Synthesis, Regression Check & Polish
Rechecked:
- QDento visual authority remains intact;
- Clinica domain/clinical authority remains intact;
- no implementation plan was accidentally introduced;
- remaining research was narrowed rather than expanded;
- earlier rejected approaches remain documented with reasons.

## 23. Second-pass verdict

The brainstorming direction is more stable after this review.

No major architectural reversal is recommended.

The next evidence work should be a narrow runtime visual-parity closure, followed by a written architectural design specification for human review.

This document does not authorize Swift implementation or an implementation plan.
