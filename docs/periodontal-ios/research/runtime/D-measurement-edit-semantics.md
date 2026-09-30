# QDento Measurement Edit Semantics

**Plan:** 01 / Task 4
**Branch:** `research/p01-t4-measurement-edit-semantics`
**Evidence scope:** QDento source inspection. No QDento runtime result is claimed.
**Status key:** `SOURCE_VERIFIED`, `RUNTIME_VERIFIED`, `SCREENSHOT_OBSERVED`, `USER_OBSERVED`, `UNRESOLVED`.

## Contract summary

QDento has three distinct input paths. PD and CAL edits each write only their directly edited stored field. GM is a derived display (`PD - CAL`) when PD or CAL changes, but editing the GM control invokes a separate legacy transition that can change both PD and CAL. Do not model these paths as interchangeable.

Keep the sign conventions separate:

| Quantity | Definition | Evidence |
|---|---|---|
| QDento displayed/edit-control GM | `PD - CAL` | `SOURCE_VERIFIED` — `PerioPresenter::refreshMeasurment`, `src/Presenter/PerioPresenter.cpp:72-83`; also `PerioToothData`, `src/Model/Dental/PerioToothData.cpp:19-26` |
| Canonical clinical GM for the new domain | `CAL - PD`; positive is recession/apical and negative is coronal | Convention supplied by the Task Goal Packet; algebraically this is the inverse of the QDento value above |
| Recession display for a surface | `max(0, CAL - PD)` across that surface's three sites | `SOURCE_VERIFIED` — `PerioPresenter::getRecession`, `src/Presenter/PerioPresenter.cpp:86-97` |

Thus `canonical clinical GM = -QDento displayed GM`, and `CAL = PD + canonical clinical GM`. Do not name QDento's signed GM field “clinical GM” without an explicit conversion.

## Direct-edit transitions

Widget ranges below are the user-editable ranges created by `PerioView::initializeSurfaces` and the `PerioSpinBox` base constructor. Stored/imported values are not guaranteed to lie within those ranges; see “Range and legacy edge behavior.” All three controls are connected independently for all 192 sites (`src/View/Widgets/PerioView.cpp:94-104`).

| Direct input | Accepted input | Persisted field(s) | Held constant | Recomputed / post-edit values | Displayed GM and canonical clinical GM | CAL relation | Contour and refresh effects | Legacy cap/constraint | Disposition |
|---|---|---|---|---|---|---|---|---|---|
| **PD** | Integer `0..19` (`PerioSpinBox` default minimum 0; PD max 19). | `PD[index]` only. | Existing `CAL[index]`. | Refresh derives `QDento GM = new PD - old CAL`; refreshes the recession value for the containing three-site group and the periodontal statistic. | Displayed GM is `new PD - old CAL`; canonical GM is `old CAL - new PD`. | The source values imply `CAL = PD + canonical GM`; no separate PD/CAL correction is applied. | Signal-blocked control refresh, then the corresponding chart point is updated from displayed GM and CAL; both GM and CAL chart polylines are set for that site. `SOURCE_VERIFIED`, `PerioPresenter.cpp:72-84,100-107`; `PerioView.cpp:220-232,273-281`; `PerioChartItem.cpp:68-75`. | No handler-side cap; widget range bounds normal user edits. | Basic one-field edit and recalculated display: `PARITY_REQUIRED`. Out-of-range imported-value policy: `DEFERRED`. |
| **CAL** | Integer `0..19` (minimum inherited as 0; CAL max 19). | `CAL[index]` only. | Existing `PD[index]`. | Refresh derives `QDento GM = old PD - new CAL`; refreshes the containing recession group and statistic. | Displayed GM is `old PD - new CAL`; canonical GM is `new CAL - old PD`. | Same algebraic relation; no separate PD/CAL correction is applied. | Same single-site chart refresh and control refresh as direct PD. `SOURCE_VERIFIED`, `PerioPresenter.cpp:72-84,109-117`; `PerioView.cpp:220-232,273-281`; `PerioChartItem.cpp:68-75`. | No handler-side cap; widget range bounds normal user edits. | Basic one-field edit and recalculated display: `PARITY_REQUIRED`. Out-of-range imported-value policy: `DEFERRED`. |
| **GM** | Signed integer `-19..19`. This is QDento's displayed GM convention, not canonical clinical GM. | Usually both `PD[index]` and `CAL[index]`; depending on branch, one may be retained while the other changes. | No general held-constant measurement. In the CAL-ceiling branch, old CAL is retained and PD is changed. | Let requested value be `g`, old PD be `p`, old CAL be `c`. If `g > p`, first set `p = g`. Let `calTemp = p - g`. If `calTemp <= 19`, set CAL=`calTemp`; otherwise retain CAL=`c` and set PD=`c + g`. Then refresh from resulting stored values. | The refreshed display is `PD - CAL`; it can differ from the requested `g` when the `g > old PD` branch raises PD and thereby makes CAL 0 (the refreshed GM then becomes 0). Canonical GM is always the negative of the refreshed QDento display, not necessarily the negative of an unclamped request. | This path does not simply hold PD and derive CAL from requested display GM. Its normal branch computes `CAL = PD - g`; the high-positive-input branch first changes PD, and the `calTemp > 19` branch preserves old CAL and adjusts PD. | Refreshes both chart polylines at the edited site from the resulting control values; refreshes the shared recession value for the three-site group and statistic. `SOURCE_VERIFIED`, `PerioPresenter.cpp:72-84,119-138`; `PerioView.cpp:220-232,273-281`; `PerioChartItem.cpp:68-75`. | `calMax = 19` is the only explicit handler cap. The overflow branch can set in-memory PD below 0; QSpinBox then displays a clamped value while signal blockers prevent writing that clamp back to the model. See below. | Exact QDento transition is `PARITY_REQUIRED` only where explicit QDento edit parity is required. The hidden PD adjustment and cap fallback are **not clinical rules**; adopting them in the new product is a `NEW_PRODUCT_RULE` requiring an explicit choice. Do not copy silently. |

`makeEdited()` is called after each edit. PD/CAL/GM refresh also updates the summary statistic (`PerioPresenter.cpp:72-84,100-138`). `PerioView::setMeasurment` blocks the three measurement signals while setting values, updates `m_Rec[index/3]`, and then refreshes the chart (`src/View/Widgets/PerioView.cpp:220-232`); no secondary edit handler is triggered by that refresh.

### Worked GM boundary examples

These are source-derived examples, not runtime observations (`SOURCE_VERIFIED` from `PerioPresenter.cpp:119-138`):

1. Old PD=5, CAL=8; enter QDento GM=7. Since `7 > 5`, PD becomes 7; `calTemp=0`; CAL becomes 0; refresh displays GM=7. The visible edit has changed both stored values.
2. Old PD=19, CAL=5; enter QDento GM=-19. `calTemp=38 > 19`; CAL stays 5 and PD becomes `5 + (-19) = -14`. The spinbox range is 0..19, so refresh may display PD=0 while status still stores -14; displayed GM is then based on clamped widget values. This is a model/UI divergence, not a recommended new-domain behavior.

## Ranges, caps, and loaded values

- `PerioSpinBox` sets minimum 0 and initial value 0 (`src/View/uiComponents/PerioSpinBox.cpp:6-16`). PD and CAL set maximum 19; GM sets range `-19..19` (`src/View/Widgets/PerioView.cpp:348-425,427-512`). Therefore normal interactive ranges are PD `0..19`, CAL `0..19`, and QDento GM `-19..19` (`SOURCE_VERIFIED`).
- The presenter imposes no further PD or CAL range check for direct edits. In GM editing, `calTemp <= 19` is checked, but there is no lower-bound check and the fallback changes PD rather than rejecting the value (`src/Presenter/PerioPresenter.cpp:119-138`).
- Parser deserialization copies supplied PD/CAL integers into status without validating them (`src/Model/Parser.cpp:372-378`). The view sets spinbox values while signals are blocked (`src/View/Widgets/PerioView.cpp:220-232`), so widget clamping does not normalize stored status. Handling malformed/legacy out-of-range imports is `DEFERRED` pending an explicit new-product validation rule.
- A disabled tooth disables its PD, CAL, and GM controls; it does not erase measurement values (`src/View/Widgets/PerioView.cpp:137-146`; `src/View/Widgets/ToothUi.h:24-49`). `PerioWithDisabled` zeroes disabled PD/CAL/BOP for its consumer but does not alter the stored status (`src/Model/Dental/PerioStatus.h:34-50`). This disabled-tooth/statistics policy is not an edit transition.

## Attached gingiva: all 64 persisted positions

Storage is split into two 32-element tooth-slot blocks: for tooth index `t` in `0..31`, slot 0 is `AG[t]` and slot 1 is `AG[t+32]` (`src/Model/Dental/PerioToothData.cpp:5-17`). Each slot corresponds to one three-site surface group: slot 0 uses measurement indices `6t..6t+2`; slot 1 uses `6t+3..6t+5` (`PerioToothData.cpp:19-26,35-42`). The source establishes these site groups but does not establish named MB/B/DB or ML/L/DL identities within each triplet; that anatomical site mapping remains `UNRESOLVED` here.

Every AG control has range `0..9` (`src/View/Widgets/PerioView.cpp:560-613`, plus default minimum above). All `AG[0..63]` entries are connected to `attachChanged`, which writes the edited array element (`PerioView.cpp:89-92`; `PerioPresenter.cpp:149-154`). The UI widget pair is interleaved by tooth, while persisted storage is split into slot blocks; use the explicit formula/table below, not the widget-array index as the persisted index.

| Tooth index `t` | Tooth (FDI) | Slot 0 persisted position / surface | Slot 1 persisted position / surface |
|---:|---:|---|---|
| 0 | 18 | `AG[0]` — upper buccal, enabled | `AG[32]` — upper palatal, disabled/not applicable |
| 1 | 17 | `AG[1]` — upper buccal, enabled | `AG[33]` — upper palatal, disabled/not applicable |
| 2 | 16 | `AG[2]` — upper buccal, enabled | `AG[34]` — upper palatal, disabled/not applicable |
| 3 | 15 | `AG[3]` — upper buccal, enabled | `AG[35]` — upper palatal, disabled/not applicable |
| 4 | 14 | `AG[4]` — upper buccal, enabled | `AG[36]` — upper palatal, disabled/not applicable |
| 5 | 13 | `AG[5]` — upper buccal, enabled | `AG[37]` — upper palatal, disabled/not applicable |
| 6 | 12 | `AG[6]` — upper buccal, enabled | `AG[38]` — upper palatal, disabled/not applicable |
| 7 | 11 | `AG[7]` — upper buccal, enabled | `AG[39]` — upper palatal, disabled/not applicable |
| 8 | 21 | `AG[8]` — upper buccal, enabled | `AG[40]` — upper palatal, disabled/not applicable |
| 9 | 22 | `AG[9]` — upper buccal, enabled | `AG[41]` — upper palatal, disabled/not applicable |
| 10 | 23 | `AG[10]` — upper buccal, enabled | `AG[42]` — upper palatal, disabled/not applicable |
| 11 | 24 | `AG[11]` — upper buccal, enabled | `AG[43]` — upper palatal, disabled/not applicable |
| 12 | 25 | `AG[12]` — upper buccal, enabled | `AG[44]` — upper palatal, disabled/not applicable |
| 13 | 26 | `AG[13]` — upper buccal, enabled | `AG[45]` — upper palatal, disabled/not applicable |
| 14 | 27 | `AG[14]` — upper buccal, enabled | `AG[46]` — upper palatal, disabled/not applicable |
| 15 | 28 | `AG[15]` — upper buccal, enabled | `AG[47]` — upper palatal, disabled/not applicable |
| 16 | 38 | `AG[16]` — lower buccal, enabled | `AG[48]` — lower lingual, enabled |
| 17 | 37 | `AG[17]` — lower buccal, enabled | `AG[49]` — lower lingual, enabled |
| 18 | 36 | `AG[18]` — lower buccal, enabled | `AG[50]` — lower lingual, enabled |
| 19 | 35 | `AG[19]` — lower buccal, enabled | `AG[51]` — lower lingual, enabled |
| 20 | 34 | `AG[20]` — lower buccal, enabled | `AG[52]` — lower lingual, enabled |
| 21 | 33 | `AG[21]` — lower buccal, enabled | `AG[53]` — lower lingual, enabled |
| 22 | 32 | `AG[22]` — lower buccal, enabled | `AG[54]` — lower lingual, enabled |
| 23 | 31 | `AG[23]` — lower buccal, enabled | `AG[55]` — lower lingual, enabled |
| 24 | 41 | `AG[24]` — lower buccal, enabled | `AG[56]` — lower lingual, enabled |
| 25 | 42 | `AG[25]` — lower buccal, enabled | `AG[57]` — lower lingual, enabled |
| 26 | 43 | `AG[26]` — lower buccal, enabled | `AG[58]` — lower lingual, enabled |
| 27 | 44 | `AG[27]` — lower buccal, enabled | `AG[59]` — lower lingual, enabled |
| 28 | 45 | `AG[28]` — lower buccal, enabled | `AG[60]` — lower lingual, enabled |
| 29 | 46 | `AG[29]` — lower buccal, enabled | `AG[61]` — lower lingual, enabled |
| 30 | 47 | `AG[30]` — lower buccal, enabled | `AG[62]` — lower lingual, enabled |
| 31 | 48 | `AG[31]` — lower buccal, enabled | `AG[63]` — lower lingual, enabled |

Tooth indices and FDI labels come from `s_tooth_FDI` (`src/Model/Dental/ToothUtils.cpp:6-10`); upper/lower widget creation is `PerioView.cpp:292-320`. Surface applicability follows `PerioView::initializeRecAndAtt` and `ToothUi::setData`: upper slot 1 is disabled because the source comment says upper teeth have no attached palatal gingiva (`src/View/Widgets/PerioView.cpp:560-618`; `src/View/Widgets/ToothUi.h:51-65`). All AG values are still serialized and parsed, including disabled upper palatal slots (`src/Model/Parser.cpp:28-30,384-386`); UI inapplicability does not imply that the stored slot is omitted or cleared. These mappings are `SOURCE_VERIFIED`, not runtime-tested.

## Derived recession: surface formula and persistence

For a measurement site index `i` in `0..191`, QDento selects surface group `r = floor(i/3)` and computes:

```text
recession[r] = max(0, max over j = 3r..3r+2 of (CAL[j] - PD[j]))
```

Equivalently, using QDento displayed GM, it is the maximum of `-GM` across the three sites, with zero as the floor. `PerioPresenter::getRecession` uses `surfaceIndex / 3` and scans those three array positions (`src/Presenter/PerioPresenter.cpp:86-97`). `PerioToothData` independently derives slot 0 from `gm[0..2]` and slot 1 from `gm[3..5]` (`src/Model/Dental/PerioToothData.cpp:19-42`). Thus tooth `t` has recession groups `6t..6t+2` and `6t+3..6t+5`, corresponding to its two AG slots.

Recession is read-only in the UI (`setReadOnly(true)` on every `m_Rec` control) and has no presenter edit handler (`src/View/Widgets/PerioView.cpp:560-613`; presenter connections at `PerioView.cpp:89-104`). It is **derived, not persisted**: `Parser::write` serializes PD, CAL, BOP, and AG but has no GM or recession field (`src/Model/Parser.cpp:12-52`); `Parser::parse` restores PD/CAL/AG (`src/Model/Parser.cpp:372-386`), from which display GM and recession are reconstructed. This is `SOURCE_VERIFIED`.

The recession widget maximum is 19. If a derived/imported result is above 19, the visible spinbox can clamp it; the formula itself has no cap (`PerioView.cpp:570-584,597-612`; `PerioPresenter.cpp:86-97`). Recession stays separate from stored canonical attachment level or canonical clinical GM in the new domain: do not persist a redundant QDento recession value. Formula parity is `PARITY_REQUIRED`; any new clinical reporting semantics layered on top are a `NEW_PRODUCT_RULE`.

## Evidence and dispositions

| Item | Evidence status | Disposition |
|---|---|---|
| Independent direct PD, CAL, and GM handlers and their resulting stored fields | `SOURCE_VERIFIED` | `PARITY_REQUIRED` for QDento edit-path parity; preserve separate transitions. |
| QDento display GM `PD-CAL`, versus packet-defined canonical clinical GM `CAL-PD` | `SOURCE_VERIFIED` for QDento; canonical convention supplied by the Task Goal Packet | `PARITY_REQUIRED` as an explicit render/edit adapter; never conflate signs. |
| Exact hidden GM PD adjustment and CAL ceiling fallback | `SOURCE_VERIFIED`; not runtime exercised | The source behavior is known. Exact emulation is `PARITY_REQUIRED` only for a compatibility path; adopting it as the ordinary clinical edit rule is a `NEW_PRODUCT_RULE` and must be explicitly approved in the product contract. |
| `AG[64]` tooth/surface mapping, range, upper palatal disabled state, and JSON persistence | `SOURCE_VERIFIED` | `PARITY_REQUIRED` for the selected QDento workspace. Whether the new domain accepts the same non-applicable upper slot is a `NEW_PRODUCT_RULE`. |
| Three-site recession formula, read-only UI, and derived/non-persisted status | `SOURCE_VERIFIED` | `PARITY_REQUIRED` for visible QDento parity; retain derived state in the new domain. |
| Exact named-site correspondence (MB/B/DB and ML/L/DL) within each stored three-site group | `UNRESOLVED` from inspected sources | `DEFERRED`; retain index order and do not invent named-site semantics from the Clinica convention. |
| Runtime transition matrix, screenshots, and live contour confirmation | `UNRESOLVED` | `DEFERRED` because no QDento executable or qmake was available in this worktree; no runtime result is asserted. |
| Policy for malformed/out-of-range imported PD/CAL/AG values and resulting widget clamps | `SOURCE_VERIFIED` that parser lacks these checks; desired product handling `UNRESOLVED` | `DEFERRED` for explicit new-product validation design. |

### Runtime availability

The worktree contains source and the Qt project file, but no built QDento executable was found and neither `qmake6` nor `qmake` was available on PATH during this task (`SOURCE_VERIFIED` environment check). I did not build or launch the application. All formulas, ranges, and transitions above are therefore source-verified only; no item is labeled `RUNTIME_VERIFIED`, `SCREENSHOT_OBSERVED`, or `USER_OBSERVED`.

## Six-cycle cumulative refinement record

1. **Accuracy & Fundamental Correction** — Verified handlers and kept QDento displayed GM (`PD-CAL`) distinct from canonical clinical GM (`CAL-PD`). Corrected the transition account for the positive-GM PD adjustment and CAL cap fallback.
2. **Completeness & Gap Analysis** — Added exact input ranges, persisted/held/recomputed fields, refresh effects, all 64 AG positions, upper-palatal applicability, serialization, and recession persistence status. Marked named-site ordering unresolved.
3. **Structure & Architecture** — Separated direct edit behavior, range/import edge cases, AG storage, recession derivation, and evidence/disposition decisions so a behavior implementer can use each contract independently.
4. **Adversarial / Critical Review** — Checked counterexamples at the GM boundary, including possible negative stored PD and signal-blocked UI clamping; checked the interleaved AG widget order against split persisted slots; avoided treating disabled controls as erased data.
5. **Usability & Goal Fit** — Made the three edit paths explicit in separate rows, stated which fields each changes, and identified where reproducing a legacy transition is a parity choice rather than a clinical rule.
6. **Final Synthesis, Regression Check & Polish** — Rechecked the report against Task 4 acceptance criteria; retained only source-supported facts, explicit unresolved items, and the allowed evidence classifications. No seventh refinement cycle was added.
