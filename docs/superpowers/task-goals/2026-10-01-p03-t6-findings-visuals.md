[$superpowers](app://plugins~Plugin_60aea7460bd4819199fd97a9553a5e12)
[@Build iOS Apps](plugin://build-ios-apps@openai-curated-remote)

# TASK GOAL PACKET — PLAN 03 / TASK 6
# STATIC FMPS/FMBS WEDGES + BOP MARKERS + PLAN 03 VISUAL ACCEPTANCE

## TASK TYPE

PROMPT for one dedicated Codex controller instance.

This is the FINAL Plan 03 implementation task.

It fills the row spaces intentionally reserved by Task 5 with:
1. read-only FMPS wedges;
2. read-only FMBS wedges;
3. read-only BOP site markers.

It uses the already accepted Domain, Parity, Geometry, and static-chart shell.

It does NOT add editing/toggle interaction.

## REQUIRED WORKFLOW

Before implementation:

1. Invoke Superpowers `using-superpowers`.
2. Invoke `using-git-worktrees`.
3. Invoke `subagent-driven-development`.
4. Use `verification-before-completion`.
5. Use Build iOS Apps.
6. Read/use:
   - `swiftui-ui-patterns`
   - `swiftui-view-refactor`
   - `ios-debugger-agent`
   - `ios-simulator-browser`
7. Use XcodeBuildMCP/current build-test workflow.
8. Perform real Simulator visual inspection/capture. If Computer/Device Hub are unavailable in the normal Codex environment, use the available simulator browser/XcodeBuildMCP interaction and screenshot tooling. Build/tests alone are not enough.

## MODEL / SUB-AGENT POLICY

You are the controller.

ALL sub-agents MUST use GPT-6 Luna only.
Never use GPT-6 Sol for a sub-agent.
No nested sub-agents.

Recommended fresh roles:
1. Wedge View Implementer — strict focused tests.
2. BOP Marker View Implementer — strict focused tests.
3. QDento Source Visual Reviewer — read-only.
4. Domain/Fixture Mapping Reviewer — read-only.
5. Static Chart Integration Reviewer — read-only.
6. Simulator Visual Reviewer — read-only if tooling allows; otherwise controller captures and reviewer inspects evidence.
7. Final SwiftUI Quality Reviewer — read-only.

Keep writes serialized through the controller.

## TARGET REPOSITORY / BASE

Repository:
`Centaurioun/periodontal-ios`

Exact accepted Task 5 HEAD:
`ec2108e90e9cd0a4807c0bd528318c5ad2dfb2af`

Create NEW isolated worktree/branch:

`ui/p03-t6-bop-wedges`

Do NOT work on the Task 5 branch directly.
Do NOT merge.
Do NOT open a PR.

## AUTHORITATIVE INPUTS

Read first:
- accepted Task 5 chart files;
- `PeriodontalIOS/Domain/FullMouthWedge.swift`;
- `PeriodontalIOS/Geometry/FullMouthWedgeGeometry.swift`;
- `PeriodontalIOS/Geometry/BOPGeometry.swift`;
- F06/F07 parity fixtures;
- `docs/contracts/rendering-parity-contract-v1.1.md`.

Read QDento source/evidence:
- `src/View/Graphics/PerioGraphicsButton.cpp/.h`;
- `src/View/Widgets/PerioView.cpp` `initializeFullMouth` + BOP construction;
- `src/View/Widgets/PerioStatusView.ui`;
- `docs/periodontal-ios/research/runtime/B-full-mouth-findings.md`
  from `research/p01-t6-parity-contract-synthesis`;
- local `screenshots/scr1.png` if available.

## FIXTURE SELECTION

Extend the existing fixture selector to expose exactly:

- F01
- F02
- F06
- F07

Default remains F01.

Do NOT expose F08 yet.

Why:
- F06 is the six-site BOP sentinel;
- F07 is the FMPS/FMBS wedge sentinel;
- all are complete 32-natural-tooth exams compatible with Task 5 contours.

Do not move fixture selection into Domain.

# PART A — FMPS/FMBS WEDGE VIEW

Create:
`PeriodontalIOS/Features/PeriodontalChart/FullMouthWedgeView.swift`

It is READ-ONLY.

## Frozen source layout

QDento creates two 30-high rows in the reserved 60-high full-mouth area:

Top row:
FMPS / plaque

Bottom row:
FMBS / bleeding

Each tooth group is source width:
`70.25 × 30`.

Wedge order:
- left
- up
- right
- down

Geometry must come from:
`FullMouthWedgeGeometry`.

Do NOT reproduce polygon math in SwiftUI.

## Source colors

Unselected:
- white

Border:
- dark gray
- ~1 logical point

FMPS selected:
- RGB 204, 228, 247

FMBS selected:
- RGB 255, 146, 148

Disabled source gray is known but current F01/F02/F06/F07 natural fixtures do not require disabled occupancy behavior.

Do not implement hover state on iPhone.

## 70.25 vs 70 chart width

QDento's source full-mouth wedge group is 70.25 wide while the current mobile parity tooth column is 70 wide.

Do NOT silently change accepted 70-point chart/tooth alignment.

NEW PRODUCT RENDERING RULE for this mobile shell:
- render the source 70.25×30 wedge geometry into a 70×30 tooth slot via a uniform horizontal scale factor `70 / 70.25`;
- keep the wedge center/triangle proportions;
- do not allow cumulative 0.25-per-tooth drift across the 1120-wide chart.

Record this explicitly in the Task 6 evidence note as a mobile layout adaptation, not recovered QDento source geometry.

## State mapping

For natural tooth record:
- FMPS reads `record.fullMouthScores.fmps`;
- FMBS reads `record.fullMouthScores.fmbs`.

The four Bool? fields preserve:
- true = selected;
- false = assessed unselected;
- nil = unassessed.

Read-only rendering rule:
- true → source selected color;
- false → white;
- nil → a quiet neutral hatch/dot/secondary fill OR source-like white with a distinct accessibility state.

Prefer visually distinguishing nil from false without inventing a bright clinical color.
F01/F02/F06/F07 use assessed false/true, so the main parity screenshots do not depend on nil styling.

Do NOT calculate plaque or bleeding percentages here.

## Accessibility

Each tooth's wedge region should expose stable identifiers, e.g.:

- `fmps-<FDI>-left`
- `fmps-<FDI>-up`
- `fmps-<FDI>-right`
- `fmps-<FDI>-down`
- `fmbs-<FDI>-left`
- etc.

Accessibility label/value must distinguish:
- selected;
- unselected;
- unassessed.

# PART B — BOP MARKER VIEW

Create:
`PeriodontalIOS/Features/PeriodontalChart/BOPMarkerView.swift`

It is READ-ONLY.

## Horizontal placement

Do NOT derive x positions in the view.

Use accepted:
`BOPGeometry.anchor(...)`
or `BOPGeometry.anchors(...)`.

For each BOP row/surface:
- there are 48 site anchors;
- map each anchor to the corresponding natural tooth/site BOP state;
- draw a marker only for `bop == true`;
- false → no marker;
- nil → no marker but preserve an accessibility value if needed.

Wrong/incomplete/non-natural state must fail softly.

## Vertical placement

QDento source proves BOP is a dedicated 20-high row using an icon button but does not independently freeze a final portable pixel-y/icon offset.

NEW PRODUCT RENDERING RULE:
- center each BOP marker vertically inside the existing 20-point reserved BOP row;
- use the accepted horizontal site anchor exactly;
- do not infer contour-relative y placement.

Record this as a presentation rule, not source-recovered geometry.

## Marker artwork

Do NOT copy `icon_BOP.png` in this task.

Use an independently implemented native/vector blood-drop marker so Plan 03 does not enlarge the GPL raster footprint.

Preferred:
- SwiftUI `Image(systemName: "drop.fill")` if its silhouette is visually adequate;
OR
- a tiny app-owned vector Shape implemented from generic teardrop geometry.

Use red.

Keep the marker compact enough for the 20-high row and centered on its site x anchor.

Do not implement toggle behavior.

## Accessibility

For all 48 sites, expose a stable accessibility element/value even when marker is absent if practical.

Identifiers:
`bop-<FDI>-<SITE>`

Value:
- positive
- negative
- unassessed

The F06 UI test must prove canonical MB is positive and the five other FDI11 sites are negative.

# PART C — STATIC CHART INTEGRATION

Modify Task 5 `ArchChartView.swift` only in the reserved areas:

1. Replace the 60-high `full-mouth-reserved` blank with a two-row FMPS/FMBS view.
2. Replace upper/facial 20-high BOP blank with BOP marker row for facial.
3. Replace lower/oral 20-high BOP blank with BOP marker row for oral.

Do NOT change:
- row order;
- label rail widths/heights;
- 1120×332 scene;
- numeric rows;
- contour code;
- tooth visuals;
- horizontal scroll behavior.

Update row labels if needed so the full-mouth rows are clear:
- top: FMPS
- bottom: FMBS

The 60-high area may have two 30-high labels rather than one 60-high combined label if this improves source mental-model parity.

Do not add percentages.

# PART D — F06 BOP SENTINEL

Expose F06 in the picker.

F06:
- FDI11 MB BOP = true;
- DB/B/ML/L/DL = false;
- FMBS all false;
- FMPS all false.

Required UI/runtime assertions:
- `bop-11-MB` value = positive;
- the other five FDI11 BOP identifiers = negative;
- no FMBS wedge for FDI11 is selected.

Visual acceptance:
- scroll so the FDI11 header and the positive BOP marker are visible in the SAME evidence frame;
- capture this frame;
- ensure the marker horizontally aligns with the MB site/numeric column.

# PART E — F07 WEDGE SENTINEL

Expose F07 in the picker.

F07 at FDI11:
- FMPS.left = true;
- FMPS.up/right/down = false;
- FMBS.down = true;
- FMBS.left/up/right = false;
- all BOP = false.

Required UI/runtime assertions:
- only `fmps-11-left` is selected;
- only `fmbs-11-down` is selected;
- all six FDI11 BOP values are negative.

Visual acceptance:
- capture a frame where the FDI11 header and its FMPS/FMBS wedges are simultaneously visible;
- selected plaque wedge uses source blue;
- selected bleeding wedge uses source pink/red;
- unselected wedges remain white;
- triangle geometry/orientation matches the accepted source geometry.

# PART F — STRUCTURAL TESTS

Create:
`PeriodontalIOSUITests/StaticFindingsFlowTests.swift`

At minimum:
1. launch;
2. switch to F06;
3. verify FDI11 MB BOP positive and all other FDI11 BOP negative;
4. verify F06 FMPS/FMBS are all unselected;
5. switch F07;
6. verify FDI11 FMPS left only selected;
7. verify FDI11 FMBS down only selected;
8. verify all FDI11 BOP negative;
9. switch lower arch and verify BOP/wedge elements remain present/aligned for representative lower tooth;
10. return upper; verify no state leakage across fixture switches.

Keep Task 5 UI tests passing.

If useful, add small unit tests around pure view-state adapters, but do not create a parallel presentation-domain.

# PART G — SIMULATOR VISUAL ACCEPTANCE

Mandatory.

Use iPhone 18 Pro / iOS 27.0.

Landscape primary.

Exercise:
- F01;
- F06;
- F07;
- upper/lower switching;
- horizontal/vertical scrolling.

## Required fresh screenshots

Create:
`docs/superpowers/evidence/p03-t6-findings/F06-upper-bop-fdi11-landscape.png`

This frame MUST show:
- FDI11 tooth-number header;
- the relevant BOP row;
- the positive MB marker;
- enough adjacent site/tooth context to assess horizontal alignment.

Create:
`docs/superpowers/evidence/p03-t6-findings/F07-upper-wedges-fdi11-landscape.png`

This frame MUST show:
- FDI11 header;
- the FMPS and FMBS rows;
- selected FMPS-left and FMBS-down wedges.

Create:
`docs/superpowers/evidence/p03-t6-findings/F01-lower-findings-landscape.png`

Use as a lower-arch baseline alignment check.

Optional portrait smoke capture if useful.

## Evidence note

Create:
`docs/superpowers/evidence/2026-10-01-p03-t6-findings-visual-review.md`

Record:
- exact implementation HEAD;
- simulator/runtime;
- each screenshot state;
- BOP marker rendering rule;
- wedge 70.25→70 horizontal scaling rule;
- source color values;
- direct simulator interaction performed;
- any visual mismatch and remediation;
- explicit statement that views remain read-only;
- no BOP PNG copied.

# QDENTO STRUCTURAL COMPARISON

If local source screenshot exists:
`/Users/yusuf/Repos/QDento/screenshots/scr1.png`

Use it structurally for:
- two full-mouth rows;
- BOP row placement;
- wedge border/selected-fill character;
- relationship to surrounding numeric rows.

Do not claim pixel-perfect parity.

# SIX-CYCLE REFINEMENT

Exactly six cumulative cycles:

1. Accuracy & Fundamental Correction
2. Completeness & Gap Analysis
3. Structure & Architecture
4. Adversarial / Critical Review
5. Usability & Goal Fit
6. Final Synthesis, Regression Check & Polish

Cycle 4 must include F06/F07 simulator inspection.
Cycle 5 must include actual scrolling + upper/lower + F01/F06/F07 switching.

No seventh cycle.

# REVIEW FOCUS

Reviewers must challenge:
1. Are wedge vertices consumed from FullMouthWedgeGeometry rather than re-derived?
2. Is 70.25→70 adaptation explicit and free of cumulative drift?
3. Are wedge directions still neutral left/up/right/down?
4. Are FMPS/FMBS state sources independent?
5. Are source selected colors exact?
6. Does BOP x come only from BOPGeometry?
7. Is BOP vertical centering explicitly a NEW_PRODUCT rendering rule rather than falsely source-verified?
8. Is BOP independent from FMBS?
9. Does F06 identify MB only?
10. Does F07 select FMPS-left and FMBS-down only?
11. Were Task 5 numeric/contour/tooth relationships left unchanged?
12. Was no QDento BOP raster copied?
13. Do screenshots include FDI11 header in the same frame as each sentinel visual?

# ALLOWED PRODUCT FILES

Create:
- `PeriodontalIOS/Features/PeriodontalChart/FullMouthWedgeView.swift`
- `PeriodontalIOS/Features/PeriodontalChart/BOPMarkerView.swift`
- `PeriodontalIOSUITests/StaticFindingsFlowTests.swift`
- Task 6 evidence files.

Modify:
- `PeriodontalIOS/App/AppModel.swift` for bounded F06/F07 fixture selection;
- `PeriodontalIOS/Features/PeriodontalChart/PeriodontalChartScreen.swift`;
- `PeriodontalIOS/Features/PeriodontalChart/ArchChartView.swift`;
- Xcode project registration as required.

Prefer NOT to modify:
- `ToothChartColumn.swift`;
- `PeriodontalContourView.swift`;
unless integration truly requires it.

Do NOT modify accepted:
- Domain;
- Parity;
- Geometry;
- Rendering/provider;
- prototype raster assets.

STOP and report before changing an accepted lower layer.

# VERIFICATION

Run:
1. focused Task 6 UI/unit tests;
2. existing Task 5 UI flow;
3. all Rendering tests;
4. all Geometry tests;
5. Domain + Parity regression;
6. full unit suite;
7. full UI-test suite;
8. app build;
9. real Simulator interaction;
10. fresh screenshot capture/review;
11. `git diff --check`.

# COMMIT

Commit:

`feat: add static periodontal findings visuals`

Push:
`ui/p03-t6-bop-wedges`

Do NOT merge.
Do NOT open a PR.

## PLAN 03 STOP

Do NOT begin Plan 04.

Return the required handoff.
The coordinator will perform Plan 03 acceptance/freeze after Task 6.

# STOP CONDITIONS

STOP rather than improvising if:
- exact Task 5 base cannot be checked out;
- accepted geometry cannot align wedge/BOP visuals without changing lower layers;
- F06/F07 fixture facts conflict with source/domain;
- real simulator visual inspection cannot be obtained;
- Task 5 regression breaks and cannot be fixed within Task 6 feature scope.

# RETURN FORMAT

Return only:
- STATUS: DONE / DONE_WITH_CONCERNS / BLOCKED
- repository
- local worktree path
- feature branch
- base commit
- final HEAD
- files changed
- Build iOS Apps skills used
- simulator/tooling use summary
- sub-agent roles/models used
- TDD/RED evidence
- F06 BOP mapping verification
- F07 wedge mapping verification
- wedge geometry/color verification
- BOP anchor/marker verification
- 70.25→70 adaptation verification
- fixture selector verification
- accessibility/UI-test result
- Task 5 UI regression result
- screenshot/evidence paths
- QDento structural comparison summary
- Rendering regression result
- Geometry regression result
- Domain + Parity regression result
- full unit-test result
- full UI-test result
- build result
- independent review verdict(s)
- visual findings/remediations
- concerns/blockers
- verification summary
- controller rulings, if any
