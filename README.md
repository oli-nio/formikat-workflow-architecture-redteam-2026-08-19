# Formikat Intelligence — Architecture & R&D Red-Team Reviews (2026-08-19)

External senior R&D reviews of Formikat Intelligence.

## Documents in this repository

1. **FORMIKAT_WORKFLOW_ARCHITECTURE_REDTEAM_2026-08-19.md**  
   Original adversarial architecture & technology review (process-mining-aware).

2. **FORMIKAT_ARCHITECTURE_GOAL_ALIGNMENT_REVIEW_2026-08-19.md**  
   Re-evaluation of the first report against the full Formikat Intelligence vision (company memory, cross-project learning, agents). Corrects over-emphasis on OCEL / process mining.

3. **FORMIKAT_30_DAY_RD_ROADMAP_REDTEAM_2026-08-19.md**  
   Red-team of the proposed 30-day implementation & experimentation plan. Focus: sequencing, evaluation gates, reuse of existing 459 events, deterministic vs LLM work, stop/pivot conditions.

## Handoff snapshot

The `handoff/` folder contains the original Formikat Codex handoff documents as of 2026-08-19 (unchanged).

## Key principle across all three reviews

Provenance-first atomic knowledge (events + objects + relations + typed claims) is the foundation.  
OCEL is useful for process analysis/export only.  
Episode / chain reconstruction is a derived higher-level layer.  
The next 30 days must maximise information gain and cheaply falsify assumptions, not build the full intelligence system.
