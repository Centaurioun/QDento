# QDento Tooth Rendering, Assets, and Provenance

Date: 2026-09-30
Task: Plan 01 / Task 5
Evidence scope: QDento source and tracked project documents. No QDento runtime or screenshot was available for this pass.

## Evidence status and boundaries

- `SOURCE_VERIFIED`: claims traced to the source and planning documents cited below.
- `RUNTIME_VERIFIED`: none.
- `SCREENSHOT_OBSERVED`: none.
- `USER_OBSERVED`: none.
- `UNRESOLVED`: artwork appearance/provenance at image level, runtime rendering, and final production asset choices.

The selected periodontal tooth strip calls `ToothPainter::getBuccalLingual()` through `PerioScene::display()`. The tooth image is one layer. `PerioChartItem` separately draws the moving gingival-margin (GM) and clinical-attachment-level (CAL) lines. These should remain separate provider/geometry concerns: the sprite's boolean periodontitis overlay is not the measurement contour. [SOURCE_VERIFIED: `src/View/Graphics/PerioScene.cpp`; `src/View/Graphics/ToothPainter.cpp`; `src/View/Graphics/PerioChartItem.cpp`]

The minimum first-demo tooth states required by the Task Goal Packet are present natural tooth, missing/extracted tooth, and implant. The minimum periodontal representation is the tooth's periodontal visual overlay plus the separately computed GM/CAL contour where the selected chart displays it. The accepted missing/implant fixture is the only fixture-dependent additional state identified for this task. No endodontic, restorative, caries, bridge, calculus, or surface-marking state is required by that fixture. [SOURCE_VERIFIED: Task Goal Packet; Plan 01 Tasks 3 and 5; `ToothPainter.cpp`; `PerioChartItem.cpp`]

## Source composition

| Component/state | Source and symbols | Composition / resource | Mapping, size, and transform | First-demo status / replaceability |
|---|---|---|---|---|
| Natural present tooth | `src/View/Graphics/ToothPainter.cpp`: `getTooth()`, `getToothPixmap()`, `getBuccalLingual()`; `ToothTextureHint::normal` in `PaintHint.h` | Base slice from `:/tooth/tooth_teeth.png` (`resources/tooth_teeth.png`), selected by permanent texture index. If periodontitis is present, the perio layer is drawn first, then the tooth body. | Permanent atlas slice width is 180 px for molars, 120 px for premolars/frontals; source tooth slice is 860 px tall. Painter canvas is 1140 px tall with 140 px impacted offset; periodontal strip pixmap is 1106 px tall. `ToothGraphicsItem` draws at its type-based `pxWidth` (36 or 54 px), with height 332 when lingual display is enabled. `PerioScene::setWidth(70)` widens the item bounds; paint fills the extra horizontal margins by stretching the edge pixels. | Required. Base silhouette can be replaced independently of state, contour geometry, and provider contract. |
| Missing / extracted | `PaintHint.cpp` maps an ordinary missing status to `ToothTextureHint::extr` (or `extr_m` if the source model marks prior authorship); `getTooth()` draws the ordinary tooth atlas at opacity 0.1 for `extr`, and adds a green tint for `extr_m`. | Reuses the tooth body from `tooth_teeth.png`; no dedicated missing-tooth sheet is needed for the plain extracted appearance. | Same index/type slice, canvas, and quadrant transform as a natural tooth. `extr_m` is a distinct legacy history/tint state; it is not necessary to conflate it with the demo's basic missing state. | Required. A future provider can replace the appearance while preserving a semantic missing/extracted descriptor. Whether the demo includes the legacy history tint is unresolved and not required by the stated fixture. |
| Implant | `PaintHint.cpp` maps implant status to `impl` or `impl_m`; `getTooth()` draws an implant crop at `coords.implantPos`. | Implant base comes from a crop in `:/tooth/tooth_common.png` (`resources/tooth_common.png`). `impl_m` adds a green tint. An implant-specific periodontal crop is selected when `tooth.perio` is true. | The common atlas has 120×860 front-tooth crops and 180×860 molar crops. The same permanent tooth-index/type selection picks implant crop width. The source applies the same quadrant transform as the tooth strip. | Required for the accepted visual-state fixture. Must remain a separate semantic visual state/entity; its art can be independently replaced. |
| Periodontal tooth overlay | `ToothPaintHint::perio`; `getTooth()` | Natural tooth uses its type/index slice from `:/tooth/tooth_perio.png`; implant uses a separate crop in `tooth_common.png`. It is painted before the tooth body, so the body overlays the texture. | Same selected permanent atlas slice and 120/180 px width classes as the body; implant overlay uses the implant position/crop. Quadrant transform is applied by `getBuccalLingual()`. | Required only when the fixture has the periodontal visual active. Independently replaceable from base tooth and contours. Source indicates a boolean visual hint; exact artwork meaning and runtime appearance remain unverified. |
| Dynamic GM/CAL periodontal lines | `src/View/Graphics/PerioChartItem.cpp`: `setMeasurment()` and `paint()` | Two separately drawn polylines: GM in dark gray and CAL in red. These are vector geometry, not raster entries in the tooth sprite sheet. | 48 measurement points; local y formulas in source are `105 + 3 * GM` and `105 - 3 * CAL`. Tooth-type-dependent x-spacing is in `getWidth()` / `calculateOffset()`. Surface/arch transforms are handled by the chart construction and surrounding view; this report does not freeze their complete orientation contract. | Required for the selected periodontal chart when contours are shown. Replaceable as geometry/presentation independently of tooth art. Preserve the source/domain sign bridge for the later parity contract; this task does not synthesize it. |

`PerioScene` creates 16 primary tooth items and a separate lower-z supernumeral item per slot. The latter is auxiliary; it is not the missing-tooth state and is excluded from the minimum demo fixture. When displaying indices 0–15, the scene uses the same item slot; indices 16–31 map to item slot `31 - index`. The generic tooth renderer applies quadrant transforms: first quadrant identity, second horizontal reflection, third 180° rotation, fourth vertical reflection with translation. `getLingualOcclusal()` uses the opposite-quadrant helper, but the periodontal tooth strip uses `getBuccalLingual()` and `rotateByQuadrant()`. [SOURCE_VERIFIED: `PerioScene.cpp:7-36,47-68`; `ToothPainter.cpp:593-627,660-677`]

### Permanent tooth index and type map

QDento's zero-based indices map through `ToothUtils.cpp` to FDI as follows. The type classes select the permanent coordinate family and the atlas texture order. Index direction and scene slot order are separate facts: do not substitute slot number for the FDI mapping.

| Index | FDI | Type | Index | FDI | Type |
|---:|---:|---|---:|---:|---|
| 0 | 18 | molar | 16 | 38 | molar |
| 1 | 17 | molar | 17 | 37 | molar |
| 2 | 16 | molar | 18 | 36 | molar |
| 3 | 15 | premolar | 19 | 35 | premolar |
| 4 | 14 | premolar | 20 | 34 | premolar |
| 5 | 13 | frontal | 21 | 33 | frontal |
| 6 | 12 | frontal | 22 | 32 | frontal |
| 7 | 11 | frontal | 23 | 31 | frontal |
| 8 | 21 | frontal | 24 | 41 | frontal |
| 9 | 22 | frontal | 25 | 42 | frontal |
| 10 | 23 | frontal | 26 | 43 | frontal |
| 11 | 24 | premolar | 27 | 44 | premolar |
| 12 | 25 | premolar | 28 | 45 | premolar |
| 13 | 26 | molar | 29 | 46 | molar |
| 14 | 27 | molar | 30 | 47 | molar |
| 15 | 28 | molar | 31 | 48 | molar |

`SpriteSheets.cpp`'s permanent texture index sequence is `0,1,2,3,4,5,6,7,7,6,5,4,3,2,1,0,8,9,10,11,12,13,14,15,15,14,13,12,11,10,9,8`. This mirrors the tooth-type drawing sequence across each arch. The periodontal screen should use permanent tooth types; temporary tooth coordinates have gaps for molar positions, and `tempIdx` contains a repeated 25 around indices 22–24. Those temporary mappings are outside this demo boundary and should not be copied into Plan 03 without separate verification. [SOURCE_VERIFIED: `src/Model/Dental/ToothUtils.cpp:6-27,59-66`; `src/View/Graphics/SpriteSheets.cpp:13-33,125-145`]

## Prototype asset manifest

The source repository registers many generic tooth/treatment sheets. Registration or loading by the general-purpose painter does not make an asset part of this demo. The table lists the minimum source-backed candidates; it does not copy, extract, or authorize redistribution of binaries.

| Asset/resource path | Purpose in this boundary | Provenance/source | QDento license context | Direct use needed for private demo? | Expected production replacement? | Notes |
|---|---|---|---|---|---|---|
| `resources/tooth_teeth.png` (`:/tooth/tooth_teeth.png`) | Permanent natural-tooth body and faint extracted/missing silhouette | Registered in `resources/Resource.qrc`; atlas sliced by `SpriteSheets.cpp`; selected by `ToothUtils` index/type | Repository has `LICENSE.txt` with GNU GPL version 3 text; project decision log states QDento is GPL-licensed. The repo license alone does not establish individual image authorship/provenance. | Yes, as the source-backed prototype candidate if asset parity is required; actual copying is outside this task. | Yes, plan for independently created or appropriately licensed replacement before any public/proprietary distribution decision. | `extr` changes opacity of this base art rather than using a separate missing asset. |
| `resources/tooth_perio.png` (`:/tooth/tooth_perio.png`) | Periodontal overlay for natural teeth | Registered in `Resource.qrc`; selected and composited by `SpriteSheets` / `ToothPainter` | Same repository GPL context and unresolved image-level provenance. No distribution clearance is asserted. | Yes, if the demo fixture preserves this legacy periodontal tooth overlay; otherwise its replacement must still expose the same semantic layer. | Yes, expected to be independently created/licensed or separately reviewed for production use. | Source overlay is a boolean hint. The GM/CAL contour is not contained in this asset. |
| `resources/tooth_common.png` (`:/tooth/tooth_common.png`) | Implant base and implant-specific periodontal overlay crops | Registered in `Resource.qrc`; crops selected in `SpriteSheets::initialize()` for front and molar widths | Same repository GPL context and unresolved image-level provenance. No distribution clearance is asserted. | Yes, if the accepted implant visual fixture uses QDento's visual reference. This one atlas also contains unrelated crops, so Plan 03 should extract/use only the needed crops if approved. | Yes, expected to be independently created/licensed or separately reviewed for production use. | Source crops: front implant x=0, perio-implant x=240, each 120×860; molar implant x=480, perio-implant x=840, each 180×860. Atlas crop detail from source; appearance not inspected at runtime. |

`tooth_roots.png` is not in the minimum set: the selected plain extracted state is a low-opacity tooth silhouette, and no accepted first-demo fixture requires the separate root hint. If a later fixture explicitly exercises a root/severely destroyed tooth, add it through a scoped evidence update. Likewise exclude endo, lesion/caries, crown, bridge, splint/fiber bridge, denture/false tooth, post, calculus, resorption, surface-specific occlusal/approximal/buccal/lingual/cervical layers, stripes, zodiac, app branding, splash, and unrelated icons. These are available to the generic painter or resource bundle but are not required by the accepted first-demo visual states. [SOURCE_VERIFIED: `ToothPainter.cpp`; `SpriteSheets.cpp`; `resources/Resource.qrc`; Plan 01 Task 3 fixture list]

## Provenance and reuse boundary

The tracked project decision log says QDento is GPL-licensed, says individual image authorship/provenance is not fully established by that repository-level license statement, and allows documented QDento reference material for early private research/demo work. It says not to assume unchanged raster reuse for future proprietary or public distribution and to prefer independently recreated/licensed graphics where needed. The approved spec likewise requires a provenance manifest and a separate asset/code licensing review for future public/proprietary distribution. [SOURCE_VERIFIED: `docs/periodontal-ios/2026-09-30-brainstorming-decision-log.md:486-505`; spec §29.4; `LICENSE.txt`]

Therefore the manifest marks the three files as private-demo prototype candidates only. This is a project planning boundary, not legal advice, not a finding that any asset is cleared for a particular use, and not an App Store or proprietary-distribution clearance claim. There are no separate tracked `docs/provenance` or asset-authorship records in this checkout; unresolved image-level provenance must remain visible in any downstream asset manifest.

## Platform-independent future rendering contract

Plan 03 can place a replaceable `ToothVisualProvider` behind a semantic input/output boundary. It should not expose QDento filenames, Qt classes, atlas indices, source pixel rectangles, `QPixmap`, `QPainter`, or QDento enum values.

**Input information** (value types owned by the iOS app):

- stable chart-position identity and arch/quadrant orientation, independent of display slot order;
- permanent tooth visual class (`molar`, `premolar`, `frontal`) or an app-owned equivalent, plus natural tooth versus implant versus missing/extracted visual state;
- whether the periodontal tooth overlay is active;
- requested presentation orientation (the selected buccal/lingual tooth strip) and available display bounds/scale;
- optional future visual attributes only when an accepted fixture requires them. Do not pass unrelated endodontic, restorative, lesion, bridge, caries, or surface-treatment fields in this first provider contract.

**Output information**:

- independently replaceable base-tooth layer for the semantic state;
- optional periodontal overlay layer, aligned to the base visual;
- stable intrinsic aspect/layout metadata or a normalized shape that can be fit to the available bounds;
- explicit orientation/anchor metadata so the caller can place the visual consistently without knowing an atlas crop;
- a clear absence result for unsupported states, rather than silently substituting a natural tooth.

The domain must continue to distinguish natural teeth, implants, and missing positions; shared rendering code may consume their visual descriptors. The provider supplies tooth art only. The chart geometry engine supplies the GM/CAL contour and its named measurement values separately. A production provider can replace one or all art layers without changing chart-position identity, periodontal data, or contour calculations. Exact output representation (vector, generated path, or raster) remains a Plan 03 implementation choice.

## Unresolved items for downstream work

1. `UNRESOLVED`: no QDento runtime render or screenshot was captured; all composition/orientation statements above are source-derived.
2. `UNRESOLVED`: the image files' individual authorship and complete provenance are not established by the source tree inspected here.
3. `UNRESOLVED`: source values describe atlas crops and painter transforms, but visual pixel appearance/quality at iPhone size is unassessed.
4. `UNRESOLVED`: whether the private demo should preserve QDento's `extr_m` green history tint; the required basic missing/extracted visual does not depend on it.
5. Temporary-tooth mapping is excluded and needs independent verification if the first demo adds primary teeth.

## Verification record

- Read the approved design spec, master roadmap, Plan 01 evidence plan, and independent plan review before source synthesis.
- Traced the required painter/sprite/scene files, `PaintHint`/`ToothGraphicsItem` composition, tooth type/index mapping, `Resource.qrc`, `LICENSE.txt`, and tracked provenance decision notes.
- Used three read-only GPT-6 Luna agents for independent rendering, sprite mapping, and provenance/YAGNI reviews; this artifact is the sole write.
- No application source or asset binary was changed or copied. No runtime/screenshot evidence was available in this pass.
- The artifact records Plan 03's needed visual boundary only; it does not freeze the final Plan 01 parity contract.
