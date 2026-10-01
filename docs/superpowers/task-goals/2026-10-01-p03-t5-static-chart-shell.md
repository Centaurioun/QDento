[$superpowers](app://plugins~Plugin_60aea7460bd4819199fd97a9553a5e12)
[@Build iOS Apps](plugin://build-ios-apps@openai-curated-remote)
[@Computer](plugin://computer-use@openai-bundled)
[@Device Hub](plugin://computer-use@openai-bundled?app=com.apple.dt.Devices)

# TASK GOAL PACKET — PLAN 03 / TASK 5
# STATIC PERIODONTAL ARCH / CHART SHELL + SIMULATOR VISUAL ACCEPTANCE

## TASK TYPE

PROMPT for one dedicated Codex controller instance.

This is the first task that integrates the accepted Domain, Parity, Geometry, and Rendering layers into a real iPhone periodontal chart screen.

The result is READ-ONLY.

This task owns:
- debug/demo fixture selection;
- upper/lower arch selection;
- static QDento-like row structure;
- tooth headers and tooth artwork;
- PD/CAL/QDento-display-GM numeric rows;
- attached gingiva and derived recession rows;
- mobility/furcation read-only presentation;
- source-like 1120×332 tooth/contour scene;
- GM/CAL contour drawing;
- horizontal + vertical mobile scrolling;
- accessibility identifiers;
- simulator screenshots and direct visual inspection.

This task does NOT own:
- editable controls;
- BOP marker rendering;
- FMPS/FMBS wedge rendering;
- summary/risk calculations;
- persistence;
- Stage/Grade;
- zoom/pinch;
- production redesign.

Task 6 will fill the reserved FMPS/FMBS and BOP visual rows.

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
7. Use the current XcodeBuildMCP build/test workflow.
8. Use **Device Hub** to launch/select the real iPhone simulator and run the app.
9. Use **Computer** to inspect and interact with the actual Simulator UI, including scrolling and fixture/arch selection.

Do NOT accept the task from tests/build alone.
Visual simulator evidence is mandatory.

## MODEL / SUB-AGENT POLICY

You are the controller.

ALL sub-agents MUST use GPT-6 Luna only.
Never use GPT-6 Sol for any sub-agent.
No nested sub-agents.

Recommended fresh roles:
1. Static Chart Implementer — primary writer, strict TDD where practical.
2. QDento Row/Layout Reviewer — read-only.
3. Contour Integration Reviewer — read-only.
4. Tooth/FDI Alignment Reviewer — read-only.
5. Accessibility/UI-Test Reviewer — read-only.
6. Simulator Visual Reviewer — read-only if Computer/Device Hub are available to that reviewer; otherwise controller performs visual inspection and reviewer inspects evidence.
7. Final SwiftUI Quality Reviewer — read-only.

Keep shared-file writes serialized through the controller.

Critical/Important findings require remediation before acceptance.

## TARGET REPOSITORY / BASE

Repository:
`Centaurioun/periodontal-ios`

Exact accepted Task 4 HEAD:
`ed54a926e4f041b84589f4f9885e0db7eb1d3b9d`

Create a NEW isolated branch/worktree:

`ui/p03-t5-static-chart-shell`

Do NOT work on prior accepted branches.
Do NOT merge.
Do NOT open a PR.
Do NOT begin Task 6.

## AUTHORITATIVE INPUTS

Read first in iOS repo:
- `docs/contracts/rendering-parity-contract-v1.1.md`
- all accepted Domain/Parity/Geometry/Rendering files;
- `docs/provenance/QDENTO_REFERENCE.md`;
- current `AppModel.swift`, app root, and launch UI test.

Read QDento read-only:
- `src/View/Widgets/PerioStatusView.ui`
- `src/View/Widgets/PerioView.ui`
- `src/View/Widgets/PerioView.cpp`
- `src/View/Graphics/PerioScene.cpp`
- `src/View/Graphics/PerioChartItem.cpp`
- local reference screenshot if available:
  `/Users/yusuf/Repos/QDento/screenshots/scr1.png`

The QDento screenshot is STRUCTURAL visual reference only.
It does not override the accepted v1.1 named-site mapping or fixture values.

# MOBILE PARITY STRATEGY

For this first static shell:

- display ONE arch at a time;
- default to upper/maxillary;
- use native Upper / Lower segmented selection;
- validate first in landscape;
- DO NOT hard-lock app orientation;
- keep the chart's clinically useful logical scale;
- DO NOT squeeze all 16 teeth into the viewport;
- horizontal scroll is required for the 1120-wide chart;
- vertical scroll is allowed/required for the ~740-high row stack;
- no pinch-to-zoom;
- no zoom buttons.

The purpose is correctness and QDento mental-model parity, not final mobile redesign.

# DEBUG FIXTURE SELECTION

Modify `AppModel` to expose a bounded static demo fixture selection.

For Task 5, the required selectable fixtures are ONLY:

- F01 healthy baseline;
- F02 asymmetric contour.

Why only F01/F02:
- both are complete 32-natural-tooth exams compatible with the accepted complete-input contour engine;
- F08 contains missing/implant positions and contour-gap behavior has NOT been frozen;
- do not invent missing/implant contour semantics in Task 5.

Render `fixture.expectedExam`.

Use a DEBUG/demo-only selector in the screen.
Do not add fixture selection to Domain.

Default:
- F01;
- maxillary arch.

# SOURCE-LIKE ROW STRUCTURE

QDento `PerioStatusView.ui` uses zero-spacing rows in this order:

1. Tooth header
2. Mobility
3. Furcation
4. Full-mouth findings area (FMPS/FMBS) — 60 high
5. Attached gingiva upper/facial side
6. Recession upper/facial side
7. GM upper/facial side
8. BOP upper/facial side
9. CAL upper/facial side
10. PD upper/facial side
11. central tooth + two-contour scene — ~332 high
12. PD lower/oral side
13. CAL lower/oral side
14. BOP lower/oral side
15. GM lower/oral side
16. Recession lower/oral side
17. Attached gingiva lower/oral side

Preserve this mental model.

Task 5 MUST reserve:
- the 60-high FMPS/FMBS area;
- the two 20-high BOP rows;

but MUST NOT render wedge controls or BOP markers yet.
Task 6 owns those visuals.

Do not add visible “coming soon” placeholders.
Use neutral blank reserved areas so the layout does not jump in Task 6.

# SOURCE-LIKE ROW HEIGHTS

Use one centralized static chart metrics definition inside the feature layer.

Recommended source-derived values:

- tooth header: 20
- mobility: 20
- furcation: 60
- full-mouth findings reserved area: 60
- each AG/Recession/GM/BOP/CAL/PD row: 20
- central tooth/contour scene: 332
- tooth slot width: 70
- 16 tooth slots → 1120 logical chart width
- fixed left row-label rail: approximately 90 points

Do not scatter these numbers throughout view bodies.

The 1120×332 scene and 70-unit tooth slots are parity-critical.

# VISIBLE TOOTH ORDER

Never derive display order from PeriodontalExam array order.

Use accepted Geometry:

Maxillary visible left→right:
18,17,16,15,14,13,12,11,21,22,23,24,25,26,27,28

Mandibular visible left→right:
48,47,46,45,44,43,42,41,31,32,33,34,35,36,37,38

Use `ToothGeometry.visibleToothOrder`.

Do not re-invent a second conflicting FDI order table in SwiftUI.

# PER-TOOTH COLUMN ALIGNMENT

Create `ToothChartColumn.swift`.

Each visible tooth occupies EXACTLY:
`70` logical points.

Within the six-site numeric rows, each surface has exactly 3 visible cells per tooth:

cell width:
`70 / 3`

Use:
`QDentoDisplayAdapter.visibleSitesLeftToRight`

for the selected tooth/surface.

Do NOT iterate canonical enum order blindly.

This guarantees the numeric rows align with the accepted contour point centers.

# NUMERIC ROW DISPLAY

Read-only values only.

## PD
Display `probingDepthMM`.

Source-like threshold:
- >=5 → red text;
- otherwise primary/dark text.

## CAL
Display `clinicalAttachmentLevel.valueMM`.

Source-like threshold:
- >=4 → red text.

## GM
QDento UI sign is required.

Do NOT display canonical clinical GM directly.

Use:
`QDentoDisplayAdapter.qdentoDisplayGM(fromClinicalGM:)`

For example:
- canonical +2 → displayed -2;
- canonical -2 → displayed +2.

No red threshold is required for GM.

## Unassessed fallback
If a value is nil:
- render a quiet dash `—`;
- do not silently display 0.

F01/F02 assessed-zero fixtures should display actual `0`.

# ATTACHED GINGIVA

One value per tooth per surface row.

Facial:
- use `facialSurface.attachedGingivaMM`.

Oral:
- mandibular natural tooth → use `oralSurface?.attachedGingivaMM`;
- maxillary natural tooth → aggregate-level NOT APPLICABLE.

For maxillary oral AG:
- render a visually disabled/secondary `—`;
- never synthesize a numeric zero from the legacy persisted QDento slot.

# RECESSION

One derived value per tooth/surface.

Use:
`NaturalToothRecord.recession(on:)`.

Source-like threshold:
- >=1 → red text.

Do NOT persist or recalculate with a second formula in the view.

# MOBILITY / FURCATION

Read-only compact display is allowed.

Mobility:
- natural tooth grade0/1/2/3 → numeric 0/1/2/3;
- nil → `—`.

Furcation:
- do not invent anatomy;
- show a compact read-only representation only for assessed grade values;
- if no assessed furcation grade exists, use `—`;
- do not treat `.notApplicable` as grade0.

The central educational priority remains sites + tooth + contours.

# NON-NATURAL SAFETY

The Task 5 selector does not expose F08.

Nevertheless view adapters must fail softly if a non-natural position reaches a natural measurement row:
- display `—`;
- do not crash;
- do not coerce implant/missing into natural site measurements.

Do NOT attempt a contour if the selected arch is not a complete assessed 16-natural-tooth surface set.

No contour-gap policy is invented here.

# CENTRAL 1120 × 332 SCENE

Create `ArchChartView.swift`.

The horizontally scrollable chart content must be exactly:
- width 1120;
- tooth/contour scene height 332 at the central scene row.

Use 16 `ToothGraphicView` instances, each centered in a 70×332 tooth slot.

At source-like 332 height:
- non-molar tooth canvas renders ~36 points wide;
- molar ~54 points wide.

Do not resize tooth artwork to fill the 70-wide slot.

Place tooth artwork BELOW contour strokes in the ZStack.
Contour strokes should remain visually readable over the tooth reference, matching QDento's chart-item overlay concept.

# CONTOUR INPUT ADAPTER

Do not put domain/geometry calculations directly in SwiftUI `body`.

Build a small feature-layer presentation/preparation value or helper inside the allowed feature files.

For each selected arch and each facial/oral ChartSurface:

1. get the 16 visible ToothIDs from ToothGeometry;
2. fetch each position by ToothID from PeriodontalExam;
3. require a natural tooth record;
4. for the three canonical sites of that surface, require:
   - gingivalMarginMM;
   - CAL value;
5. create `ContourSample`;
6. call accepted `ContourGeometry.contour`.

If any required sample is unavailable:
- surface contour becomes unavailable;
- do not substitute zeros;
- do not crash.

F01/F02 must produce complete paths.

# PERIODONTAL CONTOUR VIEW

Create:
`PeriodontalIOS/Features/PeriodontalChart/PeriodontalContourView.swift`

Use SwiftUI Canvas or an equally focused native path renderer.

Do NOT duplicate contour math.
Consume accepted `ContourPath` outputs.

For each contour polyline:
- start at `baselineStart`;
- append all 48 measurement vertices in order;
- end at `baselineEnd`;
- do not fill/close the path.

Source styling:
- CAL: red;
- GM: dark gray;
- stroke width: 5 logical points;
- round joins;
- antialias/native SwiftUI rendering.

## View-level vertical placement

Horizontal order/reversal is ALREADY handled by accepted `ContourGeometry`.
Do not mirror x again in the view.

Map local 166-high y coordinates into the 332-high scene according to QDento source placement:

### Maxillary facial
`sceneY = localY`

### Maxillary oral
`sceneY = 340 - localY`

### Mandibular facial
This is the first stored mandibular triplet / source `mandLing` item after semantic reconciliation:
`sceneY = localY - 5`

### Mandibular oral
This is the second stored triplet / source `mandBuccal` item:
`sceneY = 332 - localY`

Clip the final central scene to 1120×332, as the QDento scene rect does.

These formulas are view/presentation placement only.
Do NOT change pure ContourGeometry.

## Expected F01 baselines

F01 should visibly produce flat contours at:

Maxillary:
- facial local baseline 105 → scene y105;
- oral local baseline105 → scene y235.

Mandibular:
- facial local baseline105 → scene y100;
- oral local baseline105 → scene y227.

Add tests or debug assertions around these placement values so future refactors cannot silently invert the surfaces.

# SCREEN

Create:
`PeriodontalIOS/Features/PeriodontalChart/PeriodontalChartScreen.swift`

Top controls:
- fixture picker: F01 / F02;
- arch picker: Upper Teeth / Lower Teeth.

Keep controls compact.
Do not imitate desktop patient/smoking/risk panels in this task.

Below:
- the vertically scrollable selected arch chart.

Do not show both arches simultaneously.

# ROOT INTEGRATION

Modify:
- `PeriodontalIOS/App/AppModel.swift`;
- the existing root view file from Plan 02;
- if needed `PeriodontalIOSApp.swift` only for straightforward wiring.

Preserve root accessibility:
`periodontal-root`.

Replace the “Foundation only” placeholder with the real chart screen.

# ACCESSIBILITY IDENTIFIERS

Required:

Screen/root:
- `periodontal-root`
- `fixture-picker`
- `arch-picker`

Selected arch:
- `arch-upper`
- `arch-lower`

Per tooth:
- `tooth-<FDI>`

Contours:
- `contour-maxillary-facial`
- `contour-maxillary-oral`
- `contour-mandibular-facial`
- `contour-mandibular-oral`

Numeric cells should use stable identifiers where practical:

- `pd-<FDI>-<SITE>`
- `cal-<FDI>-<SITE>`
- `gm-<FDI>-<SITE>`
- `ag-<FDI>-facial`
- `ag-<FDI>-oral`
- `recession-<FDI>-facial`
- `recession-<FDI>-oral`

Use canonical raw site strings consistently.

# UI TEST

Create:
`PeriodontalIOSUITests/StaticChartSnapshotFlowTests.swift`

The test must at minimum:

1. launch;
2. verify `periodontal-root`;
3. verify default F01 + upper arch;
4. verify `arch-upper`;
5. verify representative upper teeth exist;
6. switch to Lower Teeth;
7. verify `arch-lower` and representative lower teeth;
8. switch back to Upper;
9. select F02;
10. verify FDI11 cells expose exact asymmetric values through accessibility:
    - DB PD=2, CAL=1, QDento-display GM=1;
    - B PD=5, CAL=3, display GM=2;
    - MB PD=8, CAL=6, display GM=2;
11. verify FDI11 facial recession=0;
12. verify FDI11 oral recession=2;
13. verify upper oral AG is not presented as numeric assessed zero.

Do not write brittle pixel-coordinate UI tests for numeric rows.

# UNIT / STRUCTURAL TESTING

If a small feature-layer pure presentation helper is introduced, add focused unit tests only if they materially improve correctness.

Do not create a large parallel presentation-domain architecture.

At minimum ensure source files/build compile with no geometry calculations in SwiftUI body.

# TASK 6 BOUNDARY

Task 5 must NOT create:
- `FullMouthWedgeView.swift`;
- `BOPMarkerView.swift`.

The 60-high full-mouth area and two BOP rows are reserved blank layout slots only.

Do not implement FMPS/FMBS colors or BOP drops in this task.

# DEVICE HUB + COMPUTER — REQUIRED VISUAL ACCEPTANCE

This is mandatory.

## A. Build and launch
Use Build iOS Apps/XcodeBuildMCP to build.

Use **Device Hub** to:
- select/boot iPhone 18 Pro / iOS 27.0 simulator;
- install/run the current app build.

Use **Computer** to inspect the actual Simulator window.

Do not accept merely because the accessibility tree exists.

## B. Landscape parity inspection

Rotate the simulator to landscape using Device Hub/Simulator controls.

DO NOT change the app to landscape-only.

Visually inspect:

1. label rail remains readable;
2. chart is not squeezed to fit;
3. horizontal scrolling works;
4. vertical scrolling works;
5. 16 tooth slots remain aligned at 70 logical points;
6. natural tooth images are centered and preserve narrow 36/54-like widths relative to their 70 slots;
7. no tooth is stretched to the full 70-slot width;
8. FDI header order is correct;
9. upper oral attached gingiva is visibly not applicable;
10. PD/CAL/GM rows align to the correct tooth/site triplets;
11. contour strokes align horizontally with their corresponding numeric cells;
12. both facial/oral contours surround the tooth strip in the intended two-surface relationship;
13. CAL is red and GM dark gray;
14. no clipping/canvas error truncates Q2/Q3/Q4 tooth graphics.

## C. F01 visual capture

Select:
- F01;
- Upper Teeth.

Scroll horizontally to a useful midline view around 11/21.

Capture a simulator screenshot.

Then inspect Lower Teeth and capture one lower-arch screenshot.

## D. F02 visual capture

Select:
- F02;
- Upper Teeth.

Return to the same useful horizontal region around FDI11.

Capture a screenshot.

Verify visually that FDI11 is asymmetric rather than flat, and that the numeric row values correspond to the visible contour direction.

## E. QDento structural comparison

Using Computer, open/read-only local reference if present:

`/Users/yusuf/Repos/QDento/screenshots/scr1.png`

Compare the source screenshot and iOS simulator structurally for:
- row hierarchy;
- tooth-centered presentation;
- 70-slot / narrow tooth-art relationship;
- two-surface contour placement;
- relationship of numeric rows to tooth/contour.

Do NOT claim pixel-perfect runtime parity.
The QDento screenshot is not the F02 sentinel fixture.

If the screenshot is missing, use primary source/UI files and report that visual source comparison was unavailable.

## F. Portrait smoke check

Rotate back to portrait.
Confirm:
- no crash;
- layout remains scrollable/readable enough for a smoke check.

Portrait optimization is NOT part of Task 5.

# VISUAL EVIDENCE ARTIFACTS

Create:

`docs/superpowers/evidence/p03-t5-static-chart/`

Store at minimum:
- `F01-upper-landscape.png`
- `F01-lower-landscape.png`
- `F02-upper-landscape.png`

Optionally capture portrait smoke evidence if useful.

Also create:

`docs/superpowers/evidence/2026-10-01-p03-t5-static-chart-visual-review.md`

Record:
- simulator device/runtime;
- exact implementation HEAD;
- fixture/arch for each screenshot;
- horizontal scroll region shown;
- Computer/Device Hub inspection notes;
- any visible mismatch;
- whether mismatch was fixed or explicitly remains;
- statement that BOP/wedge visuals are intentionally deferred to Task 6.

Do NOT copy QDento reference screenshots into the iOS repo.

# VISUAL FIX LOOP

If direct Computer inspection finds a visible defect in NEW Task 5 code:
- fix it in this Task 5 branch;
- rerun targeted tests/build;
- re-launch with Device Hub;
- re-inspect with Computer;
- replace evidence screenshots.

If the defect appears to require changing accepted Task 1–4 Domain/Parity/Geometry/Rendering semantics:
STOP and report instead of silently changing accepted files.

# REVIEW FOCUS

Independent reviewers must challenge:

1. Is display order sourced from ToothGeometry, not exam array order?
2. Are numeric site cells sourced from QDentoDisplayAdapter visible order?
3. Is GM sign adapted exactly once?
4. Are PD/CAL/recession thresholds source-like?
5. Is maxillary oral AG non-applicable rather than zero?
6. Does contour rendering consume ContourGeometry rather than recalculate x/y?
7. Is horizontal reversal applied exactly once?
8. Are view-level y placements faithful to source transforms?
9. Are tooth visuals 36/54-like inside 70 slots rather than stretched?
10. Does the screen remain one-arch-at-a-time and scroll rather than squeeze?
11. Are BOP/wedge visuals truly deferred?
12. Does F02 visually and semantically bind to FDI11 correct sites?
13. Does Computer inspection agree with code/test assumptions?

# EXACT SIX-CYCLE REFINEMENT

After initial GREEN, perform exactly six cumulative passes:

1. Accuracy & Fundamental Correction
2. Completeness & Gap Analysis
3. Structure & Architecture
4. Adversarial / Critical Review
5. Usability & Goal Fit
6. Final Synthesis, Regression Check & Polish

Cycle 4 MUST include the real Simulator/Computer inspection.
Cycle 5 MUST include actual horizontal/vertical scrolling and arch/fixture switching.

No seventh cycle.

# BUILD / REGRESSION VERIFICATION

Use:
- Xcode 27.0
- iPhone 18 Pro
- iOS 27.0
- deployment target iOS 18.0

Run:
1. any focused Task 5 unit tests;
2. `StaticChartSnapshotFlowTests`;
3. all Rendering tests;
4. all Geometry tests;
5. Domain + Parity regression;
6. full unit suite;
7. full UI-test suite;
8. app build;
9. Device Hub launch;
10. Computer visual acceptance;
11. `git diff --check`.

# ALLOWED PRODUCT FILES

Create:
- `PeriodontalIOS/Features/PeriodontalChart/PeriodontalChartScreen.swift`
- `PeriodontalIOS/Features/PeriodontalChart/ArchChartView.swift`
- `PeriodontalIOS/Features/PeriodontalChart/ToothChartColumn.swift`
- `PeriodontalIOS/Features/PeriodontalChart/PeriodontalContourView.swift`
- `PeriodontalIOSUITests/StaticChartSnapshotFlowTests.swift`
- Task 5 visual evidence files described above.

Modify:
- `PeriodontalIOS/App/AppModel.swift`
- existing root view file;
- `PeriodontalIOS/App/PeriodontalIOSApp.swift` only if required for wiring;
- `PeriodontalIOS.xcodeproj/project.pbxproj` as required.

Do NOT modify accepted:
- Domain;
- Parity;
- Geometry;
- Rendering/provider files;
- prototype PNG assets.

If any accepted file appears to require change, STOP and report.

# COMMIT

Commit accepted implementation with:

`feat: add static periodontal chart shell`

Push:

`ui/p03-t5-static-chart-shell`

Do NOT merge.
Do NOT open a PR.
Do NOT begin Task 6.

# STOP CONDITIONS

STOP and report rather than improvising if:
- exact Task 4 base cannot be checked out;
- F01/F02 cannot produce complete contour samples without changing accepted code;
- source vertical transforms materially contradict the frozen view-placement formulas;
- simulator cannot be launched for direct visual inspection;
- Device Hub/Computer cannot provide the required visual evidence;
- fixing a visible parity defect would require changing accepted Task 1–4 code;
- full regression cannot pass.

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
- Computer/Device Hub use summary
- sub-agent roles/models used
- TDD/initial failure evidence
- fixture-selection verification
- arch-selection verification
- row/alignment verification
- F01 contour verification
- F02 asymmetric contour verification
- maxillary oral AG verification
- tooth visual 70-slot / 36-54 width verification
- landscape scrolling verification
- portrait smoke verification
- accessibility/UI-test result
- screenshot/evidence paths
- QDento structural comparison summary
- focused test result
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
