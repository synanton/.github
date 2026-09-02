# Synanton

**Enterprise knowledge and execution infrastructure.**

Synanton builds open-source infrastructure for turning enterprise information into **structured knowledge, reliable decisions and durable execution**.

> **Knowledge expands the boundaries of what is possible.**

**New here?** Start with the [**Synanton Guides**](https://synanton.github.io/guides/) — plain-language walkthroughs of how search, security and deployment work, for readers who don't want to start with source code. Engineers should go straight to the [platform repository](https://github.com/synanton/platform).

------

## The Synanton architecture

Synanton is organized around two complementary layers:

```text
                         SYNANTON
                            │
             ┌──────────────┴──────────────┐
             │                             │
             ▼                             ▼
      KNOWLEDGE PLATFORM          BUSINESS LOGIC LIBRARY
             │                             │
    ┌────────┼────────┐            ┌───────┼────────┐
    │        │        │            │       │        │
 Ingest    Search   Ontology    Resolve  Schedule  Commit
    │        │        │            │       │        │
    └────────┼────────┘            └───────┼────────┘
             │                             │
             ▼                             ▼
       Enterprise                  Reliable Business
        Knowledge                     Execution
```

The **Knowledge Platform** turns heterogeneous enterprise content into searchable, structured and governed knowledge.

The **Business Logic Library** provides execution primitives for reasoning about what may happen, what can run and how intended operations are made durable.

Together they provide a foundation for systems that must operate on enterprise knowledge **and act on it reliably**.

------

## Knowledge Platform

[**Synanton Platform**](https://github.com/synanton/platform) is an open-source, AI-native enterprise knowledge platform.

It brings together:

- **Content ingestion** from heterogeneous sources
- **Structured content extraction** with provenance
- **Hybrid search** combining lexical and vector retrieval
- **Knowledge graphs and GraphRAG**
- **Ontology management and validation**
- **LLM enrichment and synthesis**
- **Multi-tenant security and access control**
- **MCP and agent interfaces**
- **Auditable query execution**
- **GPU execution infrastructure**

```text
Documents / APIs / Databases / Object Stores
                       │
                       ▼
              ┌────────────────────┐
              │       Ingest       │
              │   Extract / Parse  │
              │   Chunk / Enrich   │
              │   Embed / Persist  │
              └──────────┬─────────┘
                         │
                         ▼
           ┌──────────────────────────┐
           │ Knowledge Infrastructure │
           │                          │
           │  Hybrid Search           │
           │  Knowledge Graph         │
           │  Ontology                │
           │  Provenance              │
           └─────────────┬────────────┘
                         │
                         ▼
                ┌──────────────────┐
                │ Query / Planning │
                │    GraphRAG      │
                │    Reranking     │
                │    LLM Synthesis │
                └────────┬─────────┘
                         │
                         ▼
               REST / gRPC / MCP / Agents
```

The platform is designed around explicit contracts and replaceable implementation boundaries so that storage engines, extraction technology, graph backends, model infrastructure and deployment topology can evolve independently.

See the [**Synanton Platform repository**](https://github.com/synanton/platform) for architecture, implementation status, demos and roadmap — or the [**Synanton Guides**](https://synanton.github.io/guides/) for a business/operator-level explanation of the same system with no code required.

------

## Documentation

Synanton documentation is split by audience:

| Audience | Where |
| --- | --- |
| Product, security, operations, partners — anyone who wants to understand *what* Synanton does and *why*, without reading code | [**Synanton Guides**](https://synanton.github.io/guides/) — Search, Security, Ingestion, Deployment, Troubleshooting |
| Engineers — architecture, module contracts, implementation status | [`docs/architecture/`](https://github.com/synanton/platform/tree/main/docs/architecture), [`docs/implementation/`](https://github.com/synanton/platform/tree/main/docs/implementation), [`docs/api/`](https://github.com/synanton/platform/tree/main/docs/api) in the platform repository |
| Operators, on-call — runbooks, alerting, disaster recovery | [`docs/operations/`](https://github.com/synanton/platform/tree/main/docs/operations) in the platform repository |

The Guides link out to the normative engineering docs for anything that needs code-level precision — they explain, they don't duplicate.

------

## Business Logic Library

Synanton's execution libraries address a different problem:

**How does a system turn business intent into safe, fair and durable execution?**

| Project                                                | Core question                    | What it provides                                             |
| ------------------------------------------------------ | -------------------------------- | ------------------------------------------------------------ |
| [**Resolutor**](https://github.com/synanton/resolutor) | **What may happen?**             | Dependency analysis, conflict resolution and immutable execution planning |
| [**Equalix**](https://github.com/synanton/equalix)     | **What can run and how?**       | Eventually-fair, resource-aware scheduling for high-throughput multi-tenant systems |
| [**Commitix**](https://github.com/synanton/commitix)   | **How do we ensure it happens?** | Durable execution intent, persistence and recovery           |

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
              What can run and how?
                       │
                       ▼
                   COMMITIX
              How do we ensure it?
                       │
                       ▼
                   Execution
```

These components are deliberately independent.

They can be adopted individually, composed together, or used as architectural primitives inside larger distributed systems.

------

## Supporting infrastructure

[**Lucentrix**](https://github.com/synanton/lucentrix) provides source ingestion and connector capabilities.

```text
External Sources
      │
      ├── Files
      ├── Web
      ├── APIs
      ├── Enterprise Systems
      └── Data Stores
             │
             ▼
         Lucentrix
             │
             ▼
      Synanton Platform
             │
             ▼
   Structured Enterprise Knowledge
```

Lucentrix handles the boundary between external sources and the knowledge platform, while the platform owns processing, enrichment, indexing, relationships and knowledge semantics.

------

## A common design philosophy

Synanton projects share a set of architectural principles:

- **Separate functionality from implementation**
- **Make architectural boundaries explicit**
- **Prefer composable components over monoliths**
- **Define guarantees precisely**
- **Design for failure, recovery and change**
- **Keep business semantics independent of infrastructure**
- **Make provenance and execution state observable**
- **Replace implementation without changing the contract**

The goal is not to build one monolithic system.

The goal is to create **small, composable pieces of infrastructure with strong guarantees**.

------

## Projects

### Knowledge

- [**platform**](https://github.com/synanton/platform) — AI-native enterprise knowledge platform
- [**lucentrix**](https://github.com/synanton/lucentrix) — source ingestion and connector infrastructure

### Execution

- [**resolutor**](https://github.com/synanton/resolutor) — dependency-aware execution planning
- [**equalix**](https://github.com/synanton/equalix) — fair resource scheduling
- [**commitix**](https://github.com/synanton/commitix) — durable execution intent

------

## Where we're going

Synanton is exploring the boundary between **knowledge infrastructure and business execution**.

Enterprise systems increasingly need to do more than retrieve information or generate text. They need to:

1. understand heterogeneous information,
2. establish relationships and semantics,
3. reason about possible actions,
4. plan those actions,
5. allocate resources fairly,
6. execute them reliably,
7. recover when systems fail, and
8. preserve the evidence of what happened.

Synanton aims to provide infrastructure for that entire path:

```text
             KNOWLEDGE
                 │
                 ▼
            UNDERSTAND
                 │
                 ▼
              REASON
                 │
                 ▼
               PLAN
                 │
                 ▼
             SCHEDULE
                 │
                 ▼
              COMMIT
                 │
                 ▼
             EXECUTE
                 │
                 ▼
             OBSERVE
                 │
                 └──────────► Knowledge
```

**Knowledge should not end at retrieval.
Reliable systems turn knowledge into action.**

------

## Status

Synanton is actively developed. Individual repositories document their own implementation status, architecture and roadmap.

The organization currently focuses on the convergence of:

- enterprise knowledge infrastructure,
- structured and multimodal content processing,
- AI-native retrieval and reasoning,
- distributed execution semantics,
- resource-aware scheduling, and
- durable business operations.

------

## Contact

- **Research & general inquiries:** research@synanton.org
- **Security reports:** security@synanton.org

------

## License

Each repository specifies its applicable license.

