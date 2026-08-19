# Synanton

**Enterprise knowledge and business execution infrastructure.**

Synanton explores how enterprise knowledge and business operations can be made **explicit, composable, and reliable**.

> **Knowledge expands the boundaries of what is possible.**

```text
                         SYNANTON
                            │
             ┌──────────────┴──────────────────────┐
             │                                     │
       Knowledge Platform              Business Logic Library
             │                                     │
  ┌──────────┼────────────┐      ┌─────────────────┼────────────────┐
  │      ingestion        │      │                                  │
  │        search         │      │  Resolutor   Equalix   Commitix  │
  │    relationships      │      │      │          │          │     │
  │       ontology        │      │     Plan     Schedule   Ensure   │
  └───────────────────────┘      └──────────────────────────────────┘
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

The platform is built around explicit architectural boundaries and replaceable implementations, allowing implementation choices to evolve independently from business functionality.

## Business Logic Library

The **Synanton Business Logic Library** provides reusable building blocks for **dependency-aware planning, resource-aware scheduling and durable execution**.

| Project                                                | Question                                                                                |   Status         |
| ------------------------------------------------------ | --------------------------------------------------------------------------------------- |------------------|
| [**Resolutor**](https://github.com/synanton/resolutor) | **What may happen?** - dependency analysis, conflict resolution, and execution planning | ![Status](https://img.shields.io/badge/Status-Alpha-orange) |
| [**Equalix**](https://github.com/synanton/equalix)     | **What can run, and how?** - fair, resource-aware scheduling                            | ![Status](https://img.shields.io/badge/Status-Alpha-orange) |
| [**Commitix**](https://github.com/synanton/commitix)   | **How do we ensure it happens?** - durable execution intent and recovery                | ![Status](https://img.shields.io/badge/Status-Alpha-orange) |

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

## Status  ![Status](https://img.shields.io/badge/Status-Experimental-purple)

Synanton components are currently in the initial drafting and validation phase based on the design documentation. Individual repositories document their current implementation status, architecture and roadmap.


## Status

![Status](https://shields.io)

Synanton components are currently in the initial drafting and validation phase based on the design documentation. Individual repositories document their current implementation status, architecture and roadmap.


## License

Individual repositories specify their applicable license.

