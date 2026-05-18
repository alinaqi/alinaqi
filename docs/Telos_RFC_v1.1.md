# Telos — Intent-Grounded Testing for Autonomous Agents

**RFC v1.1 · Draft · May 2026**
**Series:** Agent Architecture Series — iCPG · Mnemos · Engram · Lexon · Polyphony · **Telos**
**Status:** Draft for peer review
**Depends on:** iCPG (intent governance), Engram (cross-session memory)
**Operational layer:** DAE — Dynamic Autonomous Evaluation
**Consumed by:** Maggy (autonomous engineering orchestrator)

> **Changes in v1.1:** Removed Soma from the dependency set. Integrated DAE as the operational layer (§9). Added grounded survey of commercial and open-source testing tooling (§11.2).

## Abstract

Every testing framework in production today answers one question: *does the output match the specification?* This is verification, and it is necessary but not sufficient. It silently assumes the specification is a faithful encoding of what was actually wanted. That assumption fails constantly — a spec can pass every test it has and still be a perfect implementation of a mistake.

Telos reframes testing around a different question: *does the artifact serve the intent?* It treats intent itself as a first-class, versioned, testable artifact, formalizes the lossy chain from real intent to observable behavior, and detects divergence at every link — not just the last one.

## The Intent Chain

```
Real Intent ──L1──▶ Stated Intent ──L2──▶ Specification ──L3──▶ Behavior
```

## Intent Failure Taxonomy (8 modes)

IF-1 Conformance drift · IF-2 Translation loss · IF-3 Articulation gap · IF-4 Intent incoherence · IF-5 Intent underdetermination · IF-6 Intent staleness · IF-7 Proxy capture · IF-8 Referent vacancy

## Three Test Planes

- **Plane 1 — Conformance:** `behavior ⊨ spec` (verification, DAE operates here)
- **Plane 2 — Validation:** `spec ⊨ stated intent` (continuous, autonomous)
- **Plane 3 — Intent integrity:** `stated intent ⊨ real intent` + coherence + completeness + freshness

## The Boundary of Autonomy

Telos is a fully autonomous testing framework with one deliberate exception: it does not originate the telos it tests against. That remains exogenous, captured at the highest available fidelity and placed under version control.

## Cross-References

- **iCPG**: `IntentNode`s seed from iCPG `ReasonNode`s; Telos verdicts feed back as drift events
- **Engram**: Cross-session intent history for staleness detection (IF-6) and referent triangulation
- **Polyphony**: Decomposition closure — union of sub-intents must be intent-equivalent to parent
- **Lexon**: Tool bindings can be spec-correct and intent-wrong; Telos audits Lexon's selections
- **Maggy**: Hosts the autonomous testing loop; IFS becomes the signal routing optimizes
- **DAE**: Operational evaluation layer — captures production traffic, dimensional scoring, regression baselines

*Part of the Agent Architecture Series.*
