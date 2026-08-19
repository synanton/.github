# Synanton

**Enterprise knowledge and business execution infrastructure.**

Synanton explores how enterprise knowledge and business operations can be made **explicit, composable, and reliable**.

The platform combines knowledge ingestion, storage, search, relationships, ontology, planning, and execution into a modular architecture.

> **Knowledge expands the boundaries of what is possible.**

## The platform

[**Synanton Platform**](https://github.com/synanton/platform) is an open-source enterprise knowledge platform designed for applications that need to work with complex, distributed organizational knowledge.

```text
Sources
   │
   ▼
Ingestion
   │
   ▼
Knowledge
   │
   ├── Content
   ├── Search
   ├── Relationships
   └── Ontology
   │
   ▼
Planning
   │
   ▼
Execution
   │
   ▼
Applications
```

The platform is intentionally built around replaceable implementations and explicit architectural boundaries.

## Business Logic Library

Synanton also develops reusable components for reliable business execution.

| Project                                                | Question it answers                                                                     |
| ------------------------------------------------------ | --------------------------------------------------------------------------------------- |
| [**Resolutor**](https://github.com/synanton/resolutor) | **What may happen?** — dependency analysis, conflict resolution, and execution planning |
| [**Equalix**](https://github.com/synanton/equalix)     | **What can run, and how?** — fair, resource-aware scheduling                            |
| [**Commitix**](https://github.com/synanton/commitix)   | **How do we ensure it happens?** — durable execution intent and recovery                |

Together:

```text
             Business Intent
                    │
                    ▼
               Resolutor
          What may happen?
                    │
                    ▼
                Equalix
        What can run, and how?
                    │
                    ▼
               Commitix
       How do we ensure it happens?
```

The components are independent and implementation-agnostic. They can be used individually or combined as part of a larger execution architecture.

## Supporting tools

[**Lucentrix**](https://github.com/synanton/lucentrix) provides pluggable ingestion connectors for bringing external content into Synanton.

```text
External Sources
       │
       ▼
   Lucentrix
       │
       ▼
    Synanton
       │
       ▼
Enterprise Knowledge
```

Lucentrix is responsible for connecting sources; Synanton remains responsible for knowledge processing and indexing.

## Design principles

Synanton projects follow a few recurring principles:

* **Separate functionality from implementation**
* **Make architectural boundaries explicit**
* **Prefer composable components over monoliths**
* **Keep guarantees precise**
* **Design for failure, recovery, and change**
* **Make business semantics explicit before optimizing implementation**

## Projects

### Knowledge Platform

* [Synanton Platform](https://github.com/synanton/platform) — enterprise knowledge infrastructure

### Business Logic Library

* [Resolutor](https://github.com/synanton/resolutor) — dependency-aware execution planning
* [Equalix](https://github.com/synanton/equalix) — fair multi-tenant resource scheduling
* [Commitix](https://github.com/synanton/commitix) — durable execution intent

### Tools

* [Lucentrix](https://github.com/synanton/lucentrix) — pluggable content ingestion

## Status

Synanton is an active architectural and engineering project. The repositories are evolving toward production-ready implementations, reference adapters, and executable demonstrations.

See each repository for its current implementation status and roadmap.

## License

Unless otherwise stated, Synanton projects are released under the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0).
