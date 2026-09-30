[$superpowers](app://plugins~Plugin_60aea7460bd4819199fd97a9553a5e12)
[@Build iOS Apps](plugin://build-ios-apps@openai-curated-remote)

# PLAN 03 / TASK 2 — SCOPED GEOMETRY REMEDIATION

Resume the existing Task 2 controller context if possible.

Do NOT begin Plan 03 Task 3.

Target repository:
`Centaurioun/periodontal-ios`

Existing branch:
`geometry/p03-t2-contour-engine`

Current candidate HEAD:
`0e1229da31a7f2fe4bcad751363b4d50d14e5732`

Read the coordinator review first from:
`Centaurioun/QDento`
branch:
`origin/docs/periodontal-ios-brainstorming`

file:
`docs/superpowers/reviews/2026-10-01-p03-t2-contour-preacceptance-review.md`

## Finding

`ContourGeometry.contour(... layout:)` uses the supplied layout for:
- y coordinates;
- baseline start/end;

but ToothGeometry x helpers always use `ChartLayout.qdento.width`.

This makes custom ChartLayout internally inconsistent.

## Required fix

Prefer propagating `ChartLayout` through the x-geometry API.

Required behavior:

- default remains `.qdento`;
- point count remains 48;
- spacing for a layout = `layout.width / 48`;
- source local x = `(j + 0.5) * layout.width / 48`;
- mandibular visible x = `layout.width - localX`;
- maxillary visible x = localX;
- ContourGeometry passes its exact supplied layout into every x helper as well as y helpers.

A clean API shape could be equivalent to:

```swift
static func pointSpacing(layout: ChartLayout = .qdento) -> Double
static func sourceLocalX(pointIndex: Int, layout: ChartLayout = .qdento) -> Double?
static func visibleX(sourcePointIndex: Int, on surface: ChartSurface, layout: ChartLayout = .qdento) -> Double?
static func visibleX(tooth: ToothID, slot: QDentoTripletSlot, on surface: ChartSurface, layout: ChartLayout = .qdento) -> Double?
```

Exact spelling may vary.

Do NOT change:
- 48-point topology;
- source tooth order;
- q-slot mapping;
- GM/CAL formulas;
- QDento default values;
- Domain or Parity code.

## TDD

Add RED regression coverage first using a custom layout with width different from 1120, preferably 560.

Verify:

1. custom spacing = 560/48;
2. local point0 x = 560/96;
3. local point47 x = 560 - 560/96;
4. maxillary visible x uses custom width;
5. mandibular reflection uses `560 - localX`;
6. a complete custom-layout contour has baselineEnd.x=560;
7. its first/last measurement x values are 560/96 and 560-560/96;
8. all 48 x values are strictly increasing and inside the same custom layout bounds.

Demonstrate RED against the current implementation, then GREEN after the minimal fix.

## ALLOWED CHANGES

Only:
- `PeriodontalIOS/Geometry/ToothGeometry.swift`
- `PeriodontalIOS/Geometry/ContourGeometry.swift`
- `PeriodontalIOSTests/Geometry/ContourGeometryTests.swift`

Do not modify:
- ChartSurface unless strictly necessary; if you believe it is necessary, STOP and explain;
- QDentoDisplayAdapter;
- Domain;
- Parity;
- UI;
- Xcode project.

## WORKFLOW / REVIEW

Use:
- using-superpowers
- subagent-driven-development
- verification-before-completion
- Build iOS Apps/XcodeBuildMCP

All sub-agents GPT-6 Luna only, never Sol.

Use:
1. fresh remediation implementer;
2. fresh read-only geometry API reviewer.

Reviewer must verify both:
- custom layout is honored coherently;
- default `.qdento` geometry and F02/F03 oracles did not change.

## VERIFICATION

Run:
1. focused ContourGeometryTests;
2. all Geometry tests;
3. Domain + Parity regression;
4. full unit suite;
5. UI launch regression;
6. app build;
7. git diff --check.

Commit separately:

`fix: honor contour layout across x geometry`

Push SAME branch:
`geometry/p03-t2-contour-engine`

Do NOT merge.
Do NOT begin Task 3.

Return only:
- STATUS
- new HEAD
- changed files
- TDD RED evidence
- custom-layout verification
- default QDento geometry verification
- focused contour-test result
- all Geometry test result
- Domain + Parity regression result
- full unit-test result
- UI regression result
- build result
- fresh reviewer verdict
- git/status/push verification
- concerns/blockers
