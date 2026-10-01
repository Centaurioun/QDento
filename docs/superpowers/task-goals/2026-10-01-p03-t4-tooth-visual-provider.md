[$superpowers](app://plugins~Plugin_60aea7460bd4819199fd97a9553a5e12)
[@Build iOS Apps](plugin://build-ios-apps@openai-curated-remote)

# TASK GOAL PACKET — PLAN 03 / TASK 4
# REPLACEABLE TOOTH VISUAL PROVIDER + PRIVATE-DEMO PROTOTYPE ASSETS

## TASK TYPE

PROMPT for one dedicated Codex controller instance.

This task implements the tooth-art provider boundary and a focused SwiftUI ToothGraphicView.

It may add the MINIMUM approved QDento-derived raster crops needed for the current private F01–F08 demo fixtures.

It does NOT build the periodontal chart screen.
It does NOT render contours, BOP markers, or FMPS/FMBS controls.
It does NOT add editing.
It does NOT add clinical logic.

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
7. Use current XcodeBuildMCP build/test workflow.

## MODEL / SUB-AGENT POLICY

You are the controller.

ALL sub-agents MUST use GPT-6 Luna only.
Never use GPT-6 Sol for a sub-agent.
No nested sub-agents.

Recommended fresh roles:
1. Visual Descriptor/Provider Implementer — TDD.
2. Asset Extraction/Manifest Implementer — bounded to approved source crops.
3. QDento Visual-Mapping Reviewer — read-only.
4. Provenance/Licensing-Boundary Reviewer — read-only.
5. SwiftUI View Reviewer — read-only.
6. Final Code Quality Reviewer — read-only.

Only the controller/integration writer may combine changes.
Do not have multiple writers editing the same files.

## TARGET REPOSITORY / BASE

Repository:
`Centaurioun/periodontal-ios`

Exact accepted Task 3 HEAD:
`7bb307ba161345890293ffc2a36f9dfd37b093c4`

Create a NEW isolated worktree/branch:

`rendering/p03-t4-tooth-visual-provider`

Do NOT work on previous branches.
Do NOT merge.
Do NOT open a PR.
Do NOT begin Task 5.

## AUTHORITATIVE INPUTS

### iOS repo

Read:
- `docs/contracts/rendering-parity-contract-v1.1.md`
- `docs/provenance/QDENTO_REFERENCE.md`
- accepted Domain:
  - ToothID
  - NaturalToothRecord
  - ImplantRecord
  - ChartPositionRecord
- accepted F01/F08 parity fixtures.

### QDento read-only source/evidence

Repository:
`Centaurioun/QDento`

Read:
- `docs/periodontal-ios/research/runtime/E-tooth-rendering-assets.md`
  from `research/p01-t6-parity-contract-synthesis`;
- `src/View/Graphics/SpriteSheets.cpp`
- `src/View/Graphics/SpriteRect.cpp`
- `src/View/Graphics/ToothPainter.cpp`
- `src/View/Graphics/ToothGraphicsItem.cpp`
- `src/Model/Dental/ToothUtils.cpp`
- `src/View/Graphics/PaintHint.cpp`

Local source checkout may be used read-only if available:
`/Users/yusuf/Repos/QDento`

Do not modify QDento.

# IMPORTANT LICENSING / PROVENANCE BOUNDARY

QDento repository is GPL-3.0.

The project decision allows QDento raster material only as a documented PRIVATE-DEMO prototype reference at this stage.

This task MUST NOT claim:
- public distribution clearance;
- App Store clearance;
- proprietary redistribution clearance;
- independent image authorship.

Public/proprietary distribution remains BLOCKED on a later asset/code licensing and replacement review.

Every copied/extracted raster must have provenance recorded.

Do not copy more QDento raster content than the current fixtures require.

# MINIMUM ASSET DECISION

For current F01–F08 fixtures, copy/extract ONLY:

1. the 16 unique permanent NATURAL tooth body slices required by the 32 permanent FDI positions;
2. the front/non-molar implant body crop;
3. the molar implant body crop.

Total prototype source-image outputs expected:
**18 raster body assets**.

Do NOT copy:
- `tooth_perio.png`;
- implant periodontal-overlay crops;
- roots;
- endo;
- lesion/caries;
- crown;
- bridge;
- calculus;
- denture;
- surface layers;
- resorption;
- stripes;
- branding/icons;
- other treatment layers.

Why periodontal overlay is excluded:
- current F01–F08 domain/fixtures do not carry a periodontal-overlay boolean state;
- GM/CAL contours are a separate accepted Geometry layer;
- adding unused GPL raster layers now would broaden provenance footprint without an accepted fixture need.

The provider contract may reserve an OPTIONAL overlay field for future replacement, but the prototype provider must not pretend an absent overlay asset exists.

If a future fixture explicitly requires this raster overlay, add it in a separate scoped task.

# SOURCE NATURAL ATLAS EXTRACTION

Source:
`resources/tooth_teeth.png`

QDento slices permanent textures sequentially at height 860.

Exact 16 unique source crops:

| variant | x | width | height |
|---|---:|---:|---:|
| 0 | 0 | 180 | 860 |
| 1 | 180 | 180 | 860 |
| 2 | 360 | 180 | 860 |
| 3 | 540 | 120 | 860 |
| 4 | 660 | 120 | 860 |
| 5 | 780 | 120 | 860 |
| 6 | 900 | 120 | 860 |
| 7 | 1020 | 120 | 860 |
| 8 | 1140 | 180 | 860 |
| 9 | 1320 | 180 | 860 |
| 10 | 1500 | 180 | 860 |
| 11 | 1680 | 120 | 860 |
| 12 | 1800 | 120 | 860 |
| 13 | 1920 | 120 | 860 |
| 14 | 2040 | 120 | 860 |
| 15 | 2160 | 120 | 860 |

Use lossless PNG crop only.
Do not resynthesize or repaint the source in this private prototype task.

Preserve alpha.

Use app-owned prototype resource names.
Do NOT expose QDento source filenames in the public provider API.

Example internal resource naming:
`PrototypeToothBodyV00` ... `PrototypeToothBodyV15`

Exact names may vary if systematic.

# FDI → INTERNAL NATURAL VARIANT

The prototype provider's PRIVATE mapping is:

- 18→0
- 17→1
- 16→2
- 15→3
- 14→4
- 13→5
- 12→6
- 11→7
- 21→7
- 22→6
- 23→5
- 24→4
- 25→3
- 26→2
- 27→1
- 28→0
- 38→8
- 37→9
- 36→10
- 35→11
- 34→12
- 33→13
- 32→14
- 31→15
- 41→15
- 42→14
- 43→13
- 44→12
- 45→11
- 46→10
- 47→9
- 48→8

Do NOT expose this integer variant as canonical Domain identity.

It is provider-internal prototype asset mapping only.

# IMPLANT CROPS

Source:
`resources/tooth_common.png`

Copy only:

### non-molar implant body
- x=0
- y=0
- width=120
- height=860

### molar implant body
- x=480
- y=0
- width=180
- height=860

QDento uses the molar implant art for molar tooth classes.
Premolar and frontal positions use the 120-wide non-molar implant art.

Use app-owned resource names such as:
- `PrototypeImplantBodyStandard`
- `PrototypeImplantBodyMolar`

Do NOT copy the other common-atlas crops.

# QDENTO MISSING-TOOTH VISUAL

Plain extracted/missing state does NOT require another asset.

QDento:
- reuses the natural tooth body;
- draws it at opacity 0.1.

Therefore prototype missing descriptor must:
- use the same natural body asset as that ToothID;
- use body opacity = 0.1.

Do NOT add the legacy `extr_m` green-history tint.
It is not required by current fixtures.

# TOOTH CLASS

Create an app-owned visual class enum, e.g.:

```swift
enum PermanentToothVisualClass: String, Codable, Sendable {
    case molar
    case premolar
    case frontal
}
```

Mapping must match QDento ToothUtils:

Molars:
- 16,17,18
- 26,27,28
- 36,37,38
- 46,47,48

Premolars:
- 14,15
- 24,25
- 34,35
- 44,45

Frontal:
all remaining permanent teeth in the 32-tooth scope, including canines.

Do not call canines a separate source class.

# VISUAL ORIENTATION

Create an app-owned orientation enum, e.g.:

- identity
- mirrorHorizontal
- rotate180
- mirrorVertical

QDento source-backed quadrant behavior for the periodontal tooth strip:

- Q1 → identity
- Q2 → horizontal reflection
- Q3 → 180° rotation
- Q4 → vertical reflection

Do NOT expose QPainter/QTransform concepts.

Tests must verify representative/all FDI mappings.

# REQUIRED RENDERING TYPES

Create:
`PeriodontalIOS/Rendering/ToothVisualDescriptor.swift`

Recommended semantic types:

```swift
enum ToothVisualState: Equatable, Sendable {
    case natural
    case missing
    case implant
}

enum PermanentToothVisualClass ...
enum ToothVisualOrientation ...

struct ToothVisualDescriptor: Equatable, Sendable {
    let toothID: ToothID
    let state: ToothVisualState
    let toothClass: PermanentToothVisualClass
    let orientation: ToothVisualOrientation
    let bodyResourceName: String
    let bodyOpacity: Double
    let periodontalOverlayResourceName: String?
}
```

Equivalent design is allowed.

Important:
- the public semantic state must NOT expose QDento texture enum names;
- resource names are output of the provider, not Domain;
- periodontal overlay should be nil/unsupported in this prototype because no such approved asset is copied in this task.

# PROVIDER PROTOCOL

Create:
`PeriodontalIOS/Rendering/ToothVisualProviding.swift`

Implement a replaceable provider boundary, equivalent to:

```swift
protocol ToothVisualProviding {
    func descriptor(for position: ChartPositionRecord) -> ToothVisualDescriptor?
}
```

The protocol must not mention:
- QDento filenames;
- atlas indexes;
- crop rectangles;
- Qt/QPixmap/QPainter;
- source texture enums.

A provider may return nil for truly unsupported visual states.

Current ChartPositionRecord cases natural/implant/missing must all be supported.

# PROTOTYPE PROVIDER

Create:
`PeriodontalIOS/Rendering/PrototypeToothVisualProvider.swift`

This is the ONLY place where prototype resource mapping may know app-owned extracted asset names / private variant mapping.

Required state mapping:

### natural
- state=.natural
- body asset = natural variant for ToothID
- opacity = 1.0

### missing
- state=.missing
- body asset = SAME natural variant for ToothID
- opacity = 0.1

### implant
- state=.implant
- body asset = molar implant body iff tooth class is molar;
- otherwise non-molar implant body;
- opacity=1.0.

No CAL/PD/GM logic.
No Stage/Grade.
No BOP/FMPS/FMBS.
No periodontal classification.

# TOOTH GRAPHIC VIEW

Create:
`PeriodontalIOS/Features/PeriodontalChart/ToothGraphicView.swift`

The view consumes a ToothVisualDescriptor.

It must:
- render the body image;
- apply descriptor opacity;
- apply descriptor orientation;
- preserve aspect ratio;
- use transparent background;
- contain NO tooth-state mapping logic;
- contain NO QDento atlas mapping;
- contain NO chart geometry;
- contain NO periodontal calculations.

If optional overlayResourceName is nil, render body only.

Use SwiftUI composition suitable for later embedding in ToothChartColumn.

Do NOT build a standalone chart screen in Task 4.

# RESOURCE ORGANIZATION

Prefer a dedicated asset catalog or clearly bounded resource directory for prototype tooth images.

Example:
`PeriodontalIOS/Resources/PrototypeToothAssets.xcassets`

All extracted files must be added to the app target only as resources.

Do not add them to Domain/Parity targets as source.

Use systematic app-owned resource names.

Do not retain temporary extraction scripts in the repo unless they are intentionally documented/reproducible and approved by the controller.

If an extraction script is useful only to make the crops, use it locally and remove it before commit.

# PROVENANCE UPDATE

Update:
`docs/provenance/QDENTO_REFERENCE.md`

Record:
- source repo: `Centaurioun/QDento`;
- exact source ref/commit used for extraction;
- source file:
  - `resources/tooth_teeth.png`;
  - `resources/tooth_common.png`;
- each crop family and exact coordinate table;
- mapping from app-owned asset names to source crop;
- source repo GPL-3.0 context;
- unresolved individual image authorship/provenance;
- PRIVATE DEMO ONLY;
- public/proprietary distribution gate remains;
- expected production replacement/licensing review;
- no QDento application code copied;
- no treatment/perio/root/etc. raster layers copied in Task 4.

Do NOT state legal conclusions beyond this project boundary.

# REQUIRED TEST FILE

Create:
`PeriodontalIOSTests/Rendering/ToothVisualProviderTests.swift`

Tests must not depend on visual screenshot judgement for provider semantics.

# TDD PHASE A — TOOTH CLASS + ORIENTATION

RED first.

Test all or representative enough to prove exact rule:

- FDI18 molar Q1 identity
- FDI14 premolar Q1 identity
- FDI11 frontal Q1 identity
- FDI21 frontal Q2 mirrorHorizontal
- FDI26 molar Q2 mirrorHorizontal
- FDI31 frontal Q3 rotate180
- FDI36 molar Q3 rotate180
- FDI41 frontal Q4 mirrorVertical
- FDI46 molar Q4 mirrorVertical

Prefer exhaustive all-32 validation for class/orientation if concise.

# TDD PHASE B — NATURAL VARIANT MAPPING

Verify all 32 ToothIDs map to the exact internal source-derived asset variant table above.

The test may inspect PrototypeToothVisualProvider output resource names.

Do not add variant index to canonical Domain.

# TDD PHASE C — STATE SEPARATION

Using same ToothID:
- natural descriptor state is natural, opacity1;
- missing descriptor state is missing, SAME body resource, opacity0.1;
- implant descriptor state is implant, opacity1, different implant resource.

Verify:
- natural and missing share body art but not visual state/opacity;
- implant does not resolve to natural body art.

# TDD PHASE D — F08

Use accepted F08:
- FDI24 natural;
- FDI25 missing;
- FDI26 implant.

Assert three distinct descriptor states.

Specifically:
- FDI24 maps to natural variant 4 with Q2 horizontal mirror;
- FDI25 maps to natural variant 3 with opacity0.1 and Q2 horizontal mirror;
- FDI26 implant maps to MOLAR implant asset with Q2 horizontal mirror.

Do not treat implant as missing.

# TDD PHASE E — RESOURCE EXISTENCE

For every bodyResourceName that can be produced by the prototype provider:
- verify the packaged app/test-host can resolve the resource image;
- verify no provider path references an asset that was not copied.

Use an appropriate iOS image/resource loading API in the test target.

Do not assert pixel appearance with brittle screenshot comparison yet.

# TDD PHASE F — PROVENANCE / YAGNI

Add bounded checks where practical that:
- exactly the approved prototype body asset set is packaged by this task;
- no prohibited QDento treatment/perio/root asset naming appears in provider mapping.

Do not make tests scrape arbitrary source repository contents.

# ASSET EXTRACTION VERIFICATION

Before committing, programmatically verify each extracted PNG:
- exists;
- dimensions match expected crop;
- alpha-capable PNG is preserved;
- no crop extends outside source image dimensions.

Verify:
- 16 natural body crops;
- 1 non-molar implant body;
- 1 molar implant body;
- no extra QDento raster copied.

Record hashes for the extracted prototype files in the provenance document or a concise manifest section.

Do not expose font files.

# REVIEW FOCUS

Independent reviewers must challenge:

1. Did provider semantics stay independent from QDento filenames/Qt?
2. Is atlas/index knowledge confined to PrototypeToothVisualProvider/provenance?
3. Do all 32 FDIs map to correct class/variant/orientation?
4. Does missing reuse natural art at 0.1 opacity?
5. Is implant a distinct visual state and correct molar/non-molar art?
6. Did Task 4 copy only the 18 approved body crops?
7. Were tooth_perio/common treatment layers accidentally copied?
8. Does ToothGraphicView contain mapping/business logic that belongs in provider?
9. Are all resource names actually loadable in the built app?
10. Does provenance clearly retain GPL/private-demo/public-distribution gate?
11. Did anyone claim image-level authorship or public licensing clearance without evidence?

# SIX-CYCLE TASK REFINEMENT

After GREEN and before final commit, perform exactly six cumulative passes:

1. Accuracy & Fundamental Correction
2. Completeness & Gap Analysis
3. Structure & Architecture
4. Adversarial / Critical Review
5. Usability & Goal Fit
6. Final Synthesis, Regression Check & Polish

No seventh cycle.

# COMPUTER / DEVICE HUB

Do NOT require Computer or Device Hub for Task 4 acceptance.

Although ToothGraphicView exists, it is not yet integrated into the full periodontal chart, so isolated visual inspection would provide weak parity evidence.

Use Build iOS Apps/XcodeBuildMCP build/tests.

Computer + Device Hub become required in Task 5 static-chart integration.

# BUILD / REGRESSION VERIFICATION

Use verified environment:
- Xcode 27.0
- iPhone 18 Pro
- iOS 27.0
- deployment target iOS 18.0

Run:
1. focused ToothVisualProviderTests;
2. all Geometry tests;
3. Domain + Parity regression;
4. full unit suite;
5. existing UI launch regression;
6. app build;
7. `git diff --check`.

Also verify app resources are present in the built bundle.

# ALLOWED PRODUCT FILES

Create:
- `PeriodontalIOS/Rendering/ToothVisualDescriptor.swift`
- `PeriodontalIOS/Rendering/ToothVisualProviding.swift`
- `PeriodontalIOS/Rendering/PrototypeToothVisualProvider.swift`
- `PeriodontalIOS/Features/PeriodontalChart/ToothGraphicView.swift`
- bounded prototype asset catalog/resource files

Update:
- `docs/provenance/QDENTO_REFERENCE.md`
- `PeriodontalIOS.xcodeproj/project.pbxproj` as needed for files/resources.

Create test:
- `PeriodontalIOSTests/Rendering/ToothVisualProviderTests.swift`

Do NOT modify accepted Domain/Parity/Geometry logic unless a blocking defect is proven. STOP and report first if so.

# COMMIT

Commit accepted implementation with:

`feat: add replaceable periodontal tooth visuals`

Push:
`rendering/p03-t4-tooth-visual-provider`

Do NOT merge.
Do NOT open a PR.
Do NOT begin Task 5.

# STOP CONDITIONS

STOP and report rather than improvising if:
- exact Task 3 base cannot be checked out;
- source PNG dimensions do not contain the frozen crop rectangles;
- local/source assets differ materially from the documented mappings;
- asset extraction would require copying unrelated atlas/treatment content into the iOS repo;
- packaged image resources cannot be loaded reliably;
- accepted Domain types would need modification;
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
- asset files added/count
- source ref/commit used for extraction
- Build iOS Apps skills used
- sub-agent roles/models used
- TDD RED evidence
- tooth-class/orientation verification
- all-32 variant mapping verification
- natural/missing/implant separation verification
- F08 visual-state verification
- resource-loading verification
- asset extraction dimension/hash verification
- provenance review summary
- focused rendering-test result
- all Geometry regression result
- Domain + Parity regression result
- full unit-test result
- UI launch regression
- build result
- independent review verdict(s)
- concerns/blockers
- verification summary
- controller rulings, if any
