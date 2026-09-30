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


# TASK GOAL PACKET — PLAN 01 / TASK 4

## TASK TYPE

PROMPT for one dedicated Codex instance.

## BRANCH

`research/p01-t4-measurement-edit-semantics`

## GOAL

Freeze the exact QDento behavior and constraints for direct PD, CAL, and GM editing, plus attached gingiva and derived recession, so the future native iOS behavior layer does not have to guess.

## ALLOWED WRITES

Create only:

`docs/periodontal-ios/research/runtime/D-measurement-edit-semantics.md`

## REQUIRED SOURCE STARTING POINTS

At minimum inspect:

- `src/Presenter/PerioPresenter.cpp/.h`
- `src/View/Widgets/PerioView.cpp/.h/.ui`
- `src/Model/Dental/PerioStatus.h`
- `src/Model/Parser.cpp`
- `src/Model/Dental/PerioToothData.cpp/.h`
- relevant spinbox/widget source if range/state behavior depends on it

Use Clinica's canonical clinical convention only as the new-domain reference:
positive GM = recession/apical;
negative GM = coronal;
CAL = PD + clinical GM.

Do not rewrite QDento facts to make them match Clinica.

## RECOMMENDED INTERNAL SUB-AGENT SPLIT

If available, dispatch read-only Luna sub-agents such as:

- Edit Handler Agent — independently trace `pdChanged`, `calChanged`, `gmChanged`, ranges, caps and refresh flow.
- Sign/Derived Agent — derive the exact QDento-display ↔ canonical-clinical GM relationship and recession calculation.
- AG/Persistence Agent — map `AG[64]`, applicability/disabled surfaces, serializer behavior, and whether recession is persisted.
- Adversarial Agent — search for edge cases where PD/CAL/GM direct edits behave differently or legacy constraints would be dangerous to copy blindly.

You remain the sole writer.

## REQUIRED WORK

Create an explicit edit-transition table with separate rows for:
- direct PD edit;
- direct CAL edit;
- direct GM edit.

For each row state:
- accepted input/range;
- persisted field(s);
- held-constant field(s);
- recomputed field(s);
- displayed GM;
- canonical clinical GM equivalent;
- CAL relation;
- contour refresh consequences;
- legacy cap/constraint behavior;
- parity status.

Do not hide the fact that QDento's three edit paths are asymmetric.

For attached gingiva:
- map all 64 positions to tooth/surface where possible;
- record max/range;
- record disabled/not-applicable surfaces;
- verify persistence.

For recession:
- derive the exact surface-level formula from source;
- state which three sites feed it;
- verify that it is read-only;
- verify whether it is persisted or derived.

Classify odd legacy behavior as:
- `PARITY_REQUIRED`
- `NEW_PRODUCT_RULE`
- `DEFERRED`

Do not copy a legacy quirk merely because it exists.

If safe runtime testing is possible, run a small transition matrix with deliberately distinctive values and compare UI result to source-derived expectation.

## ACCEPTANCE CRITERIA

A future Swift behavior implementer must be able to implement direct PD/CAL/GM editing and AG/recession behavior from this document without opening QDento source.

Any ambiguity that would change stored values or visible contours must be explicit.

## COMMIT MESSAGE

`docs: map QDento periodontal edit semantics`

## STOP CONDITION

Stop after the artifact is committed and pushed.
Do not implement the Swift behavior model.
Do not synthesize Plan 01.
