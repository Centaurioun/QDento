[$superpowers](app://plugins~Plugin_60aea7460bd4819199fd97a9553a5e12)

# TASK GOAL PACKET — PLAN 01 / TASK 6 — SYNTHESIS

## TASK TYPE

PROMPT for one dedicated Codex coordinator/synthesizer instance.

This is the sequential synthesis gate after five completed parallel Plan 01 research lanes.

Do NOT restart the research.
Do NOT re-run the broad QDento audit.
Do NOT implement Swift/iOS.
Do NOT modify QDento application source.
Do NOT modify Clinica application source.

## REQUIRED SUPERPOWERS WORKFLOW

Before repository action:
1. invoke `using-superpowers`;
2. invoke `using-git-worktrees`;
3. use `dispatching-parallel-agents` only for independent READ-ONLY reconciliation/review subproblems;
4. use `verification-before-completion` before claiming readiness.

This is synthesis, not implementation. Do not use `subagent-driven-development` as the controlling workflow.

## SUB-AGENT POLICY

You are the sole writer.

If available, use 3–4 focused read-only sub-agents for independent reconciliation, for example:
- Geometry/Named-Site Reconciler
- Measurement/Edit-Semantics Reconciler
- Findings/Summary Reconciler
- Adversarial Contract Reviewer

All sub-agents:
- read-only;
- no commits/writes/PRs/pushes;
- no nested sub-agents;
- GPT-6 Luna only;
- report evidence/conflicts to you.

Do not use GPT-6 Sol for sub-agents.

## REPOSITORY / BRANCH

Repository:
`Centaurioun/QDento`

Common Plan 01 base:
`4d21c03c11ecd1f36d05c4347f36eb4d45df4576`

Create isolated branch/worktree:

`research/p01-t6-parity-contract-synthesis`

from that exact base.

Do not work on `master`.
Do not write directly to `docs/periodontal-ios-brainstorming`.

## INTEGRATE THE FIVE ACCEPTED EVIDENCE COMMITS

Cherry-pick these commits, in this order, unchanged:

1. T1 — `e65f92d424a6aa6f9b235b28c9d91f40334d7ab9`
2. T2 — `a72f98bf15f0e3754e5ddd1552321b2704a2d597`
3. T3 — `6cdac5dee07ce1732651ebdfd48bcc0d926abcf1`
4. T4 — `f73fb75cebee1e4ede5025dd0b2df2e66ebebba3`
5. T5 — `ebdc26f3aa13963c4b7e47d45cb6d0f7c27ac8fb`

Do not edit the five evidence artifacts after cherry-picking them.

Verify each evidence file blob/content is unchanged from its source task commit.

## AUTHORITATIVE DOCUMENTS TO READ

After cherry-picking, read:

- `docs/superpowers/specs/2026-09-30-qdento-periodontal-ios-design.md`
- `docs/superpowers/plans/2026-09-30-periodontal-ios-master-roadmap.md`
- `docs/superpowers/plans/2026-09-30-01-runtime-parity-evidence-closure.md`
- `docs/superpowers/reviews/2026-09-30-periodontal-ios-plan-review.md`
- `docs/superpowers/reviews/2026-09-30-plan01-parallel-batch-review.md`
- all five `docs/periodontal-ios/research/runtime/A...E...` evidence reports
- fixture README

The approved spec remains binding authority.
The coordinator batch review contains explicit rulings that resolve cross-report ambiguities.

## COORDINATOR RULINGS — MUST APPLY

### Ruling 1 — unnamed QDento q sites are not a remaining geometry blocker

QDento q0/q1/q2 clinical names are undocumented.

Do NOT pretend otherwise.

But the new iOS app has an explicit canonical domain, so freeze this NEW-PRODUCT mapping:

Canonical visible anatomy:
- Q1 18–11 and Q4 48–41:
  - facial visible L/M/R = DB / B / MB
  - oral visible L/M/R = DL / L / ML
- Q2 21–28 and Q3 31–38:
  - facial visible L/M/R = MB / B / DB
  - oral visible L/M/R = ML / L / DL

Combine with source-derived QDento visible q order:

Maxilla q0/q1/q2 = visible left/middle/right:
- Q1 facial: DB/B/MB
- Q1 oral: DL/L/ML
- Q2 facial: MB/B/DB
- Q2 oral: ML/L/DL

Mandible q0/q1/q2 = visible right/middle/left:
- Q4 facial: MB/B/DB
- Q4 oral: ML/L/DL
- Q3 facial: DB/B/MB
- Q3 oral: DL/L/ML

Label this mapping:
`NEW_PRODUCT_CANONICAL_MAPPING`

Do not label it `QDENTO_SOURCE_VERIFIED`.

### Ruling 2 — mandibular surface naming anomaly

Use:
- first stored mandibular triplet = facial/buccal
- second stored triplet = oral/lingual

Treat the contradictory QDento `ChartPosition` enum names as an internal rendering-naming anomaly, not semantic surface authority.

### Ruling 3 — FMPS/FMBS wedge anatomy

Keep:
- left
- up
- right
- down

as neutral IDs.

Anatomical meaning remains:
`NONBLOCKING_PRODUCT_DECISION`

Do not guess mesial/distal/facial/oral names.
A later periodontist review may freeze them.

### Ruling 4 — GM/edit transition for the new iOS app

Canonical domain:
`clinicalGM = CAL - PD`

QDento parity display:
`displayGM = PD - CAL = -clinicalGM`

Direct PD edit:
- change PD only;
- keep CAL;
- recompute both GM representations.

Direct CAL edit:
- change CAL only;
- keep PD;
- recompute both GM representations.

Direct QDento-sign GM edit in the new product:
- keep PD constant;
- compute `CAL = PD - displayGM`;
- constrain valid displayGM input so resulting CAL remains in 0...19;
- for current PD `p`, allowed displayGM range is `p - 19 ... p`;
- never reproduce QDento's legacy branch that can mutate PD or leave hidden negative model PD.

Label this divergence:
`NEW_PRODUCT_RULE`

The QDento legacy GM handler remains documented for reference only.

### Ruling 5 — attached gingiva and recession

Attached gingiva:
- upper facial/buccal applicable;
- upper palatal/oral not applicable;
- lower facial/buccal applicable;
- lower lingual/oral applicable.

Do not carry a meaningful upper-palatal AG value into the canonical new domain merely because the legacy serialized array had a slot.

Recession:
- derived per surface as max(0, max canonical clinical GM across that surface's 3 sites);
- read-only;
- not persisted redundantly.

### Ruling 6 — QDento parity summaries versus modern metrics

Freeze QDento BOP/FMBS/FMPS/HI formulas exactly as source evidence describes, but isolate them as:
`PARITY_LEGACY`

Do not present QDento visible FMPS/HI as a modern plaque-positive percentage.

The later iOS architecture must allow a separate modern assessed-site metric provider.

### Ruling 7 — runtime evidence

No Qt runtime was available.

Reclassify missing sentinel/screenshots as:
`BLOCKS_PARITY_ACCEPTANCE`

not:
`BLOCKS_INITIAL_DOMAIN`
and not:
`BLOCKS_GEOMETRY`

after the canonical product mapping above is frozen.

### Ruling 8 — assets/provenance

Prototype QDento visual assets may remain private-demo/reference candidates with explicit provenance.

Individual image authorship and public/proprietary distribution clearance remain unresolved.

Classify:
`NONBLOCKING_PRIVATE_DEMO`
and
`BLOCKS_PUBLIC_DISTRIBUTION_REVIEW`

Do not make legal-clearance claims.

## ALLOWED WRITES AFTER CHERRY-PICKS

Create:

`docs/periodontal-ios/contracts/2026-09-30-rendering-parity-contract-v1.md`

Modify only:

`docs/periodontal-ios/README.md`

Do not modify the five evidence reports.

## CONTRACT CONTENT REQUIREMENTS

The synthesized Rendering/Parity Contract v1 must include at minimum:

1. authority hierarchy and evidence labels;
2. canonical FDI/tooth order;
3. canonical named-site model;
4. explicit NEW_PRODUCT named-site ↔ QDento visible/q mapping table;
5. facial/oral surface definitions and mandibular enum-name anomaly;
6. QDento local contour equations, baseline, scale, point spacing, and net transform rules;
7. canonical clinical GM ↔ QDento display GM adapter;
8. direct PD/CAL/GM edit transition contract, including the safe NEW_PRODUCT GM rule;
9. AG applicability/storage semantics;
10. derived recession rule;
11. BOP six-site cardinality/visual contract;
12. FMPS/FMBS four-wedge cardinality, neutral geometry IDs, colors, persistence;
13. QDento PARITY_LEGACY summary formulas and denominator behavior;
14. natural/missing/implant tooth visual descriptor/provider boundary;
15. prototype asset/provenance manifest summary and distribution caveat;
16. deterministic fixture IDs and any cross-task corrections/reconciliations;
17. explicit runtime-verification gaps;
18. blocker classification:
   - `BLOCKS_INITIAL_DOMAIN`
   - `BLOCKS_GEOMETRY`
   - `BLOCKS_PARITY_ACCEPTANCE`
   - `NONBLOCKING_PRODUCT_DECISION`
   - `NONBLOCKING_PRIVATE_DEMO`
   - `BLOCKS_PUBLIC_DISTRIBUTION_REVIEW`
   - `DEFERRED`
19. downstream interface requirements for Plan 02 and Plan 03;
20. final readiness verdict.

## REQUIRED RECONCILIATION EXAMPLE

Task 3 fixture F05 predates Task 4's stronger AG surface mapping.

Do not rewrite Task 3.

In the contract, explicitly resolve:
- upper attachment slot 0 = facial/buccal;
- upper slot 1 = palatal/oral not applicable.

Apply the same principle to any other evidence conflict: preserve source artifacts, reconcile in the contract.

## FINAL READINESS RULE

The contract may proceed to Plan 02 when:
- no `BLOCKS_INITIAL_DOMAIN` item remains;
- no `BLOCKS_GEOMETRY` item remains;
- unresolved runtime screenshots are only `BLOCKS_PARITY_ACCEPTANCE`;
- neutral wedge anatomy remains an explicit nonblocking later product/clinical decision.

Expected verdict if evidence supports it:
`READY_WITH_NONBLOCKING_UNRESOLVED_ITEMS`

Do not force this verdict if the integrated evidence contradicts it.

## SIX-CYCLE SYNTHESIS REFINEMENT

Before commit, refine the contract through exactly:

1. Accuracy & Fundamental Correction
2. Completeness & Gap Analysis
3. Structure & Architecture
4. Adversarial / Critical Review
5. Usability & Goal Fit
6. Final Synthesis, Regression Check & Polish

No seventh cycle.

In the adversarial cycle, explicitly attempt to falsify:
- quadrant/site mirroring;
- lower-arch surface naming;
- GM sign conversion;
- direct GM edit validity;
- BOP/FMBS/FMPS conflation;
- AG upper-palatal applicability;
- missing/implant tooth-state assumptions.

## VERIFICATION

Before committing:

- `git diff --check`
- verify all five cherry-picked evidence files match their source branch blobs/content;
- verify only the five evidence commits + contract + README differ from the common base;
- verify QDento application source is untouched;
- re-read contract against the approved spec and all eight coordinator rulings;
- verify no unresolved named-site issue remains labeled `BLOCKS_GEOMETRY` after applying the canonical product mapping.

Commit contract/README with:

`docs: freeze periodontal rendering parity contract v1`

Push branch:
`research/p01-t6-parity-contract-synthesis`

Do NOT merge.
Do NOT open a PR.

## RETURN FORMAT

Return only:

- STATUS: DONE / DONE_WITH_CONCERNS / BLOCKED
- branch
- final HEAD SHA
- integrated evidence commit SHAs
- contract path
- sub-agent roles used
- final verdict
- remaining blocker counts by class
- concise reconciliation summary
- verification summary
