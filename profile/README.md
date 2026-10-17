# Synanton

**Open-source systems research for enterprise knowledge and reliable business execution.**

Synanton explores how enterprise information can become **structured, security-aware, provenance-aware and continuously recalculable knowledge** - and how that knowledge can support **reasoning, planning and reliable execution**.

> **From information to derived knowledge - and from knowledge to reliable action.**

Synanton is both a software platform and an engineering-research project. Architectural hypotheses are made explicit in design documents, implemented as open-source components, and intended to be evaluated through reproducible experiments and benchmarks.

**New here?** Start with the [Synanton Guides](https://synanton.github.io/guides/) for a plain-language introduction. Engineers can start with the [Synanton Platform](https://github.com/synanton/platform).

**Project direction:** see the [Synanton Roadmap](https://github.com/synanton/.github/blob/main/profile/ROADMAP.md).

---

## The Research Problem

At production scale, enterprise AI and retrieval systems fail in four recurring ways:

- **Data & policy drift** — re-indexing entire corpora when a model, prompt, or access policy changes.
- **Security and representation contamination** — embeddings and graph nodes leaking across tenant boundaries through derived representations.
- **Noisy neighbors** — heavy vector-search or inference workloads saturating shared clusters and starving standard operations.
- **Unreliable asynchronous pipelines** — network and node failures during chunking or streaming producing silent data loss and corrupted projections.

Enterprise AI systems have to solve several problems at the same time:

- heterogeneous enterprise information;
- semantic interpretation and enrichment;
- knowledge that changes when sources, models, rules or policies change;
- provenance and explainability;
- security that must survive transformations and derived representations;
- search, graph reasoning and analytics over the same knowledge;
- multi-tenant resource isolation;
- distributed execution, recovery and fairness.

Synanton investigates an architecture in which these concerns remain explicit and composable rather than being hidden inside one application.

The central architectural idea is:

> **Knowledge is derived state, and analytics is derived state over knowledge and platform activity.**

This makes search indexes, vector representations, graphs and analytics projections consequences of an authoritative knowledge model rather than independent sources of truth.

---

## Platform Architecture

The platform is organized as a set of decoupled authority planes. Each owns a
distinct concern, exposes an explicit contract, and can evolve independently.

```text
                          SYNANTON PLATFORM 1.0
┌───────────────────────────────────────────────────────────────────────┐
│                         External Applications                         │
│                              │                                        │
│                         Platform API 1.32                             │
└──────────────────────────────┬────────────────────────────────────────┘
                               │
        ┌──────────────────────┼────────────────────────┐
        ▼                      ▼                        ▼
   Identity 1.29         Search 1.31             AI Runtime 1.30
        │                      │                        │
        └───────────────┬──────┴───────────────┬────────┘
                        ▼                      ▼
                 Knowledge 1.25          Eventing 1.27
                        │                      │
                        ▼                      ▼
                 Content Cache 1.26      Workflow / Equalix
                        ▲                      │
                        │                      │
                  Ingestion 1.28 ◄────────────┘
```

The central architectural rule is: Knowledge is derived state, and search
indexes are temporary projections. Changes to rules, access boundaries, or ML
models recalculate only the affected graph nodes rather than reprocessing the
entire source corpus.

This decoupling is the project's core hypothesis — what enters the platform,
how Synanton understands it, how knowledge is projected, and who is authorized
to see it are separable concerns with explicit contracts between them.

---

## Research Method

Synanton follows an engineering-research loop:

```text
              RESEARCH QUESTION
                     │
                     ▼
                 HYPOTHESIS
                     │
                     ▼
              ARCHITECTURAL DESIGN
                     │
                     ▼
                IMPLEMENTATION
                     │
                     ▼
             EXPERIMENT / BENCHMARK
                     │
                     ▼
                  MEASURE
                     │
                     ▼
             RESULT / OBSERVATION
                     │
                     ▼
             REVISED ARCHITECTURE
                     │
                     └──────────────►
```

Design documents describe the proposed architecture and its invariants.

Implementations test whether the architecture can be realized.

Experiments and benchmarks provide evidence for or against the underlying assumptions.

The [Roadmap](https://github.com/synanton/.github/blob/main/ROADMAP.md) tracks the current research and implementation direction.

---

## Current Research Areas

| Area | Research question |
|---|---|
| **Knowledge representation** | How should heterogeneous enterprise content become durable, addressable knowledge? |
| **Semantic chunking** | What makes a semantic chunk stable enough to support search, security, provenance and downstream knowledge processing? |
| **Derived knowledge** | Can interpreted knowledge remain explicitly derived from source content, definitions, models and processing runs? |
| **Incremental recalculation** | Can changes to rules, models, dictionaries, sources or policies update only the affected derived state? |
| **Provenance** | Can derived knowledge remain traceable to its sources, definitions, producers, runs and dependencies? |
| **Secure semantic search** | Can useful semantic representations be produced without allowing sensitive information to leak through derived representations? |
| **GraphRAG** | When do explicit relationships and graph traversal improve retrieval and reasoning over enterprise knowledge? |
| **Analytics over knowledge** | Can analytics remain a rebuildable projection over canonical knowledge and platform activity rather than becoming another source of truth? |
| **Fair scheduling** | Can multi-tenant workloads receive predictable resource allocation without starvation under changing load? |
| **Durable execution** | Can business intent survive process, node and infrastructure failures without coupling business semantics to infrastructure? |
| **Conflict-aware execution** | Can execution conflicts be analyzed and compiled into an immutable plan instead of being discovered only at runtime? |
| **Agent interfaces** | What guarantees are required when enterprise knowledge and actions are exposed to AI agents? |

---

### Knowledge lifecycle

The current architecture extends the platform beyond retrieval:

```text
Source Content
      │
      ▼
  Extraction
      │
      ▼
Semantic Content
      │
      ▼
Semantic Chunks
      │
      ├──────────────► Security Classification
      │
      ▼
  Annotation
      │
      ├──────────────► Provenance
      ├──────────────► Processing Run
      └──────────────► Dependencies
      │
      ▼
Derived Knowledge
      │
      ├──────────────► Reverse Index
      ├──────────────► Vector Store
      └──────────────► Graph
      │
      ▼
 Search / Applications
      │
      ▼
 Protected Analytics Boundary
      │
      ▼
 Analytics → Facts → Aggregates → Metrics → Reports
```

The current architecture is integrated by
[Synanton Platform Architecture 1.0](https://github.com/synanton/platform/blob/main/docs/architecture/synanton-platform-architecture-1.0.md),
a capstone consolidating Designs 1.22–1.34. Design 1.25 remains normative for annotation, derived knowledge, recalculation and reporting within that set.

The design deliberately separates:

> **What the source contains → what Synanton understands → how knowledge is projected → how knowledge is authorized → how knowledge is measured.**

---

## Knowledge Platform

[**Synanton Platform**](https://github.com/synanton/platform) is an open-source enterprise knowledge platform for applications that need to work with complex, distributed organizational knowledge.

It brings together:

- heterogeneous content ingestion;
- structured content extraction and provenance;
- semantic content and semantic chunking;
- hybrid lexical and vector retrieval;
- knowledge graphs and GraphRAG;
- ontology management and validation;
- LLM enrichment and synthesis;
- classification-aware security and representation;
- multi-tenant isolation;
- auditable query execution;
- MCP and agent interfaces;
- analytics and reporting over derived knowledge;
- isolated GPU execution infrastructure.

The platform is built around explicit contracts and replaceable implementation boundaries so that storage engines, extraction technology, graph backends, model infrastructure and deployment topology can evolve independently.

See the [platform repository](https://github.com/synanton/platform) for implementation and engineering documentation, or the [Synanton Guides](https://synanton.github.io/) for non-code-oriented documentation.

---

## Technology Stack

| Layer | Technologies |
|---|---|
| Languages | Go, Java (Spring Boot, Micronaut), Python |
| Search & vector fabrics | Lucene, OpenSearch, Milvus, Qdrant, ClickHouse, YDB |
| Distributed substrates | Kafka Streams, durable workflow engines, Kubernetes Operators |
| Infrastructure envelope | Physically isolated GPU runtime infrastructure, Terraform, Helm |

---

## Business Logic Library

The Synanton Business Logic Library investigates reusable primitives for turning business intent into controlled distributed execution.

| Project | Research question | Core capability |
|---|---|---|
| [**Resolutor**](https://github.com/synanton/resolutor) | **What may happen?** | Dependency analysis, conflict resolution and immutable execution planning |
| [**Equalix**](https://github.com/synanton/equalix) | **What can run, and how?** | Eventually-fair, resource-aware scheduling |
| [**Commitix**](https://github.com/synanton/commitix) | **How do we ensure it happens?** | Durable execution intent, persistence and recovery |

Conceptually:

```text
Business Intent
      │
      ▼
  RESOLUTOR
What may happen?
      │
      ▼
   EQUALIX
What can run, and how?
      │
      ▼
  COMMITIX
How do we ensure it?
      │
      ▼
  Execution
```

The components are independent and can be adopted individually or composed inside larger distributed systems.

---

## Supporting Infrastructure

[**Lucentrix**](https://github.com/synanton/lucentrix) provides pluggable source-ingestion and connector capabilities.

```text
External Sources
      │
      ▼
  Lucentrix
      │
      ▼
 Synanton Platform
      │
      ▼
Enterprise Knowledge
```

Lucentrix handles the boundary between external sources and the knowledge platform; Synanton owns processing, enrichment, indexing, relationships and knowledge semantics.

---

## Architecture Evolution

Synanton treats architecture as an explicit, versioned engineering artifact.

The design series records how the research direction evolves as new problems are identified and earlier assumptions are refined.

The current architecture is integrated by [Synanton Platform Architecture 1.0](https://github.com/synanton/platform/blob/main/docs/architecture/synanton-platform-architecture-1.0.md), a capstone consolidating Designs 1.22–1.34. Design 1.25 remains normative for annotation, derived knowledge, dependency-aware recalculation, analytics and reporting within that set, while retaining the security and representation model established in Design 1.23.

See the [architecture documentation](https://github.com/synanton/platform/tree/main/docs/architecture) for the complete design history.

---

## Experiments and Evidence

A research-oriented project needs evidence, not only architecture.

Synanton's planned evaluation model is:

```text
Research Question
      │
      ▼
Baseline
      │
      ▼
Candidate Architecture
      │
      ▼
Controlled Workload / Dataset
      │
      ▼
Metrics
      │
      ▼
Comparison
      │
      ▼
Conclusion
```

Examples of intended evaluation areas include:

- retrieval quality for semantic versus fixed-size chunking;
- hybrid retrieval effectiveness;
- sensitive-information leakage from derived semantic representations;
- incremental recalculation versus full recomputation;
- provenance completeness;
- graph-enhanced retrieval;
- interactive versus analytical workload isolation;
- multi-tenant scheduling fairness;
- durable execution recovery;
- analytical rebuildability and query performance.

### Completed evaluations

The first completed instance of this model is the **Equalix family
comparison** — a three-implementation study of the same weighted-fair
scheduling semantics across a Spring Boot reference (`equalix`), a Go
reimplementation (`equalix-go`), and a Micronaut port (`equalix-micronaut`).

The finding is bounded: AOT compilation (Micronaut) improves cold start
and image size over the Spring Boot oracle but does not extend to runtime
profile — GC pauses, p99 latency, sustained ceiling and time-to-ceiling
all overlap within measurement noise. The Go implementation has a distinct
runtime profile (memory, startup) consistent with a native binary. Every
dimension that could have extended the AOT advantage beyond startup was
tested and returned null.

- [**Equalix family comparison →**](experiments/equalix-family-comparison.md)

Experimental results should be published separately from architectural claims so that readers can distinguish **what is designed**, **what is implemented**, and **what has been measured**.

### Measured: retrieval benchmark (2026-10-11, platform SNTP-9 track)

First completed pass through the evaluation model above — design (benchmark
runner + manifests) → implementation (5 vector backends + hybrid RRF) →
controlled workloads → metrics → comparison:

- **Demo corpus** (16 docs, 28 human-confirmed queries): PG/YDB 0.9821 >
  Lucene-hybrid 0.92 > Milvus=Qdrant 0.85–0.88 > Lucene-standalone 0.80–0.83.
- **RAG-Multi-Corpus** (236 synthetic enterprise docs, 1216 upstream gold
  queries): Lucene-hybrid 0.9778 > Milvus=Qdrant 0.9572 > Lucene-standalone
  0.9490 > PG=YDB 0.7944.
- Engine ranking is **corpus-dependent** (PG/YDB lead one corpus, trail the
  other); Milvus=Qdrant byte-identical everywhere; service integration adds
  zero distortion; blind rank-fusion buries single-leg hits (measured trade
  curve, tunable boost default-off).

Full evidence: [platform selection framework](https://github.com/synanton/platform/blob/main/docs/architecture/vector-engine-selection.md)
(ADR-013 Accepted), [benchmark tracker](https://github.com/synanton/platform/blob/main/docs/implementation/benchmark-tracker.md),
[dataset registry](https://github.com/synanton/platform/blob/main/docs/benchmark/datasets.md).

---

## Research Status

Synanton intentionally distinguishes architecture from implementation.

| Status | Meaning |
|---|---|
| 🟢 **Validated** | Implemented and supported by reproducible experimental evidence |
| 🟡 **Prototype** | Implemented or partially implemented and under evaluation |
| 🔵 **Designed** | Architecture specified; implementation is phased or pending |
| ⚪ **Exploratory** | Research question or hypothesis under investigation |

Individual repositories and design documents remain authoritative for their own implementation status.

---

## Design Principles

Synanton projects share several architectural principles:

- **Separate functionality from implementation**
- **Make architectural boundaries explicit**
- **Prefer composable components over monoliths**
- **Define guarantees precisely**
- **Design for failure, recovery and change**
- **Keep business semantics independent of infrastructure**
- **Treat derived state as derived state**
- **Preserve provenance across transformations**
- **Make security a property of the data lifecycle**
- **Measure architectural assumptions rather than relying on intuition**

The goal is not to build one monolithic system.

The goal is to investigate and implement small, composable pieces of infrastructure with strong, explicit guarantees.

---

## Documentation

| Audience | Where |
|---|---|
| Product, security, operations and general technical readers | [Synanton Guides](https://synanton.github.io/guides/) |
| Engineers | [`docs/architecture/`](https://github.com/synanton/platform/tree/main/docs/architecture), [`docs/implementation/`](https://github.com/synanton/platform/tree/main/docs/implementation), [`docs/api/`](https://github.com/synanton/platform/tree/main/docs/api) |
| Operators and SREs | [`docs/operations/`](https://github.com/synanton/platform/tree/main/docs/operations) |
| Research direction and project priorities | [**Synanton Roadmap**](https://github.com/synanton/.github/blob/main/profile/ROADMAP.md) |
| Equalix (Spring Boot) developers | [Equalix documentation](https://synanton.github.io/equalix/) |
| Equalix Micronaut developers | [Equalix Micronaut documentation](https://synanton.github.io/equalix-micronaut/) |

The public documentation explains the system; the engineering documentation specifies it; the research roadmap explains where the project is going.

---

## Projects

### Knowledge

- [**platform**](https://github.com/synanton/platform) - AI-native enterprise knowledge platform
- [**lucentrix**](https://github.com/synanton/lucentrix) - source ingestion and connector infrastructure

### Execution

- [**resolutor**](https://github.com/synanton/resolutor) - dependency-aware execution planning
- [**equalix**](https://github.com/synanton/equalix) - fair resource scheduling
- [**commitix**](https://github.com/synanton/commitix) - durable execution intent

---

## Status

Synanton is an actively developed experimental research project.

Implementation status is tracked per plane in the platform repository; the Platform API (1.32) contract set is frozen and its first vertical slice is in progress, while Eventing/Workflow (1.27) is the next contract freeze.

The current focus is the convergence of:

- enterprise knowledge representation;
- structured and semantic content processing;
- classification-aware security;
- hybrid retrieval and graph reasoning;
- derived knowledge and incremental recalculation;
- analytics over knowledge;
- distributed execution semantics;
- resource-aware scheduling;
- durable business operations.

The [Roadmap](https://github.com/synanton/.github/blob/main/ROADMAP.md) is the canonical high-level view of research priorities and implementation direction.

---

## Contact

- **Research & general inquiries:** research@synanton.org
- **Security reports:** security@synanton.org

## License

Each repository specifies its applicable license.
