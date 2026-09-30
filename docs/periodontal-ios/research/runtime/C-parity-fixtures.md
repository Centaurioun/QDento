# QDento Parity Fixtures — Plan 01 / Task 3

Status: deterministic fixture specification; QDento runtime has not been executed for these cases.

These platform-independent fixture IDs are intended to be shared by later QDento runtime captures, Swift geometry tests, SwiftUI interaction tests, and final parity screenshots. Inputs below are source-oriented. `site[0…5]` means the six packed measurements loaded for one tooth by `PerioToothData` (`status index = tooth index × 6 + site offset`). It deliberately does not assign MB/B/DB/ML/L/DL names: the source examined here does not establish that anatomical mapping. Likewise, `attachment[0]`/`attachment[1]` are the two per-tooth values; Task 1 must settle their named surface mapping before a named-site/surface contract is frozen.

Unless a case says otherwise, numbering is FDI, all 32 teeth are natural and enabled, all values not listed are zero/false, and mobility/furcation are 0. “Expected” rows distinguish code-backed results from visible runtime results. A value calculated from source is `SOURCE_VERIFIED`; it is not proof that the running QDento UI displays it correctly. None of these fixture states currently has `RUNTIME_VERIFIED` or fixture-specific `SCREENSHOT_OBSERVED` evidence.

## Input conventions and source boundaries

- Internal tooth index is the zero-based position in `ToothUtils`' FDI array. FDI 11 is index 7, with measurement indices 42–47 and wedge indices 28–31. Pin FDI numbering in the app settings.
- Each measurement site contains PD, CAL, and BOP. QDento displays GM as `PD − CAL`; recession is derived as `max(0, CAL − PD)` over each group of three sites. The UI permits PD/CAL 0–19 and GM −19…19. CAL and PD are separately editable.
- Four wedge states are indexed 0–3 per tooth and remain neutral `left/up/right/down` identities. They are separate from six-site BOP. Do not infer anatomical wedge names.
- All source-derived summary fractions assume every tooth is enabled unless fixture F08 explicitly states otherwise. QDento skips disabled teeth in its legacy iterators. FMPS's displayed `HI` counts false FMPS entries; the label and behavior must not be “corrected” in the parity fixture.
- Tooth/arch identity and packed offsets below are part of the fixture input. The unresolved named anatomy of the slots is retained as an evidence gap, not guessed.

## Fixture catalogue

### P01-T3-F01 — Healthy baseline

**Purpose:** zero-state baseline and coincident, flat contour control.

**Input:** Maxilla; natural FDI 11 (internal tooth 7). `site[0…5]`: PD `[0,0,0,0,0,0]`; CAL `[0,0,0,0,0,0]`; GM is derived, not directly entered. BOP `[false,false,false,false,false,false]`. FMPS wedges `[false,false,false,false]`; FMBS wedges `[false,false,false,false]`. `attachment[0…1]=[0,0]`; mobility 0; furcation `[0,0,0]`.

**Expected results:** all six derived GM values are 0; both three-site recession values are 0; the GM and CAL contour vertices remain at their baseline y=105 in local chart coordinates. `SOURCE_VERIFIED`. Perio screen rendering and the flat visible contour in QDento: `UNRESOLVED` pending runtime capture.

**Parity-summary delta:** none from the all-zero, all-enabled baseline. `SOURCE_VERIFIED` by source formula; formatted UI values `UNRESOLVED`.

### P01-T3-F02 — Asymmetric three-site contour

**Purpose:** prevent a symmetric or mirrored implementation from passing with equal values.

**Input:** Maxilla; natural FDI 11 (index 7; measurement offsets 42–47). Site order is packed `site[0]` through `site[5]`, not an asserted anatomical name. PD `[2,5,8,3,6,4]`; CAL `[1,3,6,5,2,4]`; no directly edited GM. Derived GM `[1,2,2,-2,4,0]`; BOP all false; FMPS/FMBS all false; attachments `[0,0]`; mobility 0; furcation `[0,0,0]`.

**Expected results:** six unequal/asymmetric GM values remain associated with the corresponding packed sites; first three-site recession is 0 and second is 2. Local GM y values are `[108,111,111,99,117,105]`, and CAL y values `[102,96,87,90,87,93]`. Equations and recession: `SOURCE_VERIFIED`. Exact on-screen left/right order after arch/surface transforms: `UNRESOLVED` until Task 1 mapping and runtime evidence.

**Parity-summary delta:** none. `SOURCE_VERIFIED` by source formula; formatted UI values `UNRESOLVED`.

### P01-T3-F03 — Recession and GM-sign case

**Purpose:** exercise both signs of displayed GM and ensure recession uses CAL−PD, not displayed GM.

**Input:** Maxilla; natural FDI 11 (index 7; measurement offsets 42–47). PD `[4,4,4,4,4,4]`; CAL `[6,3,5,4,4,4]`; GM is derived and not directly edited. Derived GM `[-2,1,-1,0,0,0]`; BOP, FMPS, FMBS false; attachments `[0,0]`; mobility 0; furcation `[0,0,0]`.

**Expected results:** displayed GM is negative at sites 0 and 2 and positive at site 1; recession is 2 for the first triplet and 0 for the second. The corresponding local GM vertices are `[99,108,102,105,105,105]`; CAL vertices are `[87,96,90,93,93,93]`. `SOURCE_VERIFIED`. Actual curve orientation and screenshot appearance: `UNRESOLVED`.

**Parity-summary delta:** none. `SOURCE_VERIFIED` by source formula; formatted UI values `UNRESOLVED`.

### P01-T3-F04 — Direct CAL-edit transition

**Purpose:** prove CAL is a direct input and its edit transition differs from a GM edit.

**Input:** Maxilla; natural FDI 11 (index 7). Before edit, PD `[4,4,4,0,0,0]`, CAL `[0,1,0,0,0,0]`. Direct action: edit CAL at `site[1]` from 1 to 5; do not edit PD or GM. After edit, expected PD `[4,4,4,0,0,0]`, CAL `[0,5,0,0,0,0]`, derived GM `[4,-1,4,0,0,0]`; recession first triplet 1, second triplet 0. BOP/FMPS/FMBS false; attachments `[0,0]`; mobility 0; furcation `[0,0,0]`.

**Expected results:** direct CAL handler changes only the selected CAL value; PD stays 4 at site 1; displayed GM becomes −1 there. Recession becomes 1 for that three-site group. Handler and derived arithmetic: `SOURCE_VERIFIED`. Numeric-control/rendered transition in a running app: `UNRESOLVED`.

**Parity-summary delta:** source formula changes CAL average/distribution/max and related derived values, but no specific summary output is required for this interaction fixture. `SOURCE_VERIFIED` for formula inputs; displayed summary `UNRESOLVED`.

### P01-T3-F05 — Attached-gingiva input and derived-recession surface

**Purpose:** keep attached gingiva entry distinct from the PD/CAL-derived recession result.

**Input:** Maxilla; natural FDI 11 (index 7). Use source value `attachment[0]=7`, `attachment[1]=0`; the named physical surface for attachment slot 0 remains `UNRESOLVED` until Task 1 confirms it. For measurement slots `[0,1,2]`, PD `[4,4,4]`, CAL `[3,6,4]`; slots `[3,4,5]` all have PD/CAL 0. No direct GM edit. Derived GM `[1,-2,0,0,0,0]`; first-triplet recession 2, second-triplet recession 0. BOP/FMPS/FMBS false; mobility 0; furcation `[0,0,0]`.

**Expected results:** attachment value 7 is independently stored as an AG input; the first triplet's recession derives to 2 from PD/CAL. Changing AG alone to 8 must not change either derived recession value because recession calculation reads PD/CAL. Setter and formulas: `SOURCE_VERIFIED`. UI surface association and visible response: `UNRESOLVED` pending Task 1/runtime evidence.

**Parity-summary delta:** none expected from AG-only change. `SOURCE_VERIFIED` from handler/source dependency; displayed UI `UNRESOLVED`.

### P01-T3-F06 — Six-site BOP case

**Purpose:** distinguish six-site BOP from four-wedge FMBS.

**Input:** Maxilla; natural FDI 11 (index 7). PD/CAL all zero; BOP `[false,false,true,false,false,false]` (only `site[2]` true); FMPS and FMBS wedges all false; attachments `[0,0]`; mobility 0; furcation `[0,0,0]`.

**Expected results:** one BOP control is active at the packed site 2 position; all four FMBS wedges stay inactive. Separate arrays and handlers: `SOURCE_VERIFIED`. Which anatomical site and exact screenshot position: `UNRESOLVED` until Task 1 mapping/runtime evidence.

**Parity-summary delta:** with all 32 teeth enabled, the source BOP fraction is `1/192 × 100%`; FMBS remains `0/128`. `SOURCE_VERIFIED` from source formula. The displayed formatting and active icon state: `UNRESOLVED`.

### P01-T3-F07 — FMPS/FMBS neutral-wedge case

**Purpose:** assert separate FMPS/FMBS state and each neutral wedge identity without assigning anatomy.

**Input:** Maxilla; natural FDI 11 (index 7; wedge indices 28–31). PD/CAL/BOP all zero/false. FMPS `[true,false,false,false]`; FMBS `[false,false,false,true]`. Attachments `[0,0]`; mobility 0; furcation `[0,0,0]`.

**Expected results:** FMPS wedge 0 and FMBS wedge 3 are independently selected; other wedges and all BOP sites remain inactive. Source defines wedge direction as `index % 4`: 0 left, 1 up, 2 right, 3 down; it does not define anatomical meaning. `SOURCE_VERIFIED` for index/direction/control separation; exact on-screen response `UNRESOLVED`.

**Parity-summary delta:** with all 32 teeth enabled, FMBS/BI is `1/128 × 100%`. FMPS/HI is `127/128 × 100%` because the legacy HI calculation counts false FMPS values; this deliberately records source behavior, not clinical interpretation. `SOURCE_VERIFIED`. Display formatting: `UNRESOLVED`.

### P01-T3-F08 — Natural, missing, and implant tooth visuals

**Purpose:** compare three tooth states and record the legacy disabled-tooth denominator behavior.

**Input:** Maxilla; FDI 24 (index 11) natural, FDI 25 (index 12) missing, and FDI 26 (index 13) implant. All measurements, BOP, FMPS/FMBS, attachment, mobility, and furcation values are zero/false. Start a fresh periodontal view so missing/implant statuses initialize as disabled.

**Expected results:** natural, missing/extracted, and implant rendering paths are distinct in source; missing and implant teeth are disabled for periodontal state on initialization. `SOURCE_VERIFIED` for state routing. Exact visible QDento pixels, appearance of the disabled controls, and tooth-label alignment: `UNRESOLVED` until a named runtime capture.

**Parity-summary delta:** the active denominators are 180 BOP slots and 120 wedge slots (30 enabled teeth × 6 or 4). The legacy missing-teeth count is 2 for these disabled non-wisdom positions, including the implant, because the source counts disabled positions. `SOURCE_VERIFIED`; runtime-displayed percentages/count and intended clinical interpretation: `UNRESOLVED`.

## Evidence ledger

| Evidence class | Applied to this artifact |
|---|---|
| `SOURCE_VERIFIED` | Source-backed inputs, transitions, local equations, array cardinalities, and formulas cited above. This does not claim a running QDento result. |
| `RUNTIME_VERIFIED` | None. The QDento application was not run for these fixtures. |
| `SCREENSHOT_OBSERVED` | None for a fixture. Repository `screenshots/scr0.png`–`scr3.png` are generic README screenshots without fixture input/provenance; they are not used as expected fixture results. |
| `USER_OBSERVED` | None. |
| `UNRESOLVED` | Fixture-specific screen output, named anatomical mapping for packed measurement slots and attachment slots, and other results explicitly marked above. |

## Source references used

- `src/Presenter/PerioPresenter.cpp:72-97,100-172` — refreshed GM, recession, direct PD/CAL/GM/BOP/AG/FMPS/FMBS handlers.
- `src/View/Widgets/PerioView.cpp:94-105,348-489,515-558,560-617,620-654` — input connections, packed chart control construction, wedge setup, AG/recession controls, transforms.
- `src/Model/Dental/PerioToothData.cpp:16-42` and `src/Model/Dental/PerioStatus.h:8-31` — per-tooth measurement/wedge/attachment offsets and default arrays.
- `src/View/Graphics/PerioChartItem.cpp:5-8,68-75` — local contour baseline and scale equations.
- `src/View/Graphics/PerioGraphicsButton.cpp:12-29` — neutral wedge directions and selected colors.
- `src/Model/Dental/PerioStatistic.cpp:9-65,135-147,221-237` — disabled-tooth filtering, legacy missing count, and FMPS/FMBS/BOP summary formulas.
- `src/Model/Dental/ToothUtils.cpp:6-10,29-39` and `src/View/Graphics/PaintHint.cpp:55-71`, `src/View/Graphics/ToothPainter.cpp:306-325` — FDI ordering and missing/implant render paths.

## Cross-platform use

Use the same fixture ID and input table in QDento evidence captures, future Swift value/geometry tests, SwiftUI interaction tests, and final screenshot comparisons. Future platform implementations may use a canonical named-site model, but any translation from these QDento packed offsets to named sites must cite the resolved Plan 01 mapping report. Do not silently alter the source-oriented values when writing that adapter.
