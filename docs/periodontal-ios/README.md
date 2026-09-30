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


Plan 02 execution start:
- `docs/superpowers/task-goals/2026-09-30-p02-t1-ios-xcode-foundation.md`
  - First Swift/iOS implementation prompt.
  - Creates the private `Centaurioun/periodontal-ios` repository and verified Xcode/Simulator baseline only.
  - Consumes the frozen Plan 01 contract from `freeze/p01-rendering-parity-contract-v1`.
