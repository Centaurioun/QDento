# Periodontal iOS Exploration Notes

Status: Brainstorming / architectural discovery only.

This directory records the current thinking for a future native iPhone implementation inspired by the QDento Periodontal Measurement workspace. It is intentionally not an implementation plan and does not authorize product-code changes.

Documents:

1. 2026-09-30-brainstorming-decision-log.md
   - Complete decision history.
   - What we decided, what we changed our minds about, and why.
   - Source-of-truth boundaries between QDento, Clinica, and the future iOS application.

2. 2026-09-30-current-direction-and-open-questions.md
   - Current consolidated direction after the latest QDento and Clinica review.
   - Remaining evidence gaps.
   - Items that are deliberately deferred until later phases.

3. 2026-09-30-second-brainstorming-review.md
   - Independent second-pass adversarial review of the first brainstorming synthesis.
   - Source-level refinements for FMPS/FMBS, BOP, mobility, date/lifecycle behavior and remaining parity evidence.
   - Records the second Improved 6-Cycle Iterative Refinement pass.

Key principle:

QDento is the visual and interaction reference for the first parity-oriented iPhone demo. Clinica is the preferred source for a cleaner periodontal domain model and the newer accepted clinical calculation lineage. The future iOS application should be implemented natively rather than as a literal Qt-to-Swift architectural translation.

No Swift product implementation, Xcode project scaffolding, QDento deletion, or Clinica modification is authorized by these notes.


Formal design specification:
- `docs/superpowers/specs/2026-09-30-qdento-periodontal-ios-design.md`
  - Consolidates the two brainstorming rounds into the architectural design proposed for human approval.
  - Defines the phased multi-agent strategy and the Task Goal Packet model.
  - Must be approved before the separate implementation plan is written.


Approved-spec implementation planning set:
- `docs/superpowers/plans/2026-09-30-periodontal-ios-master-roadmap.md`
- `docs/superpowers/plans/2026-09-30-01-runtime-parity-evidence-closure.md`
- `docs/superpowers/plans/2026-09-30-02-ios-foundation-domain.md`
- `docs/superpowers/plans/2026-09-30-03-geometry-static-chart.md`
- `docs/superpowers/plans/2026-09-30-04-interaction-persistence-summary.md`
- `docs/superpowers/plans/2026-09-30-05-parity-acceptance-hardening.md`

These plans implement the approved design in gated phases. They are intended for separate Codex instances / Task Goal Packets, not one monolithic implementation session.


Independent plan review:
- `docs/superpowers/reviews/2026-09-30-periodontal-ios-plan-review.md`
  - Second adversarial review of the approved spec + implementation plans.
  - Records source-backed plan corrections for PD/CAL/GM editing, attached gingiva/recession, tooth rendering/provenance, parity-summary separation, and final reviewer expansion.

Plan 01 evidence and synthesis:
- `docs/periodontal-ios/research/runtime/A-contour-site-orientation.md`
- `docs/periodontal-ios/research/runtime/B-full-mouth-findings.md`
- `docs/periodontal-ios/research/runtime/C-parity-fixtures.md`
- `docs/periodontal-ios/research/runtime/D-measurement-edit-semantics.md`
- `docs/periodontal-ios/research/runtime/E-tooth-rendering-assets.md`
- `docs/periodontal-ios/research/runtime/fixtures/README.md`
  - Five accepted source-evidence reports and deterministic fixture/capture protocol; evidence files are preserved unchanged.
- `docs/periodontal-ios/contracts/2026-09-30-rendering-parity-contract-v1.md`
  - Synthesized rendering, edit, finding, asset, and evidence contract for downstream Plan 02/03 interfaces.
  - Freezes the explicitly labeled new-product named-site mapping and classifies missing QDento runtime captures as a later parity-acceptance gap.
  - Readiness: `READY_WITH_NONBLOCKING_UNRESOLVED_ITEMS`; no initial-domain or geometry blocker remains.
