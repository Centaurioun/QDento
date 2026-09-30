[$superpowers](app://plugins~Plugin_60aea7460bd4819199fd97a9553a5e12)

# EXECUTION MODE

This is an approved Plan 01 research task for the QDento → native iOS periodontal project.

Do NOT restart brainstorming.
Do NOT rewrite the approved architecture.
Do NOT implement Swift/iOS code.
Do NOT modify QDento application source.
Do NOT modify Clinica application source.

Before any repository action:
1. invoke Superpowers `using-superpowers`;
2. invoke `using-git-worktrees` and work in an isolated worktree/branch;
3. use `dispatching-parallel-agents` for genuinely independent research subproblems when useful;
4. before completion use `verification-before-completion`.

This is a research task, not subagent-driven implementation. Do not use `subagent-driven-development` as the controlling workflow.

## SUB-AGENT POLICY

You are the sole writer for this task.

If the harness supports sub-agents, use 2–4 focused read-only sub-agents where doing so improves confidence or speed. Good roles include independent source tracing, runtime/screenshot verification, and adversarial cross-checking.

Sub-agents:
- MUST be read-only;
- MUST NOT commit, edit repo files, create PRs, or push;
- MUST NOT spawn their own sub-agents;
- MUST use GPT-6 Luna only;
- MUST return evidence/findings to you for synthesis.

Do not use GPT-6 Sol for any sub-agent.

You may use the main Codex model selected by the user for coordination/synthesis.

## AUTHORITATIVE DOCUMENTS

Read these first from the QDento repository:

- `docs/superpowers/specs/2026-09-30-qdento-periodontal-ios-design.md`
- `docs/superpowers/plans/2026-09-30-periodontal-ios-master-roadmap.md`
- `docs/superpowers/plans/2026-09-30-01-runtime-parity-evidence-closure.md`
- `docs/superpowers/reviews/2026-09-30-periodontal-ios-plan-review.md`

The approved spec is binding authority.
The plan is its execution argument.
Do not silently expand scope.

## BASE / ISOLATION

Repository:
`Centaurioun/QDento`

Start from:
`origin/docs/periodontal-ios-brainstorming`

Create or verify an isolated worktree for the branch named in this packet.

Do not work on `master`.
Do not modify the base planning branch directly.

Before research, record:
- worktree path;
- branch;
- starting HEAD;
- clean/dirty status.

## EVIDENCE RULES

Repository source/runtime evidence outranks guesses.

Use evidence labels consistently:
- `SOURCE_VERIFIED`
- `RUNTIME_VERIFIED`
- `SCREENSHOT_OBSERVED`
- `USER_OBSERVED`
- `UNRESOLVED`

Do not infer undocumented QDento author intent.

General web research is NOT a substitute for QDento source/runtime evidence.
Do not browse the web merely to fill gaps.

If runtime execution is unavailable, continue with source analysis and explicitly record what remains unverified.

## WRITE RIGHTS

You may write ONLY the file(s) explicitly listed under ALLOWED WRITES in this packet.

Do not edit the spec, plans, prior brainstorming documents, QDento source, resources, project files, or Clinica.

## SIX-CYCLE FINAL REFINEMENT

After evidence collection but before committing, internally refine the report through exactly these six cumulative cycles:

1. Accuracy & Fundamental Correction
2. Completeness & Gap Analysis
3. Structure & Architecture
4. Adversarial / Critical Review
5. Usability & Goal Fit
6. Final Synthesis, Regression Check & Polish

Do not add a seventh meta-cycle.
Do not invent facts to make the report look complete.
Preserve unresolved evidence honestly.

## COMPLETION / HANDOFF

After writing the assigned artifact:

- inspect `git diff --check`;
- inspect `git status --short`;
- verify ONLY allowed documentation files changed;
- re-read the artifact against this packet and the Plan 01 task;
- commit with the requested commit message;
- push the task branch to origin (the user has authorized separate research branches as project scratch/evidence lanes);
- DO NOT merge;
- DO NOT open a PR unless explicitly asked later.

Return only:
- STATUS: DONE / DONE_WITH_CONCERNS / BLOCKED
- branch
- commit SHA
- artifact path(s)
- sub-agent roles actually used
- one concise evidence summary
- unresolved/blocking items
- verification summary


# TASK GOAL PACKET — PLAN 01 / TASK 5

## TASK TYPE

PROMPT for one dedicated Codex instance.

## BRANCH

`research/p01-t5-tooth-rendering-assets`

## GOAL

Identify the minimum QDento tooth-visual composition, tooth-index/type mapping, and prototype asset/provenance set required to reproduce the selected periodontal chart's educational tooth/implant/missing visuals behind a replaceable future iOS rendering provider.

## ALLOWED WRITES

Create only:

`docs/periodontal-ios/research/runtime/E-tooth-rendering-assets.md`

Do NOT copy asset binaries in this task.
This task is analysis/manifest only.

## REQUIRED SOURCE STARTING POINTS

At minimum inspect:

- `src/View/Graphics/ToothPainter.cpp/.h`
- `src/View/Graphics/SpriteSheets.cpp/.h`
- `src/View/Graphics/PerioScene.cpp/.h`
- tooth graphics item / paint hint types used by the periodontal view
- `Resource.qrc` / relevant resource manifests
- the existing QDento provenance/licensing docs in this branch
- QDento `LICENSE`

## RECOMMENDED INTERNAL SUB-AGENT SPLIT

If available, dispatch read-only Luna sub-agents such as:

- Rendering Agent — trace tooth composition, paint hints, implant/missing state and periodontal visual layer.
- Sprite Mapping Agent — independently map tooth index/type, widths, mirroring, and source resource rectangles/files.
- Provenance Agent — identify source paths/license context and distinguish prototype reuse from production suitability.
- YAGNI/Adversarial Agent — identify which generic QDento dental graphics are NOT needed for the first periodontal demo.

You remain the sole writer.

## REQUIRED WORK

Map the minimum first-demo visual states:
- natural/present tooth;
- missing/extracted tooth;
- implant;
- periodontal visual layer required by the selected screen;
- any other state only if one of the accepted deterministic fixtures actually needs it.

For each visual component record:
- source file/symbol;
- resource path;
- sprite sheet or composition source;
- tooth index/type mapping;
- size/width behavior;
- arch/quadrant mirroring or transform;
- whether it belongs to first-demo parity;
- whether it can be replaced independently later.

Create a prototype asset manifest with:
- asset/resource path;
- purpose;
- provenance/source;
- QDento license context;
- direct-use-needed-for-private-demo? yes/no/uncertain;
- expected production replacement? yes/no/uncertain;
- notes.

Do NOT interpret this as legal advice.
Do NOT claim App Store/proprietary distribution is cleared.

Define a platform-independent future rendering contract: the information an iOS `ToothVisualProvider` would need without exposing QDento filenames or Qt types.

Explicitly exclude unrelated treatment/endo/bridge/caries layers unless the selected periodontal screen/fixture requires them.

## ACCEPTANCE CRITERIA

The artifact must allow Plan 03 to build a replaceable tooth-visual provider with no need to rediscover QDento's generic painter internals and no accidental import of unrelated assets.

## COMMIT MESSAGE

`docs: close QDento tooth rendering asset evidence`

## STOP CONDITION

Stop after the analysis manifest is committed and pushed.
Do not copy assets.
Do not implement Swift.
Do not synthesize Plan 01.
