# Plan 03 / Task 2 Coordinator Pre-Acceptance Review

Date: 2026-10-01

Status: REMEDIATION_REQUIRED

Candidate:
- repo: `Centaurioun/periodontal-ios`
- branch: `geometry/p03-t2-contour-engine`
- HEAD: `0e1229da31a7f2fe4bcad751363b4d50d14e5732`

## Independent verification

The candidate is exactly one commit ahead of the accepted pre-geometry base and changes only the intended geometry/test files plus Xcode registration.

Default QDento geometry is correct:
- 1120×166 logical chart;
- baseline 105;
- scale 3;
- 48 evenly spaced points;
- mandibular whole-arch horizontal reversal;
- corrected F02 CAL-y oracle;
- typed malformed-input errors.

## Finding — custom ChartLayout is internally inconsistent

Severity: IMPORTANT

`ContourGeometry.contour(for:surface:layout:)` accepts a caller-supplied `ChartLayout`.

Y coordinates and baseline endpoints use that supplied layout.

However x coordinates are computed through `ToothGeometry.sourceLocalX` / `visibleX`, which always use:
`ChartLayout.qdento.width`.

Therefore a non-default layout can produce a path whose:
- baseline end uses the custom width;
- measurement x coordinates still occupy the 1120-wide QDento coordinate space.

This violates the API's own layout parameter and creates a downstream scaling footgun.

## Required correction

Choose ONE coherent model and make it explicit:

Preferred:
- propagate `ChartLayout` through ToothGeometry x/spacing helpers;
- keep default argument `.qdento`;
- calculate spacing as `layout.width / 48`;
- calculate local x using the supplied layout;
- reflect mandibular x using `layout.width`;
- make ContourGeometry pass the same layout through to all x and y calculations.

Alternative:
- if custom layout is intentionally unsupported, remove the custom layout API entirely and make QDento logical coordinates explicit/fixed.

Do NOT leave a parameter that is only half honored.

Given the existing API and downstream usefulness, the preferred layout-propagation fix should normally be used.

## Required regression tests

Add a non-QDento custom layout test, e.g. width 560 (other values may remain proportional or arbitrary), proving:
- point spacing is width/48;
- first x is width/96;
- last x is width-width/96;
- mandibular reflection uses the custom width;
- ContourGeometry baseline end.x equals custom width;
- all 48 path x values fit the same supplied layout and remain increasing.

Default .qdento tests must remain unchanged and passing.

## Verdict

**REMEDIATION_REQUIRED**

Do not start Task 3 until this is fixed and independently re-reviewed.
