# Formikat Workflow Architecture Red-Team Review (2026-08-19)

Adversarial R&D architecture and technology review of **Formikat Intelligence**.

## Contents

- `FORMIKAT_WORKFLOW_ARCHITECTURE_REDTEAM_2026-08-19.md` — Full structured red-team report (17 sections)
- `handoff/` — Original Formikat Codex handoff documents as of 2026-08-19

## Key takeaway

The current pipeline is directionally correct but over-layered. Prefer a Collector → Refiner → OCEL path with structural provenance (mandatory source spans) over a separate Workflow Chain Reconstructor agent + post-hoc Grounding Guard.

See the main report for KEEP / CHANGE / REPLACE / INVESTIGATE classifications and the five immediate technical actions.
