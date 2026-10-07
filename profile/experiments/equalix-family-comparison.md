# Equalix family comparison

Three implementations of one scheduler — [equalix](https://github.com/synanton/equalix)
(Java Spring Boot, the oracle), [equalix-go](https://github.com/synanton/equalix-go)
(Go reimplementation), [equalix-micronaut](https://github.com/synanton/equalix-micronaut)
(Micronaut port) — compared on startup, footprint, and runtime behavior with the
same workloads and the same executor protocol.

> **State as of:** `equalix-go` f8a2a21e · `equalix` 11ef025e · `equalix-micronaut` b12176c0
> **Measured:** 2026-10-07
> **Evidence:** `equalix-go/docs/evidence/` (char-01 … char-04, `threeway/`)

## Finding

Four characterization runs and three differential pairs converge on one story:
**AOT's advantage is bounded to cold start and image size — it does not extend
to runtime profile.** Every dimension tested for extension (RSS beyond load,
GC, latency, ceiling, time-to-ceiling) came back null, including time-to-ceiling,
which was expected to hold. Micronaut wins where the JVM hasn't started yet;
once running, the two JVMs are indistinguishable within noise.

## Provenance (read before the numbers)

| Pair | What it proves |
|---|---|
| Go-vs-Spring | Independent implementation reproduces oracle semantics (EQLX-5 claim, re-run) |
| Spring-vs-Micronaut | Direct port preserves oracle semantics across integration seams |
| Go-vs-Micronaut | Cross-runtime agreement (not independent convergence) |

"Three implementations agree" means cross-validation, never three independent
convergences — Micronaut is Spring's code in a different runtime. Full framing:
`equalix-go/docs/differential-methodology.md`; evidence:
`equalix-go/docs/evidence/threeway/` (all three pairs green at one provenance).

## Startup profile

| Phase | Spring Boot | Micronaut | Go |
|---|---|---|---|
| Spawn → `main()` | 0.23 s | 0.11 s | — (no JVM; spawn→ready 0.45 s total) |
| `main()` → context ready | 4.72 s | 2.29 s | — |
| Ready → first 200 | 0.23 s (REST; `/actuator/health` stays 503 w/o broker) | 0.81 s (`/health` 200) | — |
| Ready → first dispatch | 0.40 s | 0.37 s | — |
| **Spawn → serving** | **5.18 s** | **3.21 s** | **0.45 s** |

Micronaut context comes up ~2× faster; Go is an order of magnitude below both
(EQLX-5: 0.50 s over 11 runs; 0.45 s re-measured here). Dispatch latency is
tick-dominated and identical.

## Container footprint

| | Spring Boot | Micronaut | Go |
|---|---|---|---|
| Image size | 416 MB | 386 MB (−7%) | 16.1 MB (≈24× smaller; spot, same host) |
| Cold RSS (at readiness) | 565–765 MiB | 450–806 MiB | 9.5–12.5 MiB (spot, same host) |
| Warm RSS (+60 s idle) | 639–644 MiB | 717–806 MiB | not measured |
| RSS under load (w2000) | 795–837 MiB | 665–696 MiB | ≈33 MiB under burst-backlog pressure (same host, w2000 burst) |

Image and cold-start deltas are framework; the cold-RSS gap (≈50×) and load gap
(≈22×) are the JVM itself. Within the JVM, only load RSS separates (−17%
Micronaut, both passes); ready/idle are noise at n=2.

On the prior 4–6× figure: retired, explicitly. It was never measured against a
stated definition — this matrix is the first real measurement, and it shows
≈50× cold-idle and ≈22× under load (same host, same workload family). If a 4–6×
number exists anywhere in family lore, it reflected different (capped-heap?
warmer?) conditions and is superseded for these definitions. Go warm-idle RSS
was not measured; all Go cells above are spot measurements on the same host
(image build, spawn→ready poll, `ps`/`docker stats` snapshots), not char-01/02
protocol runs — labeled as such so method difference is never mistaken for
runtime difference.

## Runtime characteristics

| | Spring Boot | Micronaut | Go |
|---|---|---|---|
| GC pauses (p99) | 13.6–14.1 ms | 11.1–12.6 ms | n/a (native; compare p99 latency, not pauses) |
| End-to-end p99 latency | 310–325 ms | 290–306 ms | — |
| Sustained ceiling | ≈75–80/s | ≈70–80/s | — |
| Time-to-ceiling (t95) | 30–34 s | 32 s | — |

Overlapping everywhere at n=2. The ceiling is DB-bound on both JVMs; the RPS
controller converges bit-identically given paced load. CPU-per-dispatch and
recovery time unmeasured — candidates, not claims.

## Write-path divergence (livelock under sustained deep backlog)

Not a comparison cell — a behavioral difference the RSS reconciliation surfaced:
under sustained deep backlog, Go's priority calculator re-saves QUEUED rows every
tick with an unconditional `version = version + 1`, churning versions (avg 161,
max 1935 observed) into optimistic-locking livelock with the dispatcher; Java
re-saves the same rows but Hibernate dirty-checking no-ops the write (QUEUED
versions avg 1, max 1 under the identical regime). Dispatch decisions are
unaffected (same priorities) — this is write-path, not scheduler logic — but it
is production-relevant (cold-start burst is deep backlog) and invisible to the
differential, whose warmup regime sidesteps it. Full note in spec §13; Go-side
fix (save-if-dirty) is deferred, not missing.

## Caveats (read before the "green" lines)

- **Drain tolerance.** One Go-vs-MN attempt failed its 180 s drain on a single
  stuck send and was rerun green per the flake protocol. The drain as shaped
  de-facto gates stuck sends even though they're excluded from gates.
- **Warmup asymmetry.** Warmup task counts differ per side (go 105–106, java
  88–95, mn 50–57) but prephase dispatched agrees within 1% (928/928/920) —
  cosmetic (ramp-rate artifact), verified in the evidence doc.
- **Explicit outs** (named, not missing): per-side config/payload sensitivity,
  stability runs, Spring cold-startup cell, GraalVM native column,
  multi-instance Micronaut (no ShedLock), RPS-ceiling Go cell.

## Stopping point

With this matrix, the multi-repo project is at a legitimate stopping point:
Spring is the stable oracle; Go is released with all claims cited through
EQLX-9; Micronaut is ported, characterized on four dimensions, and
differentially run at one provenance. Everything remaining is deferred and
named above — not "should have done this" but "chose not to, here's why."
Nothing is pending unless a specific question needs answering.

## Update rule

The matrix updates when any of the following happens:

- A semantic change lands in one of the three implementations
- A new characterization run produces numbers that supersede the current ones
- A new implementation joins the family (a fourth column would arrive the same way)

Updates are docs-only PRs against `.github/profile/experiments/`. The three-SHA
freshness marker bumps with the change; individual implementation READMEs do
not change unless a new implementation is added to the Family section.
