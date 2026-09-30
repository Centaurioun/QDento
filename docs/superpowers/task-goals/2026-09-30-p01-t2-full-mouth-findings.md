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


# TASK GOAL PACKET — PLAN 01 / TASK 2

## TASK TYPE

PROMPT for one dedicated Codex instance.

## BRANCH

`research/p01-t2-full-mouth-findings`

## GOAL

Freeze the source/runtime behavior of QDento FMPS, FMBS, and six-site BOP needed for visual parity and parity-summary feedback, while keeping these concepts clinically and structurally distinct.

## ALLOWED WRITES

Create only:

`docs/periodontal-ios/research/runtime/B-full-mouth-findings.md`

## REQUIRED SOURCE STARTING POINTS

At minimum inspect:

- `src/View/Graphics/PerioGraphicsButton.cpp/.h`
- `src/View/Widgets/PerioView.cpp/.h`
- `src/Presenter/PerioPresenter.cpp/.h`
- `src/Model/Dental/PerioStatus.h`
- `src/Model/Parser.cpp`
- `src/Model/Dental/PerioStatistic.cpp/.h`
- relevant statistic view code when needed to connect calculation to displayed value

## RECOMMENDED INTERNAL SUB-AGENT SPLIT

If available, dispatch read-only Luna sub-agents such as:

- Wedge Agent — map `index % 4`, wedge geometry, colors, tooth-group ordering.
- BOP Agent — map six-site BOP controls to tooth/site indices and visual marker behavior.
- Statistics Agent — independently derive visible BOP/FMBS/FMPS/HI formulas and denominator behavior.
- Adversarial Agent — check disabled/missing-tooth effects, upper/lower order, and whether any formula was mislabeled.

You remain the sole writer.

## REQUIRED WORK

Verify and document four-wedge source behavior:
- 0 = left
- 1 = up
- 2 = right
- 3 = down

Verify:
- upper/lower 128-index grouping;
- tooth-to-four-wedge-group alignment;
- selected/unselected colors and geometry;
- persistence of FMPS and FMBS;
- distinction between FMBS and six-site BOP.

For BOP:
- map source index to the six-site system where evidence permits;
- record icon/state behavior;
- record presenter/statistic refresh behavior;
- record final visible placement where runtime/screenshot evidence permits.

Freeze the QDento parity-summary behavior:
- exact visible BOP statistic formula/denominator;
- exact FMBS statistic formula/denominator;
- exact FMPS/HI formula and whether it counts positive or negative/clean values;
- disabled/missing tooth denominator behavior;
- any discrepancy between variable names and displayed meaning.

Label these formulas `PARITY_LEGACY`.
They are NOT final clinical authority.

Do NOT anatomically name left/up/right/down wedges unless source evidence explicitly does so.

If runtime is available, capture at least:
- one FMPS wedge active;
- one FMBS wedge active;
- one BOP site active;
on a clearly identified tooth.

## ACCEPTANCE CRITERIA

The artifact must allow later code to reproduce:
- four-wedge visual placement;
- tooth grouping;
- BOP site visualization;
- QDento-like parity summary updates;
without conflating these findings with Clinica six-site plaque/BOP clinical metrics.

## COMMIT MESSAGE

`docs: map QDento full-mouth finding controls`

## STOP CONDITION

Stop after the artifact is committed and pushed.
Do not implement a Swift provider.
Do not synthesize Plan 01.
