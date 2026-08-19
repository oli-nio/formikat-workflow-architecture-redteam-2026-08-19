# FORMIKAT_WORKFLOW_ARCHITECTURE_REDTEAM_2026-08-19

**External Senior R&D Reviewer Report**  
**Date:** 2026-08-19  
**Scope:** Adversarial architecture & technology review of Formikat Intelligence (workflow reconstruction from historical project email / communication).  
**Constraint observed:** No implementation, no rewrite-from-scratch recommendations that ignore existing validated components, pure challenge + evidence-based guidance.

---

## 1. Executive Verdict

**The current pipeline is directionally correct but structurally premature and over-layered.**

```
MAIL INDEX → SOURCE EVIDENCE → EVENT DISCOVERY → WORKFLOW CHAIN RECONSTRUCTOR → GROUNDING GUARD → GOLD BENCHMARK → VALIDATED WORKFLOW MEMORY
```

**Verdict classification:**

| Layer | Status | Recommendation |
|-------|--------|----------------|
| Mail Index + Source Evidence | KEEP | HIGH confidence |
| Event Discovery | CHANGE | HIGH |
| Workflow Chain Reconstructor as separate agent | REPLACE (merge) | HIGH |
| Grounding Guard (post-hoc) | CHANGE → structural provenance | HIGH |
| Gold Benchmark (19 cases) | KEEP + expand carefully | HIGH |
| Trigger → Action → Result ontology | REPLACE with minimal object-centric schema | HIGH |
| Qwen 3.5 4B as primary extractor | INVESTIGATE / hybrid | MEDIUM |

**Core risk:** Formikat is inventing a multi-agent “chain reconstructor + grounding guard” stack while the research community (especially 2025–2026 object-centric process mining from text) has already converged on a cleaner **Collector → Refiner → OCEL** pattern that directly produces grounded, object-centric event logs. Continuing on the current path risks locking into an architecture that cannot leverage PM4Py / OCEL 2.0 tooling and that overfits a tiny 19-case benchmark.

**Net recommendation:** Freeze further expansion of the Chain Reconstructor agent. Pivot the next 2–4 weeks to (1) mapping existing 459 events into a minimal OCEL-compatible schema with mandatory source spans, and (2) measuring whether a Collector/Refiner style extraction (inspired by Buss et al. 2025/2026) outperforms the current multi-stage agent pipeline on the Gold Benchmark.

---

## 2. What Formikat Got Right

- **Separation of discovery from grounding** (“Discovery may suggest. Grounding must verify.”) is philosophically correct and rare. Most LLM process-mining prototypes skip provenance.
- Building a human-reviewed Gold Benchmark early (even if only 19 cases) is excellent discipline.
- Treating email as the primary unstructured source is high-value; structured ERP logs miss exceptions, blockers, and informal handoffs.
- Explicit tracking of handoffs (300), decisions (82), blockers (66), feedback loops (12) shows domain insight into design/project workflows.
- Incremental improvement stance (“Do not rebuild existing components”) is wise given the current state.

These strengths should be preserved.

---

## 3. Critical Architecture Risks

1. **Unnecessary agent boundary.** The Workflow Chain Reconstructor V1 currently only links event candidates to MAIL-IDs. Semantic reconstruction (Trigger/Action/Result) is planned as the next phase. This creates an artificial split that forces two LLM calls and two places where hallucination can enter. The 2025/2026 literature collapses collection and refinement into a tighter loop.

2. **Post-hoc grounding is weak.** Exact match 3/19 and field coverage 32/57 indicate the Guard is not yet constraining the model strongly enough. Unsupported claims can still be generated and then filtered; they should be structurally impossible to emit.

3. **Case-centric bias risk.** The planned Trigger → Action → Result schema is essentially a flattened case view. Real project workflows are multi-object (issue, document version, person, milestone, decision). Object-centric representations are the 2024–2026 consensus for this reason.

4. **Benchmark overfitting danger.** 19 human-reviewed cases is useful for calibration but dangerous as an optimization target. Any improvement measured only against these 19 is likely to be non-generalizable.

5. **Temporal = causal fallacy.** Linking by chronological proximity or simple MAIL reference chains will produce spurious causality (especially in long email threads with quoted replies and parallel sub-threads).

6. **Small local model over-reliance.** Qwen 3.5 4B (or Qwen3-4B) can be useful for classification and light interpretation, but pure generative extraction of multi-argument events from email is still hard even for larger models (MailEx 2023 results remain relevant).

---

## 4. Relevant 2025/2026 Research

### Primary paper (must-read)
- **Buss, Kecht, Kratsch, Röglinger, Sadeghianasl, Wynn (2025/2026)**  
  - Conference version: “From Words to Workflows: Extracting Object-Centric Event Logs from Textual Data” (CAiSE 2025 Forum, Springer).  
  - Journal version: “Process mining between the lines: Extracting object-centric event logs from textual data”, *Information Systems* Vol. 140, 2026, Article 102713.  
  - Architecture: **Collector** (identifies events, objects, attributes, relationships) + **Refiner** (consolidates, cleans, deduplicates across multiple texts).  
  - Four variants: HEU-HEU, HEU-GEN, GEN-HEU, GEN-GEN. Generative collector + heuristic refiner showed strongest generalization.  
  - GitHub: https://github.com/Alinabuss/OCEL-extractor (code + evaluation data).  
  - Direct relevance: almost exactly the problem Formikat is solving, but outputs standard OCEL instead of proprietary chains.

### Other high-signal work
- Egger et al. (2025) “Refining the process picture: Unstructured data in object-centric process mining” – OCRAUD reference architecture for combining unstructured + structured sources into OCEL.
- PMAx / ProMoAI (Kourani et al., 2026) – agentic process mining that keeps computation deterministic and local (Engineer agent generates PM4Py code; Analyst interprets). Strong privacy and anti-hallucination design.
- MailEx (Srivastava et al., EMNLP 2023) – still the best public email-thread event extraction dataset (1.5k threads, 10 event types, 76 arguments). Challenges remain: non-continuous/shared triggers, non-named-entity arguments, conversational history.
- OCEL 2.0 specification (Berti et al., 2024, arXiv:2403.01975) – now the de-facto standard; supports object changes, qualified relationships, multiple exchange formats.
- Grounding / provenance: AEVS (2026) Anchor-Extraction-Verification-Supplement framework; evidence-gated extraction pipelines that refuse to emit triples without sentence-level provenance; PaperTrail and VeriGraph for claim-level evidence graphs.

---

## 5. Relevant GitHub Projects

| Repository | Status / Activity | Relevance to Formikat |
|------------|-------------------|-----------------------|
| https://github.com/Alinabuss/OCEL-extractor | Active (2025 paper code) | **Direct candidate for Collector/Refiner concepts and evaluation methodology.** Can replace or heavily inform Event Discovery + Chain Reconstructor. |
| https://github.com/process-intelligence-solutions/pm4py | Very active, maintained by PIS | **Must-adopt** for any OCEL handling, discovery, filtering, object graphs. Do not re-implement. |
| https://github.com/fit-process-mining/ProMoAI (incl. PMAx) | Active 2026 | Agentic analytics pattern; Engineer/Analyst separation; local code execution for determinism. |
| https://github.com/salokr/Email-Event-Extraction | Stable (MailEx 2023) | Taxonomy, annotation guidelines, baseline models for email event extraction. Use for evaluation, not production. |
| https://github.com/ocpm/ocpa | Maintained | Object-centric process analysis library (alternative/complement to PM4Py OCEL features). |
| https://www.ocel-standard.org | Standard site | Schema definitions, example logs, exchange formats. |

No high-quality open-source “email → full workflow chain” end-to-end system was found that Formikat should simply adopt. The gap Formikat is filling is real; the architecture is the problem.

---

## 6. Relevant Hugging Face Models/Datasets

- MailEx dataset: originally released with the paper; check Hugging Face / GitHub for current hosting (salokr/MailEx mentioned in 2023 announcements).
- Qwen3 / Qwen3.5 family (including 4B variants): currently used by Formikat. Recent local process-mining experiments (GraphRAG conversational PM) have successfully used quantized Qwen3-4B for reasoning over already-extracted graphs, but not as the primary event extractor.
- No dedicated “email-to-OCEL” model exists on HF as of this review. Fine-tuning or strong prompting of a mid-size model on domain data remains necessary.

---

## 7. Relevant X Discussions / Emerging Techniques

X search for recent technical threads on object-centric process mining + LLM + email/text yielded low signal (mostly non-technical or off-topic). Demonstrated systems are still concentrated in academic GitHub + arXiv rather than public Twitter demos. Treat any “we extracted workflows from Slack with GPT-4” claims as anecdotal until code + evaluation appear.

Emerging technique worth watching: **structural provenance enforcement** (anchors / source spans required before a claim can be written to the store). This is stronger than post-hoc grounding.

---

## 8. Comparison With Object-Centric Process Mining

**Buss et al. Collector/Refiner vs Formikat Event Discovery → Chain Reconstructor → Grounding Guard**

| Aspect | Buss et al. (2025/2026) | Formikat current | Recommendation |
|--------|-------------------------|------------------|----------------|
| Output format | Standard OCEL 2.0 | Proprietary workflow_chains.json + MAIL refs | CHANGE → emit OCEL (or OCEL-compatible) |
| Extraction stages | Collector then Refiner | Discovery then separate Chain agent then Guard | MERGE / simplify |
| Object-centric | Native (events linked to multiple objects) | Implicit / planned later | ADOPT |
| Grounding | Implicit via extraction constraints + cleaning | Explicit post-hoc Guard | STRENGTHEN with structural spans |
| Evaluation | Synthetic texts from 6 public OCEL logs + 2 natural corpora; F1 reported | 19 human Gold cases | LEARN methodology; expand benchmark |
| LLM role | Generative collector works best | Planned Qwen for interpretation | ALIGN |

**Conclusion:** Formikat should adopt the Collector/Refiner conceptual split and the OCEL target schema. The separate “Workflow Chain Reconstructor” agent is largely redundant if the collector already produces linked events with provenance.

---

## 9. Grounding & Provenance Assessment

**Current design (LLM interpretation → separate Grounding Guard) is the weaker of the two viable architectures.**

Stronger pattern observed in 2025–2026 work:
- Every extracted element must carry a **source span** (character offsets or sentence ID) at extraction time.
- Downstream stores refuse to accept any claim lacking a valid `derived_from` pointer.
- Verification becomes a structural check + lightweight entailment, not a full second LLM pass that can itself hallucinate.

Recommended evolution for Grounding Guard:
1. Make provenance mandatory in the event schema.
2. Move verification earlier (during collection).
3. Keep a lightweight Guard for higher-level inferences (e.g., “this sequence constitutes a blocker”) that are derived rather than directly stated.

This is the only way “unsupported claims literally cannot enter the knowledge store.”

---

## 10. Gold Benchmark Assessment

- **19 cases is enough for the current prototype stage** (calibration, smoke-testing, regression).
- It is **not enough** for model selection, hyper-parameter tuning, or claiming generalization.
- Legitimate uses: measuring exact-match / field coverage of the Guard; detecting obvious regressions.
- Forbidden uses: primary optimization target for the extractor or reconstructor.

**Expansion strategy (recommended order):**
1. Project holdout (new projects never seen during development).
2. Temporal holdout (emails after a cutoff date).
3. Hard negatives (plausible but incorrect event links, parallel threads, quoted-reply noise).
4. Ambiguous cases (partial evidence, conflicting statements).
5. Adversarial / edge cases (missing timestamps, multi-party handoffs, implicit decisions).
6. Cross-project evaluation once ≥50–80 high-quality cases exist.

Target: 50–80 human-reviewed cases before treating the benchmark as a reliable development signal. Continue human review; do not auto-label.

---

## 11. Recommended Event Schema

**Smallest useful schema (object-centric, provenance-first):**

```text
Event:
  - event_id
  - activity (string, controlled vocabulary preferred)
  - timestamp (or temporal interval / relative order)
  - actors: list of participant IDs / roles
  - objects: list of {object_id, object_type, qualifier}
  - attributes: key-value (free or typed)
  - source_spans: list of {mail_id, start_char, end_char}   ← mandatory
  - confidence (optional)

Object:
  - object_id
  - object_type (Issue, Document, Decision, Milestone, Person, …)
  - attributes (versioned if needed)

Relationship:
  - event-to-object
  - object-to-object (with qualifier)
```

**Explicitly drop or demote for now:**
- Rigid Trigger / Action / Result as first-class top-level fields (can be derived later).
- Full state_before / state_after unless clearly evidenced.
- Complex constraint / dependency graphs until the basic event log is solid.

This schema is already compatible with OCEL 2.0 and can be consumed by PM4Py.

---

## 12. Recommended Target Architecture

```
MAIL INDEX
    ↓
SOURCE EVIDENCE (with spans)
    ↓
COLLECTOR  (LLM or hybrid → candidate events + objects + spans)
    ↓
REFINER    (deterministic + light LLM cleaning, deduplication, temporal ordering with causality safeguards)
    ↓
PROVENANCE ENFORCEMENT (structural – no span → rejected)
    ↓
OCEL / object-centric event log
    ↓
GROUNDING GUARD (only for higher-level derived claims)
    ↓
GOLD BENCHMARK evaluation
    ↓
VALIDATED WORKFLOW MEMORY (queryable via PM4Py + lightweight semantic layer)
```

The separate “Workflow Chain Reconstructor” agent disappears; its useful logic moves into the Refiner and into standard process-mining algorithms operating on the OCEL.

---

## 13. Build vs Adopt Matrix

| Component | Recommendation | Confidence |
|-----------|----------------|------------|
| Mail indexing & source evidence | KEEP (build) | HIGH |
| Event / object extraction core | ADOPT concepts + code patterns from OCEL-extractor; adapt to email | HIGH |
| OCEL schema & storage | ADOPT (PM4Py + OCEL 2.0) | HIGH |
| Process discovery / visualization / filtering | ADOPT (PM4Py / ocpa) | HIGH |
| Provenance / source-span enforcement | BUILD (structural) | HIGH |
| Higher-level claim verification (Guard) | KEEP but simplify | MEDIUM |
| Gold Benchmark process | KEEP + expand | HIGH |
| Full agentic Chain Reconstructor | REPLACE | HIGH |
| Trigger-Action-Result ontology | REPLACE with object-centric | HIGH |
| End-to-end workflow memory query layer | BUILD (thin) on top of OCEL | MEDIUM |
| Small local LLM for extraction | INVESTIGATE hybrid (LLM for hard cases, rules/embeddings for easy) | MEDIUM |

---

## 14. Immediate Changes

1. **Stop expanding the Chain Reconstructor agent.** Document its current linking capability and freeze feature work.
2. **Define the minimal event + object schema with mandatory source_spans** and map the existing 459 events onto it (even if many fields are null).
3. **Clone and study https://github.com/Alinabuss/OCEL-extractor.** Run its evaluation methodology on a subset of Formikat emails.
4. **Instrument the Gold Benchmark** so every prediction can be inspected for span support, not just exact/field match.
5. **Add explicit “causality vs chronology” flags** in the Refiner stage.

---

## 15. Things NOT To Build

- Another full multi-agent orchestration layer on top of the existing one.
- A proprietary process-model notation when OCEL + PM4Py already exist.
- Heavy reliance on a 4B model for unconstrained generative event extraction without strong structural constraints.
- Optimization loops whose only success metric is the current 19-case Gold Benchmark.
- Assuming chronological order in an email thread equals causal order.
- Re-implementing object graphs, variant detection, or conformance checking.

---

## 16. Experiments To Run Next

1. **Collector baseline:** Take 30–50 Formikat emails, run a prompt that forces OCEL-style output + source spans; measure span-supported precision/recall against the Gold cases.
2. **Ablation:** Current multi-stage pipeline vs single-pass constrained extraction on the same 19 cases.
3. **Temporal robustness:** Manually inject parallel threads and quoted replies; measure how often the system invents false handoffs.
4. **Model size sweep:** Qwen 4B vs 7B/9B vs a larger API model on the same constrained extraction task (cost vs quality).
5. **Hard-negative set:** Construct 20–30 cases that look like events but are not (status updates, pure opinions, FYI). Measure false-positive rate.

---

## 17. Sources

**Papers / standards**
- Buss et al., “From Words to Workflows…” (CAiSE 2025) and “Process mining between the lines…” (*Information Systems* 2026).
- Srivastava et al., “MailEx: Email Event and Argument Extraction”, EMNLP 2023, arXiv:2305.13469.
- OCEL 2.0 Specification, arXiv:2403.01975; https://www.ocel-standard.org.
- ProMoAI / PMAx papers and tool (2024–2026).
- Various 2025–2026 grounding/provenance papers (AEVS, evidence-gated extraction, VeriGraph, PaperTrail).

**GitHub**
- https://github.com/Alinabuss/OCEL-extractor
- https://github.com/process-intelligence-solutions/pm4py
- https://github.com/fit-process-mining/ProMoAI
- https://github.com/salokr/Email-Event-Extraction
- https://github.com/ocpm/ocpa

**Other**
- PM4Py documentation and OCEL support.
- Hugging Face / arXiv searches performed 2026-08-19.
- Formikat handoff documents (CURRENT_STATE.md, ARCHITECTURE.md, etc.).

All claims above are grounded in the cited primary sources or direct inspection of the provided Formikat state documents. Items that could not be fully verified (exact current stars/activity of every repo, private Slack/X demos) are marked implicitly by reliance on public academic artifacts only.

---

**If I were responsible for Formikat Intelligence tomorrow morning, I would:**

1. Freeze all further feature work on the Workflow Chain Reconstructor agent and write a one-page “current capability + known limitations” note.
2. Define and implement the minimal provenance-first event/object schema (mandatory source spans) and convert the existing 459 event candidates into that schema.
3. Clone https://github.com/Alinabuss/OCEL-extractor, read the prompts and evaluation code, and run a controlled Collector-style extraction on a 30-email Formikat sample.
4. Expand the Gold Benchmark by at least 15 carefully chosen hard cases (project holdout + temporal holdout + hard negatives) under the same human-review protocol.
5. Instrument every extraction path so that any claim without a verifiable source span is rejected before it reaches the Guard or Memory.

These five actions prevent further investment in the current over-layered design while preserving everything Formikat has already validated.
