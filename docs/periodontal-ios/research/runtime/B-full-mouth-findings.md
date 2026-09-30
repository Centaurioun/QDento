# QDento full-mouth findings: FMPS, FMBS, and BOP

- **Task:** Plan 01 / Task 2
- **Branch:** `research/p01-t2-full-mouth-findings`
- **Worktree:** `/Users/yusuf/.codex/worktrees/p01-t2-full-mouth-findings/QDento`
- **Starting revision:** `4d21c03c11ecd1f36d05c4347f36eb4d45df4576`
- **Starting status:** clean at the recorded revision.
- **Evidence basis:** QDento repository source. No QDento runtime or screenshot was available in this checkout for this task.
- **Scope:** Read-only source/runtime behavior mapping for the three findings. No QDento or Clinica application source was modified.

## Evidence labels

- `SOURCE_VERIFIED`: directly established by checked-in QDento source.
- `RUNTIME_VERIFIED`: confirmed by executing QDento. None for this report.
- `SCREENSHOT_OBSERVED`: confirmed from a QDento screenshot. None for this report.
- `USER_OBSERVED`: supplied by a user observation. None for this report.
- `UNRESOLVED`: source does not establish the claim, or runtime evidence is needed.

All percentages below are `PARITY_LEGACY`: they reproduce QDento's summary implementation and are not final clinical authority.

## FMPS and FMBS: four-wedge controls

### Geometry and control states — `SOURCE_VERIFIED`

`PerioGraphicsButton` uses `index % 4` with identities, in order, `0 = left`, `1 = up`, `2 = right`, `3 = down` (`src/View/Graphics/PerioGraphicsButton.cpp`, `PerioGraphicsButton` constructor). Each wedge is a triangle joining the relevant two corners of a `70.25 × 30` rectangle to its center; `surfButtonWidth` and `surfButtonHeight` are declared in `src/View/Graphics/PerioGraphicsButton.h`. The source names only geometric directions. It does not assign anatomical meanings to them.

The item paints the wedge white when unselected, and paints the selected wedge with its finding-specific color. Plaque/FMPS selected color is RGB `(204, 228, 247)`; Bleeding/FMBS selected color is RGB `(255, 146, 148)`. A disabled wedge is filled RGB `(236, 236, 236)`; an enabled hovered wedge receives a translucent gray overlay. Dark-gray, one-pixel cosmetic outlines trace the triangle and the full rectangle (`PerioGraphicsButton.cpp`, `paint`).

The control toggles its checked state on mouse press unless disabled, then reports the global index and finding type to `PerioView::PerioGraphicClicked` (`PerioGraphicsButton.cpp`, `mousePressEvent`; `src/View/Widgets/PerioView.cpp`, `PerioGraphicClicked`). There are no independent click handlers or shared state between the two finding types.

### Index grouping and displayed tooth order — `SOURCE_VERIFIED`

`PerioStatus` declares separate `FMPS[128]` and `FMBS[128]` arrays (`src/Model/Dental/PerioStatus.h`, lines 21–27). Both findings have four consecutive indexes per tooth, and the tooth data/view mappings use `toothIndex * 4 + wedgeIndex` (`src/Model/Dental/PerioToothData.cpp`, lines 29–33; `PerioView.cpp`, lines 262–267). Thus the grouping is:

| Global index range | Jaw / display sequence | Tooth index `t` | Four control indexes |
|---|---|---:|---|
| 0–63 | Maxilla, header index 0 through 15 from left to right | 0–15 | `4t` through `4t+3` |
| 64–127 | Mandible, header index 31 down through 16 from left to right | 16–31 | `4t` through `4t+3` |

In the mandibular scene the groups are created with global indexes 64–127 while the x position decreases once per four controls; this aligns the displayed order with the lower tooth header, which iterates 31 down to 16 (`PerioView.cpp`, `initializeFullMouth` and `initializeCommon`). FDI tooth numbers for indexes 0–31 are defined by `s_tooth_FDI` in `src/Model/Dental/ToothUtils.cpp`, lines 6–10. These mappings establish which tooth group owns a wedge, not an anatomical interpretation of any wedge direction.

FMPS controls occupy the upper row of the paired graphics rows and FMBS controls the lower row: `initializeFullMouth` places FMPS at y=0 and FMBS at y=the 30-unit button height. The source identifies the finding types as Plaque and Bleeding, respectively.

### Persistence and refresh — `SOURCE_VERIFIED`

The finding controls call distinct presenter methods. `FMPSChanged` writes `m_perioStatus.FMPS[index]`; `FMBSChanged` writes `m_perioStatus.FMBS[index]`. Each method recomputes and sends a `PerioStatistic` to the view, then marks the examination edited (`src/Presenter/PerioPresenter.cpp`, lines 156–174). The `PerioStatus` arrays are serialized separately under JSON keys `FMPS` and `FMBS`, and parsed back into their own arrays (`src/Model/Parser.cpp`, lines 36–42 and 392–398). Presenter save inserts a new status or updates the existing status (`PerioPresenter.cpp`, lines 239–247).

## BOP: six separate tooth slots

### Indexing and distinction from FMBS — `SOURCE_VERIFIED`

BOP is a separate `bool bop[192]` array; FMBS is `bool FMBS[128]` (`src/Model/Dental/PerioStatus.h`, lines 21–27). `PerioToothData` groups BOP indexes `toothIndex * 6 + i` for `i=0..5`, while its four FMBS values use `toothIndex * 4 + i` (`src/Model/Dental/PerioToothData.cpp`, lines 19–33). `PerioView::getUIbyTooth` attaches the six contiguous BOP controls to the same six slots (`PerioView.cpp`, lines 253–267). This establishes six anonymous QDento site slots per tooth; it does not establish MB/B/DB/ML/L/DL anatomical names or a clinically valid mapping to those names.

### Icon, state, and source-defined rows — `SOURCE_VERIFIED`

All 192 BOP controls are checkable and receive the same `:/icons/icon_BOP.png` icon (`PerioView.cpp`, lines 370–402 and 446–481). `PerioButton::paintEvent` draws a white enabled, unchecked cell with a gray outline; when enabled and checked it paints the assigned icon, and when disabled uses the theme background (`src/View/uiComponents/PerioButton.cpp`, lines 34–65). Tooth disable marks its six BOP controls disabled (`src/View/Widgets/ToothUi.h`, lines 24–47).

BOP controls are added to paired `bopUpLayout` and `bopDownLayout` rows for each jaw (`PerioView.cpp`, lines 370–402 and 446–481). The upper source loops call the rows buccal and palatal and map their controls to `ChartPosition::maxBuccal` and `ChartPosition::maxPalatal`. The lower loops have a naming inconsistency: the loop commented “buccal” maps to `ChartPosition::mandLing` in the up row, and the loop commented “lingual” maps to `ChartPosition::mandBuccal` in the down row (`PerioView.cpp`, lines 355–407 and 431–485). Therefore this report records row and source-index construction, not a resolved anatomical six-site order.

The six BOP controls are each connected to `PerioPresenter::bopChanged(index, checked)` (`PerioView.cpp`, lines 94–106). That presenter updates `status.bop[index]`, refreshes the statistic view, and marks the exam edited (`PerioPresenter.cpp`, lines 140–147). JSON persistence uses key `BOP`, separately from `FMBS` (`Parser.cpp`, lines 24–26 and 380–383).

## QDento parity-summary calculations (`PARITY_LEGACY`)

`getPercent(sum,total)` returns `100 * sum / total`, or zero when `total == 0` (`src/Model/Dental/PerioStatistic.cpp`, line 7). `calculatePercent` tests each visited boolean against `countExisting` (default `true`) and increments the denominator for every visited slot, regardless of whether the slot is true (`PerioStatistic.cpp`, lines 51–65).

| Visible label | Internal statistic / source array | Numerator | Denominator |
|---|---|---|---|
| BOP | `BOP` / `status.bop[192]` | True BOP slots | All six BOP slots on each non-disabled tooth |
| FMBS | `BI` / `status.FMBS[128]` | True FMBS wedges | All four FMBS wedges on each non-disabled tooth |
| FMPS | `HI` / `status.FMPS[128]` | **False / unselected** FMPS wedges | All four FMPS wedges on each non-disabled tooth |

The constructor calls are at `PerioStatistic.cpp`, lines 221–225. The statistic view displays `BI` with the label FMBS, `HI` with the label FMPS, and `BOP` with the BOP label (`src/View/SubWidgets/PerioStatisticView.cpp`, lines 53–62). In particular, visible “FMPS” is not the percentage of selected/positive FMPS wedges: its source formula counts false values. No correction or clinical reinterpretation is made here.

The iterator step is `array size / 32`: four slots per tooth for the two full-mouth score arrays and six for BOP. It skips the complete slot block for every `disabled[32]` tooth (`PerioStatistic.cpp`, lines 9–49). Therefore the denominators are `4 × enabled-tooth-count` for FMPS and FMBS and `6 × enabled-tooth-count` for BOP. If all teeth are disabled, each percentage is zero. There is no per-slot “not assessed” state in these arrays: a false value contributes to the denominator and is indistinguishable from an explicitly unselected value.

At presenter initialization, Missing, Impacted, and Implant tooth statuses set `disabled[tooth.index()] = true` (`src/Presenter/PerioPresenter.cpp`, lines 36–45). Manually toggling a tooth also changes this flag and refreshes the summary (`PerioPresenter.cpp`, lines 55–67). The tooth UI disables controls without clearing the arrays (`src/View/Widgets/ToothUi.h`, lines 24–47); `PerioToothData` reads the stored values when the tooth is enabled again (`src/Model/Dental/PerioToothData.cpp`, lines 19–33). These disabled slots are excluded from the three parity percentages while disabled; their stored values are not shown as erased by this behavior.

## Evidence gaps and unresolved items

- `RUNTIME_VERIFIED`: none. The checkout contained no built QDento executable/app or existing runtime capture used for this task. Runtime launch and screenshot capture were not performed.
- `SCREENSHOT_OBSERVED`: none. No screenshot establishes the final rendered positions of individual BOP sites or a clearly identified tooth with an active BOP site, FMPS wedge, or FMBS wedge.
- `UNRESOLVED`: final pixel placement and visual spacing of the BOP site icons, including how the lower up/down rows appear after widget layout.
- `UNRESOLVED`: mapping of the six anonymous BOP slots to named clinical sites. Source shows contiguous slot order and layout construction but does not prove those anatomical associations.
- `UNRESOLVED`: anatomical meaning of the neutral four-wedge directions. Keep `left/up/right/down` identities only.

## Refinement record

The report was refined through the six required cumulative passes: accuracy and correction; completeness and gap analysis; structure; adversarial review; usability and task fit; final synthesis and regression review. Claims that lacked source evidence remain explicitly unresolved. No seventh refinement pass was added.
