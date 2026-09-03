# Synanton Project Roadmap

**Status:** Experimental research project  
**Last updated:** 2026-09-03

This roadmap describes the intended research and implementation direction for Synanton. It is a planning document, not a release commitment.

The authoritative technical specifications remain the architecture documents in the [platform repository](https://github.com/synanton/platform/tree/main/docs/architecture). Repository-specific implementation status remains authoritative in each repository.

---

## Roadmap Principles

Synanton development follows several rules:

1. **Research questions come before features.**
2. **Architectural hypotheses are documented before major implementation.**
3. **Implementation status is distinguished from architectural intent.**
4. **Experiments should measure important assumptions.**
5. **Negative or inconclusive results are valid research outcomes.**
6. **Security and provenance are cross-cutting properties, not optional features.**
7. **Derived state must remain rebuildable from authoritative inputs.**
8. **Public contracts should remain independent from replaceable infrastructure.**
9. **Small, composable components are preferred over a single platform monolith.**
10. **Roadmap items may change when experimental evidence invalidates an assumption.**

---

# Research Program

## R1 - Establish the Knowledge Model

**Objective:** make enterprise knowledge an explicit, addressable and provenance-aware derived state.

### Work

- Complete the annotation model.
- Define annotation definitions and versions.
- Establish dependency relationships between derived objects.
- Define processing runs and provenance.
- Define canonical knowledge versus derived projections.
- Establish stable semantic-chunk identity.
- Define lifecycle and rebuild semantics.

### Evidence

- Knowledge objects can be traced to source content and processing runs.
- Derived projections can be rebuilt without changing canonical knowledge.
- Dependencies are sufficient to determine affected objects after an input change.

**Status:** 🔵 Designed / 🟡 implementation phased

---

# R2 - Semantic Content and Chunking

**Research question:** Does structure-aware semantic chunking provide a better foundation for retrieval and downstream knowledge processing than generic fixed-size chunking?

### Work

- Structured content extraction.
- Semantic chunk construction.
- Stable chunk identity.
- Chunk-level provenance.
- Chunk-level classification.
- Compare semantic chunking with fixed-size baselines.
- Evaluate effects on lexical retrieval, vector retrieval and downstream synthesis.

### Metrics

- Recall@K
- Precision@K
- MRR / NDCG
- context utilization
- answer relevance / faithfulness
- number and size of chunks
- processing cost

### Evidence required

A reproducible dataset, baseline configuration, candidate configuration and quantitative comparison.

**Status:** 🟡 Prototype / active validation

---

# R3 - Secure Semantic Representations

**Research question:** Can semantic representations remain useful for retrieval while preventing sensitive information from leaking through derived representations?

### Work

- Classification-aware representations.
- Single / Dual / Masked-only representation outcomes.
- Deterministic classification before downstream publication.
- Representation selection before ranking and candidate generation.
- Negative security tests for derived artifacts.
- Investigate leakage from embeddings and other derived representations.
- Evaluate tenant reuse where the public representation is semantically equivalent.

### Security evaluation

Test leakage through:

- direct search results;
- search terms and statistics;
- vector candidates;
- caches;
- aggregates;
- reports;
- MCP responses;
- derived representations;
- timing-sensitive behaviour.

### Evidence required

Measure both:

1. utility loss caused by sanitization; and
2. residual sensitive-information leakage.

**Status:** 🔵 Designed / based on Design 1.23 security baseline

---

# R4 - Derived Knowledge and Incremental Recalculation

**Research question:** Can knowledge be recalculated incrementally when its source, rule, model, dictionary or policy changes?

### Work

- Dependency graph construction.
- Affected-object analysis.
- Recalculation planning through Resolutor.
- Controlled execution through Equalix.
- Provenance of recalculation runs.
- Failure and retry semantics.
- Rebuild from authoritative inputs.
- Avoid unnecessary recomputation.

### Metrics

- affected-object ratio;
- recalculation latency;
- compute amplification;
- stale-state duration;
- retry volume;
- plan size;
- execution throughput.

### Baseline

Full recomputation of the affected knowledge population.

**Status:** 🔵 Designed in Design 1.25

---

# R5 - Analytics as Derived State

**Research question:** Can analytics observe knowledge and platform activity without becoming an alternative source of truth?

### Work

- Protected analytics boundary.
- Analytics events.
- Analytical facts.
- Aggregates.
- Metrics.
- Reports.
- Analytical lineage.
- Versioned Analytics Registry.
- Tenant isolation.
- Rebuildability from the analytical event boundary.

### Initial evaluation

The current Design 1.25 architecture identifies ClickHouse as an initial implementation candidate behind a storage-independent contract.

Evaluate:

- ingestion throughput;
- aggregation throughput;
- query latency;
- concurrent query behaviour;
- rebuild time;
- tenant isolation;
- impact on interactive workloads.

The design establishes an initial interactive analytics target of **p95 < 500 ms** and requires analytical workloads to be isolated from interactive processing.

**Status:** 🔵 Designed in Design 1.25

---

# R6 - GraphRAG and Knowledge Reasoning

**Research question:** When does explicit graph structure improve retrieval and reasoning compared with lexical and vector retrieval alone?

### Work

- Knowledge graph projection.
- Relationship provenance.
- Edge relevance signals.
- Graph-aware retrieval.
- Hybrid lexical/vector/graph retrieval.
- GraphRAG query planning.
- Compare graph-enhanced retrieval with non-graph baselines.

### Metrics

- Recall@K
- Precision@K
- MRR / NDCG
- answer quality
- reasoning depth
- latency
- graph traversal cost

**Status:** 🟡 Prototype / continuing evaluation

---

# R7 - Fair Multi-Tenant Scheduling

**Research question:** Can shared AI and distributed workloads receive predictable resource allocation without starvation?

### Work

- Virtual-time scheduling.
- Weighted priorities.
- Hard tenant quotas.
- Adaptive RPS.
- Fixed-memory in-flight counting.
- Anti-starvation behaviour.
- Interactive / batch / background workload classes.
- GPU workload isolation.

### Metrics

- weighted fairness;
- starvation rate;
- queue latency;
- tail latency;
- resource utilization;
- quota violations;
- scheduling overhead.

### Components

- [Equalix](https://github.com/synanton/equalix)
- GPU execution architecture in [Synanton Platform](https://github.com/synanton/platform)

**Status:** 🟡 Prototype / active development

---

# R8 - Conflict-Aware Execution Planning

**Research question:** Can distributed execution conflicts be analyzed and compiled before runtime rather than discovered through distributed locking?

### Work

- Conflict graph construction.
- Immutable execution-plan IR.
- Graph compilation.
- Graph coloring for parallel execution.
- Cost-based optimization.
- Resumable execution.
- Backpressure.
- Locality-aware planning.

### Metrics

- planning latency;
- achievable parallelism;
- critical-path duration;
- conflict rate;
- lock contention avoided;
- execution throughput.

### Component

[Resolutor](https://github.com/synanton/resolutor)

**Status:** 🟡 Prototype / active development

---

# R9 - Durable Execution Intent

**Research question:** Can business intent be persisted independently from worker execution so that failures do not lose intended work?

### Work

- Transaction-scoped execution intents.
- At-least-once semantics.
- Fencing.
- Recovery.
- Replay.
- Worker failure handling.
- Clear separation between business intent and infrastructure execution.

### Metrics

- recovery latency;
- lost-intent rate;
- duplicate execution rate;
- fencing correctness;
- recovery throughput.

### Component

[Commitix](https://github.com/synanton/commitix)

**Status:** 🟡 Prototype / active development

---

# R10 - Agent and MCP Security

**Research question:** What security and provenance guarantees are required when enterprise knowledge and actions are exposed to AI agents?

### Work

- MCP access boundaries.
- Security-aware representation selection.
- Tenant isolation.
- Auditable agent requests.
- Provenance propagation.
- Action authorization.
- Negative security tests.
- Failure and degraded-mode behaviour.

### Evidence required

Agent-facing operations must preserve the same security guarantees as direct platform access.

**Status:** 🔵 Architecture defined / evaluation planned

---

# Cross-Cutting Work

## C1 - Provenance

Establish a consistent provenance model across:

```text
Source
  ↓
Extraction
  ↓
Semantic Content
  ↓
Chunk
  ↓
Annotation
  ↓
Derived Knowledge
  ↓
Search / Graph / Vector
  ↓
Analytics
  ↓
Report / Agent Response
```

Every derived artifact should have a traceable explanation of where it came from and which processing run produced it.

---

## C2 - Security

Security evaluation should cover the entire lifecycle rather than only the API boundary:

```text
Classification
      ↓
Representation
      ↓
Storage
      ↓
Index
      ↓
Query
      ↓
Cache
      ↓
Aggregation
      ↓
Reporting
      ↓
MCP / Agents
```

The security baseline remains the representation and classification model established by Design 1.23 and carried forward by Design 1.25.

---

## C3 - Observability

Every major pipeline should expose enough telemetry to answer:

- what happened?
- why did it happen?
- which inputs caused it?
- which model/rule/version produced it?
- how long did it take?
- what was recalculated?
- what resources were consumed?
- which tenant/workload was affected?
- what happened after failure?

---

## C4 - Reproducible Evaluation

Experiments should record:

- dataset;
- corpus version;
- configuration;
- model version;
- baseline;
- workload;
- environment;
- metrics;
- result;
- interpretation.

Results should be reproducible independently of the author's development environment where practical.

---

# Delivery Phases

The roadmap uses capability phases rather than fixed release dates.

## Phase A - Foundation

**Goal:** establish the experimental foundation.

- [x] Core platform architecture
- [x] Explicit architecture/design documentation
- [x] Contract-oriented module boundaries
- [x] Source ingestion foundation
- [x] Hybrid search foundation
- [x] Classification-aware security architecture
- [x] Business execution primitives
- [x] GPU execution isolation architecture

**Outcome:** coherent architecture and independently testable components.

---

## Phase B - Semantic Knowledge

**Goal:** establish structured semantic knowledge as a first-class platform concept.

- [ ] Structured content extraction
- [ ] Semantic chunking
- [ ] Chunk provenance
- [ ] Annotation model
- [ ] Derived knowledge model
- [ ] Knowledge dependency graph
- [ ] Initial recalculation engine integration
- [ ] Semantic chunking benchmark

**Primary evidence:** measurable retrieval and processing improvements over a defined baseline.

---

## Phase C - Secure Derived Knowledge

**Goal:** validate that derived semantic state can preserve security guarantees.

- [ ] Complete representation-selection pipeline
- [ ] Public/private representation evaluation
- [ ] Embedding leakage experiments
- [ ] Cross-tenant reuse experiments
- [ ] Negative security test suite
- [ ] Security lineage through analytics and reporting

**Primary evidence:** quantified utility/leakage trade-off.

---

## Phase D - Recalculation and Analytics

**Goal:** make derived knowledge continuously maintainable and measurable.

- [ ] Dependency-aware recalculation
- [ ] Resolutor integration
- [ ] Equalix-controlled recalculation
- [ ] Analytics event boundary
- [ ] Analytical facts and aggregates
- [ ] Metrics and reports
- [ ] Analytics Registry
- [ ] Rebuild experiments
- [ ] Interactive/analytical isolation benchmark

**Primary evidence:** incremental recomputation and rebuildability measurements.

---

## Phase E - Knowledge Reasoning

**Goal:** evaluate graph-enhanced enterprise reasoning.

- [ ] Graph projection
- [ ] Graph relevance model
- [ ] GraphRAG query path
- [ ] Hybrid lexical/vector/graph experiments
- [ ] Reasoning benchmark
- [ ] Provenance-aware answers

**Primary evidence:** comparison against lexical/vector baselines.

---

## Phase F - Reliable Action

**Goal:** connect knowledge-driven decisions to reliable distributed execution.

- [ ] Conflict-aware planning
- [ ] Fair resource scheduling
- [ ] Durable execution intent
- [ ] Recovery experiments
- [ ] Multi-tenant execution benchmarks
- [ ] End-to-end knowledge → plan → schedule → commit flow

**Primary evidence:** measured reliability, fairness and recovery behaviour.

---

## Phase G - Agentic Enterprise Infrastructure

**Goal:** expose knowledge and controlled actions to AI agents without weakening platform guarantees.

- [ ] MCP security model
- [ ] Agent identity and tenant isolation
- [ ] Auditable agent queries
- [ ] Secure agent-facing representations
- [ ] Authorized action execution
- [ ] Agent failure/degraded-mode tests
- [ ] End-to-end agent evaluation

**Primary evidence:** security and provenance invariants remain intact through the agent boundary.

---

# What Success Looks Like

Synanton should eventually demonstrate-not merely claim-that:

1. heterogeneous enterprise content can become structured semantic knowledge;
2. derived knowledge remains traceable to its sources and processing history;
3. changes can trigger targeted recalculation instead of uncontrolled full recomputation;
4. search, graph, vector and analytics projections remain derived and rebuildable;
5. security survives semantic transformation and downstream projection;
6. multi-tenant workloads can share resources without uncontrolled starvation;
7. execution conflicts can be planned explicitly;
8. business intent can survive infrastructure failure;
9. agents can operate through the same security and provenance boundaries as other clients.

The objective is not to maximize the number of components.

The objective is to determine which architectural assumptions survive implementation and measurement.

---

# How the Roadmap Is Maintained

- **Architecture documents** define normative system behaviour.
- **Repository READMEs** define component purpose and current implementation status.
- **GitHub Issues / Projects** track concrete implementation tasks.
- **Experiments** record evaluation methodology and results.
- **Release notes** record completed implementation work.
- **This roadmap** records the higher-level research direction.

Completed implementation work should move out of the active roadmap and into repository documentation, release notes and experimental results.

The roadmap is reviewed when a major architecture version changes or when experimental evidence materially changes the research direction.
