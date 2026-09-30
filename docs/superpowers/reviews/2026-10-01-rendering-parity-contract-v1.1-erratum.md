# Rendering / Parity Contract v1 → v1.1 Erratum

Date: 2026-10-01

Status: ACCEPTED ERRATUM BEFORE PLAN 03

## Problem

The frozen v1 contract contained an internal inconsistency in §3.

The coordinator batch review correctly froze screen anatomy as:

- Q1 and Q4 visible facial L/M/R = DB / B / MB;
- Q2 and Q3 visible facial L/M/R = MB / B / DB;
- oral equivalents use DL/L/ML or ML/L/DL respectively.

The v1 contract's q0/q1/q2 mappings were also consistent with that ruling.

However, the v1 table's **New-product visible left / middle / right** cells for Q3 and Q4 were accidentally swapped.

## Corrected cells

| Quadrant | Surface | Correct visible L/M/R |
|---|---|---|
| Q3 | facial | MB / B / DB |
| Q3 | oral | ML / L / DL |
| Q4 | facial | DB / B / MB |
| Q4 | oral | DL / L / ML |

The q mappings do not change:

- Q3 facial q0/q1/q2 = DB/B/MB, while QDento renders q0 right and q2 left;
- Q3 oral q0/q1/q2 = DL/L/ML, while QDento renders q0 right and q2 left;
- Q4 facial q0/q1/q2 = MB/B/DB, while QDento renders q0 right and q2 left;
- Q4 oral q0/q1/q2 = ML/L/DL, while QDento renders q0 right and q2 left.

## Impact

- Plan 02 domain code: no impact.
- Plan 02 parity fixtures: no impact; current F02–F07 named-site translation targets FDI 11 (Q1) and remains correct.
- Plan 03 geometry: MUST consume v1.1, not the erroneous Q3/Q4 visible-order cells in v1.
- The original v1 remains immutable for audit history.

## Authority

This is a documentation correction restoring consistency with the already-approved coordinator ruling. It is not a new clinical product decision.
