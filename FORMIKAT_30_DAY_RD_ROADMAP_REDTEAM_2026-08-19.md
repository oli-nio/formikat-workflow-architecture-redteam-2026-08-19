# FORMIKAT_30_DAY_RD_ROADMAP_REDTEAM_2026-08-19

**Follow-up to:**  
1. FORMIKAT_WORKFLOW_ARCHITECTURE_REDTEAM_2026-08-19.md  
2. FORMIKAT_ARCHITECTURE_GOAL_ALIGNMENT_REVIEW_2026-08-19.md  

**Date:** 2026-08-19  
**Role:** External R&D Red-Team of the proposed 30-day implementation & experimentation plan  
**Constraint:** No new broad architecture redesign. Strategic architecture is provisionally accepted. Maximise information gain per engineering hour. Prefer cheap falsification, reuse, deterministic methods, and small validated samples.

---

## 1. Executive Verdict

The proposed 30-day plan is **directionally sound** and already incorporates the most important lessons from the two prior reviews (provenance-first atomic layer, claim typing, episode reconstruction as derived, OCEL as non-central, multimodal readiness).

**However, the sequencing is still too front-loaded on engineering and too light on early falsification gates.**

Main risks if executed as written:

| Risk | Severity | Why it matters |
|------|----------|----------------|
| Week 1 spreads across too many source types before the atomic schema is locked | HIGH | Schema decisions become expensive to reverse once CAD/image/PDF collectors start writing |
| Cross-source temporal alignment in Week 2 can silently introduce false causality | HIGH | Contaminates the evidence layer with interpretations |
| Episode reconstruction in Week 3 starts before a hard evaluation of atomic claim quality | MEDIUM–HIGH | Higher-level structures will inherit and amplify errors |
| Week 4 jumps to cross-project patterns and “current project intelligence” queries | MEDIUM | Premature if the episode layer is not yet trustworthy |
| Insufficient explicit reuse plan for the existing 459 events / Gold Benchmark / Guard | HIGH | Risk of parallel recomputation |

**Overall verdict:**  
**KEEP** the four-week spine and the parallel-worker strategy.  
**CHANGE** the internal order of Week 1 and the evaluation gates of Weeks 2–3.  
**MOVE EARLIER** a small end-to-end atomic-schema + provenance test on the existing Gold cases.  
**DEFER** any non-trivial cross-project pattern mining and any “current project intelligence” UI until the end of Week 3 has a clear pass/fail.

Confidence in this verdict: **HIGH**.

---

## 2. Critical Changes to Proposed Plan

| Proposed element | Recommendation | Confidence | Reason |
|------------------|----------------|------------|--------|
| Broad source-type inventory + version-family detection in Week 1 | **MOVE LATER** (start only after atomic schema is frozen on Gold cases) | HIGH | Schema must be stable before collectors write into it |
| Minimal atomic knowledge representation test on 5–10 Gold cases | **MOVE EARLIER** (Day 1–2) | HIGH | Cheapest falsifier of the whole architecture |
| CAD deterministic diff pilot in Week 2 | **KEEP** but gate behind schema freeze | HIGH | Valuable, but must not invent claim types on the fly |
| Cross-source temporal view in Week 2 | **CHANGE** – only BEFORE/AFTER/RELATED_TO; no CAUSED_BY | HIGH | Explicitly required by the prompt and by prior reviews |
| Episode reconstruction in Week 3 | **KEEP** but only after atomic-layer precision/recall gate | HIGH | |
| Cross-project patterns + current-project queries in Week 4 | **MOVE LATER** / shrink scope | HIGH | Treat as optional stretch goal, not core exit |
| Parallel PC strategy (docs / CAD / images) | **KEEP** | HIGH | Good use of existing workers |
| Codex role (read → matrix → plan → implement) | **KEEP** | HIGH | Correct sequencing for Codex |
| Reuse of 459 events, Gold Benchmark, Guard, Chain Reconstructor V1 | **STRENGTHEN** – make explicit migration Day 1–3 | HIGH | Otherwise duplicate work is almost guaranteed |

---

## 3. Revised Week 1 — Evidence Foundation & Schema Freeze

**Original intent is correct; order is wrong.**

### What is correct
- Audit of existing outputs
- EXISTING / PARTIAL / MISSING matrix
- Avoiding duplicate scans
- Testing a minimal atomic representation on human-reviewed Gold cases
- Distinguishing deterministic observation from interpretation (CAD, images)

### What is premature
- Full inventory of every source type across all projects
- File version-family detection at scale
- Extending deterministic metadata “where missing” across the whole corpus

### What is missing
- Explicit migration of the existing 459 events into the candidate atomic schema
- Claim-type (FACT / INTERPRETATION / HYPOTHESIS) labelling on the Gold cases
- A hard schema-freeze decision gate before any new collector writes data

### Revised Week 1 sequence

**Days 1–2 (highest priority)**  
1. Load the 19 Gold Benchmark cases + the 459-event inventory.  
2. Draft the minimal atomic schema (Event, Object, Relation, Claim + polymorphic SourceAnchor + claim_type).  
3. Manually map 5–10 Gold cases into the schema.  
4. Measure: can every important assertion keep a source span? Can FACT vs INTERPRETATION be kept distinct?  
5. Decide: freeze or iterate the schema (max 1 iteration).

**Days 3–4**  
6. Produce EXISTING / PARTIAL / MISSING matrix for all current workers/outputs (email events, CAD analysis, image analysis, document inventory, OCR, project identity, Chain Reconstructor V1, Grounding Guard).  
7. Write migration adapters for the 459 events (do **not** rescan mail).  
8. Define the write-time validator (reject missing anchor or missing claim_type).

**Days 5–7**  
9. Light inventory of available source types on **2–3 completed projects only** (not the whole archive).  
10. Identify which existing deterministic CAD/image/document outputs can be attached as FACT claims without reinterpretation.  
11. Document the first “do not invent causality” rules for temporal links.

**Status of original Week 1 goals:**  
- Audit + matrix → **KEEP**  
- Broad version-family work → **MOVE LATER** (Week 2)  
- Minimal atomic test → **MOVE EARLIER** and make it the exit gate  
- Multi-source inventory → **CHANGE** to 2–3 projects only  

Confidence: **HIGH**.

---

## 4. Week 1 Exit Criteria

Must all be true before Week 2 starts:

1. Atomic schema document exists and is marked **FROZEN** (or “frozen pending one named change”).  
2. ≥ 8 Gold cases are fully mapped with source spans and claim types; human reviewer signs off that no FACT was stored as INTERPRETATION and vice versa.  
3. EXISTING / PARTIAL / MISSING matrix is complete for every known worker output.  
4. Migration path for the 459 events is written and at least 50 events have been successfully migrated (or a clear blocker is documented).  
5. Write-time validator prototype rejects claims that lack anchor or claim_type.  
6. No new large-scale scan of the full archive has been started.

If any of 1–3 fails → **STOP** and redesign the atomic layer before continuing.

---

## 5. Revised Week 2 — Change + Cross-Source Layer (Gated)

### What is correct
- Deterministic CAD/Rhino version comparison (objects added/removed, geometry, position, dimension, layer, block changes)
- Image identity, duplicates, near-duplicates, temporal series
- Document/PDF version comparison where useful
- Explicit separation of temporal relations from causal claims
- Parallel worker assignment (docs / CAD / images)

### What is premature
- Building a “first cross-source temporal view” that is already used for reasoning
- Any automatic promotion of BEFORE/AFTER into CAUSED_BY

### What is missing
- A small controlled experiment that deliberately includes known non-causal temporal proximity (e.g. two independent changes that happened the same morning) and checks that the system does **not** invent causality
- Re-use check: which existing CAD/image analysis outputs can be ingested as FACT without re-running vision models

### Revised focus
1. Version-family indexer (hashes + deterministic metadata) on the 2–3 pilot projects from Week 1.  
2. Pilot deterministic 3DM diff on a small number of version pairs that are already known to humans.  
3. Attach existing image-analysis outputs to stable image identities.  
4. Represent only:  
   - `BEFORE` / `AFTER` (temporal)  
   - `RELATED_TO` / `POSSIBLY_RELATED` (weak)  
   - `CAUSED_BY` only when an explicit evidence claim of type FACT or strong INTERPRETATION exists  
5. Produce one cross-source timeline for **one** project as a human-inspectable artefact, not as a production index.

**Status:** **KEEP** deterministic change detection; **CHANGE** temporal alignment to be strictly non-causal by default; **MOVE LATER** any automated multi-project temporal index.  
Confidence: **HIGH**.

---

## 6. Week 2 Exit Criteria

1. At least one deterministic 3DM version-diff produces a list of added/removed/changed objects that a human CAD user confirms is ≥ 80 % correct on the pilot pairs.  
2. Image identity + near-duplicate detection runs on the pilot projects and produces a human-spot-checked sample.  
3. A written rule document exists: “Temporal proximity never becomes CAUSED_BY without an explicit claim.”  
4. One human-readable cross-source timeline for a single project is reviewed; any invented causality is logged as a defect.  
5. No claims of type CAUSED_BY have been written by an automated process.

Failure of 1 or 3 → do not proceed to episode reconstruction at scale.

---

## 7. Revised Week 3 — Episode Reconstruction + Workflow Discovery

### What is correct
- Using grounded atomic events only
- Treating Trigger→Action→Result as derived, not storage
- Re-evaluating the existing Workflow Chain Reconstructor in the higher-level role
- Looking for repeated structures (clarification loops, decision episodes, missing-info → late-change patterns)

### What is premature
- Running on “multiple projects” before a quantitative atomic-layer quality gate
- Beginning to “identify repeated workflow structures” as if they are already reliable learnings

### What is missing
- An explicit evaluation set of 10–15 human-labelled episodes (or episode fragments) drawn from the Gold cases + 1–2 additional projects
- A precision-oriented metric: “Of the episodes the reconstructor proposes, what fraction does a human accept as real and correctly bounded?”

### Revised focus
1. First, measure atomic-layer quality on the migrated + newly collected pilot data (span support, claim-type correctness).  
2. Only if that gate passes: run the repositioned Chain/Episode Reconstructor on the same pilot projects.  
3. Human review of proposed episodes; record false boundaries, missing steps, and invented causality.  
4. One controlled comparison: Chain Reconstructor V1 linking logic vs. the new derived-episode approach on the same events.  
5. Catalogue candidate repeated structures **as hypotheses**, not as learnings.

**Status:** **KEEP** the goal; **MOVE EARLIER** the atomic quality gate; **CHANGE** “begin identifying repeated structures” into “catalogue candidate structures with provenance”.  
Confidence: **HIGH**.

---

## 8. Week 3 Exit Criteria

1. Atomic-layer sample (Gold + pilot) achieves agreed minimums (e.g. span support ≥ 90 % on reviewed claims, claim-type accuracy ≥ 85 %).  
2. ≥ 10 proposed episodes have been human-reviewed; precision of “this is a coherent real episode” is measured and reported.  
3. No episode is stored without links back to its atomic events and source anchors.  
4. A short written assessment of the existing Workflow Chain Reconstructor V1: what linking logic is reusable, what must be discarded.  
5. Decision recorded: “Episode layer is / is not ready for multi-project pattern search.”

If 1 or 2 fails → **STOP** episode work and return to atomic quality; do not enter Week 4 pattern mining.

---

## 9. Revised Week 4 — Formikat Intelligence V1 (Scoped)

### What is correct
- Multi-project experiment only after the lower layers are trustworthy
- Every learning must retain the chain learning → pattern → episodes → events → evidence
- Primitive “have we seen this before?” questions as conceptual tests
- Explicit refusal to build autonomous agents yet

### What is premature
- Treating “detect first cross-project patterns” as a Week-4 deliverable rather than an exploratory probe
- Testing a broad list of analytic questions (blockers, missing info, handoffs, delays, production consequences) in parallel

### What is missing
- A single, narrow, high-value question chosen in advance (e.g. “late dimension information → later plinth/showcase changes”) so that success/failure is unambiguous
- An explicit “this is still hypothesis, not company learning” labelling

### Revised focus
1. Select **one** candidate pattern that the pilot data already suggests.  
2. Trace it fully: evidence → events → episodes → candidate pattern.  
3. Ask the three primitive current-project questions **conceptually** on one active or recent project (no production UI).  
4. Write an end-of-month assessment: “Is the architecture ready to scale?” with explicit remaining risks.  
5. Freeze all further feature work; document what the next 30 days should and should not contain.

**Status:** **KEEP** the spirit; **CHANGE** from broad pattern discovery to one deep, fully-traced probe; **DEFER** most of the listed analytic questions.  
Confidence: **HIGH**.

---

## 10. Week 4 Exit Criteria

1. One candidate cross-project pattern is fully traced to source evidence and labelled as HYPOTHESIS or PATTERN with confidence.  
2. The three primitive questions have been answered for one project in a human-readable memo (not a product).  
3. End-of-month architecture readiness report exists (pass / conditional pass / fail).  
4. No agent, no recommendation engine, no automated learning write-back into the knowledge store.  
5. All new claims created in Weeks 1–4 are still queryable by claim_type and source anchor.

---

## 11. Overnight Worker Plan

Safe for unsupervised overnight runs (deterministic, idempotent, no LLM interpretation writes):

| Worker | Task | Notes |
|--------|------|-------|
| PC 1 | Hashing, basic PDF/document metadata, version-family candidate lists on pilot projects | Read-only on sources; write only to a staging index |
| PC 2 | Deterministic 3DM geometric/attribute diffs on pre-selected version pairs | Output as FACT candidates only; no causal language |
| PC 3 | Image perceptual hashes, EXIF/metadata, near-duplicate clustering | Same rule: identity and similarity only |

**Never overnight without human gate:**
- Any LLM claim extraction or claim-type assignment
- Any write into the atomic knowledge store
- Any episode reconstruction
- Any promotion of temporal links to CAUSED_BY

---

## 12. Human Review Plan

| Activity | Who | Frequency | Blocking? |
|----------|-----|-----------|-----------|
| Gold-case schema mapping + claim-type check | Domain expert + one technical reviewer | Days 1–2, then as needed | Yes – Week 1 exit |
| EXISTING/PARTIAL/MISSING matrix sign-off | Technical lead | End of Day 4 | Yes |
| CAD diff sample validation | Person who knows the Rhino files | After first pilot diffs | Yes for Week 2 exit |
| Episode boundary review | Domain expert | Mid/late Week 3 | Yes for Week 3 exit |
| Final pattern-trace + readiness memo | Both | End of Week 4 | Yes |

Human review is the primary quality gate; automated metrics are supporting evidence only.

---

## 13. Codex Development Plan

**Correct order (KEEP):**

1. Read handoff + both architecture reviews + this roadmap red-team.  
2. Inspect existing code and result artefacts.  
3. Produce / refine EXISTING / PARTIAL / MISSING matrix.  
4. Identify reusable components (especially migration of 459 events, Guard logic, Chain Reconstructor linking).  
5. Produce a migration-safe implementation plan with explicit “do not recompute” list.  
6. Only then implement.

**Preferred implementation targets (in order):**
- Atomic schema + validator (write-time)
- Migration adapters for existing events
- Version-family indexer (deterministic)
- CAD diff result ingestion (FACT only)
- Image identity index
- Evaluation tooling for Gold / episode precision
- Thin cross-source refiner (temporal relations only)

**Remain experimental scripts (do not harden yet):**
- Episode reconstructor prompts / heuristics
- Any cross-project pattern detector
- Any “current project intelligence” query prototype

---

## 14. Open-Source Components To Evaluate

**Before writing custom code, check:**

| Need | Candidate | Why check first |
|------|-----------|-----------------|
| Event / object log handling, basic process views | PM4Py (OCEL support) | Already identified in prior reviews; use for export/analysis only |
| Near-duplicate / perceptual image hashing | imagehash, pHash, or existing internal vision outputs | Avoid re-implementing |
| PDF text + structure | pdfminer.six, pypdf, or existing OCR pipeline | Reuse before new extraction |
| 3DM / Rhino geometry access | Existing deterministic Rhino/compute pipeline (already mentioned as existing work) | Do not start a second CAD stack |
| Graph storage for events/objects/claims | Simple property graph (NetworkX for experiments, or lightweight DB) | Only if the frozen schema needs it; do not introduce Neo4j etc. yet |
| Embedding / similarity for later episode matching | Existing embedding stack if any; otherwise defer | Not needed in first 30 days |

**Status:** **INVESTIGATE** each before custom implementation. Confidence: **HIGH** for PM4Py and imagehash-class tools; **MEDIUM** for graph choice.

---

## 15. Stop / Pivot Conditions

Stop the 30-day plan and re-open architecture if any of the following occurs:

1. Gold-case mapping shows that a large fraction of important assertions cannot keep a clean source span or claim type.  
2. Deterministic CAD or image pipelines cannot be attached as FACT without heavy reinterpretation.  
3. Episode reconstructor precision on the first human-reviewed sample is < ~60 % and does not improve after one iteration.  
4. Migration of the 459 events proves lossy or forces a schema that contradicts the Goal-Alignment review.  
5. Any automated process begins writing CAUSED_BY or LEARNING claims without human approval.  
6. Parallel workers start producing conflicting identities for the same real-world object/artifact.

Any of these is cheaper to discover in Weeks 1–2 than after a large corpus has been written.

---

## 16. Things Explicitly Deferred

- Autonomous agents / tool calling / action execution  
- Recommendation engine  
- Production “current project intelligence” UI  
- Full cross-project pattern mining across the whole archive  
- Any learning that is written back as company knowledge without human review  
- New proprietary process-model notation  
- Re-scanning of the entire mail archive  
- Multi-agent orchestration for extraction  
- Hardening of episode reconstructor into a product component  
- Optimisation against the 19 Gold cases as the primary objective function  

**Status:** **DEFER**. Confidence: **HIGH**.

---

## 17. End-of-Month Target State

At the end of 30 days the system should be able to demonstrate, on a small number of projects:

1. A frozen atomic schema with mandatory provenance and claim types.  
2. The existing 459 events (or a large fraction) migrated without loss of MAIL references.  
3. Deterministic CAD and image identity/change facts attached for the pilot projects.  
4. Strictly non-causal temporal relations.  
5. A small set of human-reviewed episodes with full provenance chains.  
6. One fully traced candidate cross-project hypothesis.  
7. A written readiness assessment: “We can / cannot yet scale this foundation.”  

It should **not** yet be a company intelligence product, a pattern library, or an agent platform.

That is the maximum information-gain target for the month.

---

## 18. Exact First 10 Actions Starting Now

1. **Freeze a working draft of the atomic schema** (Event, Object, Relation, Claim, SourceAnchor, claim_type) – even if imperfect.  
   **Status:** BUILD now. Confidence: HIGH.

2. **Map 5 Gold cases by hand** into that schema; record every place a span or claim type is ambiguous.  
   **Status:** BUILD now. Confidence: HIGH.

3. **Produce the EXISTING / PARTIAL / MISSING matrix** for all current workers and result artefacts (include the 459 events, Guard, Chain Reconstructor V1, CAD, images, OCR, project identity).  
   **Status:** BUILD now. Confidence: HIGH.

4. **Write the migration adapter sketch** for the 459 events (field mapping only; no rescan).  
   **Status:** BUILD now. Confidence: HIGH.

5. **Implement or prototype the write-time validator** (reject missing anchor or claim_type).  
   **Status:** BUILD now. Confidence: HIGH.

6. **Select 2–3 completed pilot projects** for all subsequent Week 1–2 work; do not open the full archive yet.  
   **Status:** DECIDE now. Confidence: HIGH.

7. **List every existing deterministic CAD and image output** that can be ingested as FACT without new inference.  
   **Status:** BUILD now. Confidence: HIGH.

8. **Draft the “no automatic causality” rule** and circulate for sign-off.  
   **Status:** BUILD now. Confidence: HIGH.

9. **Schedule the first human review session** for the Gold-case mappings (blocking Week 1 exit).  
   **Status:** ORGANISE now. Confidence: HIGH.

10. **Hand the two architecture reviews + this roadmap red-team to Codex** with the explicit instruction: read first, matrix second, implement only after migration-safe plan.  
    **Status:** HANDOFF now. Confidence: HIGH.

---

### Summary of recommendation tags used in this document

| Tag | Meaning in this context |
|-----|-------------------------|
| KEEP | Retain as proposed |
| CHANGE | Keep the goal, alter method or scope |
| MOVE EARLIER | Pull forward in the 30-day sequence |
| MOVE LATER | Push back within or beyond the 30 days |
| DEFER | Explicitly out of scope for these 30 days |
| REMOVE | Do not do |
| INVESTIGATE | Check open-source / existing assets before building |

Confidence levels reflect the degree of evidence from the two prior reviews and the internal consistency of the proposed plan.

---

**If the objective of the next month is maximum information gain about whether the minimum architecture works, I would now start with the 10 actions above, freeze the atomic schema on a handful of Gold cases before any large scan, keep temporal relations strictly non-causal, treat episode reconstruction as a gated experiment, and treat Week 4 as a single deep pattern probe rather than a broad intelligence launch.**
