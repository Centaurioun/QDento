# Periodontal Rendering / Parity Contract v1

Date: 2026-10-01
Status: Revision 1.1 — supersedes v1 for Plan 03+ geometry consumption
Authority: approved QDento-inspired periodontal iOS design specification; coordinator rulings in the Plan 01 parallel batch review resolve evidence-lane conflicts.
Scope: portable rendering, source-compatible display/edit behavior, fixture identity, and acceptance evidence. This is not an iOS implementation and does not change QDento or Clinica.

Revision 1.1 erratum: the v1 table's Q3/Q4 **New-product visible left / middle / right** cells were transposed relative to the already-correct coordinator ruling and q0/q1/q2 mappings. Only those four visible-order cells are corrected here. No canonical site identity, q-slot mapping, domain rule, fixture value, GM rule, AG rule, or summary rule changes.

## 1. Authority and evidence labels

### Authority hierarchy

1. The approved spec at `docs/superpowers/specs/2026-09-30-qdento-periodontal-ios-design.md` governs product architecture and scope.
2. Clinica-derived canonical domain semantics govern named sites, natural tooth/implant separation, and clinical GM sign convention as adopted by the spec.
3. QDento source evidence governs the selected screen's packed order, local contour geometry, visual controls, source edit paths, and legacy parity summaries.
4. The coordinator rulings in `docs/superpowers/reviews/2026-09-30-plan01-parallel-batch-review.md` resolve ambiguities for the new product. A ruling is not promoted to QDento source evidence.
5. Runtime/screenshot evidence, when later captured, can verify rendered behavior. No such fixture-specific runtime or screenshot evidence exists in this batch.

### Evidence/disposition labels

- `SOURCE_VERIFIED`: supported by inspected QDento source or an approved local project document.
- `RUNTIME_VERIFIED`: behavior exercised in the identified QDento build; none in this batch.
- `SCREENSHOT_OBSERVED`: original, provenance-recorded fixture capture supports the visual claim; none in this batch.
- `USER_OBSERVED`: direct user report; none in this batch.
- `NEW_PRODUCT_CANONICAL_MAPPING`: coordinator-frozen anatomical mapping for the new iOS product, not a recovered QDento clinical label.
- `NEW_PRODUCT_RULE`: an explicit new-product behavior that intentionally differs from a legacy QDento transition.
- `PARITY_LEGACY`: source-exact legacy display/statistic semantics retained only for parity.

Evidence artifacts A–E remain unchanged and retain their lane-specific statuses. This contract reconciles them without rewriting their findings.

## 2. Canonical FDI and tooth order

Use permanent FDI numbering. The canonical QDento source index adapter is:

| QDento tooth index | FDI order | Arch/quadrant |
|---|---|---|
| 0–7 | 18, 17, 16, 15, 14, 13, 12, 11 | Maxilla Q1 |
| 8–15 | 21, 22, 23, 24, 25, 26, 27, 28 | Maxilla Q2 |
| 16–23 | 38, 37, 36, 35, 34, 33, 32, 31 | Mandible Q3 |
| 24–31 | 41, 42, 43, 44, 45, 46, 47, 48 | Mandible Q4 |

The new domain uses stable FDI tooth identity; it must not expose QDento packed array offsets as its public model. Tooth index, chart display order, and visual slot are distinct concepts. The table documents the adapter to the accepted permanent-tooth QDento evidence only; temporary-tooth mapping is out of scope and `DEFERRED` pending separate evidence.

## 3. Canonical named-site model and QDento adapter

Canonical facial/buccal named-site order is `MB, B, DB`; canonical oral order is `ML, L, DL`. The midpoint is B on facial surfaces and L on oral surfaces. Mesial is toward the dental midline and distal is away from it.

QDento source-derived `q0/q1/q2` labels below mean packed order within each three-value triplet, not QDento-authored anatomical labels. Source establishes visible left/middle/right order in the maxilla and right/middle/left order in the mandible. The named anatomy is supplied by this coordinator ruling and is explicitly new-product semantics.

| FDI quadrant | Surface | Canonical named order (mesial → distal) | New-product visible left / middle / right | QDento visible q order | q0 | q1 | q2 |
|---|---|---|---|---|---|---|---|
| Q1 18–11 | Facial/buccal | MB / B / DB | DB / B / MB | left / middle / right | DB | B | MB |
| Q1 18–11 | Oral/palatal | ML / L / DL | DL / L / ML | left / middle / right | DL | L | ML |
| Q2 21–28 | Facial/buccal | MB / B / DB | MB / B / DB | left / middle / right | MB | B | DB |
| Q2 21–28 | Oral/palatal | ML / L / DL | ML / L / DL | left / middle / right | ML | L | DL |
| Q3 31–38 | Facial/buccal | MB / B / DB | MB / B / DB | right / middle / left | DB | B | MB |
| Q3 31–38 | Oral/lingual | ML / L / DL | ML / L / DL | right / middle / left | DL | L | ML |
| Q4 48–41 | Facial/buccal | MB / B / DB | DB / B / MB | right / middle / left | MB | B | DB |
| Q4 48–41 | Oral/lingual | ML / L / DL | DL / L / ML | right / middle / left | ML | L | DL |

This whole table is labeled `NEW_PRODUCT_CANONICAL_MAPPING`. Never label its named-site association `QDENTO_SOURCE_VERIFIED`. QDento q0/q1/q2 clinical names remain undocumented. The approved product mapping resolves initial domain and geometry selection; asymmetric simulator parity evidence is still required later.

Each tooth's six measurement/BOP positions use surface triplets `[0,1,2]` facial/buccal followed by `[3,4,5]` oral/palatal or oral/lingual. For tooth index `t`, measurement/BOP packed indexes are `6t+q` for the first triplet and `6t+3+q` for the second, q=0..2. Named BOP association follows the same `NEW_PRODUCT_CANONICAL_MAPPING`; source establishes six contiguous BOP slots, but not their anatomy.

### Mandibular surface reconciliation

Use the first stored mandibular triplet as facial/buccal and the second as oral/lingual. The contradictory QDento `ChartPosition` enum identifiers are an internal rendering-naming anomaly. The source UI row comments and the T4 attached-gingiva surface mapping are the semantic evidence used by the coordinator ruling. Do not let those enum names swap domain surface identities.

## 4. Contour geometry and transforms

The selected QDento `PerioView` creates default `PerioChartItem`s with local bounds 1120 × 166. Local baseline is y=105 and the vertical scale is 3 display units per millimeter:

- `displayGM = PD − CAL`
- `GM_y = 105 + 3 × displayGM`
- `CAL_y = 105 − 3 × CAL`

There are 48 contour points with even x spacing. For point j=0..47, `x_j = (j + 0.5) × 1120/48`; spacing is 23⅓ units, with first/last centers at 11⅔ and 1108⅓. The default even-spacing path is used by all four selected surfaces. Tooth image type widths (36/54) and 70-unit scene slots do not change contour point spacing.

Source transform composition and resulting visible triplet orientation:

| Surface | Source transform composition | Net visible q order |
|---|---|---|
| Maxillary facial/buccal | Identity | q0→left, q1→middle, q2→right |
| Maxillary oral/palatal | x reflection plus 180° rotation | q0→left, q1→middle, q2→right |
| Mandibular facial/buccal (first stored triplet) | y reflection plus 180° rotation | q0→right, q1→middle, q2→left |
| Mandibular oral/lingual (second stored triplet) | x and y reflection | q0→right, q1→middle, q2→left |

These are source-derived transform compositions, not live screenshots. A native renderer must reproduce the final meaningful visible order in its own geometry layer; it must not copy Qt classes or plumbing. The tooth raster/provider and moving GM/CAL contours are separate layers and concerns.

## 5. GM adapter and editing transitions

Canonical domain GM is `clinicalGM = CAL − PD` (positive apical/recession; negative coronal). QDento parity display GM is `displayGM = PD − CAL = −clinicalGM`. Convert only at an explicit adapter boundary.

For valid demo PD/CAL values in 0..19:

| Direct edit | Update | Hold constant | Recompute |
|---|---|---|---|
| PD | PD only | CAL | clinical GM, parity display GM, both contours, affected derived summaries |
| CAL | CAL only | PD | clinical GM, parity display GM, both contours, affected derived summaries |
| QDento-sign display GM | Use the `NEW_PRODUCT_RULE` below | PD | CAL, clinical GM, display GM, both contours, affected derived summaries |

The new-product direct GM rule keeps PD=`p` constant and computes `CAL = p − displayGM`. Constrain accepted displayGM so CAL remains 0..19; for current PD p, allowed integer range is `p − 19 ... p`. Recompute the canonical clinical value as the negative of the accepted display value. Reject/clamp behavior at the control boundary must be explicit in the later UI implementation, but no transition may mutate PD to accommodate an invalid CAL or store hidden negative PD. This is `NEW_PRODUCT_RULE` and deliberately does not copy QDento's legacy direct-GM branches.

QDento's direct PD and CAL paths change only their edited field. Its legacy direct GM handler can change PD and CAL, may produce a refreshed display different from the requested value, and can leave in-memory PD below zero while a signal-blocked widget displays a clamp. Preserve that behavior only as documented `PARITY_LEGACY` reference; do not use it as the new product's edit contract.

## 6. Attached gingiva and recession

Attached gingiva surface applicability/storage contract:

| Arch/surface | Legacy source slot | New-domain meaning |
|---|---|---|
| Upper facial/buccal | `AG[t]` | Applicable value |
| Upper palatal/oral | `AG[t+32]` | Not applicable; do not carry a meaningful value into the canonical domain |
| Lower facial/buccal | `AG[t]` | Applicable value |
| Lower lingual/oral | `AG[t+32]` | Applicable value |

The source input range is 0..9. QDento persists 64 slots, including the disabled upper-palatal legacy slots. The legacy serialization slot is not evidence that the new domain should expose an assessed value there.

For each surface, derive recession as `max(0, max(clinicalGM at its three canonical sites))`, equivalently `max(0, max(CAL − PD))`. It is read-only and derived on demand; do not persist a redundant recession field. Changing AG alone does not change recession. This resolves fixture `P01-T3-F05`: `attachment[0]=7` on upper FDI 11 is facial/buccal AG; upper slot 1 is palatal/oral not applicable. Preserve T3 unchanged; this contract applies the stronger T4 mapping.

## 7. BOP and four-wedge findings

### BOP

BOP is a six-site Boolean finding per tooth, 192 source slots total, separate from FMBS and FMPS. It has direct active/inactive visual feedback, using the source blood-drop concept as a parity reference. Its named-site mapping in the new product follows the canonical six-site adapter in §3; anatomy is a new-product mapping, not a QDento source label. Source reports do not establish exact final pixel spacing or lower-row placement.

### FMPS and FMBS

FMPS and FMBS are separate persisted arrays with four Boolean wedges per tooth each (128 each). Neutral geometry identities are exactly `left`, `up`, `right`, `down`, corresponding to index 0,1,2,3. Do not assign mesial/distal/facial/oral meanings. Selected source colors: FMPS RGB(204,228,247); FMBS RGB(255,146,148). The source paints unselected wedges white and uses dark-gray one-pixel outlines; disabled and hover appearance is source-described in evidence B. Preserve independent state and persistence. Wedge anatomy is `NONBLOCKING_PRODUCT_DECISION` for later periodontist review.

## 8. QDento parity summaries and modern metrics

All QDento BOP/FMBS/FMPS summary formulas below are frozen as `PARITY_LEGACY`, not modern clinical metric definitions. For enabled teeth, `getPercent = 100 × numerator / denominator`, or 0 when denominator is 0. Disabled teeth are skipped as complete blocks; stored values are not erased by disabling.

| Legacy visible label | Array | Numerator | Denominator |
|---|---|---|---|
| BOP | 192 BOP slots | true BOP slots | 6 × enabled tooth count |
| FMBS (internal `BI`) | 128 FMBS wedges | true FMBS wedges | 4 × enabled tooth count |
| FMPS (internal `HI`) | 128 FMPS wedges | false/unselected FMPS wedges | 4 × enabled tooth count |

When no teeth are enabled all three percentages are zero. A false slot is included in the denominator and is not distinguishable from explicitly assessed-negative state in the legacy arrays. Do not describe visible legacy FMPS/HI as a modern plaque-positive percentage or silently invert it. Plan 02/04 architecture must allow a separate modern assessed-site metric provider, with explicit assessed/missing semantics, independent from this legacy parity provider.

## 9. Tooth visuals and provider boundary

Keep semantic tooth state distinct: natural present tooth, missing/extracted position, and implant are not one domain entity with alternate artwork. In the source reference, a natural tooth uses a permanent-tooth atlas slice with an optional periodontal overlay; basic missing/extracted uses the tooth silhouette at 0.1 opacity; implant uses separate front/molar crops from the common atlas and may have its own periodontal overlay. The rendering boundary must accept stable FDI identity, arch/quadrant orientation, permanent type class, semantic state, periodontal overlay state, requested buccal/lingual view, and bounds/scale. It returns replaceable base and optional periodontal overlay layers plus intrinsic layout/orientation metadata or an explicit unsupported-state result. Tooth art is separate from named measurement contours and clinical state.

Prototype asset/provenance manifest summary:

| Source asset | Prototype role | Provenance/distribution status |
|---|---|---|
| `resources/tooth_teeth.png` | Permanent natural-tooth body; low-opacity missing/extracted silhouette | QDento GPL repository context; individual image authorship not established |
| `resources/tooth_perio.png` | Natural-tooth periodontal overlay | Same unresolved image-level authorship/provenance |
| `resources/tooth_common.png` | Implant and implant-periodontal crops | Same unresolved provenance; atlas includes unrelated art |

These are private-demo/reference candidates only (`NONBLOCKING_PRIVATE_DEMO`), not assets cleared for distribution. Do not claim legal clearance. Public/proprietary release requires a separate asset/code licensing review and appropriate replacement or verified permissions (`BLOCKS_PUBLIC_DISTRIBUTION_REVIEW`). Runtime appearance at iPhone size, exact artwork interpretation, and optional green history tint are not established here. Temporary-tooth art/mapping is out of scope and `DEFERRED`.

## 10. Deterministic fixture identities and reconciliation notes

Use the eight existing IDs and source-oriented inputs without editing the T3 artifact:

| Fixture | Contract purpose |
|---|---|
| `P01-T3-F01` | Healthy baseline; flat contour |
| `P01-T3-F02` | Asymmetric six-site contour and mirror/orientation oracle |
| `P01-T3-F03` | Both GM signs and recession sign separation |
| `P01-T3-F04` | Direct CAL-only edit transition |
| `P01-T3-F05` | AG versus derived recession; apply upper slot reconciliation in §6 |
| `P01-T3-F06` | One active six-site BOP slot, with FMBS inactive |
| `P01-T3-F07` | Independent FMPS/FMBS neutral wedge selections |
| `P01-T3-F08` | Natural, missing, implant states and disabled legacy denominators |

Fixture F02 asymmetric values and F03 sign values must be consumed by later geometry tests; do not replace them with symmetric values. F06/F07 guard against conflating BOP with four-wedge findings. F08 requires distinct natural/missing/implant rendering descriptors; missing and implant status starts disabled in QDento and is excluded from parity denominators. Its legacy missing count is two for the disabled missing and implant positions, including the implant. That legacy behavior does not erase stored findings or establish a modern clinical assessed state.

Evidence files A–E and `research/runtime/fixtures/README.md` are immutable in this synthesis. No fixture capture exists; the README protocol governs later original captures and sidecars.

## 11. Verification gaps and blocker ledger

No Qt executable/qmake was available in the evidence work. No `RUNTIME_VERIFIED` or fixture-specific `SCREENSHOT_OBSERVED` result exists. Missing sentinel/screenshots are `BLOCKS_PARITY_ACCEPTANCE`: later QDento ↔ iOS simulator evidence must use asymmetric fixtures to verify point orientation, BOP/wedge placement, control response, and tooth-state presentation. They are not `BLOCKS_INITIAL_DOMAIN` or `BLOCKS_GEOMETRY` after the canonical new-product mapping is frozen.

| Classification | Count | Items at this gate |
|---|---:|---|
| `BLOCKS_INITIAL_DOMAIN` | 0 | None |
| `BLOCKS_GEOMETRY` | 0 | None after applying `NEW_PRODUCT_CANONICAL_MAPPING`; QDento source q labels remain undocumented but are not carried as product geometry ambiguity |
| `BLOCKS_PARITY_ACCEPTANCE` | 1 | Missing QDento runtime/sentinel/screenshot verification and exact pixel placement evidence |
| `NONBLOCKING_PRODUCT_DECISION` | 1 | Clinical/anatomical meaning of FMPS/FMBS left/up/right/down wedges |
| `NONBLOCKING_PRIVATE_DEMO` | 1 | Prototype QDento assets as private reference/demo candidates with provenance caveat |
| `BLOCKS_PUBLIC_DISTRIBUTION_REVIEW` | 1 | Image-level authorship/provenance and distribution clearance for any public/proprietary release |
| `DEFERRED` | 3 | Temporary-tooth mapping/assets; malformed imported PD/CAL/AG input policy (source parser does not validate these); final modern metric formulas/clinical interpretation beyond the separate provider boundary |

Counts are unique grouped issue items in this contract, not counts of every individual unknown. A later acceptance report may split/group them with traceable evidence.

## 12. Downstream interfaces

### Plan 02 — canonical domain

- Model permanent teeth by FDI identity and explicit named sites; do not expose anonymous QDento offsets.
- Keep natural tooth, implant, and missing position semantically distinct.
- Store PD, CAL, canonical clinical GM convention, BOP, separate FMPS/FMBS wedges, mobility/furcation, and only applicable AG values.
- Represent upper palatal/oral AG as not applicable, not an assessed zero.
- Keep recession derived and non-persisted.
- Preserve legacy parity summaries and any modern assessed-site provider behind separate interfaces.
- Use fixtures F01–F08 as deterministic domain inputs; keep evidence status with each expected result.

### Plan 03 — geometry and static chart

- Consume the §3 site adapter and §4 local geometry/visible-transform contract.
- Implement a pure geometry layer independent of SwiftUI, with tests for all quadrant/surface orders and asymmetric F02/F03 values.
- Keep clinical GM and QDento display GM separated behind the adapter; test both signs.
- Keep BOP six-site geometry distinct from neutral four-wedge geometry.
- Use a replaceable `ToothVisualProvider` semantic boundary; do not expose QDento filenames/Qt atlas details to the domain.
- Preserve prototype-asset provenance and label geometry as source-derived until simulator/runtime parity evidence exists.

Plan 04 editing must apply the PD/CAL/GM transitions in §5; Plan 05 must close `BLOCKS_PARITY_ACCEPTANCE` with provenance-recorded QDento and iOS evidence. This contract does not authorize starting those plans or implementing Swift.

## 13. Six-cycle synthesis refinement record

Exactly six synthesis refinement cycles were applied; no seventh cycle was added.

1. **Accuracy & Fundamental Correction** — Separated source facts from coordinator product rulings; corrected the initial geometry blocker disposition and froze site mapping as `NEW_PRODUCT_CANONICAL_MAPPING`.
2. **Completeness & Gap Analysis** — Included all required surfaces, edit transitions, summaries, AG/recession, finding cardinalities, assets, fixtures, downstream interfaces, and classified gaps.
3. **Structure & Architecture** — Organized the contract at portable boundaries: domain naming, adapter, geometry, edit behavior, findings, provider, evidence, and downstream consumers.
4. **Adversarial / Critical Review** — Checked Q1/Q2 and Q3/Q4 mirror directions against the coordinator table; checked mandibular first/second triplets against UI comments and AG ownership; checked `clinicalGM = −displayGM` and that `displayGM ∈ [p−19,p]` keeps CAL in 0..19; checked separate 6-site BOP versus 4-wedge FMPS/FMBS arrays and formulas; checked upper-palatal AG as not applicable despite legacy persistence; checked F08's distinct natural/missing/implant states and legacy disabled denominator. The source semantics and resulting product mapping are separated in §§3–11.
5. **Usability & Goal Fit** — Kept rules actionable for Plan 02/03 and preserved neutral wedge IDs and explicit applicability rather than inventing clinical meanings.
6. **Final Synthesis, Regression Check & Polish** — Reconciled T3 F05 with T4, checked labels against coordinator rulings, retained all evidence artifacts unchanged, and confirmed only allowed output files are written.

## 14. Final readiness verdict

**`READY_WITH_NONBLOCKING_UNRESOLVED_ITEMS`** — no `BLOCKS_INITIAL_DOMAIN` or `BLOCKS_GEOMETRY` item remains. Runtime/screenshot evidence is reserved for `BLOCKS_PARITY_ACCEPTANCE`; wedge anatomy remains a later nonblocking product/clinical decision. Private-demo asset provenance is explicit; public/proprietary distribution remains gated by review. This verdict authorizes Plan 02 to consume the frozen contract when separately scheduled; it does not authorize or begin Plan 02 here.
