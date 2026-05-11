# Mnemos - Task-Scoped Memory Lifecycle for Autonomous Agents

A framework defining how an agent acquires, organizes, compresses, inherits, and hands off knowledge while executing a single task -- from initiation to completion. Primary motivation: preventing premature context crashes in long-running Claude Code sessions and multi-agent task hierarchies. Companion RFC to Engram (cross-session amnesia) and iCPG (code intent memory).

## 0. Minimal Viable Mnemos: Start Here

The full system -- eight node types, extraction pipeline, tiered fatigue model, external REM Process, SkillNode algebra, merge algebra, orchestrator protocol -- is too heavy for initial adoption. Mnemos is a layered architecture where each tier adds value independently and every tier is a superset of the one below. Deploy Tier 0 first. Validate it. Then add tiers as production failure modes demand them.

| Level | Fatigue Model | Consolidation | Orchestrator | Handoff | Implementation Delta |
| --- | --- | --- | --- | --- | --- |
| **Tier 0** (Start Here) | Token utilization only -- no sidecar | Checkpoint at 0.60 + resume from checkpoint | None -- operator reads logs | Untyped summary | Intercept context_remaining from Claude Code hooks. Write CheckpointNode to disk. Resume from checkpoint on restart. Six hours of implementation. |
| **Tier 1** (Add Fatigue) | Four-dimension composite score via extraction sidecar | Micro at 0.40, Mini-REM at 0.60, Full REM at 0.75 | Fatigue Report upward. Orchestrator may preempt. | Typed ResultNodes + SkillNodes in HandoffNode | Add extraction pipeline (tool call taxonomy). Add REM Process sidecar. Add orchestrator signal receiver. |
| **Full Mnemos** | All four dimensions + TCS + fallback hierarchy | All three tiers with async Mini-REM option | Full five-signal protocol + orchestrator fleet health | Complete HandoffNode with provenance, conflicts, fleet diagnostics | Add TCS with fallback. Add async Mini-REM. Add orchestrator fleet dashboard. Add formal merge algebra. |

> The minimum credible implementation is MVM Tier 0: token utilization monitoring + checkpoint writing at a fixed threshold + resume from checkpoint. This alone prevents the most common failure mode -- context wall crash with no recovery path. Experiment G validates this claim before any other experiment runs. If Tier 0 does not help, pause all other work and diagnose before proceeding.

## 1. Motivation: Agents That Crash Before the Task Is Done

The central failure mode is not subtle: a Claude Code agent working on a long task runs out of context mid-execution, compresses badly or not at all, and either produces degraded output or halts entirely. Current agents treat memory as a flat append-only buffer. Everything goes in. Nothing comes out until the buffer is full. At that point the only available response is bulk summarization -- a lossy, untyped operation that destroys exactly the structured knowledge the agent needs most: active constraints, in-progress reasoning, proven skill patterns.

The framework is grounded in five observed failure patterns from Claude Code long-session operation:

- **Context wall crash:** agent hits the 200k limit mid-task. No checkpoint exists. Full restart required. All progress lost.
- **Degraded resumption:** agent writes a last-minute summary before crashing, but the summary loses constraint context. Agent resumes but violates architectural boundaries it had previously respected.
- **Sub-agent cascade:** parent orchestrator receives no signal from a crashed sub-agent. Dependent sub-agents wait indefinitely. Orchestrator times out the entire task.
- **Redundant tool calls under pressure:** as context fills, the agent loses track of what it has already done and re-executes tool calls. Token cost accelerates. Wall approaches faster.
- **Skill amnesia:** agent solves a pattern in sub-task 3, then re-derives the same solution from scratch in sub-task 17. No mechanism promotes the pattern into reusable memory.

### 1.1 The Multi-Agent Compounding Problem

Single-agent context exhaustion is bad. Multi-agent context exhaustion is catastrophic for a different reason: temporal coupling. In a Claude Code multi-agent workflow, a supervisory orchestrator delegates sub-tasks to specialist sub-agents. When a sub-agent crashes mid-task, the orchestrator has three bad options: restart from scratch (expensive), reconstruct state from its own context (lossy), or abort the parent task (wasteful). No current system gives the orchestrator visibility into sub-agent memory pressure before the crash. The orchestrator cannot preempt. It can only react.

The goal is not to make individual agents survive longer. It is to make the entire multi-agent task hierarchy complete reliably -- with every agent managing fatigue preemptively and every orchestrator making informed decisions about sub-agent health before anything fails.

### 1.2 Explicit Scope

Mnemos is scoped to task-local memory -- the knowledge an agent holds while executing a single task from initiation to completion. It is not a session memory system, a knowledge base, or a RAG layer.

**In Scope:**

- Task-local memory: knowledge held while executing a single task from initiation to handoff
- Node extraction mechanism: how MnemoNodes are created from agent output
- Memory structure: typed, weighted MnemoNodes with eviction policies
- Fatigue model: composite pressure signal with graduated responses before the context wall
- Tiered REM consolidation: Micro (in-context), Mini-REM (partial, async), Full REM (external process)
- SkillNode promotion: semantic fingerprinting, equivalence detection, promotion algebra
- Sub-agent inheritance: scope-isolated slicing and formal merge algebra
- Supervisory fatigue coordination: orchestrator visibility and preemptive intervention
- HandoffNode: structured artifact for Engram encoding at task completion
- Resumption protocol: clean restart from CheckpointNode

**Out of Scope (explicit):**

- Long-term memory across sessions -- Engram RFC
- Code intent, drift, ReasonNodes -- iCPG RFC
- Tool calls, external capabilities, RAG retrieval
- Peer-to-peer multi-agent coordination beyond parent <-> child
- Model weights, fine-tuning, parametric memory

### 1.3 Position in the RFC Stack

Mnemos is the middle layer of a three-RFC architecture. iCPG governs code intent memory across a codebase. Engram governs operational memory across sessions. Mnemos governs what happens inside a single task -- the connective tissue between the two. When a Mnemos task completes, it produces a HandoffNode for Engram to encode. Mnemos owns the full lifecycle from task initiation through handoff. Engram owns what happens after.

| RFC | Scope | Primary Primitive | Problem Addressed | Primary Consumer |
| --- | --- | --- | --- | --- |
| iCPG | Code sessions, repositories | ReasonNode, DriftEvent | Specification drift in code -- agent modifying code outside original intent | Agent querying before it writes code |
| Mnemos | Single task execution | MnemoNode (8 types) | Context crash, task drift, premature halt in long-running and multi-agent tasks | Agent executing right now |
| Engram | Cross-session, multi-agent | EngramRecord | Amnesia across sessions, channels, entities | Next session needing prior context |

## 2. Prior Research: What Exists and What Is Missing

Agent memory research has expanded rapidly since late 2024. The December 2025 Zhang et al. survey "Memory in the Age of AI Agents" found the field increasingly fragmented, with loosely defined terminologies. The January 2026 "Anatomy of Agentic Memory" survey maps over 80 recent papers. This section maps the closest prior work against Mnemos.

| System | What It Does | What Mnemos Adds / Distinction |
| --- | --- | --- |
| Focus (Jan 2026) | Active Context Compression for agentic coding. Agent consolidates learnings into a "Knowledge" block and prunes history. 22.7% token reduction on SWE-bench Lite. | Binary consolidate/prune. No typed nodes, no fatigue score, no REM phases, no sub-agent protocol. Reactive, not predictive. |
| Atlas (Mar 2026) | Four-layer epistemic memory: Fresh (run-scoped), Task (workspace-scoped), Contextual (one episode per task), Historical (verified facts). Instruction rewriting. | Fresh and Task layers are passive accumulators -- no fatigue model, no typed node structure within layers, no consolidation trigger. No sub-agent inheritance. |
| EverMemOS (Jan 2026) | Engram-inspired memory OS: Episodic Trace Formation -> Semantic Consolidation -> Reconstructive Recollection. MemCells clustered into MemScenes. SOTA on LoCoMo. | Scoped to conversational long-term memory, not task execution. No fatigue signal, no checkpoint resumption, no sub-agent protocol. |
| AgeMem (Jan 2026) | Unified LTM + STM via RL. Three-stage progressive training. Agent invokes memory operations via tool interface. | RL-trained -- not architecturally typed. No explicit fatigue model. No supervisory coordination. |
| ACON (Oct 2025) | Learns compression guidelines. Reduces context 26-54%. Preserves factual history, action-outcome relationships, evolving state. | Compression-only. No typed nodes, no fatigue prediction, no multi-agent protocol. All context treated uniformly. |
| Multi-Agent Memory Architecture Survey (Mar 2026) | Frames multi-agent memory as computer architecture. Three-layer hierarchy. Identifies memory access protocol and consistency as unsolved. | Position paper. Identifies the gap Mnemos fills but proposes no typed inheritance or merge-back protocol. |
| Memory in the Age of AI Agents Survey (Dec 2025) | Comprehensive landscape. Forms-Functions-Dynamics taxonomy. Confirms field is fragmented. | Survey, not a system. Confirms: no existing work defines task-scoped typed memory lifecycle with supervisory fatigue coordination. |
| ENGRAM (Nov 2025) *Naming note* | Lightweight memory orchestration for conversational agents using typed stores and semantic retrieval. SOTA on LongMemEval. | Different concept. Published ENGRAM is a retrieval optimization system. The Engram RFC in this stack is a pathology-diagnostic framework defining seven amnesia types. Distinct in scope, purpose, and design. |

### 2.1 What Is Novel

No prior work combines task-scoped typed memory nodes, a composite fatigue signal with graduated responses, within-task REM consolidation via an external process, action-sequence WorkingNode inference, three-signal SkillNode fingerprinting, formal sub-agent inheritance and merge algebra, and orchestrator-level supervisory fatigue coordination in a single unified architecture.

| Capability | Prior Work | Closest Partial | Mnemos Contribution |
| --- | --- | --- | --- |
| Typed task-scoped memory nodes | None | Focus (untyped), Atlas (cross-task layers) | MnemoNode taxonomy: 8 types with lifecycle constraints scoped to a single task execution |
| Composite fatigue score | None | None | Multi-dimensional pressure signal with graduated responses before context wall |
| Within-task REM consolidation | None | EverMemOS (cross-session Semantic Consolidation) | Four-phase within-task REM via external process -- resolves fatigued-agent-doing-REM paradox |
| Within-task SkillNode promotion | None | Mem^p (cross-session) | Mid-task chunking dividend: WorkingNode sequences compress to SkillNodes during execution |
| Action-sequence WorkingNode inference | None | None | WorkingNodes inferred from tool call patterns -- decoupled from thinking block availability |
| Three-signal fingerprinting | None | None | Structural hash + semantic embedding + outcome signature with two-of-three equivalence |
| Sub-agent inheritance + merge algebra | None | Intrinsic Memory Agents (role-aligned, no inheritance rules) | Formal per-node-type slice rules + conflict-surfacing merge algebra with explicit precedence |
| Supervisory fatigue coordination | None | None | Orchestrator-level fatigue visibility, preemptive intervention, fleet health monitoring |
| Tiered consolidation | None | None | Micro (in-context) / Mini-REM (partial, async) / Full REM (complete, external) |
| Minimal Viable Mnemos | None | None | Tier 0 deployment in ~6 hours providing immediate value before full system is in place |

## 3. The MnemoNode Data Model

Mnemos organizes task memory as a typed graph of MnemoNodes -- the MnemoGraph. Every piece of knowledge the agent holds during a task is a node with an explicit type, activation weight, eviction policy, and lifecycle status. At any moment, a typed subset of the MnemoGraph is loaded into active context -- the agent's working memory. The rest is held in the graph structure outside the context window, retrievable by node type, activation weight, or scope tag. This is the core architectural shift: the agent does not hold everything in context. It holds a typed, weighted slice and knows how to retrieve the rest.

### 3.1 Node Types and Eviction Policy

| Type | Contents | Eviction Policy | Biological Parallel |
| --- | --- | --- | --- |
| GoalNode | Root intent and success criteria | Never evicted | Prefrontal goal representation |
| ConstraintNode | Hard rules the task must not violate | Never evicted | Inhibitory control signals |
| ContextNode | Background loaded at task start | Evictable: activation_weight < 0.2 and no link to active WorkingNode | Episodic retrieval into working memory |
| WorkingNode | Active reasoning, in-flight decisions | Expires after sub-goal. Compressed (not deleted) -- summary retained. | Phonological loop / central executive |
| ResultNode | Confirmed outputs, completed sub-task results | Compressible at REM. Evicted when summarized. | Consolidated short-term episodic |
| SkillNode | Compressed reusable action sequences | Not evicted -- loaded on-demand by scope_tag | Procedural / basal ganglia chunks |
| CheckpointNode | Serialized MnemoGraph state at a moment in time | Persisted outside context window | Hippocampal snapshot |
| HandoffNode | Curated task completion artifact for Engram | Persisted, passed to Engram at task end | Sleep-phase neocortical transfer |

### 3.2 MnemoNode Schema

```
MnemoNode {
  // Identity
  id:                UUID
  type:              GoalNode | ConstraintNode | ContextNode | WorkingNode
                     | ResultNode | SkillNode | CheckpointNode | HandoffNode
  task_id:           UUID
  parent_node_id:    UUID?            // for sub-task inheritance tracking

  // Content
  content:           string
  summary:           string?          // compressed form, written by REM Process

  // Weight and lifecycle -- computed by extraction pipeline, not by agent
  activation_weight: Float[0..1]      // a*recency + b*access_frequency + g*centrality
  created_at:        Timestamp
  last_accessed:     Timestamp
  access_count:      Int              // reinforcement proxy
  status:            ACTIVE | COMPRESSED | EVICTED | PROMOTED | HANDED_OFF

  // Provenance -- prevents task-level confabulation
  origin:            LOADED | DERIVED | TOOL_RESULT | INHERITED | AGENT_GENERATED
  confidence:        Float[0..1]

  // Relationships
  links:             Set<MnemoNodeRef>
  scope_tags:        Set<string>      // which sub-tasks this node is relevant to
  fingerprint:       SemanticFingerprint?  // SkillNodes only -- see section 6.2
}
```

Activation weight formula: `activation_weight = a*recency_score + b*access_frequency + g*centrality_score`. Default: a=0.5, b=0.3, g=0.2. Recency uses exponential decay with half-life tuned per node type -- ContextNodes decay faster than ConstraintNodes. Centrality is approximated by in-degree in the MnemoGraph link graph. Computed by extraction pipeline sidecar -- zero agent context cost.

## 4. Node Extraction: How MnemoNodes Are Actually Created

MnemoNodes are not created by asking the agent to classify its own knowledge -- this would consume context and produce inconsistent results. Instead, an external extraction pipeline sidecar intercepts three observable signals: the task initiation prompt, tool call inputs and outputs, and (where available) extended thinking blocks. Node type is determined primarily by structural position and tool call taxonomy -- not by LLM classification of content.

| Node Type | Source Signal | Extraction Mechanism | Reliability |
| --- | --- | --- | --- |
| GoalNode | Task initiation prompt / orchestrator instruction | Structural: root prompt is always the GoalNode. No ambiguity. | High -- deterministic |
| ConstraintNode | CLAUDE.md rules, system prompt invariants, explicit prohibition language | Keyword + structural: modal verbs (must/never/always), system prompt sections tagged as constraints. | High -- rule-based from structured sources |
| ContextNode | File reads, doc fetches, background at task start | Tool call taxonomy: read_file, fetch_url, list_directory at initiation -> ContextNode. Mid-task tool results -> ResultNode. | High -- tool call type determines node type |
| WorkingNode (Tier 1 -- Primary) | Tool call sequences -- always available | Infer from action sequences: (tool_type, input_pattern, output_status). A read_file->bash->write_file pattern implies a sub-goal. Content reconstructed from structure, not observed directly. | Medium-High -- sufficient for SkillNode fingerprinting |
| WorkingNode (Tier 2 -- Enhanced) | Extended thinking blocks -- when available | Direct extraction from thinking blocks. Richer semantic content. | High when available. Degrades to Tier 1 when thinking is disabled. |
| WorkingNode (Tier 3 -- Fallback) | Agent output structure | Infer from code comments, plan statements, section headers. High noise. | Low-Medium -- only when Tier 1 signals are sparse |
| ResultNode | Tool call outputs after task initiation, confirmed facts | Tool outcome taxonomy: write_file, bash output, test results -> ResultNode. Confidence from outcome status. | High -- tool outcome determines type and confidence |
| SkillNode | Promoted from WorkingNode sequences by REM Process | Synthesized by REM Process -- not extracted from raw output. Requires 3+ equivalent executions. | N/A -- synthesized, not extracted inline |
| CheckpointNode | Written by REM Process at fatigue thresholds | Structured serialization of MnemoGraph state -- programmatic, not LLM-extracted. | High -- deterministic |
| HandoffNode | Written by REM Process at task completion | Curated distillation of final MnemoGraph -- synthesized, not extracted. | High -- deterministic |

### 4.1 WorkingNode Extraction: Three-Tier Hierarchy

WorkingNode extraction is the weakest link when thinking blocks are unavailable. V4 resolves this by treating thinking blocks as an enhancement, not a dependency. Action-sequence inference is the primary path and is always available.

- **Tier 1 (Primary -- always available):** infer WorkingNode from tool call sequences. Three observable signals: tool type sequence (ordered list of tool call types without content), input pattern (structural shape stripped of specific values), output status (success/failure/partial). This reconstructs what the agent was doing without requiring access to why.
- **Tier 2 (Enhanced -- when available):** direct extraction from extended thinking blocks. Richer semantic content. More accurate WorkingNode typing. Degrades gracefully to Tier 1 when thinking is disabled.
- **Tier 3 (Fallback):** infer from agent output structure -- code comments, plan statements, section headers. High noise. Used only when Tier 1 signals are also sparse.

The critical property: SkillNode fingerprinting operates on structural action patterns, not semantic reasoning content. This means Tier 1 action-sequence inference provides sufficient signal for the primary downstream use case even without thinking blocks. Experiment B measures the quality degradation quantitatively.

### 4.2 Extraction Pipeline Limits

Honest accounting of what the pipeline cannot reliably do:

- Pattern detection for SkillNode candidates requires the REM Process, not the extraction pipeline. The pipeline flags candidates; the REM Process synthesizes SkillNodes.
- Real-time semantic contradiction detection between tool call outputs is deferred to the merge algebra. The pipeline does not catch contradictions inline.
- WorkingNode extraction in reasoning-heavy phases with few tool calls degrades even with Tier 1. Planning phases produce few repeatable patterns suitable for SkillNode promotion -- the loss is bounded.

## 5. The Fatigue Model

Memory fatigue is the composite pressure signal that drives all consolidation decisions. It is computed continuously by the extraction pipeline sidecar -- not by the agent -- so it consumes zero agent context tokens. Without a fatigue model, the only signal available is the hard context wall. By the time that fires, graceful recovery is impossible.

### 5.1 Fatigue Dimensions

| Dimension | What It Measures | Computation | Weight (proposed) |
| --- | --- | --- | --- |
| Token Utilization | Context window fill level | active_tokens / context_limit | 0.40 |
| Task-Coherence Score | Fraction of active nodes aligned to current sub-goal via scope_tag overlap (see 3.2) | 1 - TCS(active_nodes, current_subgoal) | 0.25 |
| Retrieval Latency | Rising tool calls per sub-goal = harder to find relevant context | Rolling 5-subgoal mean tool calls / baseline | 0.20 |
| Task Graph Depth | Sub-task nesting depth relative to maximum | current_depth / max_depth | 0.15 |

```
fatigue_score = 0.40*token_util + 0.25*(1-TCS) + 0.20*retrieval_latency + 0.15*task_depth
```

Weights are proposed, not empirically validated. Experiment A produces data-driven weights from labeled Claude Code session traces.

### 5.2 Task-Coherence Score and Fallback Hierarchy

The Task-Coherence Score (TCS) measures alignment of active nodes to the current sub-goal using scope_tag overlap -- structural, not semantic, and cheaper to compute than embedding similarity. TCS = fraction of active nodes with at least one scope_tag matching the current sub-goal's scope. However, TCS depends on scope_tag quality. The fallback hierarchy ensures the system remains functional when tags are sparse.

| Tier | Trigger Condition | Metric Computed | Notes |
| --- | --- | --- | --- |
| **Tier 1 (Primary)** Scope-Tag TCS | scope_tag coverage >= 40% of active nodes | Fraction of active nodes with >=1 scope_tag matching current sub-goal | Most accurate. Directly measures contextual relevance to current sub-task. |
| **Tier 2 (Fallback)** Recency-Weighted Pressure | scope_tag coverage < 40% OR tags are overly broad (< 3 distinct tag values) | Fraction of active nodes accessed in last N sub-goals, weighted by recency | Less precise. Captures temporal relevance rather than task alignment. Better than nothing. |
| **Tier 3 (Last Resort)** Conservative Token Pressure | Tier 2 also unreliable (e.g., task restart, all nodes equally recent) | Pure token utilization with 0.85x multiplier on fragmentation weight -- admits signal is unknown | Errs toward earlier consolidation. Admitting ignorance is safer than computing a false coherence signal. |

Tag coverage is improved by two mechanisms: propagating GoalNode scope_tags downward to all nodes created at task start, and inferring scope_tags from tool call patterns during action-sequence extraction (file paths imply module scope, test commands imply testing scope).

### 5.3 Fatigue Thresholds and Responses

| Score | State | Agent + Orchestrator Response |
| --- | --- | --- |
| 0.0 - 0.4 | **FLOW** | No intervention. Agent operates freely. |
| 0.4 - 0.6 | **COMPRESS** | Micro-Consolidation: compress oldest ResultNodes, evict cold ContextNodes. In-context, rule-based, zero latency. |
| 0.6 - 0.75 | **PRE-SLEEP** | Write CheckpointNode. Report fatigue to orchestrator. Spawn Mini-REM Process (partial graph, async-capable). Agent may continue on checkpoint. |
| 0.75 - 0.9 | **REM** | Full REM Process: all four phases. Agent suspended. Context rebuilt from compressed graph. Wake Signal sent to orchestrator on completion. |
| 0.9+ | **EMERGENCY** | Halt immediately. Write emergency CheckpointNode. Escalate to orchestrator. Do not proceed without human or orchestrator resolution. |

> The EMERGENCY threshold should never be reached in a well-managed fleet. Every architectural decision -- graduated thresholds, upward reporting, preemptive intervention, fleet checkpoints -- exists to ensure fatigue is addressed at PRE-SLEEP or REM, not at EMERGENCY. A fleet that frequently reaches EMERGENCY is a fleet without working supervisory coordination.

## 6. Tiered Consolidation: Matching Cost to Pressure

A single consolidation model -- Full REM at 0.75 -- is the wrong granularity. A fatigue score of 0.42 does not need a 10-30 second external process. A score of 0.78 during a real-time interactive coding loop cannot afford Full REM latency. Three consolidation tiers match cost to pressure level and task context.

| Tier | Trigger | Where | Tokens | Latency | What It Does | Agent Suspended? | Use When |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Micro-Consolidation | 0.40-0.60 | In-context | < 500 | < 100ms | Compress 3 oldest ResultNodes. Evict 1 cold ContextNode. Rule-based -- no LLM call. | No | Always. No configuration needed. |
| Mini-REM | 0.60-0.75 | External process, partial graph | 2,000-5,000 | 2-8 sec | Phase 1 (Slow-Wave) + SkillNode candidate flagging. Partial Wake Reconstruction. No Phase 2. | Yes (async option available) | Long-running tasks > 20 sub-goals, not time-critical. |
| Full REM | 0.75+ | External process, full graph | 4,500-12,500 | 10-30 sec | All four phases. Full SkillNode promotion. Full Wake Reconstruction. HandoffNode draft. | Yes (no async -- full graph cannot be modified while agent continues) | Tasks > 30 sub-goals where break-even confirmed. |

### 6.1 Tier Selection Logic

```
if fatigue < 0.40:                          no_consolidation
if 0.40 <= fatigue < 0.60:                  micro_consolidation
if 0.60 <= fatigue < 0.75:
  if session_subgoal_count < 20:            micro_consolidation
  elif task_context == INTERACTIVE:         micro_consolidation
  else:                                     mini_rem (async if configured)
if fatigue >= 0.75:
  if session_subgoal_count < 30:            mini_rem
  else:                                     full_rem
```

task_context (INTERACTIVE / BATCH / LONG_HORIZON) is set at task initiation from the orchestrator or inferred from tool call frequency. Async Mini-REM buffers new tool call outputs during consolidation and integrates them post-completion. Full REM always suspends the agent -- the full graph cannot be modified while the agent continues.

### 6.2 The REM Process Architecture: Resolving the Fatigued-Agent Paradox

A critical flaw in naive REM design: if an agent is at fatigue 0.75, it has minimal spare capacity. Asking it to run summarization, pattern extraction, and graph restructuring inside that same context is asking it to do expensive LLM work with the least headroom. This is the fatigued-agent-doing-REM paradox.

The resolution: REM cannot run inside the fatigued agent's context. It runs in a separate, fresh context -- a dedicated REM Process that receives a serialized MnemoGraph dump, consolidates externally, and returns a compressed graph. The agent's context is frozen during REM. The REM Process's token expenditure comes from a fresh budget, not from the fatigued agent's remaining headroom.

```
REMInput {
  task_id:          UUID
  mnemo_graph:      MnemoNode[]         // all nodes, all statuses
  current_subgoal:  string              // what agent was working on when suspended
  fatigue_state:    FatigueBreakdown    // which dimensions drove the trigger
  skill_candidates: WorkingNodeSequence[]  // flagged by extraction pipeline
}
```

### 6.3 The Four REM Phases

**Phase 1 -- Slow-Wave: Episodic Compression**

- Batch all ACTIVE ResultNodes with last_accessed > N sub-goals ago into groups of 5-10.
- For each batch: generate a summary. Set status COMPRESSED. Store in summary field. Evict content.
- Extract cross-cutting facts appearing in 3+ ResultNodes -- promote to persistent semantic fact.
- Evict ContextNodes with activation_weight < 0.2 and no scope_tag overlap with current_subgoal.

**Phase 2 -- Skill Consolidation** (see 6.4 for full specification)

- Process skill_candidates from extraction pipeline.
- Compute semantic fingerprint for each candidate. Compare against existing SkillNode library.
- Promote, merge, or reject per promotion algebra. Mark promoted WorkingNode sequences PROMOTED.

**Phase 3 -- Task Graph Pruning**

- Mark completed sub-tasks CRYSTALLIZED: pointer only, content evicted.
- Rebuild active task graph from current_subgoal forward -- PENDING and BLOCKED nodes only.
- Re-evaluate BLOCKED sub-tasks against updated graph. Consolidation sometimes reveals unblocked paths.

**Phase 4 -- Wake State Reconstruction**

- Always load: GoalNode, current sub-task's ConstraintNodes, current WorkingNode.
- Load: SkillNodes with scope_tag overlap with current_subgoal.
- Load: most recent CheckpointNode reference as orientation anchor.
- Load: ResultNodes with activation_weight > 0.5 linked to current_subgoal.
- Target: Wake State active context <= 50% of pre-REM size. Send Wake Signal to orchestrator.

### 6.4 SkillNode Promotion Algebra

SkillNode promotion is the mechanism by which the agent compounds efficiency over long tasks. Three steps:

**Step 1 -- Three-Signal Semantic Fingerprinting**

| Signal | What It Captures | Match Condition | Known Blind Spot |
| --- | --- | --- | --- |
| **Signal 1** Structural Hash | Hash of tool call type sequence only -- not content. read_file->bash->write_file produces the same hash regardless of which files. | Identical hashes = same pattern class | Cannot distinguish same-structure patterns with different semantics |
| **Signal 2** Semantic Embedding | Mean embedding of tool call inputs/outputs, stripped of specific paths and values before embedding. | Cosine similarity > 0.82 = semantically similar | Embedding drift across long sessions. Mitigated by stripping specifics before embedding. |
| **Signal 3** Outcome Signature | Hash of output status pattern: (success/failure/partial) across sequence steps. | Identical outcome signatures = same success/failure structure | Cannot distinguish two patterns that both succeed for different reasons |

**Step 2 -- Equivalence Decision**

- **Strong equivalence** (all three signals agree): high confidence merge. Confidence bump +0.10.
- **Standard equivalence** (structural hash + one other signal): normal promotion at default confidence.
- **Weak equivalence** (embedding similarity only, no hash match): flag CANDIDATE. Require 5+ occurrences before promotion.
- **No equivalence:** reject. Do not promote.

**Step 3 -- Promotion Algebra**

- **NEW:** sequence appears 3+ times with equivalent fingerprint -> create SkillNode. Confidence = 0.6.
- **REINFORCE:** existing SkillNode fingerprint matches -> increment execution_count, confidence +0.05 per success, cap 0.95.
- **MERGE:** two SkillNodes with equivalent fingerprints exist -> merge with higher confidence and combined execution_count.
- **REJECT:** < 3 occurrences, OR similarity < 0.82, OR execution failed -> do not promote.

**Known Fingerprint Failure Modes and Mitigations**

| Failure Mode | Cause | Mitigation |
| --- | --- | --- |
| False merge | Two different patterns match on structural hash AND similar embeddings despite different intent | Two-of-three requirement. Outcome signature as tiebreaker. FINGERPRINT_COLLISION logged for review. |
| Missed merge | Same pattern embeds differently due to context drift in long session | Structural hash is context-independent -- catches cases where embedding drifts. Weak equivalence path (5+ occurrences) catches slow-building patterns. |
| Domain collision | Two domains use same tool sequence for different purposes | scope_tags on SkillNodes prevent cross-domain promotion. "auth"-tagged SkillNode never merges with "logging"-tagged SkillNode. |
| Threshold brittleness | 0.82 threshold too tight or loose for specific task types | Experiment D produces per-domain calibration. Threshold is configurable per scope_tag domain. |

### 6.5 REM Cost Model and Break-Even

| Phase | Token Cost (est.) | Cost Driver | Scaling Notes |
| --- | --- | --- | --- |
| Phase 1: Slow-Wave | 1,500-4,000 | One LLM call per batch of 5-10 ResultNodes. Cheap per node. | 30 ResultNodes = 3-6 LLM calls. |
| Phase 2: Skill Consolidation | 2,000-6,000 | Pattern comparison across WorkingNode sequences. Most expensive phase. | Skipped if no promotion candidates. Cost avoidable. |
| Phase 3: Graph Pruning | 200-500 | Structural pointer updates. Minimal LLM involvement. | Near-zero. Deterministic. |
| Phase 4: Wake Reconstruction | 800-2,000 | One LLM call to rebuild minimal active context from compressed graph. | Fixed cost. Does not scale with graph size. |
| **Total REM (typical)** | **4,500-12,500** | 20-40 ResultNodes, 5-10 SkillNode candidates, moderate depth. | |
| **Context recovered by REM** | **15,000-50,000+** | Tokens freed by evicting compressed/promoted nodes from active context. | Net positive at sessions > ~30 sub-goals. |

Break-even: `context_saved > rem_cost + (agent_idle_tokens * rem_duration_steps)`

At typical sessions with 20+ sub-goals, a single REM cycle (4,500-12,500 tokens) recovers 15,000-50,000 tokens of active context. Break-even is comfortably positive for sessions longer than ~30 sub-goals. REM is net-negative only for very short tasks -- the fatigue threshold naturally prevents REM triggering in those cases since token utilization stays low.

## 7. Supervisory Fatigue Coordination

Individual agent fatigue management is necessary but not sufficient for multi-agent Claude Code workflows. The orchestrator must have real-time visibility into sub-agent fatigue and the ability to intervene preemptively -- before any sub-agent reaches EMERGENCY. Mnemos defines a lightweight signaling protocol for this, with three implementation tiers that can be deployed independently.

### 7.1 The Fatigue Signaling Protocol

| Signal | Direction | Trigger | Payload | Orchestrator Action |
| --- | --- | --- | --- | --- |
| Fatigue Report | Sub-agent -> Orchestrator | Every PRE-SLEEP threshold crossing | fatigue_score, dominant_dimension, tokens_to_emergency | Log + decide: let agent REM, reassign sub-task, or preempt earlier. |
| REM Notification | Sub-agent -> Orchestrator | When REM Process spawned | progress_at_checkpoint, estimated_duration, eviction_plan | Hold dependent tasks. Do not spawn replacement. Await Wake Signal. |
| Wake Signal | Sub-agent -> Orchestrator | When REM completes | post_rem_fatigue, active_context_size, next_subgoal | Resume dependent task queue. Update fleet health. |
| Preempt Request | Orchestrator -> Sub-agent | Sub-agent fatigue >= 0.65 before self-report | Instruction to write checkpoint and enter PRE-SLEEP now | Sub-agent complies. Writes CheckpointNode. Spawns REM Process. Reports. |
| Abort + Handoff | Orchestrator -> Sub-agent | Fleet-level deadlock risk (multiple EMERGENCY simultaneously) | Write emergency checkpoint and terminate | Writes emergency CheckpointNode. Task reassigned to fresh agent seeded from checkpoint. |

### 7.2 Orchestrator Implementation Tiers

The orchestrator protocol has a complexity ceiling. The Proactive tier should not be deployed until the Reactive tier is stable in production.

| Level | Signals Consumed | Behavior | Deployment Note |
| --- | --- | --- | --- |
| Minimal Orchestrator | Fatigue Report only | Log sub-agent fatigue scores. No active intervention. Human operator reviews logs. | Adds observability with zero coordination logic. Low risk. Deploy first. |
| Reactive Orchestrator | Fatigue Report + REM Notification + Wake Signal | Receive signals. Hold dependent tasks during REM. Resume queue on Wake Signal. No preemption. | Prevents cascade from REM timing. Medium risk. Deploy after Minimal is stable. |
| Proactive Orchestrator | All five signals | Full fleet health model. Preemptive intervention. Rebalancing. Orchestrator REM. Fleet circuit breaker. | Maximum resilience. High complexity. Deploy only after Reactive is stable in production. |

### 7.3 Orchestrator Fleet Health Model (Logical)

The orchestrator maintains a logical view of fleet health -- the fatigue state of all active sub-agents. This is a data model requirement, not a UI requirement. The orchestrator's decision-making depends on:

- Current fatigue score per sub-agent and which dimension is dominant.
- How many sub-agents are simultaneously in PRE-SLEEP or above.
- Estimated time to REM completion for agents currently consolidating.
- Which parent tasks are blocked waiting for sub-agent results.

With this data, the Reactive Orchestrator holds dependent tasks during sub-agent REM and resumes on Wake Signal. The Proactive Orchestrator additionally preempts agents approaching PRE-SLEEP, reschedules new sub-task spawning when fleet fatigue is high, and rebalances sub-tasks from high-fatigue to lower-fatigue agents.

### 7.4 The Orchestrator's Own Fatigue

The orchestrator is itself subject to memory pressure. In large fleets (10+ sub-agents, long task horizons), the orchestrator's context fills with status signals and merge results. Mnemos addresses this with OrchestratorNodes -- compressed, role-aligned memory nodes containing only fleet-level state, not sub-task details. Sub-agent result details are held in ResultNodes tagged to the relevant sub-task scope and not loaded into orchestrator active context unless directly needed.

When orchestrator fatigue reaches PRE-SLEEP, it writes a fleet checkpoint -- a serialized snapshot of the full task graph state and each sub-agent's last reported fatigue score -- before spawning its own REM Process. If orchestrator reaches EMERGENCY, all sub-agents receive Abort + Handoff. The entire fleet can be resumed from checkpoints. Orchestrator self-fatigue monitoring is a Proactive Orchestrator feature and should not be implemented until the Reactive tier is stable.

## 8. Sub-Agent Memory Inheritance and Merge Algebra

When a task delegates to a sub-agent, the parent produces a typed memory slice. The sub-agent executes with its own MnemoGraph seeded from this slice. On completion, results are merged back using the formal merge algebra. Inheritance size is a fatigue management decision, not just a correctness decision -- a sub-agent given the full parent ContextNode set may start at fatigue 0.3 before executing a single step. Scope-filtered ContextNode inheritance is the most important fatigue control in the protocol.

### 8.1 Inheritance Rules by Node Type

| Node Type | Sub-Agent Receives | Merge-Back Rule |
| --- | --- | --- |
| GoalNode | Read-only slice (sub-goal only) | Returned as ResultNode, merged per merge algebra |
| ConstraintNode | Full copy -- always | Violations surface as CONSTRAINT_CONFLICT ConflictRecord. Never overwritten. |
| ContextNode | Scope-filtered slice only | Parent retains full. Child receives only nodes tagged to sub-task scope_tags. Critical fatigue control -- prevents sub-agent starting at elevated pressure. |
| WorkingNode | None | Sub-agent starts with empty WorkingNode -- its own reasoning space. |
| SkillNode | Full copy | Child can extend. Merged back per SkillNode merge algebra on completion. |
| CheckpointNode | None | Sub-agent manages its own checkpoints independently. |

### 8.2 Formal Merge Algebra

One absolute rule governs all merge decisions: ConstraintNodes are never overwritten by ResultNodes regardless of confidence. A result that contradicts a constraint is a conflict requiring human or orchestrator resolution -- not an automatic update.

| Conflict Type | Condition | Resolution |
| --- | --- | --- |
| Result vs ContextNode (fact) | sub_confidence > parent_confidence | Sub-agent result wins. Parent ContextNode updated. Provenance logged. |
| Result vs ContextNode (fact) | sub_confidence <= parent_confidence | Parent wins. Sub-agent result logged as low-confidence alternative. No update. |
| Result vs ConstraintNode | Any | CONSTRAINT_CONFLICT. Never overwrite. Escalate. Halt merge for this node pair. Absolute rule. |
| SkillNode vs SkillNode (same fingerprint) | sub_confidence > parent AND sub_exec_count > parent | Merge: update with higher confidence and combined execution count. |
| SkillNode vs SkillNode (same fingerprint) | Either condition fails | Retain both. Tag sub version CANDIDATE. Promote after next successful parent execution. |
| Partial ResultNode | outcome = PARTIAL | Merge partial facts individually. Log missing fields as PARTIAL_RESULT. Do not mark sub-task complete. |
| Result vs GoalNode | Any | Read-only comparison only. GoalNode never modified. Goal drift -> GOAL_DRIFT ConflictRecord. |

After merge-back, the parent recalculates its fatigue score -- merging sub-agent results increases parent context and may trigger COMPRESS. The delegation event is logged as a ResultNode in the parent's graph: what was delegated, what was returned, what conflicts were detected.

## 9. The HandoffNode: Lifecycle Completion

When a task completes, the REM Process produces a HandoffNode -- the artifact that crosses from task-local memory into Engram's long-term store. The HandoffNode is a curated, typed distillation -- not a full MnemoGraph dump -- structured for maximum Engram encoding fidelity.

### 9.1 HandoffNode Schema

```
HandoffNode {
  task_id:                UUID
  task_goal:              string               // from root GoalNode
  outcome:                COMPLETED | PARTIAL | FAILED
  completion_time:        Timestamp

  // Knowledge to persist
  key_results:            ResultNode[]         // high activation_weight, high confidence
  promoted_skills:        SkillNode[]          // newly promoted during this task
  active_constraints:     ConstraintNode[]     // constraints relevant beyond this task
  open_questions:         string[]             // unresolved items for next session

  // Provenance
  sub_agent_results:      ResultNode[]         // what sub-agents returned
  conflicts_detected:     ConflictRecord[]     // merge conflicts surfaced

  // Fleet diagnostic meta -- enables post-task health analysis
  peak_fatigue_score:     Float                // how hard was this task?
  rem_cycles:             Int                  // how many consolidations were needed
  checkpoint_count:       Int                  // how many checkpoints were written
  emergency_halts:        Int                  // should be zero in healthy execution
  sub_agent_count:        Int
  sub_agent_peak_fatigue: Map<AgentId, Float>  // per-agent peak pressure
}
```

### 9.2 Resumption Protocol

If a task must restart -- after emergency halt, process interruption, or deliberate pause -- Mnemos loads the most recent CheckpointNode rather than starting from scratch. The CheckpointNode contains: the complete MnemoGraph state at checkpoint time (node status, weights, links), the active sub-task, the fatigue score at checkpoint time, and a human-readable summary of progress for operator visibility.

Resumption costs a fraction of full-reload because GoalNodes, ConstraintNodes, and SkillNodes do not need to be re-derived. The REM Process reconstructs Wake State from the checkpoint and the agent continues from the last known sub-goal. The emergency_halts field in the HandoffNode records whether clean resumption was achieved -- a non-zero value flags a task that required emergency recovery.

## 10. Validation Plan

Eight experiments sequenced by dependency. Experiment G runs first and gates all others. If Tier 0 does not demonstrate meaningful improvement in task completion rate, all other experiments should be paused until the root cause is understood.

| Exp | Name | Method and Success Threshold |
| --- | --- | --- |
| G | MVM Tier 0 Baseline (run first) | Deploy Tier 0 (token utilization + checkpoint/resume) on 20 real Claude Code long-session tasks. Measure task completion rate vs no-Mnemos baseline. Token overhead of checkpoint writing. Success: > 30% improvement in task completion for sessions > 50 sub-goals. If this fails, pause all other experiments and diagnose before proceeding. |
| A | Fatigue Correlation + Weight Calibration | Label 20+ Claude Code sessions that hit context limits. Plot fatigue trace over session lifetime -- show score rising before failure. Compare four-dimension composite vs token-utilization-only baseline. Produce data-driven weights. Success: AUC > 0.70, fatigue > 0.60 in the 10 sub-goals preceding failure. |
| B | WorkingNode Tier Fidelity | Compare SkillNode quality when WorkingNodes sourced from (a) thinking blocks, (b) action-sequence inference, (c) output structure. Measure promotion precision and recall at each tier. Success: Tier 1 action-sequence achieves > 80% of Tier 2 quality -- confirming it as viable primary path. |
| C | Tiered Consolidation Break-Even | Measure actual token cost and context tokens recovered for Micro, Mini-REM, and Full REM across 50+ consolidation events. Identify task types where Mini-REM is sufficient. Success: break-even confirmed for Mini-REM at > 20 sub-goals, Full REM at > 30. |
| D | Fingerprint Collision Rate | Run SkillNode promotion on 50+ sessions. Human-label true vs false equivalences. Measure precision/recall at similarity threshold 0.82. Produce per-domain calibration. Success: false positive rate < 10% at default threshold. |
| E | Merge Conflict Detection | Inject synthetic ConflictRecords into controlled sub-agent sessions. Measure detection rate. Success: detection rate > 90%, zero silent constraint overwrites. |
| F | Node Extraction Reliability | Run extraction pipeline on 100 session logs. Human-annotate 200 sampled nodes. Measure extraction precision per node type. Success: GoalNode > 0.95, ConstraintNode > 0.85, ResultNode > 0.85, WorkingNode Tier 1 > 0.70. |
| H | Orchestrator Preemption Effectiveness | Compare long multi-agent Claude Code tasks with Minimal vs Reactive vs Proactive orchestrator. Measure EMERGENCY halt rate and task completion rate. Success: EMERGENCY halts > 50% lower with Reactive vs Minimal. Run only after Reactive is stable in production. |

### 10.1 Experiment Sequencing

- **G first** -- MVM Tier 0 baseline. Gate: > 30% task completion improvement before proceeding.
- **A** -- fatigue correlation and weight calibration. Produces data-driven weights for all subsequent experiments.
- **B** -- WorkingNode tier fidelity. Confirms action-sequence as viable primary path.
- **C** -- tiered consolidation break-even. Calibrates tier selection thresholds.
- **D** -- fingerprint collision rate. Produces per-domain threshold calibration.
- **E** -- merge conflict detection. Hard requirement: zero silent constraint overwrites.
- **F** -- node extraction reliability. Validates foundation of extraction pipeline.
- **H** -- orchestrator preemption effectiveness. Requires Reactive Orchestrator stable in production.

### 10.2 Honest Limitations

- Claude Code session logs are one agent type. Results may not generalize to Maia (multi-channel) or hive (multi-source) without re-calibration.
- WorkingNode extraction quality depends on thinking block availability. Results on sessions without thinking blocks may underestimate Tier 1 fidelity.
- Mini-REM async safety requires formal consistency analysis before production deployment.
- Experiment H (orchestrator preemption) requires a controlled multi-agent harness that is harder to isolate in production logs than single-agent experiments.
- SkillNode promotion threshold (3+ occurrences) is proposed, not validated. Experiments B and D together produce a concrete, empirically grounded value.
- HandoffNode minimum fidelity requires joint design with the Engram team -- the answer depends on what Engram can usefully encode.

## 11. Open Questions

| Q | Question | Discussion |
| --- | --- | --- |
| Q1 | WorkingNode Tier Fidelity | How much signal is lost when WorkingNodes are inferred from action sequences vs thinking blocks? Hypothesis: sufficient for SkillNode fingerprinting (structural patterns) but insufficient for high-fidelity task graph reconstruction. Experiment B measures this. |
| Q2 | Fatigue Weight Calibration | Proposed weights (0.40, 0.25, 0.20, 0.15) are not empirically validated. Experiment A produces data-driven values. Are weights task-type-specific? Is token utilization genuinely dominant in practice? |
| Q3 | Mini-REM Async Safety | Can a fatigued agent safely continue on a checkpoint while Mini-REM consolidates the older graph? New tool call outputs must be buffered and integrated post-REM. Requires formal analysis of consistency hazards before async deployment in production. |
| Q4 | Fingerprint Collision Rate | At cosine threshold 0.82, what is the false positive rate for SkillNode equivalence? Expected higher in repetitive domains. Experiment D produces per-domain calibration data. Threshold is configurable per scope_tag domain. |
| Q5 | TCS Tag Coverage Threshold | TCS fallback triggers at < 40% scope_tag coverage. Is this the right threshold? In narrow-scope tasks (single-file refactoring), natural coverage may be low without indicating poor tagging. Requires domain-specific calibration. |
| Q6 | SkillNode Promotion Threshold | How many equivalent executions before promotion? 3+ is proposed. Too low = noise. Too high = chunking dividend never realized. Experiments B and D together produce this number. |
| Q7 | Merge Algebra at Depth > 2 | At sub-agent nesting depth 3+, do ConflictRecords cascade upward automatically, or does each level re-run the algebra independently? Conflict amplification at deep nesting is a risk. Formal analysis required before deep nesting is deployed. |
| Q8 | Orchestrator Self-Fatigue | The orchestrator is itself subject to fatigue. Fleet checkpoint at orchestrator PRE-SLEEP is specified. Who monitors the orchestrator? A dedicated lightweight monitoring sidecar may be needed for large fleets. |
| Q9 | HandoffNode Minimum Fidelity | What is the minimum viable HandoffNode that allows meaningful session resumption via Engram? Full MnemoGraph dump is too expensive. Minimal summary loses signal. Joint design with Engram team required -- the answer depends on what Engram can usefully encode. |
| Q10 | MVM Upgrade Path Compatibility | Does a Tier 0 deployment accumulate technical debt that makes upgrading to Tier 1 or Full Mnemos harder? Specifically: CheckpointNode format compatibility across tiers. Requires specification of a stable CheckpointNode schema that works at all tiers. |

## 12. What Would Need to Happen

- Deploy MVM Tier 0. Intercept context_remaining from Claude Code hooks. Write CheckpointNode at 0.60. Resume from checkpoint. Measure task completion rate (Experiment G). This is the gate -- everything else follows from here.
- Build extraction pipeline sidecar. Tool call taxonomy for node typing. Action-sequence WorkingNode inference across all three tiers. TCS computation with fallback hierarchy. Activation weight computation. Run Experiments A and F.
- Implement tiered consolidation. Micro-Consolidation in-context. Mini-REM as external process with async option and consistency buffer. Full REM as external process. Validate break-even (Experiment C).
- Implement three-signal SkillNode fingerprinting and promotion algebra. Run Experiments B and D for fidelity and collision calibration. Produce the promotion threshold empirically.
- Implement formal merge algebra. Run Experiment E. Zero silent constraint overwrites is a hard requirement before any multi-agent deployment.
- Deploy Minimal Orchestrator (Fatigue Report logging only). Add Reactive Orchestrator after Minimal is stable. Run Experiment H only after Reactive is stable in production. Only then evaluate Proactive Orchestrator.
- Implement HandoffNode schema and Engram encoding adapter. Joint design session with Engram team to specify minimum fidelity requirements and stable CheckpointNode format across all MVM tiers.

> Agents crash not because the context window is too small, but because there is no architecture for managing what lives in it. REM does not run inside a fatigued agent -- it runs in a fresh process while the agent waits. You do not need the full system to start. MVM Tier 0 takes six hours and prevents the most common failure mode. Here is the complete architecture. Here is the evidence.

---

*Mnemos RFC -- Concept Stage -- Companion to Engram v3 and iCPG v8 -- Primary validation: Claude Code long-session multi-agent task hierarchies -- Start with MVM Tier 0*
