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
