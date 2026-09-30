# P01-T3-F02 CAL-y Fixture Erratum

Date: 2026-10-01

Status: ACCEPTED ARITHMETIC CORRECTION BEFORE PLAN 03 / TASK 2

## Source-backed formula

QDento `src/View/Graphics/PerioChartItem.cpp` defines:

- `y_pos = 105`
- `y_coef = 3`
- CAL vertex y = `105 - 3 × CAL`

## F02 source values

Fixture `P01-T3-F02` defines source CAL slots:

`[1, 3, 6, 5, 2, 4]`

Therefore the correct source-local CAL y values are:

- slot0: 105 - 3×1 = 102
- slot1: 105 - 3×3 = 96
- slot2: 105 - 3×6 = 87
- slot3: 105 - 3×5 = 90
- slot4: 105 - 3×2 = 99
- slot5: 105 - 3×4 = 93

Correct array:

`[102, 96, 87, 90, 99, 93]`

## Error

The original Plan 01 fixture artifact and the initial Swift parity fixture catalog accidentally recorded slot4 as 87:

`[102, 96, 87, 90, 87, 93]`

That contradicts both the source CAL value (2) and the frozen source formula.

## Impact

- canonical F02 PD/CAL/clinical-GM data: correct; no change;
- named-site translation: correct; no change;
- recession expectations: correct; no change;
- GM-y expectations: correct; no change;
- only `sourceLocalCALY.slot4` is wrong;
- Plan 03 Task 1 display/site adapter: unaffected;
- Plan 03 Task 2 contour geometry MUST use the corrected array.

The original Plan 01 evidence artifact remains unchanged for audit history. This erratum is the downstream authority for the corrected expected CAL-y value.
