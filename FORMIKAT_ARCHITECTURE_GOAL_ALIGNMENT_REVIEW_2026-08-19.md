# FORMIKAT_ARCHITECTURE_GOAL_ALIGNMENT_REVIEW_2026-08-19

**Follow-up to:** FORMIKAT_WORKFLOW_ARCHITECTURE_REDTEAM_2026-08-19.md  
**Date:** 2026-08-19  
**Purpose:** Re-evaluate the previous red-team recommendations against the actual long-term Formikat Intelligence vision (company memory + empirical learning + prediction + agents), not against process-mining optimality.

**Constraint observed:** No new broad technology search. Prior report re-read. Optimize for Formikat Intelligence.

---

## 1. Revised Executive Verdict

**Previous recommendation (QUALIFY → CHANGE):**

> “Freeze further expansion of the Chain Reconstructor agent and pivot toward Collector → Refiner → OCEL.”

**Revised stance:**

The pivot toward a clean **Collector → Refiner** extraction pattern and mandatory provenance remains correct and high-value for Stage 1 (historical reconstruction).  

However, treating **OCEL as the natural centre of gravity** and declaring the Workflow Chain Reconstructor largely redundant was optimized for process-mining tooling, not for the Formikat Intelligence goal stack (Stages 2–8: empirical workflow discovery, cross-project patterns, learning, current-project intelligence, prediction, recommendation, agents).

**New executive classification:**

| Previous recommendation | Revised status | Confidence |
|-------------------------|----------------|------------|
| Freeze Chain Reconstructor expansion as extraction agent | **KEEP** (do not expand it as extractor) | HIGH |
| Make Chain Reconstruction disappear | **CHANGE** → reposition as higher-level reasoning component over grounded events | HIGH |
| Pivot hard to OCEL as primary target | **QUALIFY** → OCEL is excellent for the structured event/object layer only, not the company knowledge model | HIGH |
| Demote Trigger → Action → Result | **CHANGE** → keep as derived reasoning ontology for episodes, not as atomic storage | HIGH |
| Structural provenance first | **KEEP** (and strengthen with claim-type distinction) | HIGH |
| 19-case Gold Benchmark expansion strategy | **KEEP** | HIGH |

**Core revised risk statement:**  
If Formikat locks the atomic storage layer into a pure OCEL/process-log ontology, higher-level constructs (why a decision was made, alternatives rejected, lessons learned, hypotheses, cross-project causal patterns, agent preconditions) will later have to be bolted on awkwardly or forced into event attributes. That damages the path to Stages 4–8.

**Core revised opportunity:**  
Build a minimal, provenance-first, claim-typed event/object/relationship layer now (compatible with OCEL for analysis where useful) that already distinguishes FACT / INTERPRETATION / HYPOTHESIS, and treat causal chains, decision episodes and Trigger-Action-Result as derived views. This preserves the existing 459 events and benchmark while opening the road to company memory.

---

## 2. Which Previous Recommendations Still Stand

| Recommendation from Red-Team | Status | Why it still holds under the Intelligence vision |
|------------------------------|--------|--------------------------------------------------|
| Separation of discovery from grounding (“Discovery may suggest. Grounding must verify.”) | **KEEP** | HIGH – even more critical once the system produces learnings and recommendations |
| Mandatory source spans / structural provenance | **KEEP** | HIGH – required for fact/interpretation distinction and later agent trust |
| Do not optimise primarily against the 19-case Gold Benchmark | **KEEP** | HIGH – risk of overfitting remains |
| Expand Gold Benchmark with holdouts, hard negatives, ambiguous cases | **KEEP** | HIGH |
| Do not re-implement process discovery / object graphs / conformance that PM4Py already provides | **KEEP** | HIGH – still true for the lower process layer |
| Avoid chronological proximity = causality | **KEEP** | HIGH – becomes existential for learning and prediction |
| Incremental improvement; do not rebuild existing validated components | **KEEP** | HIGH – 459 events, clusters, handoffs, decisions, blockers, Guard, benchmark must be migrated, not discarded |
| Small local LLM (Qwen-class) is insufficient alone for unconstrained generative extraction | **KEEP** | MEDIUM – still true; hybrid remains advisable |

---

## 3. Which Previous Recommendations Change

| Previous recommendation | Revised action | Reason (Intelligence vision) | Confidence |
|-------------------------|----------------|------------------------------|------------|
| Workflow Chain Reconstructor should largely disappear / be merged into Refiner | **CHANGE** → reposition as higher-level causal / decision / episode reconstructor over grounded atomic events | Needed for Stage 2–4 (workflow discovery, patterns, learning) and later agent situation recognition | HIGH |
| Target schema = minimal OCEL-compatible with Trigger/Action/Result demoted | **CHANGE** → atomic storage = events + objects + relationships + typed claims + provenance; Trigger-Action-Result becomes a derived episode view | OCEL cannot cleanly carry “why”, alternatives, lessons, hypotheses, confidence of interpretation | HIGH |
| Pivot the next 2–4 weeks primarily to OCEL mapping + Collector/Refiner measurement | **QUALIFY** → do the clean extraction + provenance work, but simultaneously define the claim-type and relationship model that will outlive pure event-log thinking | Avoid locking the knowledge architecture into an interchange format | HIGH |
| Grounding Guard remains a separate post-hoc component | **CHANGE** → provenance + claim-type enforcement becomes structural at write time; Guard becomes lighter and focused on higher-level derived claims | Matches the required FACT vs INTERPRETATION vs HYPOTHESIS distinction | HIGH |

---

## 4. The Actual Role of OCEL

**Recommendation: B / C hybrid — event/process layer + interchange/analysis format. Not A. Not D.**

| Role | Verdict | Confidence |
|------|---------|------------|
| A) Central knowledge model for all Formikat Intelligence | **No** | HIGH |
| B) Structured event / object / relationship layer | **Yes – primary useful role** | HIGH |
| C) Interchange / analysis format (PM4Py, discovery, visualisation, conformance) | **Yes – secondary role** | HIGH |
| D) Not used | **No** | HIGH |

**Rationale:**  
OCEL (especially 2.0) is excellent at representing “what happened to which objects when, and how objects relate”. That is precisely Layer B in the vision document.  

It is weak or silent on:
- why a decision was taken,
- which alternative was rejected,
- design rationale,
- open hypotheses,
- confidence of an interpretation,
- cross-project pattern statements,
- recommendations and their preconditions,
- agent permission scopes.

Forcing those into OCEL attributes or event types would either under-express them or produce a non-standard, non-interoperable “OCEL+”.  

**Practical rule:**  
Emit or map to OCEL when process-mining tooling adds value (discovery of empirical phases, object lifecycles, frequent paths, bottlenecks). Keep the richer company-memory constructs in a parallel, provenance-linked claim/relationship store. The two can share the same event and object identifiers.

**Status:** **CHANGE** relative to previous report’s stronger OCEL-centric tilt. Confidence: **HIGH**.

---

## 5. Revised Collector / Refiner Model

**Previous:** Single Collector → Refiner inspired by Buss et al., oriented toward OCEL.

**Revised:**

```
Modality-specific Collectors
        ↓
Shared Raw Evidence Store (with spans / media anchors)
        ↓
Shared Refiner / Normaliser
        ↓
Atomic Event + Object + Relationship + Typed-Claim layer
        (provenance enforced)
```

- **Email Collector** (current priority) – extract candidate events, actors, artifacts, decisions, blockers, with source spans.
- Future collectors: Document, CAD/Rhino metadata + change history, Image/Rendering (visual similarity + annotations), Transcript, Schedule/Offer, Agent-action log, HCI log.
- All collectors write into the same evidence and atomic structured layer.
- The Refiner remains largely modality-agnostic: deduplication, temporal ordering, object resolution, claim-type classification, contradiction detection.

**Status:** **CHANGE** (from single-modality OCEL extractor to multi-collector shared layer). Confidence: **HIGH**.

This is the cleanest way to satisfy “email is useful now but must not become structural dependency”.

---

## 6. Proper Role of Workflow Chain Reconstruction

**Previous recommendation to make it largely disappear is revised.**

**New role (KEEP the component idea, CHANGE its place in the stack):**

The Workflow Chain Reconstructor should **not** be an extraction agent that invents chains from raw mail.  

It should become a **higher-level reasoning / derivation component** that operates only on already-grounded atomic events and typed claims. Its job is to propose:

- causal chains (problem → response → result),
- decision chains (options considered → decision → rationale → consequence),
- handoff sequences,
- clarification / feedback loops,
- project episodes (coherent multi-event units that correspond to “a design review cycle”, “a dimension-clarification episode”, etc.),
- dependency hypotheses.

These derived structures are themselves claims of type INTERPRETATION or HYPOTHESIS (or later LEARNING) and must carry provenance back to the atomic events.

**Why this is useful under the Intelligence vision:**
- Stage 2 (empirical workflow discovery) needs episodes and loops, not only flat events.
- Stage 3–4 (patterns and learning) operate on comparable episodes across projects.
- Stage 5–7 (current project, prediction, recommendation) need to match “we are in a situation similar to episode type X”.
- Future agents need compact situation descriptions, not thousands of atomic events.

**Status:** **CHANGE** (reposition, do not delete). Confidence: **HIGH**.

---

## 7. Atomic Storage vs Derived Workflow Semantics

**Clear separation required.**

| Layer | What lives here | Example |
|-------|-----------------|---------| 
| **Atomic storage ontology** | Events, objects, relationships, typed claims, provenance spans, timestamps, actors | “MAIL-0083 contains a statement by Person P that colour looks like RAL 7032” (FACT) |
| **Derived reasoning ontology** | Episodes, Trigger→Action→Result views, causal chains, decision chains, loops, phase assignments | “This sequence constitutes a clarification loop about exhibit dimensions” (INTERPRETATION / HYPOTHESIS) |

Trigger → Action → Result is **not** demoted to oblivion; it is promoted to a useful **derived** view for human and agent reasoning about project episodes. It must never be the only or primary storage schema.

**Status:** **CHANGE** from previous demotion. Confidence: **HIGH**.

---

## 8. Provenance Architecture

**Strengthen beyond the previous report.**

Every stored assertion must carry:

1. **Source anchor(s)** – mail_id + span, or document page + region, or CAD revision + object id, etc.
2. **Claim type** – one of:  
   `FACT` | `INTERPRETATION` | `HYPOTHESIS` | `PATTERN` | `LEARNING` | `RECOMMENDATION`
3. **Confidence** (optional but useful)
4. **derived_from** links when the claim is not atomic
5. **contradicts** / **supports** links when relevant

The write path must reject any claim that lacks a valid source anchor (structural enforcement).  

The previous post-hoc Grounding Guard becomes thinner: it mainly validates higher-level derived claims and maintains consistency of claim types.

**Example from the vision document:**

```
FACT:          Participant stated “Sieht für mich nach RAL 7032 aus.”
INTERPRETATION: RAL 7032 was considered a probable match.
HYPOTHESIS:    (none yet)
FACT about approval: not evidenced → must not be written.
```

**Status:** **KEEP** structural provenance + **CHANGE** to include mandatory claim typing. Confidence: **HIGH**.

---

## 9. Minimum Knowledge Architecture

**Build only this now (Layers A + B + thin typed claims):**

```
A. RAW EVIDENCE
   - mail, files, future modalities
   - immutable, with addresses/spans

B. STRUCTURED ATOMIC LAYER
   - Events (activity, time, actors, objects, attributes)
   - Objects (type, attributes, version if needed)
   - Relationships (event-object, object-object, qualified)
   - Typed Claims (FACT / INTERPRETATION / …) with provenance
   - Identifiers stable across projects

C. (thin) PROCESS / EPISODE VIEW
   - Derived only; produced by the repositioned Chain / Episode Reconstructor
   - Optional OCEL export for analysis tools

D–G. (not built yet)
   - Company knowledge / pattern / learning / prediction / agent layers
   - grow later on top of the stable atomic + provenance foundation
```

This is the smallest architecture that:
- solves current email reconstruction,
- preserves the 459 events,
- enforces provenance from day one,
- is modality-agnostic at the storage level,
- can later support empirical workflow discovery and cross-project learning,
- does not force everything into an event-log ontology.

**Status:** **CHANGE** relative to pure OCEL target. Confidence: **HIGH**.

---

## 10. Multimodal Expansion Path

The architecture above already survives the loss of email dominance:

- New modality → new Collector → same Raw Evidence + Atomic Layer.
- No schema change required for adding CAD change events, image annotations, transcript utterances, or agent tool calls.
- Object identity resolution (same physical exhibit, same drawing revision, same person) becomes the main cross-modality hard problem; design object identity carefully now.

**What to change now to avoid migration pain:**
- Do not hard-code mail-thread or Message-ID assumptions into the core event schema.
- Make source anchors polymorphic (mail span | document region | CAD object+revision | transcript timestamp | …).
- Treat “project” and “artifact” as first-class objects from the beginning.

**Status:** **KEEP** the multi-collector direction; implement the polymorphic anchor now. Confidence: **HIGH**.

---

## 11. Cross-Project Learning Path

Required representation (to be grown later, but anticipated now):

1. Stable object and event identifiers across projects (or reliable resolution).
2. Episode / situation types (derived) that can be matched across projects.
3. Typed PATTERN and LEARNING claims that reference sets of episodes.
4. Ability to ask: “In how many projects of type T did missing dimension info before design approval lead to later plinth changes?”

The atomic layer + episode reconstructor + claim typing is the necessary substrate.  
Do **not** build a full pattern-mining or case-based reasoning engine yet.

**Status:** **DEFER** full cross-project engine; **KEEP** identifier and claim-type design that makes it possible. Confidence: **HIGH**.

---

## 12. Path Toward Prediction

Prediction needs:
- current project state described in the same atomic + episode vocabulary as history,
- historical episodes with outcomes,
- similarity / matching over situations,
- causal (not merely temporal) hypotheses.

The minimum architecture already supplies the vocabulary.  
Prediction models themselves are Stage 6 and should not be built now.

**Status:** **DEFER**. Confidence: **HIGH**.

---

## 13. Path Toward Agentic Execution

A future agent needs at minimum:

| Need | Representation required |
|------|-------------------------|
| Where are we? | Current project state as set of open episodes + recent events + missing expected information |
| What usually happens next? | Derived empirical phase / episode transition patterns |
| What is missing? | Expected information / artifact types that historically appear before the next phase |
| What historically caused problems here? | LEARNING / PATTERN claims linked to similar episodes |
| What should I recommend? | RECOMMENDATION claims with preconditions and provenance |
| What am I allowed to perform? | Explicit action permissions + safety constraints (outside the knowledge core) |

The atomic + typed-claim + episode layer is the knowledge foundation an agent will query.  
Agent runtime, tool calling, permissioning and safety are separate and later.

**Status:** **DEFER** agent runtime; **KEEP** the knowledge foundation that agents will need. Confidence: **HIGH**.

---

## 14. What To Build Now

1. **Polymorphic raw evidence store** with stable anchors (mail spans first).
2. **Atomic event / object / relationship schema** with mandatory provenance and claim-type field.
3. **Migration of the existing 459 events** (and clusters, handoffs, decisions, blockers) into that schema, preserving all MAIL references.
4. **Modality-specific Email Collector + shared Refiner** that emit only grounded, claim-typed statements.
5. **Structural write-time enforcement** (no span → reject; claim type required).
6. **Light higher-level Episode / Chain Reconstructor** that proposes derived structures (as INTERPRETATION/HYPOTHESIS) from the atomic layer.
7. **Gold Benchmark continued use and careful expansion** (holdouts, hard negatives, claim-type correctness).
8. **Optional OCEL export** for any process-mining analysis that proves useful.

Everything else is later.

**Status:** Minimum viable company-memory foundation. Confidence: **HIGH**.

---

## 15. What NOT To Build Yet

- Full cross-project pattern mining engine
- Predictive models
- Recommendation engine
- Agent runtime / tool calling / autonomy
- Heavy multi-agent orchestration on top of extraction
- Forcing all knowledge into a single OCEL document
- Manual idealised workflow diagrams that the data must conform to
- Optimisation loops whose only success metric is the current 19 cases
- New proprietary process-model notation

**Status:** **DEFER** all of the above. Confidence: **HIGH**.

---

## 16. Impact on Existing 459 Events / Gold Benchmark / Grounding Guard

| Asset | Action | Rationale |
|-------|--------|-----------|
| 459 events + 54 clusters + 300 handoffs + 82 decisions + 66 blockers + 12 feedback loops | **Migrate**, do not discard | Already valuable atomic and derived signals; map into new schema with provenance |
| Gold Benchmark V1 (19 cases) | **KEEP** as regression + claim-type test set; expand carefully | Still the only human-reviewed truth; add claim-type labels |
| Grounding Guard | **Evolve** into structural write-time checks + lighter higher-level validator | Post-hoc only is too weak for the FACT/INTERPRETATION distinction |
| Workflow Chain Reconstructor V1 | **Reposition** its linking logic as the seed of the higher-level episode reconstructor | Do not expand its extraction responsibilities |

No work is thrown away; the centre of gravity simply moves upward one layer.

---

## 17. Revised Target Architecture

```
RAW EVIDENCE (polymorphic anchors)
        ↓
MODALITY COLLECTORS (email first; others later)
        ↓
SHARED REFINER / NORMALISER
        ↓
ATOMIC LAYER
  • Events
  • Objects
  • Relationships
  • Typed Claims (FACT | INTERPRETATION | HYPOTHESIS | …)
  • Mandatory provenance
        ↓
STRUCTURAL ENFORCEMENT (write-time)
        ↓
DERIVED LAYER (Episode / Chain / Decision / Loop reconstructor)
  • produces INTERPRETATION / HYPOTHESIS claims only
        ↓
OPTIONAL OCEL EXPORT (for process-mining tools)
        ↓
GOLD BENCHMARK + human review
        ↓
( later ) PATTERN / LEARNING / PREDICTION / RECOMMENDATION / AGENT layers
```

This satisfies the vision’s Layer A–G distinction without building G today.

---

## 18. Exact Next Five Technical Actions

1. **Define and document the atomic schema** (events, objects, relationships, claim types, polymorphic source anchors) and the migration mapping from the current 459 events.  
   **Status:** BUILD now. Confidence: HIGH.

2. **Implement structural write-time enforcement** (reject any claim without valid anchor or claim type) and evolve the Grounding Guard accordingly.  
   **Status:** BUILD now. Confidence: HIGH.

3. **Migrate the existing event inventory** into the new schema, preserving every MAIL reference and current labels (handoff, decision, blocker, …).  
   **Status:** BUILD now. Confidence: HIGH.

4. **Reposition (do not expand) the Chain Reconstructor** so it only proposes derived episode/chain structures as typed claims over the atomic layer; freeze any further extraction features inside it.  
   **Status:** CHANGE now. Confidence: HIGH.

5. **Extend the Gold Benchmark protocol** with claim-type correctness and at least one project-holdout / hard-negative batch (target +15–20 carefully reviewed cases).  
   **Status:** BUILD now. Confidence: HIGH.

---

## Final Statement

If the long-term Formikat Intelligence vision is the objective rather than process mining itself, I would now treat OCEL and process-mining tools as valuable but subordinate instruments for the structured event/object layer only. I would keep the clean Collector → Refiner extraction discipline and the absolute insistence on provenance, but I would refuse to make an event-log ontology the ceiling of the knowledge model. Instead I would build a minimal atomic layer of events, objects, relationships and claim-typed statements (FACT / INTERPRETATION / HYPOTHESIS / …) that already distinguishes evidence from interpretation. I would reposition the Workflow Chain Reconstructor as a higher-level derivation engine that proposes causal and decision episodes from those grounded atoms. I would migrate the existing 459 events and the Gold Benchmark into this foundation rather than discard them. I would design source anchors to be polymorphic from day one so that CAD, images, transcripts and agent logs can later attach without schema rupture. I would defer all pattern mining, prediction, recommendation and agent runtime until the atomic + provenance + episode substrate is solid and human-validated. This is the smallest architecture that solves today’s email reconstruction problem while remaining a credible foundation for a continuously learning company intelligence.
