[$superpowers](app://plugins~Plugin_60aea7460bd4819199fd97a9553a5e12)
[@Build iOS Apps](plugin://build-ios-apps@openai-curated-remote)

# PLAN 03 / TASK 4 — SCOPED TOOTH-VISUAL REMEDIATION

Resume the SAME Task 4 Codex/controller context if possible.

Do NOT begin Plan 03 Task 5.

Target repository:
`Centaurioun/periodontal-ios`

Existing branch:
`rendering/p03-t4-tooth-visual-provider`

Current candidate HEAD:
`87f853e2f0d382e2b9c9d610a30b384c1ac5b52c`

Read the coordinator review first:

Repository:
`Centaurioun/QDento`

Branch:
`origin/docs/periodontal-ios-brainstorming`

File:
`docs/superpowers/reviews/2026-10-01-p03-t4-tooth-visual-preacceptance-review.md`

## Required fix A — QDento periodontal-strip canvas semantics

Do NOT add or recrop any assets.

Keep the existing 18 body PNGs unchanged.

QDento source composition is:

- raw natural/implant body resource: 120×860 or 180×860;
- `getToothPixmap()`: transparent 1140-high canvas; body translated down by 140;
- `getBuccalLingual()`: final periodontal-strip canvas height = 1106;
- quadrant transform is applied to the FULL 1106-high strip;
- `ToothGraphicsItem::showLingual(true)`: displayed tooth height = 332;
- source displayed body/canvas width class:
  - frontal/premolar: 36;
  - molar: 54;
- each tooth slot itself is 70 wide.

The current SwiftUI view incorrectly uses the raw 120/180×860 crop as its intrinsic aspect ratio.

Correct the descriptor/view boundary.

Introduce a small explicit layout value or equivalent, for example:

```swift
struct ToothVisualLayout: Equatable, Sendable {
    let canvasPixelSize: CGSize
    let bodyFrame: CGRect

    var aspectRatio: CGFloat {
        canvasPixelSize.width / canvasPixelSize.height
    }
}
```

Prototype layout:

For non-molar body:
- canvas = 120×1106
- bodyFrame = (x:0, y:140, width:120, height:860)

For molar body:
- canvas = 180×1106
- bodyFrame = (x:0, y:140, width:180, height:860)

Apply the same width-specific canvas semantics to:
- natural;
- missing;
- implant.

The resource PNG itself remains 860 high.

`ToothGraphicView` must:

1. construct a transparent logical canvas using descriptor layout;
2. place the body image into `bodyFrame`;
3. apply body opacity there;
4. apply orientation to the ENTIRE composed canvas;
5. preserve the canvas aspect ratio.

Do NOT simply aspect-fit the raw 860-high image.

Do not hard-code FDI/resource mapping inside the view.

Do not introduce chart-shell spacing here.

## Required fix B — remove unused speculative API

The current overload with:

- `view`
- `bounds`
- `scale`

does not use bounds or scale, and the current prototype art is the combined QDento periodontal `getBuccalLingual` strip rather than independently selected buccal/lingual body resources.

Simplify now before Task 5 consumes the API.

Required:
- public provider protocol remains:
  `descriptor(for position: ChartPositionRecord)`;
- remove unused `bounds` and `scale`;
- remove `ToothVisualView` and descriptor `view` unless you can show a current source-backed behavior that genuinely needs them.

Do not add speculative replacement inputs.

## TDD — RED FIRST

Add/update tests before implementation.

Required tests:

1. non-molar natural descriptor:
   - resource pixel size remains 120×860;
   - visual canvas = 120×1106;
   - bodyFrame = (0,140,120,860);
   - descriptor intrinsic aspect ratio = 120/1106.

2. molar natural descriptor:
   - resource = 180×860;
   - canvas = 180×1106;
   - bodyFrame = (0,140,180,860);
   - aspect = 180/1106.

3. missing:
   - same natural resource/layout for ToothID;
   - opacity 0.1.

4. non-molar implant:
   - resource 120×860;
   - canvas/bodyFrame follows 120-wide layout.

5. molar implant:
   - resource 180×860;
   - canvas/bodyFrame follows 180-wide layout.

6. every bodyFrame is fully contained within its descriptor canvas.

7. source display-scale sanity:
   if the canvas is rendered at height 332,
   - 120/1106×332 is approximately 36;
   - 180/1106×332 is approximately 54.
   Use reasonable floating-point tolerance; this is source-composition regression evidence, not a new UI hard-lock.

8. F08 state/resource/orientation tests remain passing.

9. all 18 resources still resolve.

10. there is no public provider overload that requires ignored bounds/scale semantics.

Demonstrate RED against current candidate, then GREEN after minimal correction.

## ALLOWED CHANGES

Only:
- `PeriodontalIOS/Rendering/ToothVisualDescriptor.swift`
- `PeriodontalIOS/Rendering/PrototypeToothVisualProvider.swift`
- `PeriodontalIOS/Features/PeriodontalChart/ToothGraphicView.swift`
- `PeriodontalIOSTests/Rendering/ToothVisualProviderTests.swift`
- `docs/provenance/QDENTO_REFERENCE.md` only if needed to record source composition metadata.

Do NOT change:
- the 18 PNG files;
- `ToothVisualProviding.swift` unless signature cleanup is genuinely necessary (the intended protocol already has the correct minimal signature);
- Domain;
- Parity;
- Geometry;
- Xcode project/resource membership;
- app root/chart shell.

## WORKFLOW

Use:
- using-superpowers
- subagent-driven-development
- verification-before-completion
- Build iOS Apps / XcodeBuildMCP

All sub-agents MUST use GPT-6 Luna only.
Never use GPT-6 Sol.

Use:
1. fresh remediation implementer;
2. fresh read-only QDento composition reviewer;
3. fresh read-only SwiftUI rendering-boundary reviewer.

Reviewers must explicitly verify:
- raw resource dimensions remain 860 high;
- logical visual canvas is 1106 high;
- y=140 body placement is preserved;
- orientation acts on the full canvas;
- no new raster was added;
- no Task 5 chart-shell concerns leaked into Task 4.

## VERIFICATION

Run:
1. focused ToothVisualProviderTests;
2. all Geometry tests;
3. Domain + Parity tests;
4. full unit suite;
5. existing UI launch regression;
6. app build;
7. built-bundle resource count still exactly 18;
8. `git diff --check`.

Confirm no PNG hash changed.

Commit remediation separately:

`fix: preserve QDento tooth strip canvas layout`

Push SAME branch:

`rendering/p03-t4-tooth-visual-provider`

Do NOT merge.
Do NOT open a PR.
Do NOT begin Task 5.

Return only:
- STATUS
- new HEAD
- changed files
- TDD RED evidence
- non-molar canvas/layout verification
- molar canvas/layout verification
- missing/implant layout verification
- full-canvas orientation verification
- provider API cleanup summary
- F08 regression result
- resource/hash verification
- focused rendering-test result
- Geometry regression result
- Domain + Parity regression result
- full unit-test result
- UI regression result
- build result
- fresh reviewer verdicts
- git/status/push verification
- concerns/blockers
