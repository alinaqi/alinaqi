# Telos — Intent-Grounded Testing for Autonomous Agents

**RFC v1.1 · Draft · May 2026**
**Series:** Agent Architecture Series — iCPG · Mnemos · Engram · Lexon · Polyphony · **Telos**
**Status:** Draft for peer review
**Depends on:** iCPG (intent governance), Engram (cross-session memory)
**Operational layer:** DAE — Dynamic Autonomous Evaluation (§9)
**Consumed by:** Maggy (autonomous engineering orchestrator)

> **Changes in v1.1:** Removed Soma from the dependency set — the system-state-monitor integration in v1 was extrapolated from a one-line description and could not be substantiated. Integrated DAE as the operational layer rather than an ecosystem peer (§9). Added a grounded survey of commercial and open-source testing tooling (§11.2).

---

## Abstract

Every testing framework in production today answers one question: *does the output match the specification?* This is **verification**, and it is necessary but not sufficient. It silently assumes the specification is a faithful encoding of what was actually wanted. That assumption fails constantly — a spec can pass every test it has and still be a perfect implementation of a mistake.

Telos reframes testing around a different question: *does the artifact serve the intent?* It treats **intent itself as a first-class, versioned, testable artifact** rather than a fixed input, formalizes the lossy chain from real intent to observable behavior, and detects divergence at every link in that chain — not just the last one. Telos is a fully autonomous testing loop: it generates intent-tests, runs them, and issues verdicts without human steps. What it does *not* do — and this RFC is precise about the boundary — is originate the telos it tests against. That remains exogenous, captured at the highest available fidelity and placed under version control so the autonomous loop has a stable referent.

Telos defines *what* to test and *why*. Its operational layer — the runtime that captures production traffic, scores it continuously, holds baselines, enforces gates, and manages evaluation cost — is **DAE**, integrated in §9. Telos is the intent layer; DAE is the engine.

## The Intent Chain

Telos models the path from desire to behavior as a chain of four artifacts joined by three lossy translation links:

```
   Real Intent ──L1──▶ Stated Intent ──L2──▶ Specification ──L3──▶ Behavior
```

## Intent Failure Taxonomy (8 modes)

IF-1 Conformance drift · IF-2 Translation loss · IF-3 Articulation gap · IF-4 Intent incoherence · IF-5 Intent underdetermination · IF-6 Intent staleness · IF-7 Proxy capture · IF-8 Referent vacancy

## Three Test Planes

**Plane 1 — Conformance:** `behavior ⊨ spec` — verification, the only plane conventional testing instruments.

**Plane 2 — Validation:** `spec ⊨ stated intent` — does the specification faithfully encode what was asked? Detects IF-2.

**Plane 3 — Intent integrity:** `stated intent ⊨ real intent` + coherence + completeness + freshness. The novel plane. Detects IF-3 through IF-6.

## The IntentNode

To test intent, intent must be a structured object, not free text. The `IntentNode` carries: id, version, statement, acceptance criteria, invariants, **anti-criteria** (behaviors that pass the spec but violate intent — making IF-7 detectable), referent (pointers to richer sources for L1 testing), fidelity (last measured Intent Fidelity Score), provenance, and status.

## The Intent Fidelity Scale

```
IFS = F1 × F2 × F3
```

Multiplicative, not averaged, because the links form a signal chain: intent lost at L1 cannot be recovered at L2 or L3.

## The Boundary of Autonomy

Telos is a fully autonomous testing framework with one deliberate, principled exception: it does not originate the telos it tests against. The autonomous loop is *more* trustworthy, not less, for having an explicit, versioned, exogenous referent rather than a self-generated one.

## Cross-References (Agent Architecture Series)

| Primitive | Relationship to Telos |
|---|---|
| **iCPG** | Source and sink. `IntentNode`s seed from iCPG `ReasonNode`s; Telos's L2/L3 verdicts feed back as iCPG drift events. |
| **Mnemos** | Task scope. A task's `IntentNode`s live alongside its `MnemoNode`s. |
| **Engram** | Temporal substrate. IF-6 staleness detection and referent triangulation require cross-session intent history. Engram stores it; Telos queries it. |
| **Lexon** | Audit target. A tool binding can be spec-correct and intent-wrong. Telos audits Lexon's selections. |
| **Polyphony** | Decomposition check. Union of sub-intents must be intent-equivalent to parent — no telos lost in the split. |
| **Maggy** | Host. Runs the Telos loop for claude-bootstrap projects; IFS becomes the signal routing optimizes. |

## Novelty

Five contributions are genuinely new:
1. Intent as a first-class, versioned, testable artifact
2. The intent chain with per-link drift detection
3. Intent-level metamorphic relations
4. The autonomy boundary as a design principle
5. A two-layer V&V stack spanning Plane 1 (where the commercial market sits) and Planes 2–3 (where nothing currently does)

## Survey of the Commercial Testing Landscape (§11.2)

Every tool surveyed — traditional (pytest, JUnit, Playwright, Hypothesis) and AI-native (DeepEval, Ragas, Promptfoo, Braintrust, LangSmith, Arize, Maxim) — evaluates `output ⊨ reference`. **Not one of these systems validates the specification against intent.** The entire market sits on Telos Plane 1.

---

*Telos — RFC v1.1 — Draft for peer review. Operational layer: DAE. Part of the Agent Architecture Series.*
