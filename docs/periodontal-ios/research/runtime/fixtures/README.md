# QDento Parity Fixture Captures

Fixture definitions live in [`../C-parity-fixtures.md`](../C-parity-fixtures.md). This directory is for future original QDento reference captures and their provenance only. No fixture screenshots are currently established here. Do not create an illustrative image and label it as a QDento result.

## Capture protocol

For every capture:

1. Start from the fixture's complete inputs in `C-parity-fixtures.md`; record any setup/reset action needed to reach that exact state. Use a test patient with no unrelated statuses. Pin FDI numbering, date, and fresh-view behavior. State the source commit/ref and QDento version/build.
2. Record platform/OS, Qt version, display resolution, OS scaling, application window size, and graphics-view zoom. Use the same values for captures compared to one another. Do not crop away controls or context needed to identify the state.
3. Keep the relevant arch and surface visible. Include the whole target tooth plus its adjacent tooth identities and the associated measurement/control rows. For a summary assertion, include the summary region and all teeth contributing to its denominator. For a tooth-state case, show the natural/missing/implant comparison together.
4. Record the exact fixture ID, arch, FDI tooth, internal tooth index, measurement or wedge offsets, entered values, direct-edit field/action, final values, and evidence status in the capture record. Include unresolved slot-to-anatomy mappings rather than guessing.
5. Save the original capture unchanged. If a crop is needed for review, retain the original and label the derivative separately; record the crop rectangle. Never redraw values, recolor controls, remove UI, or synthesize a screenshot.

## File naming

Use:

`<FIXTURE-ID>__<arch>__FDI-<tooth>__<surface-or-control>__<evidence>__<source-ref>__<sequence>.png`

Example format only (not an existing capture):

`P01-T3-F02__maxilla__FDI-11__perio-chart__SCREENSHOT_OBSERVED__<short-sha>__01.png`

Use `all-teeth` where a capture demonstrates a full-mouth state; use `NA` for a field that does not apply. Do not place spaces in filenames. Keep each filename's evidence label consistent with the evidence record below.

## Required sidecar record

For every image, add a same-stem `.md` record containing:

- fixture ID and exact source commit/ref;
- QDento version/build identifier, OS, Qt version, display resolution/scaling, window dimensions, and view zoom;
- arch, FDI numbering mode, FDI tooth/tooth index (or full-mouth coverage), and visible surface/control;
- exact entered input values, BOP state, FMPS/FMBS neutral wedge indices, AG, mobility/furcation, and natural/missing/implant state as relevant;
- direct-edit action and before/after values for transition fixtures;
- crop rectangle if the image is cropped, with original filename/hash retained;
- evidence label and a short statement of precisely what the image establishes;
- any missing evidence, ambiguous mapping, or setup limitation.

## Evidence labels

- `SOURCE_VERIFIED`: implementation source supports a claim; no screenshot is implied.
- `RUNTIME_VERIFIED`: the named QDento build was executed and the fixture state was set through its UI or a documented deterministic setup path.
- `SCREENSHOT_OBSERVED`: the original image itself shows the claim; include image provenance and capture conditions.
- `USER_OBSERVED`: a user directly reported the behavior; identify the report/date without upgrading it to runtime evidence.
- `UNRESOLVED`: no sufficient source, runtime, screenshot, or user evidence exists.

A source calculation and a screenshot are separate claims; label each result individually. Never promote a screenshot observation to runtime verification unless execution and setup are documented. Existing repository screenshots `screenshots/scr0.png`–`scr3.png` have no per-fixture setup record and must not be copied into this directory as fixture evidence.
