# Synaos Staff Frontend Challenge

## Architectural Review & Improved Design Proposal

---

## 1. Executive Summary

The current PLAN.md is strong: it is pragmatic, avoids unnecessary rewrites, and correctly identifies the telemetry hot path with a worker + facade + dedicated rendering approach.

However, the real challenge is not rendering 300 robots — it is operating a **real-time, multi-team frontend platform** that:

- Scales reliably
- Is observable end-to-end
- Recovers safely from failure
- Evolves without breaking production

This proposal strengthens the plan by introducing:

- Dependency awareness
- End-to-end observability
- Explicit recovery strategy (RTO/RPO)
- Snapshot + delta consistency model
- Real-time testing strategy
- Measurable performance targets

---

## 2. Key Architectural Principles

### 2.1 Dependency Awareness

The system must make dependencies explicit across:

- Micro-frontends
- Shared libraries
- Backend contracts
- Runtime configuration

**Key practices:**

- Dependency map (remotes, shared packages, contracts)
- Clear ownership per capability
- Semantic versioning
- Contract testing
- Compatibility matrix

> Rule: Micro-frontends must not depend on each other's internal state.

---

### 2.2 Contract-First Real-Time Architecture

Telemetry must be:

- Versioned
- Backward-compatible
- Validated at runtime

**Model:**

- Snapshot (authoritative state)
- Delta updates (incremental changes)
- Sequence numbers (ordering + recovery)

---

### 2.3 Real-Time Isolation

Separate:

- High-frequency telemetry (robot positions)
- Low-frequency UI/application state

**Implementation:**

- Web Worker processes telemetry
- Facade exposes minimal, stable API to Angular
- Avoid pushing high-frequency updates into global state (NGXS)

---

### 2.4 Dedicated Rendering Pipeline

Rendering must be:

- Decoupled from Angular lifecycle
- Optimized for moving entities

**Techniques:**

- Batching / instancing
- Level of Detail (LOD)
- Interpolation between frames
- GPU-friendly updates

---

### 2.5 Observability (First-Class Requirement)

> “We cannot operate what we cannot observe.”

Track the full pipeline:
