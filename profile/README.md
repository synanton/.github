# Synanton

**Enterprise knowledge and business execution infrastructure.**

Synanton explores how enterprise knowledge and business operations can be made **explicit, composable, and reliable**.

> **Knowledge expands the boundaries of what is possible.**

```text
                         SYNANTON
                            │
             ┌──────────────┴────────────────┐
             │                               │
       Knowledge Platform         Business Logic Library
             │                               │
  ┌──────────┼────────────┐      ┌───────────┼────────────────┐
  │      ingestion        │      │                            │
  │        search         │      │ Resolutor Equalix Commitix │
  │    relationships      │      │    │        │       │      │
  │       ontology        │      │  Plan  Schedule   Ensure   │
  └───────────────────────┘      └────────────────────────────┘
             ▲
             │
      ┌───────────────┐
      │   Lucentrix   │
      │    source     │
      │   connectors  │
      └───────────────┘
```

## Enterprise Knowledge Platform

[**Synanton Platform**](https://github.com/synanton/platform) is an open-source enterprise knowledge platform for applications that need to work with complex, distributed organizational knowledge.

The platform is built around explicit architectural boundaries and replaceable implementations, allowing infrastructure choices to evolve independently from business functionality.

## Business Logic Library

The **Synanton Business Logic Library** provides reusable building blocks for **dependency-aware planning, resource-aware scheduling, and durable execution**.

| Project                                                | Question                                                                                |
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

## Design Principles

Synanton projects follow a few recurring principles:

* **Separate functionality from implementation**
* **Make architectural boundaries explicit**
* **Prefer composable components over monoliths**
* **Keep guarantees precise**
* **Design for failure, recovery, and change**
* **Define business semantics before optimizing implementation**

## Supporting Tools

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

## Explore

**Knowledge Platform**
[Synanton Platform](https://github.com/synanton/platform) — enterprise knowledge infrastructure

**Business Logic**
[Resolutor](https://github.com/synanton/resolutor) · [Equalix](https://github.com/synanton/equalix) · [Commitix](https://github.com/synanton/commitix)

**Ingestion**
[Lucentrix](https://github.com/synanton/lucentrix) — pluggable content connectors

## Status

Synanton is under active development. Individual repositories document their current implementation status, architecture, and roadmap.

## License

Individual repositories specify their applicable license.

