# DAE — Dynamic Autonomous Evaluation

**RFC v1 · May 2026**
**Series:** Agent Architecture Series — Telos (intent layer) · **DAE** (operational layer)
**Status:** Phase 0–1 validated, Phase 2 planning
**Consumed by:** Telos (§9 — operational evaluation substrate)
**Depends on:** Telos (for intent-layer cross-reference)

---

## 1. The Problem

Traditional software testing assumes deterministic behavior: given input X, expect output Y. Autonomous agentic AI systems violate this assumption at every level. The same user request processed twice may produce different tool call sequences, different intermediate reasoning, and different final outputs — all of which could be equally correct.

**Failure modes that traditional testing misses:**

| Failure Mode | Impact | Detection Difficulty |
|---|---|---|
| Hallucinated facts | Incorrect claims in output | High — reads plausibly |
| Off-brand tone | Copy correct but wrong voice | Very High — subjective |
| Tool argument drift | Wrong customer ID or budget | Medium — deterministic check possible |
| Cost blowout | Premium model called 40x for simple lookup | Low — measurable but rarely monitored |
| Stale context | References discontinued product | High — requires temporal knowledge |
| Channel mismatch | Email delivered as WhatsApp format | Medium — schema validation possible |
| Silent regression | Previously passing scenarios now fail | Very High — no pre-deploy gate |

## 2. The DAE Framework

DAE models evaluation as a graph with five node types and a 5-stage pipeline:

**Node Types:**

| Node | Description |
|---|---|
| **Scenario** | A test case with input, expected behavior, and scoring rubric |
| **Rubric** | A set of weighted scoring dimensions |
| **Dimension** | A single evaluable axis with description and weight |
| **Judgment** | A score + reasoning from an evaluator |
| **Baseline** | A pinned set of judgments representing acceptable quality |
| **Gate** | A decision point requiring human review |

### The 5-Stage Pipeline

```
CAPTURE → SCORE → COMPARE → GATE → LEARN
```

- **CAPTURE:** Record interaction trace (input, model, tools, output, tokens, latency)
- **SCORE:** Two-track evaluation — deterministic schema validation (always runs, sub-millisecond) + LLM judge (sampled per risk tier)
- **COMPARE:** Compare against baseline. Flag regressions by severity (LOW/MEDIUM/HIGH)
- **GATE:** Tiered human approval based on business risk (auto-execute / human-on-the-loop / human-in-the-loop)
- **LEARN:** Adaptive sampling, dynamic scenario generation, model routing optimization

### Dimensional Scoring (not binary pass/fail)

| Dimension | Weight | Description |
|---|---|---|
| brand_alignment | 0.25 | Output reflects brand voice, values, identity |
| factual_accuracy | 0.25 | No hallucinated claims, dates, product details |
| task_completion | 0.20 | Request fully addressed |
| tool_quality | 0.15 | Correct tools called with valid arguments |
| cost_efficiency | 0.10 | Appropriate model used, no unnecessary tool calls |
| safety_compliance | 0.05 | No harmful content, policy adherent |

### Risk-Tiered Adaptive Sampling

| Risk Tier | Sampling Rate | Evaluation Depth | Est. Cost |
|---|---|---|---|
| Critical (campaign launch, budget) | 100% | Full rubric + multi-judge | ~$0.50/eval |
| Medium (copy generation, strategy) | 25% | Full rubric, single judge | ~$0.05/eval |
| Low (data retrieval, FAQ) | 5% | Deterministic + spot-check | ~$0.001/eval |
| Internal (no user output) | 1% | Schema only | ~$0.0001/eval |

### Multi-Judge Protocol

- 3 judges minimum for critical actions
- Median score used (not mean — reduces outlier impact)
- Spread check: if max – min > 25, flag for human review
- Cost: ~3x single-judge, but reduces bias by 30–40%

## 3. Implementation Status

### Phase 0 — Foundation (Shipped)
- Binary pass/fail evaluation with rule-based criteria
- LLM-as-judge scoring with configurable rubric dimensions (0–100 per dimension)
- Deterministic tool output scoring against schema registry
- Dynamic test suite generation

### Phase 1 — Baselines + Regression Detection (Validated)
- **66 scenarios** (20 built-in + 46 audit-seeded/custom)
- **25 active baselines** across 9 audit categories
- **4 regression detection types:** score_drop, pass_to_fail, model_escalation, tool_divergence
- **Regression severity:** LOW (5–10pt) / MEDIUM (10–20pt) / HIGH (>20pt)
- **Average baseline score:** 68.3 across 25 scored baselines
- **Std dev:** 2.5–3.3 across 3 runs per scenario
- **Agent-evaluator isolation:** Separate database tables (logical isolation)

### Phase 2–5 (Planned)

| Phase | Capability |
|---|---|
| **Phase 2** | Production monitoring, adaptive sampling, cost tracking |
| **Phase 3** | Tiered gating with human-in-the-loop approval |
| **Phase 4** | Multi-judge panels, channel-specific scoring, judge calibration |
| **Phase 5** | Advanced dynamic generation, adaptive test selection (ATLAS-style) |
| **Phase 6** | Telos Plane 2 — rubric validation against IntentNode |
| **Phase 7** | Telos Plane 3 — intent-integrity testing |

## 4. What DAE Enables

1. **Continuous Quality Assurance** — every production interaction scored, not just pre-deployment
2. **Regression-Safe Prompt Updates** — shadow evaluation before deployment
3. **Cost-Aware Model Routing** — appropriate model per evaluation need
4. **Dynamic Coverage** — new scenarios auto-generated as capabilities expand
5. **Human Approval Intelligence** — full reasoning trace + dimensional scores for approvers
6. **Audit Trail** — complete trace from input → reasoning → tools → output → evaluation

## 5. Comparison with Existing Approaches

| Capability | Static Benchmarks | LLM-as-Judge | EDDOps | CLEAR | DAE |
|---|---|---|---|---|---|
| Continuous scoring | ✗ | ✗ | ✓ | ✓ | ✓ |
| Multi-dimensional scoring | ✗ | ✓ | ✓ | ✓ | ✓ |
| Regression detection | ✗ | ✗ | ✓ | Partial | ✓ |
| Human-in-the-loop gating | ✗ | ✗ | ✓ | ✗ | ✓ |
| Multi-judge panels | ✗ | ✗ | ✗ | ✗ | ✓ |
| Adaptive sampling | ✗ | ✗ | ✓ | ✗ | ✓ |
| Tool output validation | ✗ | ✗ | ✗ | ✗ | ✓ |
| Production monitoring | ✗ | ✗ | ✓ | ✓ | ✓ |
| Benchmark tamper resistance | ✗ | ✗ | Partial | ✗ | ✓ |

## 6. Relationship to Telos

DAE is Telos's operational layer. The seam is exact:

- **DAE tests:** `behavior ⊨ rubric` (Telos Plane 1 — verification)
- **Telos tests:** `rubric ⊨ intent` (Telos Plane 2 — validation)
- **Together:** A two-layer V&V stack spanning all three test planes

When integrated, a DAE `Rubric` carries an `intent_node` reference linking it to its Telos `IntentNode`. Telos Plane 2 runs over DAE's rubrics: when a rubric diverges from its IntentNode, Telos raises IF-2 (translation loss) and routes a rubric revision back to DAE.

## 7. What DAE Does NOT Do

- Replace human judgment — augments, not eliminates
- Guarantee correctness — scores probabilistically, not absolutely
- Work without configuration — requires rubric definitions and gating thresholds
- Evaluate model training — runtime framework, not training framework
- Originate the telos — that is Telos's boundary (§8)

---

*DAE — RFC v1. Part of the Agent Architecture Series. Operational layer for Telos.*
