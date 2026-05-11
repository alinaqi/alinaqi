# iCPG - Intent-Augmented Code Property Graph


*Reason Graph for Agentic Software Development*

> An infrastructure layer for agentic coding systems -- Claude Code, Lovable, Bolt, and similar -- that makes intent formally specifiable, specification drift continuously detectable, and agent accountability traceable. Grounded in a live legacy migration (zenloop v1 to v2). Validation in progress.

## Version History

| Version | Date | Changes |
| --- | --- | --- |
| v1.0 | March 2026 | Initial RFC. Intent Graph as 4th CPG layer. Single drift score. IntentNode as primary primitive. |
| v2.0 | March 2026 | Reframed as Reason Graph. Drift decomposed into 6 measurable dimensions. Agent-as-primary-user shift. Three canonical pre-task agent queries defined. Asana repositioned as input signal, not backbone. AST/CFG/PDG analogy softened. |
| v3.0 | March 2026 | Added Prior Research section. Maps iCPG against 30+ years of Design Rationale literature: Burge et al. (2008), SEURAT, Kantara (2023), ADRs, Microsoft RiSE. Analyses applicability of each to agentic development. Clarifies novel contribution. Renamed to ReasonNode throughout. |
| v4.0 | March 2026 | Added formal validation research: Design by Contract (Meyer 1986), ABC, AgentSpec (ICSE 2026), Agent-C. ReasonNode now carries preconditions, postconditions, and invariants as structured predicates. Drift defined as predicate failure (requires empirical validation of predicate quality). Three-layer enforcement stack introduced. Two new open questions (Q9, Q10). |
| v5.0 | March 2026 | Added Section 0: Central Thesis. Formally reframes hallucination as specification drift -- the observable output of an agent operating outside its formal decision plane. Introduces the hallucination-to-drift equivalence table. Extends scope to all agentic systems beyond software development. |
| v6.0 | March 2026 | Updated all 10 open questions to reflect paper's current maturity. Questions now address: strength of hallucination-drift equivalence, contract inference vs authoring, dynamic language tractability, multi-agent conflicts, duplication threshold calibration, performance envelope, bootstrap accuracy validation, non-software generalisation, portfolio-scale storage, and research vs engineering sequencing. |
| v7.0 | March 2026 | Revised following peer review. Removed absolute hallucination-drift equivalence claim. Reframed as bounded hypothesis about a class of software agent failures. Cut non-software agent scope. Added Section 11: Evaluation Plan with four concrete experiments using zenloop migration dataset. Fixed product-copy language. Narrowed to software development domain. |
| v8.0 | March 2026 | Second peer-review revision. Removed all 22-company dataset references (not yet available). Reframed 0.93 recall as fragile enabling assumption. Softened formally-defined drift language throughout -- predicate quality is an open problem, not a solved one. Fixed open question residue referencing removed sections. Corrected version count. Scoped Q8 to software-development failure modes iCPG does not address. |

## 0. Motivation: Specification Drift in Agentic Software Development

This paper is grounded in a specific, bounded problem: the failure modes that emerge when AI coding agents -- Claude Code, Lovable, Bolt, Antigravity, and similar systems -- operate on real codebases over time. We focus deliberately on software development. Generalisation to other agent domains is future work and not claimed here.

We are writing this in the context of two immediate, concrete needs: managing a live legacy migration (zenloop v1 to v2) and governing agentic development workflows across multiple production SaaS products. The theory is motivated by practice, and the planned validation is empirical.

### 0.1 A More Precise Framing of Agent Failure

The industry uses the term *hallucination* loosely to describe a wide range of agent failures. We want to be careful here. Not all agent failures are the same thing, and we do not claim they are. Model incapacity, missing world knowledge, tool unreliability, stochastic error, and reward misspecification are all distinct failure modes with different causes.

What we do claim is that a significant and practically important class of agent failures in software development -- going off-track, producing irrelevant code, violating architectural constraints, duplicating existing work -- shares a common structure: the agent's effective context has diverged from the original intent specification to the point where its outputs are no longer grounded in the actual problem. We call this **specification drift**, and we propose it as a more operationally useful frame than *hallucination* for this class of failures.

This is a hypothesis, not a demonstrated result. The paper proposes a framework and a validation plan. We explicitly do not claim specification drift explains all agent failures -- only that it explains a meaningful subset, and that subset is currently addressed by no existing tooling.

| Failure pattern in coding agents | Specification drift interpretation |
| --- | --- |
| Agent introduces unrelated changes | Context has drifted: agent is solving a related but different problem than specified in the ReasonNode |
| Agent duplicates existing capability | No intent graph query before acting: agent is unaware prior work satisfies the same specification |
| Agent violates module boundary | Invariant missing or unchecked: no formal constraint prevented the boundary crossing |
| Agent's code passes tests but breaks things | Postcondition too narrow: tests verify a subset of the intent, not the full ReasonNode contract |
| Context rot across long sessions | Progressive drift: accumulated context diverges from original intent specification without detection |
| Vibe coding failure | Absent specification: no ReasonNode exists, nothing to drift from -- all outputs are ungrounded by design |

### 0.2 Why Existing Tooling Does Not Address This

The current response to agent failures in software development falls into two categories: better models (higher capability, larger context windows, improved instruction following) and better prompting (CLAUDE.md files, AGENTS.md, structured task specifications). These approaches reduce drift probability but do not detect or prevent it structurally.

What is missing is infrastructure that: (1) formally captures intent before execution, (2) monitors whether execution stays within the specification, and (3) detects drift early enough to intervene. This is the gap iCPG addresses -- not by replacing models or prompting, but by adding a persistent, queryable specification layer that agents check against before and during execution.

> **The iCPG hypothesis (to be validated):**
> A significant class of failures in agentic software development share the structure of specification drift: the agent's context has diverged from the original intent specification, undetected. An intent graph that makes specification explicit, links it to code structure, and monitors invariants continuously can reduce this class of failures -- in ways that better models or better prompting alone cannot.

### 0.3 Immediate Practical Context

This is not a purely theoretical exercise. iCPG emerged from two concrete operational problems:

- A live legacy migration: zenloop v1 to v2, where the central challenge is knowing whether v2 correctly preserves the business capabilities and edge-case handling of v1 -- information that exists in no structured form in the Git history.
- Portfolio-wide agentic governance: managing Claude Code sessions across multiple production SaaS products, where agents write significant portions of code with no shared memory of past decisions, constraints, or failures.

The validation plan in Section 11 uses the zenloop migration as the primary empirical dataset. This is intentional: the theory should earn its claims through the migration, not through abstract argument.

## 1. The Problem: Agentic Software Development Has No Specification Layer

In 100% agentic development, the human operates at the goal and constraint level. Agents operate at the implementation level. That means the most valuable artefact is no longer just the code diff -- it is the mapping between: requested intent, implemented change, observed outcome, and retained system constraints.

Git tracks *what* changed and *who* changed it. It does not track *why*. In a human-centric world that was tolerable -- you could ask the developer. In an agentic world, agents have no persistent memory between sessions, no knowledge of decisions made months ago, and no accountability for whether their changes preserved the system's original design intent.

This produces five failure modes that are now observable across production codebases:

- **Redundant work** -- multiple agents solve the same problem differently, producing PR spam and wasted compute
- **Silent constraint violations** -- agents optimise for local specs but break auth boundaries, performance assumptions, or business invariants
- **Drift explosion** -- each iteration slightly shifts behaviour with no tracking of alignment to original goals
- **Loss of institutional memory** -- agents don't know about past failures, edge cases, or weird production realities
- **Overfitting to specs** -- agents satisfy tests but miss implicit expectations and real-world usage patterns

> *"Please do not use clankers to add more noise to PRs. We're working on a solution to this, and this is making my job harder."*
> -- Peter Steinberger, creator of OpenClaw, after receiving 6 duplicate PRs with identical intent

It is not just one project. Redox OS, Gentoo, NetBSD, QEMU, Ghostty, tldraw, LLVM, and curl have all banned or restricted AI contributions -- not because AI is bad, but because the infrastructure to make AI contributions legible, accountable, and non-duplicative does not exist.

| Metric | Value |
| --- | --- |
| **75%** | of tech decision-makers face severe AI-generated technical debt by 2026 |
| **8x** | increase in duplicated code blocks in repos with high AI adoption (GitClear 2025) |
| **29%** | of developers trust AI output -- down from 40% -- despite 84% adoption |

## 2. The Core Reframe: Agents Are the Primary User

The v1 framing treated iCPG as documentation infrastructure -- humans use the graph to understand what agents did. That was wrong.

The correct framing: in a 100% agentic development workflow, the primary consumer of the graph is not a developer reading context. It is an agent deciding what to do next. This changes the design target completely.

| Old framing (documentation) | New framing (agent memory) |
| --- | --- |
| Humans use the graph to understand code | Agents query the graph to decide what to do next |
| Nice dashboards, visual graphs, human-readable summaries | Structured queries, machine-readable signals, decision APIs |
| Passive analysis layer | Active guardrail and coordination system |

### 2.1 The Three Canonical Pre-Task Agent Queries

We propose that before an agent writes a single line of code, it should run three queries against the iCPG system. The hypothesis -- to be validated empirically -- is that these queries structurally address the majority of specification drift failure modes identified in Section 1:

| Query | What it gives the agent |
| --- | --- |
| `search_prior_work(intent: string)` | Has this problem been attempted before? What was the outcome? Surfaces prior tasks, their results, and what code they touched. |
| `get_constraints(scope: string[])` | What invariants, boundaries, and rules apply to the modules I am about to touch? Prevents silent constraint violations. |
| `get_risk_profile(symbols: string[])` | What is the drift score, ownership history, and incident history of the code I am about to modify? Flags fragile areas before changes are made. |

## 3. Prior Research: Standing on 30 Years of Design Rationale

Capturing the "why" behind software decisions is not a new problem. The field of Design Rationale has studied this since the early 1990s. iCPG draws on this body of work -- and departs from it in specific, important ways. Understanding both the inheritance and the gap is essential for positioning iCPG's novel contribution.

### 3.1 The Foundational Text: Rationale-Based Software Engineering

Burge, Carroll, McCall and Mistrik (2008), *Rationale-Based Software Engineering* (Springer) -- the field's canonical reference, covering rationale capture and use across the full software lifecycle from requirements through maintenance and reuse.

| | Burge et al. (2008) | iCPG |
| --- | --- | --- |
| Primary user | Human developers reading documentation | AI agents querying at runtime |
| Capture method | Manual -- developer writes rationale records | Automatic -- byproduct of agent execution |
| Storage | Text docs, wikis, specialised tools (Compendium, DRed) | Graph database fused with CPG |
| Linked to code? | Loose references at best | Direct symbol-level edges (CREATES, MODIFIES) |
| Drift tracking | Not addressed | Six-dimension drift model with continuous monitoring |
| Agentic use | Not designed for it | Primary design target |

**Applicability to agentic development: Low as a direct system.** The conceptual framework is foundational -- the problem statement, the terminology, the insight that rationale is distinct from documentation -- all carry over. But the entire capture model assumes a human authoring rationale records. In a 100% agentic workflow, the capture must be automatic. Burge et al. identified this as the field's core unsolved problem in 2008: the cost of capture consistently outweighs the perceived benefit for human developers. Agents eliminate that cost -- but the field hasn't caught up to that yet.

### 3.2 SEURAT -- Software Engineering Using RATionale

Burge and Brown (2004-2007). The most technically sophisticated prior system. SEURAT uses an argument ontology and inference engine to evaluate decision alternatives and perform impact assessment when requirements or assumptions change.

- Maintains a rationale graph with decisions, alternatives, criteria, and argument relationships
- Can detect inconsistencies in reasoning -- when two decisions contradict each other
- Performs impact assessment: "if this assumption changes, which decisions are affected?"
- Integrated with development environments (Eclipse plugin)

**Applicability to agentic development: Medium** -- the impact assessment capability is directly relevant and maps to our blast radius + constraint checking use case. The inconsistency detection maps to our DUPLICATES edge. The core limitation: SEURAT's rationale graph is disconnected from the code structure graph. It knows about design decisions but not about the functions that implement them. iCPG's key innovation is fusing SEURAT-style rationale with CPG-style code structure into a single queryable graph. An agent can traverse from a production incident back through the code symbol, to the ReasonNode that created it, to the constraints it was supposed to preserve -- in one graph query. SEURAT requires separate lookups across disconnected systems.

### 3.3 Kantara -- Automated Rationale Reconstruction from Commits

Dhaouadi, Oakes and Famelis (2022-2025, ASE, MODELS). The most directly relevant recent research. Kantara is an end-to-end pipeline that uses NLP and LLMs to extract rationale from commit messages, structure it in a knowledge graph, and detect conflicting decisions.

- Extracts decision-rationale pairs from commit messages automatically -- no human annotation required
- Structures output as an ontology-based knowledge graph queryable for similar and contradictory decisions
- Validated on the Linux kernel OOM-Killer component -- a real, large, long-lived codebase
- Shows ~40-50% of commit message sentences contain rationale information
- Multi-classification with XGBoost shows promising extraction accuracy

Kantara is the closest prior work to iCPG's bootstrapping step. The intent inference pass described in Section 6.2 is essentially a Kantara-style pipeline applied to Git history. iCPG inherits their core technique and should be validated against their benchmark results on the Linux kernel dataset.

**Applicability to agentic development: High for bootstrapping -- low as a runtime system.** Kantara produces reports and visualisations for human developers to read. It is not designed as a machine-queryable API that agents call before acting. It also does not link reconstructed rationale to CPG nodes -- the knowledge graph is rationale-only, not fused with code structure. The gap iCPG fills: making the Kantara-style graph queryable by agents at runtime, fusing it with the code property graph, and extending it with the six-dimension drift model and ownership attestation.

### 3.4 Architecture Decision Records (ADRs)

Michael Nygard (2011, blog post) popularised ADRs. Now a standard practice recommended by AWS, Azure, GOV.UK, and most major engineering organisations. An ADR captures a single architectural decision -- context, options considered, decision made, consequences.

- Practitioner-level standard -- widely adopted, low tooling overhead
- Append-only log: when a decision is superseded, a new ADR is written, not the old one edited
- AWS describes ADRs as the primary mechanism for understanding why architectural choices were made
- GOV.UK has mandated ADR frameworks across the UK public sector

**Applicability to agentic development: Low as implemented -- high as inspiration.** ADRs as markdown files in a `/docs/adr/` folder are completely inaccessible to agents. They're not machine-readable, not linked to code symbols, not version-tracked at the function level, and not queryable. But the ADR mental model -- every significant decision gets a record with context, options, rationale, and consequences -- is exactly what iCPG's ReasonNode formalises as a first-class graph primitive. iCPG is what ADRs would look like if they were designed for agents instead of humans. An important practical note: teams already writing ADRs have the highest-quality input signal for bootstrapping iCPG -- the LLM inference pass over ADR history would produce significantly more accurate ReasonNodes than commit message inference alone.

### 3.5 Microsoft Research RiSE -- Intent Inference

Microsoft Research's Software Engineering group (RiSE) is actively working on inferring intent from natural artefacts -- documentation, code, runtime traces -- to accelerate formal verification of software configurations. They describe the goal as: "future programming systems will allow humans to specify their intent naturally, with the computer distilling this intent interactively into a precise formal specification."

- Applying intent inference to Azure configuration verification -- 5 billion calls per day at production scale
- Using NLP and formal methods in combination to extract specifications from natural language
- Specifically addresses the gap between what developers say and what the system does

**Applicability to agentic development: High directional signal -- different application domain.** Microsoft is solving intent inference for configuration verification. iCPG is solving it for general software development with agent coordination. The techniques (NLP over artefacts, formal representation of inferred intent) are shared. The application is different: Microsoft's system produces specifications for verification tools to check. iCPG produces ReasonNodes for agents to query before acting. The existence of this work at Microsoft scale validates that automated intent inference from software artefacts is tractable -- and that the problem is worth serious engineering investment.

### 3.6 Formal Validation Research: Design by Contract and Agent Behavioral Contracts

The prior sections show that design rationale has been studied for decades -- but always as a documentation artefact for humans to read. The question your question surfaces is sharper: can rationale be expressed as computable predicates that agents can verify mechanically? This is where formal methods research becomes directly applicable.

**Design by Contract -- Bertrand Meyer (1986)**

The foundational answer to making intent formally verifiable. Meyer's Design by Contract (DbC) prescribes that every software component should carry a formal contract with three elements:

- **Precondition** -- what must be true before a function is called (obligation on the caller)
- **Postcondition** -- what the function guarantees on exit (obligation on the supplier)
- **Invariant** -- what must remain true about a class throughout its lifetime, regardless of which method executes

The critical property: a precondition violation is the caller's bug. A postcondition or invariant violation is the supplier's bug. This makes accountability computable -- you always know whose contract was broken.

**Connection to iCPG:** A ReasonNode's `stated_purpose` is currently natural language -- readable by humans but not verifiable by machines. DbC gives us the vocabulary to make it formal. Each ReasonNode can carry a contract layer alongside its prose description:

| Contract element | What it means for a ReasonNode |
| --- | --- |
| Precondition | What must be true in the codebase before this intent can execute. Example: `payment_service.has_network_calls == true AND retry_logic_exists == false` |
| Postcondition | What must be true when the intent is fulfilled. Example: `max_retries >= 3 AND existing_tests_pass == true AND no_new_external_dependencies == true` |
| Invariant | What must remain true throughout and after -- the constraints the change must never violate. Example: `auth_boundary.unchanged == true AND payment_timeout_ms <= 5000` |

This is how drift gets a formal definition. A symbol has drifted when its current behaviour no longer satisfies the postconditions of the ReasonNode that created it, OR when a codebase invariant specified in that ReasonNode is no longer held. Drift is not a vague feeling -- it is a predicate failure, computable once correct predicates exist. Obtaining correct predicates is the hard problem.

**Agent Behavioral Contracts -- ABC, AgentSpec, Agent-C (2025-2026)**

A cluster of very recent work applies formal contract theory directly to LLM agent sessions -- exactly the context iCPG operates in.

**ABC (Agent Behavioral Contracts, 2025):** Generalises Design by Contract from individual function calls to autonomous agent sessions. Introduces invariants that must hold across an entire agent session, governance constraints over actions, recovery mechanisms for soft violations, and a compositionality theorem for multi-agent chains. Critically, ABC integrates behavioral drift detection as a leading indicator -- enabling preemptive intervention before a constraint is fully violated.

**AgentSpec (ICSE 2026):** A rule-based DSL for specifying safety properties of LLM agents at runtime. Agents can recover from violations by reflecting on their intentions or re-deriving subgoals. Supports both preventive (before action) and corrective (after violation) enforcement.

**Agent-C (2025):** Introduces a DSL for temporal properties, translates specs to first-order logic, and uses SMT solving interleaved with constrained token generation. Achieves 100% constraint conformance in evaluation on real-world retail and airline reservation agents.

**Connection to iCPG:** These systems provide the enforcement layer that iCPG's constraint model implies but does not yet specify. The architecture becomes a three-layer stack:

- **Layer 1 -- DbC at the symbol level:** every function carries preconditions, postconditions, and invariants derived from the CPG
- **Layer 2 -- ReasonNode contracts:** each ReasonNode carries a formal contract that an agent must satisfy to mark an intent as fulfilled
- **Layer 3 -- Agent session contracts (ABC/AgentSpec):** when an agent starts a task, it receives a session contract derived from the ReasonNode and relevant invariants -- governing its entire execution, not just individual calls

**LLM-based Specification Inference (2025):** Parallel research reports recall up to 0.93 for precondition/postcondition inference in specific benchmark settings. We treat this as a promising but fragile enabling assumption: those benchmarks involve cleaner task formulations than the messy historical rationale inference iCPG requires. Whether this precision transfers to real-world ReasonNode contract inference is itself an open empirical question -- and one of the central risks to the architecture.

The practical implication, if contract inference achieves sufficient quality: an agent receiving a ReasonNode could check preconditions before acting, verify postconditions after completing, and monitor invariants continuously. Whether this is achievable at useful precision on real codebases is the central empirical question this RFC proposes to answer -- not a solved problem.

### 3.7 Summary: What iCPG Inherits and What is Novel

| Prior work | What iCPG inherits | The gap iCPG fills |
| --- | --- | --- |
| Burge et al. | Problem framing, rationale taxonomy, lifecycle coverage | Automatic capture, agent-first design, CPG fusion |
| SEURAT | Impact assessment, inconsistency detection, argument ontology | Code-level linkage, runtime queryability, no manual entry |
| Kantara | LLM-based rationale extraction from commits, knowledge graph structure | Agent API, CPG fusion, drift model, runtime use |
| ADRs | ReasonNode mental model, append-only decision log, context-options-consequences structure | Machine-readable, symbol-linked, agent-queryable, automated |
| Microsoft RiSE | Intent inference from artefacts is tractable at scale | Software dev context, agent coordination, not just verification |
| DbC / Meyer | Precondition-postcondition-invariant contract vocabulary for making intent formally verifiable | Applied to ReasonNodes not just functions; formal definition of drift as invariant failure |
| ABC / AgentSpec / Agent-C | Session-level behavioral contracts for LLM agents, runtime enforcement, drift as leading indicator | Linked to code graph, derived from ReasonNodes, cross-session memory, software dev context |

The novel contribution of iCPG is not any single element -- each piece has prior art. The novelty is the combination: fusing a rationale/reason layer with a Code Property Graph into a single queryable structure where agents are the primary runtime consumer, with automatic capture, formally verifiable ReasonNode contracts (preconditions, postconditions, invariants), a formal definition of drift as contract failure, six-dimension drift detection, and ownership attestation. No prior work combines all of these.

## 4. Foundation: What CPG Already Gives Us

The Code Property Graph (CPG), introduced by Yamaguchi et al. at the University of Gottingen in 2013, unifies three program representations into a single queryable graph. iCPG adds a fourth layer -- but with an important caveat noted below.

| Layer | Abbreviation | Description |
| --- | --- | --- |
| Layer 1 | **AST** | Abstract Syntax Tree. Syntactic structure -- functions, classes, arguments. Answers: *what is this code shaped like?* |
| Layer 2 | **CFG** | Control Flow Graph. Execution paths -- branches, loops, conditionals. Answers: *how can execution flow through this code?* |
| Layer 3 | **PDG** | Program Dependence Graph. Data and control dependencies. Answers: *what depends on what, and how does data flow?* |
| Layer 4 (NEW) | **IG** | Intent Graph / Reason Graph. The *why* layer -- why this code exists, who owns it, what decision created it, what constraints it must preserve. Answers: *what was this code trying to do, and is it still doing it?* |

**Important:** AST, CFG, and PDG are formally derivable from code -- you can compute them. Intent and reasons cannot be computed from code alone. At best you infer weak proxies from tickets, docs, commit messages, test names, and ownership metadata. The IG layer requires explicit capture, not just computation. This is a fundamental difference from the other three layers.

## 5. The iCPG Data Model

iCPG = AST + CFG + PDG + Reason Graph (RG). The v1 RFC called this the Intent Graph (IG). Based on feedback, Reason Graph is more precise -- it captures what is actually tractable: stated purpose, decision history, constraints, evidence of conformance, and drift indicators.

### 5.1 What the Reason Layer Captures

"Intent" is not a single thing. A function might exist because: the business wanted enterprise SSO, the architect chose a specific auth boundary, a developer added a workaround for a legacy customer, an agent refactored it for performance, and a later patch preserved backwards compatibility. All of these are intent -- at different levels. The reason layer captures all of them as separate, linked records:

- Originating task or request
- Architectural decision or rationale
- Invariants and constraints the code must preserve
- Owner or domain steward
- Verification evidence (tests, audits)
- Subsequent modifying decisions

### 5.2 New Node Types

**ReasonNode** (formerly IntentNode)

Each ReasonNode carries both a human-readable description and a machine-verifiable contract layer, drawing directly from Design by Contract principles:

- `id`: UUID
- `stated_purpose`: NaturalLanguageString -- what this was created to do (human-readable)
- `decision_type`: Enum [ business_goal | arch_decision | task | workaround | constraint | patch ]
- `scope`: Set\<SymbolRef\> -- which graph regions are in play
- `owner`: ActorRef -- human or agent accountable for the outcome
- `agent`: AgentIdentity? -- null if human-authored
- `sources`: Set\<SourceRef\> -- Asana task, PR, doc, commit that originated this
- `status`: Enum [ proposed | executing | fulfilled | rejected | drifted ]
- `attestation`: CryptoSignature -- signed by owner at fulfillment

**Formal contract layer (DbC-derived):**

- `preconditions`: Set\<Predicate\> -- what must be true in the codebase before this intent can execute
- `postconditions`: Set\<Predicate\> -- what must be true when the intent is fulfilled (the fulfillment criteria)
- `invariants`: Set\<Predicate\> -- what must remain true throughout and after -- constraints the change must never violate

Predicates are expressed as structured assertions over CPG nodes and edges -- for example: `auth_boundary.symbol_count == PREV(auth_boundary.symbol_count)` (the auth boundary must not grow). These can be hand-authored for high-risk ReasonNodes, or inferred by LLM from `stated_purpose` -- though inference accuracy on real-world rationale is an open question (see Section 12, Q2).

**DriftEvent**

- `id`: UUID
- `symbol`: SymbolRef -- the node that drifted
- `from_reason`: ReasonRef -- the original creating record
- `by_reason`: ReasonRef -- the record that caused the drift
- `drift_dimensions`: Set\<DriftDimension\> -- which dimensions are affected (see Section 5)
- `severity`: Float[0..1] -- 0 = cosmetic, 1 = contract-breaking

### 5.3 Edge Types

| Edge | From | To | Semantics |
| --- | --- | --- | --- |
| CREATES | Reason | Symbol | This decision created this node |
| MODIFIES | Reason | Symbol | This decision changed this node |
| REQUIRES | Reason | Reason | Causal dependency between decisions |
| DUPLICATES | Reason | Reason | Semantic overlap detected between goals |
| VALIDATED_BY | Reason | Test | Evidence that the goal was fulfilled |
| DRIFTS_FROM | Symbol | Reason | Current behaviour no longer matches creating decision |

## 6. Multi-Dimensional Drift Model -- Now Formally Defined

v1 proposed a single drift score. v2 decomposed it into six dimensions. v4 proposes a definition grounded in Design by Contract -- drift as predicate failure. Whether this definition is practically computable depends on the quality of the inferred predicates, which is an open empirical question.

### 6.1 Formal Definition of Drift

**Definition:** A symbol S has drifted from ReasonNode R if and only if:

**(1)** `current_behaviour(S)` does not satisfy `postconditions(R)`

The current behaviour of S no longer satisfies the postconditions specified in the ReasonNode that created it.

**(2)** `current_state(codebase)` does not satisfy `invariants(R)`

A codebase invariant specified in R is no longer held -- a constraint has been silently violated.

This definition is tractable in principle: postconditions and invariants are structured predicates over CPG nodes and edges, and the CPG provides the structural ground truth to evaluate them against. In practice, the hard problem is not writing the definition -- it is obtaining correct, sufficiently complete predicates from messy natural-language rationale and historical artefacts. The formalism is sound; the predicate quality problem is the central open challenge.

This also resolves the accountability question: when drift is detected, the system knows exactly which ReasonNode's invariant was violated and which subsequent ReasonNode caused the violation (via the DRIFTS_FROM edge). Accountability is traced to specific decisions, not vague "code quality decline".

### 6.2 The Six Drift Dimensions

The formal definition covers spec drift and invariant violations. The six dimensions below provide a practical decomposition for monitoring, triage, and reporting:

| Drift Dimension | What it means |
| --- | --- |
| Spec drift | Code behaviour no longer matches stated requirement or spec |
| Decision drift | Code no longer aligns with the architectural rationale that introduced it |
| Ownership drift | No clear steward -- touched by many actors without coherent oversight |
| Test drift | Tests no longer validate the original invariants |
| Usage drift | Code is used in contexts beyond what it was created for |
| Dependency drift | Downstream coupling has changed enough that original assumptions are invalid |

### 6.3 Drift as Active Agent Feedback

In a 100% agentic workflow, drift signals become real-time behavioral constraints on agents -- not post-hoc reports. Drawing on ABC and AgentSpec research, the system can:

- **Block** an agent from modifying a symbol when doing so would violate a ReasonNode invariant
- **Require** an agent to re-evaluate its approach when postcondition checks fail mid-execution
- **Surface** drift warnings before an agent starts a task: "this module has 3 unresolved invariant violations -- proceed with caution"
- **Escalate** to human review when drift severity crosses a configurable threshold

**Example:** A feature was introduced for enterprise custom survey routing. The code now powers several unrelated flows. Tests only cover the original route. The owner has changed twice. Runtime shows new dependencies. Formally: `current_behaviour(survey_router)` does not satisfy `postconditions(R_enterprise_routing)` because the isolation invariant is violated. Drift is not a feeling -- it is a failed predicate.

## 7. Input Sources and Capture Strategy

A key concern with the v1 RFC was manual maintenance burden -- "humans won't document this." In a 100% agentic workflow this concern largely dissolves: agents generate everything (specs, plans, tests, code, PRs, comments), so capture can be automated as a byproduct of normal agent operation.

### 7.1 Input Signals

- **Task management (Asana, Linear, Jira)** -- originating goals, plans, results, comments. Useful as one input signal; noisy and transactional but rich in stated purpose.
- **Git history** -- commit messages, PR descriptions, co-changed symbol clusters. LLM inference pass can reconstruct ~60-75% of intent on repos with good commit hygiene.
- **Agent session outputs** -- specs, plans, test results, code summaries generated by agents during normal operation. Cleanest signal in a fully agentic workflow.
- **Architecture decision records (ADRs)** -- high-level rationale docs. Curated but infrequent.
- **Test names and coverage data** -- implicit intent signals.
- **Incident history and runtime metrics** -- observed behaviour vs declared intent.

On Asana specifically: task systems are noisy, mix administrative chatter with durable rationale, and have weak linkage to final code artefacts. Asana (or any task system) is a valuable input source but should not be the core storage substrate. The reason graph is the substrate; task systems are one of many feeders into it.

### 7.2 Bootstrap from Existing Codebases

- Build the CPG from the current codebase using tree-sitter and existing tooling (Joern, Fraunhofer CPG library).
- Replay commit history to construct graph snapshots. Start with 90 days.
- Intent inference pass: LLM over commit messages, PR descriptions, linked tickets, and co-changed symbol clusters. Reconstruct ReasonNodes and CREATES/MODIFIES edges.
- Initial drift scan: compare inferred intent specs against current symbol behaviour. Surface orphaned nodes and high-drift modules.

## 8. High-Value Starting Workflows

The right question is not "can intent be modelled?" but "what high-value decision becomes easier or safer if this graph exists?" Four workflows have strong justification:

### 8.1 Legacy Migration Equivalence

For each area of old system vs new system: what business capability existed, what constraints mattered, what edge cases were intentional, what is preserved, what is intentionally dropped, what is unimplemented but hidden by structural differences. Migration completeness becomes queryable, not a matter of judgment.

**Current application:** zenloop v1 to v2 migration. Run reason inference over v1 git history, tag all v2 changes to ReasonNodes, query: which v1 intents have no corresponding v2 node?

### 8.2 Agent Task De-duplication

Before generating a PR: detect overlapping intent with in-flight or recent work, surface prior attempts, prevent six agents from implementing the same goal differently. Directly solves the OpenClaw problem.

### 8.3 Safe Change Review

When an agent changes code: identify impacted reason nodes and constraints, check whether the change preserves required invariants, route high-risk intent boundaries to humans.

### 8.4 Refactor Safety

Before removing code that looks dead: show why it was introduced, whether the underlying condition still exists, what production behaviours or contracts depended on it.

## 9. Comparison with Existing Approaches

| System | Structural Graph | Semantic Analysis | Reason Layer | Agent Decision Support | Active Guardrails |
| --- | --- | --- | --- | --- | --- |
| Git | No | No | No | ~ commit SHA | No |
| CPG / Joern | Yes (AST+CFG+PDG) | Yes (security) | No | No | No |
| code-review-graph | ~ AST only | No | No | No | No |
| Greptile | ~ call graph | ~ review | No | No | No |
| **iCPG** | **Yes (full)** | **Yes (full)** | **Yes (decision chain)** | **Yes (3 pre-task queries)** | **Yes (drift signals)** |

## 11. Evaluation Plan: The zenloop Migration Experiment

The reviewer feedback on this RFC was unambiguous: the strongest claims require empirical validation, not argument. This section describes a concrete evaluation plan using the zenloop v1 to v2 migration as the primary dataset. This experiment is already underway and is the immediate practical motivation for this work.

### 11.1 Dataset

- **zenloop v1 codebase** -- 3+ years of git history, approximately 50,000 lines of code, multiple contributors, significant AI-assisted development in the final 18 months
- **zenloop v2 codebase** -- new architecture (Supabase to AWS RDS migration), actively being built
- **Secondary codebases** -- additional production repositories available for cross-validation, details to be determined based on zenloop results

### 11.2 Four Validation Experiments

**Experiment A -- ReasonNode Inference Accuracy**

*Research question:* how accurately can an LLM inference pass reconstruct ReasonNodes from git history?

- **Method:** run inference pass over the last 90 days of zenloop v1 git history. Manually label a stratified sample of 200 commits with ground-truth ReasonNodes.
- **Metrics:** precision, recall, and F1 of inferred vs ground-truth ReasonNodes. Secondary: confidence calibration -- do low-confidence inferences correlate with wrong labels?
- **Baseline:** commit message keyword extraction (no LLM)
- **Success threshold:** F1 > 0.65 at default threshold, precision > 0.80 at high-confidence tier

**Experiment B -- Drift Detection vs Known Bugs**

*Research question:* does iCPG drift detection correlate with real production incidents?

- **Method:** run drift scan over zenloop v1 history. Cross-reference with known bug reports and production incidents from the same period. Measure whether high-drift modules and high-drift symbols are disproportionately associated with incidents.
- **Metrics:** area under ROC curve for drift score as a predictor of incident occurrence. Comparison against lines-changed and cyclomatic complexity as baselines.
- **Success threshold:** AUC > 0.70 (drift is a better predictor than naive baselines)

**Experiment C -- Migration Coverage: Can v2 Satisfy v1 Intents?**

*Research question:* can iCPG surface v1 business capabilities and constraints that v2 has not yet addressed?

- **Method:** build the Reason Graph for v1. Tag v2 changes to ReasonNodes. Query: which v1 ReasonNodes have no corresponding v2 CREATES or MODIFIES edge?
- **Metrics:** manual evaluation of surfaced gaps -- are they real functional gaps, known intentional divergences, or false positives? Precision of gap detection.
- **Baseline:** code-diff between v1 and v2 (structural comparison without intent layer)
- **Success threshold:** iCPG surfaces at least one real functional gap not visible in the structural code diff

**Experiment D -- Duplicate Intent Detection**

*Research question:* can DUPLICATES edge detection reduce redundant agent work?

- **Method:** identify historical cases within the zenloop codebase and available secondary repositories where two separate Claude Code sessions independently implemented the same or overlapping functionality. Measure whether iCPG's embedding-based similarity would have flagged them as duplicates before the second implementation started.
- **Metrics:** precision and recall of duplicate detection at varying similarity thresholds. Manual evaluation of flagged pairs.
- **Baseline:** no deduplication (current state)
- **Success threshold:** precision > 0.75 at a threshold that achieves recall > 0.60

### 11.3 Honest Limitations of This Evaluation

We acknowledge the following limitations upfront:

- zenloop v1 is one codebase with a specific history and team. Results may not generalise to large-scale open source or enterprise monorepos.
- Manual labelling of ground-truth ReasonNodes is subjective and the labelling process itself needs an inter-rater agreement study.
- Experiment C is partly circular -- we are building iCPG and using iCPG to find its own gaps. An independent implementation would strengthen the result.
- Experiment D relies on historical reconstruction of whether duplicate work occurred. The counterfactual -- would detection have changed agent behaviour? -- cannot be directly measured.

These limitations are real. We present them not to undermine the work but to be explicit about what a first empirical pass can and cannot establish. If Experiments A-D produce positive results, the next step is a prospective study: deploy iCPG on a new migration and measure outcomes in real time.

## 12. Open Questions

These questions reflect where the paper actually is after seven versions of refinement. Early questions about whether drift is real or whether intent can be modelled are settled. The open questions now are about implementation depth, formal precision, and empirical validation.

**Q1 -- The Hallucination-Drift Equivalence: How strong is the claim?**

Section 0 argues that hallucination in agentic systems is formally equivalent to specification drift -- the agent operating outside its formal decision plane. How precisely does this hold, and where does it break down?

The claim holds clearly when a ReasonNode contract exists and is violated. It is weaker for vibe coding failures where no formal spec exists -- there is no decision plane to measure drift from. Is "absent specification" a degenerate case of infinite drift, or a categorically different failure mode? And does the equivalence hold for non-software agents (customer service, research) where intent is harder to formalise?

**Q2 -- Contract Inference vs Contract Authoring: Which is the right default?**

ReasonNode contracts (preconditions, postconditions, invariants) can be hand-authored or LLM-inferred. Prior research on specification inference is promising but not directly applicable to the messy rationale inference problem iCPG requires. What is the right default, and what is the minimum useful contract quality threshold?

A wrong invariant is worse than no invariant -- it either blocks legitimate work or fails to catch real drift. Should the system start with inferred contracts marked as low-confidence and promote them to verified only after a human has confirmed them? What is the minimum viable contract for a ReasonNode to be useful -- does even a natural language postcondition with no formal predicate provide value?

**Q3 -- Formal Decision Plane for Dynamic Languages: Is it tractable?**

DbC predicates over CPG nodes are tractable for well-typed languages (Java, Go, Rust, TypeScript). Python metaprogramming, Ruby DSLs, and JavaScript make CPG construction unreliable. Does the formal drift definition break down for these languages?

Three options: (1) accept degraded precision for dynamic languages and flag it, (2) require type annotations or runtime traces to compensate, (3) use a weaker but more general drift definition based on embedding distance rather than predicate failure for these cases. Which is right, and what is the precision/recall trade-off at each level?

**Q4 -- Multi-Agent Invariant Conflicts: Locking or Compositionality?**

When two agents execute concurrently and their ReasonNode invariants conflict -- one must expand the auth boundary, one must freeze it -- how does iCPG resolve this without deadlock or silent violation?

ABC addresses this with a formal compositionality theorem for multi-agent contracts. Does iCPG need the same formal treatment, or is a practical symbol-level locking and queuing mechanism sufficient for the software development case? The concern: a formal compositionality proof is expensive to compute at the scale of a real codebase with hundreds of concurrent agent sessions.

**Q5 -- Semantic Duplication Threshold: Where is the line?**

DUPLICATES edges require detecting "same goal, different implementation" across agents. Embedding-based similarity on (stated_purpose + scope) is the practical starting point. What similarity threshold creates a DUPLICATES edge, and how do you avoid both false positives (blocking legitimate parallel work) and false negatives (missing the OpenClaw problem)?

This is an empirical question that can only be answered with real data. The zenloop migration is the first validation target. What is the ground truth for duplicate detection? PRs that a human maintainer would have merged into one, or PRs that touch the same CPG subgraph? These may produce different thresholds.

**Q6 -- Incremental Graph Maintenance at Scale: What is the Performance Envelope?**

code-review-graph achieves 2-second incremental updates with AST-only. Full CPG (adding CFG and PDG) is harder -- edges change transitively. Adding the Reason Graph layer means ReasonNode contracts must be re-evaluated when their scope symbols change. What is the realistic update latency for a 500k-line codebase with 20 concurrent agents?

This is the primary engineering risk. If graph updates are too slow, agents cannot query the graph before acting -- removing the main value proposition. Is there a tiered approach: instant AST+RG updates for pre-task queries, deferred CFG+PDG updates for post-commit drift scans? What does the performance envelope look like across codebase sizes?

**Q7 -- Bootstrap Accuracy: Is 60-75% Inference Good Enough?**

The inference pass over Git history reconstructs ~60-75% of ReasonNodes correctly. A wrong ReasonNode is actively misleading -- it can cause an agent to preserve a constraint that no longer applies or skip one that does. At what accuracy does the bootstrapped graph provide net positive value vs no graph at all?

We believe the answer is: even 60% accuracy is positive if wrong inferences are clearly flagged as low-confidence. But this needs empirical validation. Propose: run the inference pass on the zenloop v1 codebase, manually validate a sample, and measure how many wrong ReasonNodes would have caused a bad agent decision vs a correct one. This is the validation experiment that would most sharpen the paper.

**Q8 -- Scope Boundaries: Where Does the Model Break Down?**

This paper is deliberately scoped to software development. Within that scope, what classes of codebase or workflow are likely to break the model -- and how badly?

Obvious candidates for breakdown: Python metaprogramming and Ruby DSLs make reliable CPG construction harder; infrastructure-as-code where files are templates not systems; generated code where the source of truth is the generator not the output. How do these cases affect the core drift definition -- are they marginal or central to typical agentic development workflows?

**Q9 -- Graph Storage at Portfolio Scale: Shared or Per-Repo?**

As iCPG is applied across multiple codebases, should each have an isolated Reason Graph, or is there value in a shared graph that surfaces patterns across projects?

A shared graph could detect that two projects independently solved the same problem or that a particular invariant pattern consistently predicts drift. But cross-project edges raise data governance and IP questions. What is the minimal shared model that provides cross-project intelligence without exposing proprietary structure? This is a design question for when single-codebase validation is complete.

---

*iCPG RFC Draft v8 -- March 2026 -- Concept stage -- Not affiliated with any existing CPG project*
