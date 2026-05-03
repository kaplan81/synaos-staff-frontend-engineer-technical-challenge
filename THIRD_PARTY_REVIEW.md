# Synaos Staff Frontend Challenge — Architectural review & design add-ons

## 1. Executive summary

[PLAN.md](PLAN.md) is strong: pragmatic, no unnecessary rewrites, and it correctly focuses the telemetry hot path on a worker, façade, and dedicated renderer. The harder problem at this scale is running a **multi-team realtime frontend** that scales reliably, can be **observed end-to-end**, **recovers predictably**, and evolves without surprise breakage.

This note tightens the story around **dependency awareness**, **observability**, **RTO/RPO-style recovery**, **snapshot + delta + sequence numbers**, **realtime-oriented testing**, and **explicit performance targets**.

## 2. Key architectural principles

### 2.1 Dependency awareness

Dependencies must be explicit: Module Federation remotes, shared packages, backend contracts, runtime config. Practices: dependency map, ownership per capability, semver, contract tests, compatibility matrix. **Rule:** micro-frontends must not depend on each other’s internal state.

### 2.2 Contract-first realtime

Telemetry: versioned, backward-compatible, validated at runtime. **Model:** authoritative **snapshots**, **deltas**, **sequence numbers** (ordering + recovery).

### 2.3 Realtime isolation

Separate high-frequency telemetry (e.g. positions) from low-frequency app state. **Worker** parses/merges telemetry; **façade** exposes a minimal stable API to Angular; avoid high-frequency traffic through global NGXS.

### 2.4 Dedicated rendering pipeline

Decouple rendering from Angular’s lifecycle; optimise for many moving entities. Batching/instancing, LOD, interpolation, GPU-friendly updates.

### 2.5 Observability (first-class)

End-to-end visibility: backend → gateway → browser → worker → renderer. **Minimum signals:** E2E latency to render, FPS / frame time, memory trend, worker queue depth, dropped updates, stale entity count, reconnect events, active contract/version and feature flags.

### 2.6 Resilience

Design for graceful degradation under unstable network, stale data, partial failures, and rendering saturation.

## 3. Recovery strategy (RTO / RPO)

Define explicit expectations (tune in Phase 0): e.g. dashboard usable after failure **< ~2 min**; websocket reconnect + resync **< ~10 s**; user-visible staleness threshold **~3–5 s**. **On reconnect:** fetch authoritative snapshot, resume deltas from a known sequence, mark entities stale until confirmed fresh. **Degraded mode:** last-known positions, timestamps + stale indicators, system health banner. Non-critical failures must not take down the main operational view.

## 4. Performance targets

Architecture must be measurable: **~300 robots** without an FPS cliff; **p95** backend→render **~250 ms** (validate); stable memory over **8-hour** sessions; reconnect/resync in seconds; explicit stale handling; **second control-room view** must not degrade the primary posture.

## 5. Testing strategy (realtime)

Unit tests are not enough. Add: synthetic websocket load (**300+** robots); contract tests on telemetry and snapshots; **8-hour** soaks (memory, FPS, stale counts); failure-mode tests (disconnects, reconnect storms, out-of-order and duplicate events); visual regression; canary/shadow deployments before full rollout.

## 6. Core risks → mitigations

- **Render path does not scale** → profiling, dedicated renderer package, synthetic load tests.
- **Main-thread overload / UI lag** → worker + façade, keep hot path out of NGXS.
- **Weak backend contracts** → versioning, sequence numbers, snapshot + delta discipline.
- **Weak observability / high MTTR** → pipeline metrics and traceability.
- **Hidden coupling** → dependency map, ownership, governance.
- **No recovery story** → RTO/RPO targets + snapshot-based resync.

## 7. Delivery roadmap (aligned with PLAN.md cadence)

**0** — Weeks 1–2: baseline, profiling, success metrics, draft RTO/RPO. **1** — Weeks 3–6: contract versioning, worker + façade, observability hooks. **2** — Weeks 7–11: dedicated renderer hardening/rollout. **3** — Weeks 12–15: resilience, recovery UX, second control-room flows. **4** — Weeks 16–18: load tests, customer-scale rehearsals. **5** — Weeks 19–20: hardening, rollback playbook, go-live readiness.

## 8. Business impact

Observability lowers **MTTR**; dependency discipline cuts integration risk; snapshot/delta recovery reduces ambiguous state; performance budgets make behaviour legible to product and OEM conversations; RTO/RPO clarifies operational promises; feature flags support safer releases.

## 9. Thesis

At this scale the dominant risks are **controlling change, observing the realtime stack, recovering safely, and keeping five teams from accumulating invisible coupling**—not the headline number of robots on the canvas alone.
