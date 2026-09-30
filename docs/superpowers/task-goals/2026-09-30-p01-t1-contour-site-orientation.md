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


# TASK GOAL PACKET — PLAN 01 / TASK 1

## TASK TYPE

PROMPT for one dedicated Codex instance.

## BRANCH

`research/p01-t1-contour-site-orientation`

## GOAL

Create the authoritative source/runtime mapping between QDento periodontal measurement indices, visible chart positions, and the canonical six named periodontal sites needed by the later iOS geometry engine.

Do not redesign anything.
Do not implement anything.
This task answers: “Which QDento measurement index/site ends up at which visible contour point and surface/orientation?”

## ALLOWED WRITES

Create only:

`docs/periodontal-ios/research/runtime/A-contour-site-orientation.md`

## REQUIRED SOURCE STARTING POINTS

At minimum inspect:

- `src/View/Widgets/PerioView.cpp`
- `src/View/Widgets/PerioView.h`
- `src/View/Graphics/PerioChartItem.cpp`
- `src/View/Graphics/PerioChartItem.h`
- `src/View/Graphics/PerioScene.cpp`
- relevant tooth/index helpers if needed

Use Clinica only for the canonical semantic names/order:
`MB, B, DB, ML, L, DL`.

Do NOT let Clinica override QDento's observed visual orientation.

## RECOMMENDED INTERNAL SUB-AGENT SPLIT

If available, dispatch read-only Luna sub-agents such as:

- Source Mapping Agent — trace all `chartIndex[index].position/index` assignments and measurement-array ordering.
- Transform/Geometry Agent — independently trace maxillary/mandibular and facial/oral transforms, baseline, scale, x-spacing and tooth widths.
- Adversarial Verification Agent — attempt to find mirrored/quadrant/order mistakes in the first two analyses and check representative indices.

You remain the sole writer and adjudicator.

## REQUIRED WORK

Produce a complete mapping for the 192 PD/CAL/GM/BOP site indices.

Record:
- tooth/QDento tooth index;
- FDI where source evidence supports the association;
- QDento measurement index;
- QDento chart position;
- local chart index;
- source construction order;
- final visible left/middle/right location for each 3-site surface group where verifiable;
- maxillary buccal/facial orientation;
- maxillary palatal/oral orientation;
- mandibular buccal/facial orientation;
- mandibular lingual/oral orientation;
- exact source transforms;
- local baseline/scale and x-spacing/tooth-width behavior.

Use asymmetric sentinel reasoning. If local runtime can be executed safely, use distinguishable values such as 1/4/8 at one representative tooth per arch/surface to confirm orientation. Capture evidence paths/commands in the report. Do not modify runtime source merely to make the test easier.

Clearly separate:
- QDento source fact;
- runtime observation;
- canonical Clinica site naming;
- inference;
- unresolved mapping.

Do not assign a named clinical site merely because it “looks likely.”

## ACCEPTANCE CRITERIA

The artifact must make it possible for a later Swift geometry implementer to map a canonical `ToothID + PeriodontalSite` to the correct QDento-faithful visible contour point without inspecting QDento source again.

No `BLOCKS_GEOMETRY` ambiguity may be hidden.

## COMMIT MESSAGE

`docs: map QDento contour site orientation`

## STOP CONDITION

Stop after the evidence artifact is committed and pushed.
Do not synthesize the final cross-task parity contract.
Do not start any other Plan 01 task.
