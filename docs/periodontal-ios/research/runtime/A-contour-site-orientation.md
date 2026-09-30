# QDento Contour / Six-Site Orientation Evidence

Date: 2026-09-30
Task: Plan 01 / Task 1 — contour and site orientation
Branch: `research/p01-t1-contour-site-orientation`
Starting HEAD: `4d21c03c11ecd1f36d05c4347f36eb4d45df4576`

## Result and blocking item

This report accounts for every QDento PD/CAL/GM/BOP measurement index and maps each index to its chart item, local vertex slot, and source-derived left/middle/right x location. It records the visible upper/lower surface transforms and the local contour geometry.

**`BLOCKS_GEOMETRY`:** QDento source does not identify which of the three vertices in either surface triplet means mesial, middle, or distal. The approved canonical vocabulary is `MB, B, DB, ML, L, DL`, but that vocabulary alone does not establish QDento's measurement-offset-to-name association. Accordingly, this artifact does not claim a verified mapping from a canonical named site to one of QDento's three local vertices. A later geometry implementer must not freeze a named-site mapping from this report without additional evidence (a labeled source/UI contract, or an approved clinical mapping plus runtime sentinel confirmation).

The source-level index-to-visible-point map is complete. The requested named-site-to-point contract is not complete, so the acceptance condition “without inspecting QDento source again” is **not met** yet.

## Evidence classes and authority

- `SOURCE_VERIFIED`: directly expressed by QDento source, cited below.
- `SCREENSHOT_OBSERVED`: visible in a committed screenshot; screenshots do not identify which measurement index generated a point.
- `RUNTIME_VERIFIED`: not obtained in this task.
- `UNRESOLVED`: source/runtime evidence does not settle the claim.

The approved design spec names Clinica as the authority for `MB, B, DB, ML, L, DL` and their facial/oral grouping (`docs/superpowers/specs/2026-09-30-qdento-periodontal-ios-design.md`, §5). This is canonical terminology only. It does not label QDento's packed offsets. QDento remains the authority for the display order and transforms.

## Source construction and measurement storage

`PerioView.h:16-22,34,43-48` declares four chart positions, a `ChartIndex {position,index}` lookup for 192 measurements, and 192-element PD/CAL/GM/BOP control arrays. `PerioView.cpp:94-105` connects control number `i` to the presenter with that same `i`; `PerioView.cpp:273-280` uses `chartIndex[i]` to update the contour; `PerioToothData.cpp:19-26` reads six consecutive offsets beginning at `toothIndex * 6`, deriving `GM = PD - CAL`. QDento therefore has six consecutive packed measurement offsets per tooth, but the data model has no named site enum or per-offset label (`PerioStatus.h:21-29`).

The maxillary construction loops assign the first three offsets of every tooth to the buccal UI row and the next three to the palatal UI row. The mandibular loops' comments call the first triplet the buccal row and the second the lingual row, while their `ChartPosition` assignments are respectively `mandLing` and `mandBuccal` (`PerioView.cpp:351-425,429-512`). Those enum assignments are reported literally below; the comments and enum names disagree for the mandible. This is a source anomaly, not silently corrected here.

The formulas below expand to all 192 offsets:

- Tooth index `t = 0..15` (maxilla): offsets `6t..6t+2` → `maxBuccal`, local slots `3t..3t+2`; offsets `6t+3..6t+5` → `maxPalatal`, local slots `3t..3t+2`.
- Tooth index `t = 16..31` (mandible): offsets `6t..6t+2` → `mandLing`, local slots `3(t-16)..3(t-16)+2`; offsets `6t+3..6t+5` → `mandBuccal`, local slots `3(t-16)..3(t-16)+2`.

In each formula, the offset order within the triplet maps to local vertex order 0, 1, 2. “Site q0/q1/q2” below means only this packed triplet order; it is deliberately not a clinical site label.

### Expanded tooth/index ledger

FDI association is source-backed by `ToothUtils.cpp:6-10`; QDento tooth indices 0–15 are FDI 18 through 28, and 16–31 are FDI 38 through 48 in the array order shown. `PerioView.cpp:292-320` constructs the upper UI in ascending tooth-index order and the lower UI in descending tooth-index order.

| QDento tooth index | FDI | PD/CAL/GM/BOP offsets q0–q2 | QDento chart position; local slots q0–q2 | PD/CAL/GM/BOP offsets q3–q5 | QDento chart position; local slots q0–q2 |
|---:|---:|---|---|---|---|
| 0 | 18 | 0–2 | `maxBuccal`; 0–2 | 3–5 | `maxPalatal`; 0–2 |
| 1 | 17 | 6–8 | `maxBuccal`; 3–5 | 9–11 | `maxPalatal`; 3–5 |
| 2 | 16 | 12–14 | `maxBuccal`; 6–8 | 15–17 | `maxPalatal`; 6–8 |
| 3 | 15 | 18–20 | `maxBuccal`; 9–11 | 21–23 | `maxPalatal`; 9–11 |
| 4 | 14 | 24–26 | `maxBuccal`; 12–14 | 27–29 | `maxPalatal`; 12–14 |
| 5 | 13 | 30–32 | `maxBuccal`; 15–17 | 33–35 | `maxPalatal`; 15–17 |
| 6 | 12 | 36–38 | `maxBuccal`; 18–20 | 39–41 | `maxPalatal`; 18–20 |
| 7 | 11 | 42–44 | `maxBuccal`; 21–23 | 45–47 | `maxPalatal`; 21–23 |
| 8 | 21 | 48–50 | `maxBuccal`; 24–26 | 51–53 | `maxPalatal`; 24–26 |
| 9 | 22 | 54–56 | `maxBuccal`; 27–29 | 57–59 | `maxPalatal`; 27–29 |
| 10 | 23 | 60–62 | `maxBuccal`; 30–32 | 63–65 | `maxPalatal`; 30–32 |
| 11 | 24 | 66–68 | `maxBuccal`; 33–35 | 69–71 | `maxPalatal`; 33–35 |
| 12 | 25 | 72–74 | `maxBuccal`; 36–38 | 75–77 | `maxPalatal`; 36–38 |
| 13 | 26 | 78–80 | `maxBuccal`; 39–41 | 81–83 | `maxPalatal`; 39–41 |
| 14 | 27 | 84–86 | `maxBuccal`; 42–44 | 87–89 | `maxPalatal`; 42–44 |
| 15 | 28 | 90–92 | `maxBuccal`; 45–47 | 93–95 | `maxPalatal`; 45–47 |
| 16 | 38 | 96–98 | `mandLing`; 0–2 | 99–101 | `mandBuccal`; 0–2 |
| 17 | 37 | 102–104 | `mandLing`; 3–5 | 105–107 | `mandBuccal`; 3–5 |
| 18 | 36 | 108–110 | `mandLing`; 6–8 | 111–113 | `mandBuccal`; 6–8 |
| 19 | 35 | 114–116 | `mandLing`; 9–11 | 117–119 | `mandBuccal`; 9–11 |
| 20 | 34 | 120–122 | `mandLing`; 12–14 | 123–125 | `mandBuccal`; 12–14 |
| 21 | 33 | 126–128 | `mandLing`; 15–17 | 129–131 | `mandBuccal`; 15–17 |
| 22 | 32 | 132–134 | `mandLing`; 18–20 | 135–137 | `mandBuccal`; 18–20 |
| 23 | 31 | 138–140 | `mandLing`; 21–23 | 141–143 | `mandBuccal`; 21–23 |
| 24 | 41 | 144–146 | `mandLing`; 24–26 | 147–149 | `mandBuccal`; 24–26 |
| 25 | 42 | 150–152 | `mandLing`; 27–29 | 153–155 | `mandBuccal`; 27–29 |
| 26 | 43 | 156–158 | `mandLing`; 30–32 | 159–161 | `mandBuccal`; 30–32 |
| 27 | 44 | 162–164 | `mandLing`; 33–35 | 165–167 | `mandBuccal`; 33–35 |
| 28 | 45 | 168–170 | `mandLing`; 36–38 | 171–173 | `mandBuccal`; 36–38 |
| 29 | 46 | 174–176 | `mandLing`; 39–41 | 177–179 | `mandBuccal`; 39–41 |
| 30 | 47 | 180–182 | `mandLing`; 42–44 | 183–185 | `mandBuccal`; 42–44 |
| 31 | 48 | 186–188 | `mandLing`; 45–47 | 189–191 | `mandBuccal`; 45–47 |

The lower `local slots q0–q2` formula may look forward relative to loop execution: the C++ loops visit the indices in reverse and assign local indices 47 down to 0. The final lookup is nevertheless q0→lowest local slot of that tooth group and q2→highest local slot (`PerioView.cpp:433-457,463-489`).

## Local chart geometry and x placement

`PerioChartItem.cpp:5-8,33-59,68-75` gives source-verified geometry:

- Local chart bounds: `(0,0,1120,166)` for the default `PerioChartItem()` used by all four surfaces.
- Local baseline: `y = 105`.
- Vertical coefficient: 3 display units per measurement unit.
- GM point: `y = 105 + 3 * GM`.
- CAL point: `y = 105 - 3 * CAL`.
- The default constructor is `even=true`; therefore 48 points have even x spacing. `evenOffset = 1120/48 = 23.333…`; point `j` is at `x = (j + 0.5) * 1120/48`, i.e. point 0 at 11.667 and point 47 at 1108.333.
- The alternative `even=false` path and its `getWidth`/`calculateOffset` tooth-type widths (54/36/36) do not apply to this PerioView: every chart is constructed with the default constructor (`PerioView.cpp:625,629,641,647`).

The tooth scene independently uses 16 slots of 70 units each: `PerioScene.cpp:14-41` sets every tooth item's width to 70 and advances by its `boundingRect().width()`. `ToothGraphicsItem.cpp:7-23,117-123` shows the actual tooth image width is 54 or 36 centered within the 70-unit slot. Three chart points therefore span each 70-unit tooth slot; the contour spacing is fixed/even and does not use those tooth image widths.

### Source-derived final left/middle/right order of triplet offsets

“Left/middle/right” here is the order of the three contour vertices across the visible chart, not a claim about mesial/distal anatomy.

| Surface shown by UI construction comment | Chart item | Effective horizontal transform | q0 | q1 | q2 | Evidence status |
|---|---|---|---|---|---|---|
| Maxillary buccal/facial | `maxBuccal` | none | left | middle | right | `SOURCE_VERIFIED` geometry; no live sentinel |
| Maxillary palatal/oral | `maxPalatal` | x reflection and 180° rotation compose to no net x reversal | left | middle | right | `SOURCE_VERIFIED` transform composition; no live sentinel |
| Mandibular UI buccal row (source comment) | `mandLing` | y reflection plus 180° rotation | right | middle | left | `SOURCE_VERIFIED` transform composition; enum/comment conflict retained |
| Mandibular UI lingual row (source comment) | `mandBuccal` | x and y reflection | right | middle | left | `SOURCE_VERIFIED` transform; enum/comment conflict retained |

Transform declarations are at `PerioView.cpp:620-654`; chart vertices and their local x ordering are at `PerioChartItem.cpp:33-59`. Composition above is a mathematical reading of the declared reflections/rotation, not a screenshot or runtime observation. Thus lower q order is source-derived but not `RUNTIME_VERIFIED`.

## Surface labels and canonical sites

QDento constructs these input rows:

- Maxilla: the first triplet is explicitly commented `buccal` (`PerioView.cpp:355-379`); the second is `palatal` (`:383-407`).
- Mandible: the first triplet is explicitly commented `buccal row` (`:431-457`); the second `lingual` (`:459-489`). Their contour items are named `mandLing` and `mandBuccal`, respectively, as noted above.
- Canonical names/order supplied by the approved design are facial `MB, B, DB`, then oral `ML, L, DL` (§5). QDento source uses none of these labels in its periodontal model/view.

Consequently, the following claims remain `UNRESOLVED`:

1. Whether q0/q1/q2 on a QDento buccal/facial row mean MB/B/DB, DB/B/MB, or another order.
2. Whether q0/q1/q2 on a QDento palatal/lingual row mean ML/L/DL, DL/L/ML, or another order.
3. Whether the mandible's contradictory UI row comments and `ChartPosition` names reflect only contour placement or a site/surface swap in any quadrant.
4. A runtime-confirmed association between any of the 192 indices, a canonical named site, and a visible contour point.

No named clinical site is assigned to q0/q1/q2 in the ledger. The site name column required for a later portable contract must stay blocked until these questions are resolved.

## Existing screenshot and runtime checks

- `screenshots/scr1.png` is `SCREENSHOT_OBSERVED`: it shows the upper periodontal view with tooth labels, six measurement rows, BOP icons and contours. It is not an asymmetric sentinel fixture, has no documented source index attached to its plotted values, and does not establish a named-site mapping.
- Other committed screenshots (`scr0.png`, `scr2.png`, `scr3.png`) do not provide a labeled asymmetric contour/index capture for this task.
- No QDento runtime was launched and no sentinel values were entered. `qmake`/`qmake6` are not available in the execution environment; the repository README identifies Qt 6.8+ as a requirement. No source edits were made for testing.
- The requested 1/4/8 sentinel at one tooth per arch/surface remains `UNRESOLVED` and should be captured when a working Qt runtime exists. It can verify index-to-vertex visual order, but cannot by itself prove what QDento's undocumented vertex labels mean; that semantic link requires explicit label evidence or an approved clinical mapping.

## Source anomaly to carry forward

In the mandibular buccal-loop block, the CAL spin box is constructed with `ui.maxilla` as parent but inserted into `ui.mandibula->ui.calUpLayout` (`PerioView.cpp:441-444`). This looks like a parent-widget mismatch. It does not change the `chartIndex` arithmetic, but runtime layout behavior remains unverified. Do not “fix” it as part of a geometry implementation or use it to infer site order.

## Handoff matrix

| Question | Result | Status |
|---|---|---|
| All 192 PD/CAL/GM/BOP offsets accounted for? | Yes; formulas plus expanded 32-tooth ledger | `SOURCE_VERIFIED` |
| QDento tooth index → FDI? | Yes; explicit source array | `SOURCE_VERIFIED` |
| Offset → chart item/local slot? | Yes; table/formulas include the lower reverse-loop expansion | `SOURCE_VERIFIED` |
| Local slot → visible left/middle/right? | Source transform-derived; upper q0→left, lower q0→right | `SOURCE_VERIFIED`, not runtime-verified |
| Local vertex → canonical named site? | Not established by QDento source or available labeled runtime evidence | `UNRESOLVED`, `BLOCKS_GEOMETRY` |
| Asymmetric runtime sentinel? | Not run; Qt build/runtime unavailable | `UNRESOLVED` |
| Does this meet final geometry acceptance? | No; named-site association is still blocking | **No** |

## Evidence locations

- `src/View/Widgets/PerioView.h:16-48` — chart enum/index map and measurement arrays.
- `src/View/Widgets/PerioView.cpp:94-105,240-280,292-320,351-512,620-654` — event/storage linkage, tooth grouping, all four site-surface loops, and transforms.
- `src/View/Graphics/PerioChartItem.cpp:5-59,68-75,91-118` — bounds, local point construction, equation and line rendering.
- `src/View/Graphics/PerioScene.cpp:7-45,47-68` — tooth scene order and lower tooth-index reversal.
- `src/View/Graphics/ToothGraphicsItem.cpp:7-23,117-123` — image widths and 70-unit slot resize behavior.
- `src/Model/Dental/ToothUtils.cpp:6-10,18-27` — tooth-index/FDI association and tooth type helper.
- `src/Model/Dental/PerioToothData.cpp:19-26` and `src/Model/Dental/PerioStatus.h:21-29` — six packed readings/tooth and stored fields.
- `docs/superpowers/specs/2026-09-30-qdento-periodontal-ios-design.md:90-109` — canonical site vocabulary and grouping.
- `screenshots/scr1.png` — existing upper-chart screenshot, not a sentinel/index trace.
