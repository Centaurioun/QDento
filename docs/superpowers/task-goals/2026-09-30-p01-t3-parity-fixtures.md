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


# TASK GOAL PACKET — PLAN 01 / TASK 3

## TASK TYPE

PROMPT for one dedicated Codex instance.

## BRANCH

`research/p01-t3-parity-fixtures`

## GOAL

Define deterministic, reproducible QDento parity fixtures that will become the shared oracle for later Swift unit/UI tests and visual comparison.

This task creates fixture specifications, not product code.

## ALLOWED WRITES

Create only:

- `docs/periodontal-ios/research/runtime/C-parity-fixtures.md`
- `docs/periodontal-ios/research/runtime/fixtures/README.md`

If screenshot files already exist locally and can be added without inventing/altering evidence, you may place ONLY clearly provenance-labeled reference captures inside:
`docs/periodontal-ios/research/runtime/fixtures/`

Do not generate fake “QDento screenshots.”

## RECOMMENDED INTERNAL SUB-AGENT SPLIT

If available, dispatch read-only Luna sub-agents such as:

- Fixture Coverage Agent — design the smallest fixture set that catches mirrored site/orientation and sign errors.
- Reproducibility Agent — check whether another person can reproduce every fixture from written inputs alone.
- Adversarial Agent — look for gaps such as direct CAL editing, AG/recession, missing/implant visuals, or denominator behavior.

You remain the sole writer.

## REQUIRED FIXTURE SET

Define at least these eight fixtures:

1. healthy baseline;
2. asymmetric 3-site contour;
3. recession / GM-sign case;
4. direct CAL-edit transition case;
5. attached-gingiva + derived-recession surface case;
6. BOP case;
7. FMPS/FMBS wedge case;
8. missing/implant tooth-visual case.

For every fixture state:
- fixture ID;
- arch/tooth/FDI;
- exact site values;
- PD/CAL/GM values and which value is directly edited when relevant;
- BOP values;
- FMPS/FMBS neutral wedge values;
- attached gingiva when relevant;
- mobility/furcation when relevant;
- tooth state (natural/missing/implant);
- expected visible relationships;
- expected parity-summary change only where source behavior is explicitly known;
- evidence status per expected result.

Design asymmetric values specifically so left/right or facial/oral mirroring cannot accidentally pass.

## SCREENSHOT CAPTURE PROTOCOL

Define:
- file naming;
- required context/crop;
- arch/surface visible;
- tooth identity;
- entered values;
- screenshot scale/zoom;
- evidence label;
- source/runtime version/ref.

A future agent must be able to reproduce a fixture without asking what you meant.

## ACCEPTANCE CRITERIA

The fixture specification must be platform-independent and strong enough that:
- QDento runtime evidence;
- Swift geometry unit tests;
- SwiftUI interaction tests;
- final parity screenshots

can all refer to the same fixture IDs.

## COMMIT MESSAGE

`docs: define QDento parity fixtures`

## STOP CONDITION

Stop after fixture docs/evidence are committed and pushed.
Do not implement fixtures in Swift.
Do not synthesize Plan 01.
