# Engram

## The Amnesia Model for Agentic Memory Systems

> RFC DRAFT -- MARCH 2026 -- V3

A pathology-first framework for diagnosing, measuring, and eliminating memory failure in autonomous AI agents. Validated against three production agentic systems: Maia (Protaige), hive (Ideaminer.io), and Deepak (Protaige Chief of Staff). V3 adds operationalised measurement protocol, the positive transfer open question, and three system diagrams.

| Version | Date | Changes |
| --- | --- | --- |
| v1.0 | Dec 2025 | Initial concept: amnesia taxonomy, RAG-Amnesia Scale, three-mechanism model, retrieve-time synthesis, context hierarchies. |
| v2.0 | Mar 2026 | Renamed to Engram. Prior research section. Three production agents (Maia, hive, Deepak) as validation dataset with observed failure evidence. |
| v3.0 | Mar 2026 | Operationalised Amnesia Score measurement protocol with concrete labelling procedure, sampling method, inter-rater agreement, and confidence intervals. New open question Q7: positive transfer vs namespace isolation tradeoff. Three system diagrams: EngramRecord lifecycle, namespace isolation architecture, multi-path retrieval. |

## 0. Motivation: Three Agents, One Problem

This paper is grounded in three production AI agents - Maia (Protaige), hive (Ideaminer.io), and Deepak (Protaige Chief of Staff) - that each exhibit distinct, observable memory failure patterns in daily operation. These failures are not edge cases. They are the central limitation preventing these agents from becoming more effective as their operational scope grows.

| Agent | Role | Memory at stake |
| --- | --- | --- |
| Maia | Protaige. Full marketing intelligence: brand analysis, campaign creation, execution, performance analysis, optimisation. 150+ tools. WhatsApp, voice, meetings via Recall.ai, phone. | Brand context across campaigns. User preference across channels. Campaign history and performance. Optimisation decisions across sessions. Client voice and constraint persistence. |
| hive | Ideaminer.io. AI-as-CEO OS: 10+ API integrations (Asana, Pipedrive, Planhat, GitHub, Render, BambooHR, SendGrid, Lemlist, ZenSurveys), meeting logs, morning briefs, escalation detection, people analytics, data hygiene. 9 sub-agents. | Company state continuity across daily briefs. Escalation history. Pipeline and customer health trends. People analytics without interference between individuals. |
| Deepak | Protaige. Chief of Staff: competitive intelligence, internal team analysis, user pattern analysis, strategic synthesis. Parallel to hive, optimised for analytical depth. | Competitive landscape over time. Team performance trends. User pattern changes across weeks and months. Strategic context across analysis sessions. |

> **The Engram thesis:** Memory is not a feature these agents happen to use. It is the mechanism by which they become genuinely useful rather than impressive demos. Every amnesia type in this RFC has a direct, observed manifestation in Maia, hive, or Deepak. The framework is distilled from production failure - not proposed in the abstract.

### 0.1 Why Existing Memory Systems Are Insufficient

Mem0, Zep, and similar systems were designed for a specific archetype: a conversational assistant that remembers user preferences across sessions. That archetype is a single user, single channel, single purpose. Our three agents violate all three constraints simultaneously.

| Constraint | Maia | hive | Deepak |
| --- | --- | --- | --- |
| Single user | No - serves multiple users across Protaige's client brands | No - serves the CEO but touches data about all employees, customers, and partners | No - synthesises intelligence from multiple internal and external sources |
| Single channel | No - WhatsApp, voice, meetings, phone, web chat | No - API integrations, meeting logs, morning briefs, alert emails | No - competitive data, team analytics, user patterns, strategic docs |
| Single purpose | No - brand analysis, campaign creation, execution, measurement, optimisation | No - pipeline, customer health, people analytics, data hygiene, escalations | No - competitive, internal, user pattern, strategic synthesis |

A memory system designed for single-user, single-channel, single-purpose assistants will produce interference failures in Maia (brand memories leaking across clients), source amnesia in hive (facts remembered without knowing which employee or customer they pertain to), and temporal amnesia in Deepak (competitive observations stored without timestamps, making it impossible to track how a competitor's positioning has evolved). These are not hypothetical risks. They are observed failure modes.

## 1. The Three Production Agents: Observed Memory Failures

This section documents specific memory failure patterns from the three agents. Each maps to amnesia types defined formally in Section 4.

### 1.1 Maia - Marketing Intelligence Agent

**Architecture:** 150+ tools. Claude Agent SDK core. Multi-channel: WhatsApp, voice calls, meeting participation via Recall.ai, phone. Full campaign lifecycle: brand analysis, strategy, tactic creation, execution, performance analysis, optimisation. Serves multiple client brands across Protaige's user base.

Memory is not incidental to Maia's function - it is constitutive of it. A Maia that cannot remember last month's campaign performance cannot optimise. A Maia that cannot remember a brand's voice constraints will generate copy that violates them. A Maia that confuses two clients' brand contexts is actively harmful.

**Observed failure - Anterograde:** Maia correctly analyses a brand in session N and produces a detailed voice profile. In session N+1, called through a different channel (WhatsApp vs voice), the voice profile is unavailable. Maia starts the brand analysis from scratch.

> **Root cause:** Session data not persisted to long-term store. Channel-scoped memory that does not propagate across channels.

**Observed failure - Interference:** Maia serves Brand A (formal, B2B software) and Brand B (casual, consumer lifestyle) for the same Protaige user. After extensive Brand B interactions, Maia's campaign suggestions for Brand A carry Brand B's casual register.

> **Root cause:** Brand contexts not isolated in separate namespaces. Shared embedding space allows Brand B's patterns to contaminate Brand A retrieval.

**Observed failure - Temporal:** Campaign optimisation recommendations do not account for the fact that the baseline was established 6 weeks ago. Maia treats the original brief as current even after two rounds of performance review have updated the strategy.

> **Root cause:** No temporal validity windows on campaign context records. Strategy documents are retrieved without recency weighting.

**Observed failure - Context-binding:** Maia has the correct brand voice guideline in memory but fails to surface it during tactic execution because the retrieval query (focused on creative output) does not semantically match the storage encoding (focused on brand constraints).

> **Root cause:** Single-path semantic retrieval. Brand constraints not linked to execution contexts at encoding time.

| Amnesia type | Impact on Maia effectiveness |
| --- | --- |
| Anterograde | Most critical: each new channel session or session gap starts from scratch. Maia cannot compound learning across the campaign lifecycle. |
| Interference | Most insidious: brand contamination is invisible until a client notices. High reputational risk in a multi-brand marketing context. |
| Temporal | Compounds over time: optimisation recommendations become increasingly disconnected from current strategy as temporal drift accumulates. |
| Context-binding | Limits autonomous execution: Maia has the right constraints but cannot surface them at the right moment in the workflow. |
| Confabulation | Lowest current risk: Maia's tool-use architecture reduces confabulation vs conversational agents. But inferred brand preferences presented as stated preferences are a real failure mode. |

### 1.2 hive - CEO-Assistant Operating System

**Architecture:** 10+ API integrations (Asana, Pipedrive, Planhat, GitHub, Render, BambooHR, SendGrid, Lemlist, ZenSurveys). Meeting log ingestion. Morning briefs. 9 specialised sub-agents: CEO, Customer Success, Sales, Project Management, Analyst, Advisor, Alert Detector, Reporter, Orchestrator. 47 tools. Full permission system (autonomous / requires approval / restricted).

hive operates at the highest stakes of the three agents: it makes or surfaces recommendations about employees, customers, pipeline, and operations. Its memory failures carry direct business and human consequences - as demonstrated in the "should this employee's holiday be approved?" incident, where hive correctly identified low engagement but had no temporal context about why, no history of similar assessments, and no memory that this type of recommendation had caused cultural friction before.

**Observed failure - Source amnesia:** hive morning brief correctly surfaces a pipeline risk but cannot attribute whether the signal came from Pipedrive activity, Planhat health score, meeting transcript analysis, or a combination. The recommendation is correct; the reasoning is opaque. Human override is impossible without source tracing.

> **Root cause:** Multi-source ingestion without Origin metadata propagation. Facts arrive from 10+ APIs and are stored without source tagging.

**Observed failure - Interference (people context):** hive analyses employee engagement patterns across the company. High-volume data from a large team contaminates pattern recognition for smaller teams. An employee in a 2-person team is assessed against whole-company utilisation patterns.

> **Root cause:** No context namespace isolation for people analytics. Individual and team-level patterns share the same retrieval space.

**Observed failure - Retrograde:** hive loses track of escalation resolutions. An issue that was raised, escalated, and resolved re-surfaces in the morning brief three weeks later as if it were current.

> **Root cause:** Storage decay without reinforcement. Resolved incidents are not marked as superseded; retrieval surfaces them without temporal validation.

**Observed failure - Temporal:** Data hygiene automation identifies stale Pipedrive leads from 2018-2022. hive can clean them up, but has no persistent memory of which leads were cleaned, why they were stale, or what patterns produced the staleness - so the same patterns can re-accumulate.

> **Root cause:** Actions (data hygiene operations) not encoded as memory records. The agent acts but does not remember acting.

| Amnesia type | Impact on hive effectiveness |
| --- | --- |
| Source amnesia | Most critical: a CEO-assistant that cannot explain where its intelligence came from cannot be trusted for decisions with human consequences. |
| Interference (people) | Most sensitive: people analytics requires the strictest context isolation of any agent we operate. Contamination across individuals or teams has legal and cultural implications. |
| Retrograde | Practical impact: re-surfacing resolved issues wastes executive attention and erodes trust in the morning brief format. |
| Temporal | Compounds at scale: as hive's dataset grows (more integrations, more meeting logs), temporal drift without validity windows becomes the dominant retrieval failure. |

### 1.3 Deepak - Chief of Staff Agent

**Architecture:** Protaige's strategic intelligence agent. Competitive analysis (tracking competitor positioning, pricing, and product moves), internal team pattern analysis (identifying performance trends and bottlenecks), user pattern analysis (tracking how Protaige's users interact with the platform across cohorts), and strategic synthesis (connecting competitive signals to internal capabilities and user needs). Parallel architecture to hive but optimised for analytical depth over operational breadth.

Deepak's memory failure mode is distinct from Maia's or hive's. Its core function is temporal pattern recognition: how has a competitor's positioning shifted over the past quarter? Which user cohort behaviour change predicts churn? How does this week's internal team velocity compare to the same period last quarter? Without memory of how things were, Deepak cannot identify how things have changed - and pattern recognition without temporal context is just observation without intelligence.

**Observed failure - Temporal amnesia (primary):** Deepak correctly identifies that Competitor X has shifted from a feature-centric to an outcomes-centric narrative. But it cannot tell you when this shift began, what triggered it, or how it correlates with Competitor X's product releases. Every analysis starts from the current state with no accumulation of temporal signal.

> **Root cause:** Competitive observations stored as facts, not as temporally-stamped events. No validity windows. No change-detection across observation records.

**Observed failure - Confabulation:** Deepak synthesises a user pattern analysis and presents "users in this cohort consistently prefer X feature" as an established finding. The finding was inferred from a single week of behavioural data three months ago and has never been re-validated. Deepak presents it with the same confidence as a finding validated across 12 weeks of data.

> **Root cause:** No Origin distinction between inferred patterns and validated patterns. Confidence scores not stored or propagated. Observation age not factored into retrieval ranking.

**Observed failure - Context-binding:** Deepak has competitive intelligence about Competitor X stored from a brand positioning analysis. When conducting a pricing strategy analysis, the relevant competitive price positioning is not retrieved because the storage encoding (brand analysis context) does not match the retrieval query (pricing strategy context).

> **Root cause:** Intelligence stored in analysis-context silos. Multi-path retrieval not implemented - competitive facts are only retrievable in the context in which they were originally captured.

| Amnesia type | Impact on Deepak effectiveness |
| --- | --- |
| Temporal amnesia | Existential: Deepak's core value proposition is pattern recognition over time. Without temporal memory, it is a single-session analytical tool, not a strategic intelligence system. |
| Confabulation | High risk: strategic decisions made on inferred patterns presented as validated findings have direct commercial consequences. The confidence calibration problem is acute in an analytical agent. |
| Context-binding | Significant: intelligence captured in one analytical context (competitive analysis) should surface in related contexts (pricing, positioning, product strategy) but currently does not. |

### 1.4 Cross-Agent Memory Failure Patterns

Examining the three agents together reveals structural failure patterns that are not agent-specific but architectural. These cross-agent patterns are the strongest evidence that the amnesia taxonomy is the right diagnostic frame:

| Cross-agent pattern | Affected agents | Structural cause |
| --- | --- | --- |
| Channel-scoped memory that does not propagate across channels | Maia (WhatsApp vs voice vs meeting). hive (API data vs meeting logs). Deepak (web analysis vs meeting intelligence). | Memory encoded with channel as an implicit scope, not as an explicit metadata field. Cross-channel retrieval requires a namespace bridge that does not exist. |
| Actions not encoded as memory | hive (data hygiene operations). Maia (tactic execution). Deepak (competitive assessments delivered). | Agent-executed actions are logged for debugging but not encoded as EngramRecords. The agent knows what it decided; it does not remember that it decided it. |
| Confidence not propagated from encoding to synthesis | Maia (inferred brand preferences). Deepak (inferred user patterns). hive (multi-source intelligence synthesis). | Confidence is assessed at retrieval time by the LLM, not stored at encoding time and propagated. High-confidence generation masks low-confidence underlying data. |
| Context contamination under multi-entity load | Maia (brand contamination). hive (people analytics contamination). Deepak (competitor pattern contamination). | All three agents operate on multiple entities of the same type (brands, people, competitors) within a shared vector space. No explicit namespace isolation between entities of the same class. |

## 2. The Core Reframe: From Optimisation to Diagnosis

The memory systems field treats memory as a retrieval optimisation problem. The production evidence from Maia, hive, and Deepak demonstrates why this frame is insufficient: these agents' failures are encoding failures, storage attribution failures, temporal validity failures, and context isolation failures. Better retrieval algorithms cannot fix any of these - because the problem is upstream of retrieval.

| Optimisation frame (current field) | Engram diagnostic frame |
| --- | --- |
| How accurate is memory retrieval? | What type of amnesia does this agent exhibit? |
| Maximise recall on LongMemEval | Map observed failures to amnesia dimensions |
| Better retrieval algorithm | Targeted fix for specific pathology |
| Single performance score | Amnesia profile across 7 dimensions |
| System comparison on benchmark | Agent-specific amnesia diagnosis and treatment |

**Amnesia Score:** 0.0 = perfect memory (no amnesia of any type). 1.0 = total amnesia. Every real system sits between these extremes across seven independent dimensions. Maia, hive, and Deepak each have a distinct amnesia profile - not a single memory quality score. The goal is to diagnose each dimension, trace it to its architectural cause, and apply the targeted fix.

## 3. Prior Research: Standing on a Fragmented Field

The December 2025 survey "Memory in the Age of AI Agents" (Zhang et al., arXiv 2512.13564) noted the field has become "increasingly fragmented" with "loosely defined terminologies and inconsistent taxonomies." This section maps major prior works against Engram - identifying what each contributes and where it falls short for multi-entity, multi-channel, multi-purpose agents.

### 3.1 Mem0 - The Market Leader

| Property | Details |
| --- | --- |
| What it is | The dominant production memory layer. Dynamically extracts, consolidates, and retrieves memories. Graph-based variant with Conflict Detector and LLM-powered Update Resolver. $24M Series A (October 2025). AWS exclusive memory provider for Agent SDK. 186M API calls in Q3 2025. |
| Key innovation | Memory as first-class objects with explicit write/forget operations. Conflict detection at write time. 26% accuracy improvement over plain vector retrieval; 91% lower p95 latency. |
| What Engram inherits | Memory-as-object mental model. Conflict detection as a write-time primitive. The explicit forget operation as an architectural necessity, not an afterthought. |
| Engram gap (production evidence) | Mem0's user-scoped storage helps Maia's brand contamination only partially - it scopes by user, not by brand within a user. hive's cross-source intelligence has no Origin propagation model in Mem0. Deepak's temporal pattern tracking has no validity window support. |
| Amnesia types addressed | Anterograde (strong). Source amnesia (partial). Temporal, interference, confabulation (weak). |

### 3.2 Zep / Graphiti - Temporal Knowledge Graphs

| Property | Details |
| --- | --- |
| What it is | Temporally-aware dynamic knowledge graph. Three tiers: episode, semantic entity, community subgraphs. Every graph edge carries explicit validity intervals. Temporal metadata used to update or invalidate without discarding outdated information. |
| Key innovation | Temporal validity as a first-class graph primitive. Tri-tier storage enables both episode-level retrieval and community-level pattern recognition. |
| What Engram inherits | The temporal validity window model. Preservation (not overwriting) of conflicting temporal facts. The tri-tier architecture as a model for Deepak's multi-resolution competitive intelligence. |
| Engram gap (production evidence) | Zep's temporal model is excellent for Deepak's competitive tracking use case. But context namespace isolation between brands (Maia) and between individuals (hive) is not a primary design target. The graph is structured around entities and time, not context hierarchies. |
| Amnesia types addressed | Temporal (strong). Retrograde (moderate). Context-binding, interference, source amnesia (weak). |

### 3.3 MemGPT / Letta - The OS Memory Metaphor

| Property | Details |
| --- | --- |
| What it is | OS-inspired memory hierarchy: main context as RAM, external storage as disk. Agents control memory through explicit function calls. Stateful memory server with always-available core memory blocks (goals, preferences, persona). |
| Key innovation | Memory management as an explicit agentic action. Agents decide what to remember and what to archive. Core memory blocks as reliable always-available context. |
| What Engram inherits | Core memory blocks as a model for hive's persistent CEO-context (company strategy, key relationships, ongoing escalations). The explicit archiving decision as a complement to Engram's decay-rate model. |
| Engram gap (production evidence) | Agent-controlled memory introduces a new failure mode: the agent may archive information it will need. Maia's cross-channel memory propagation requires system-level encoding, not agent-controlled archival. hive's 10+ API sources cannot rely on the agent to decide what is worth remembering - encoding must be automatic. |
| Amnesia types addressed | Working memory (strong). Anterograde (moderate). Confabulation, source amnesia, interference (weak). |

### 3.4 Hindsight - Current SOTA (December 2025)

| Property | Details |
| --- | --- |
| What it is | 91.4% on LongMemEval. Four logical networks: world (objective facts), bank (agent experiences), opinion (subjective judgments with updatable confidence), observation (preference-neutral entity summaries). |
| Key innovation | Separation of factual from opinionated memory. Confidence scores on the opinion network update as evidence accumulates. Reduces confabulation by marking beliefs as beliefs. |
| What Engram inherits | The four-network content taxonomy as a model for differentiating Deepak's intelligence types (facts vs inferences vs validated patterns). Confidence score model for Deepak's confabulation risk. |
| Engram gap (production evidence) | Hindsight's four networks are content categories, not functional mechanisms or context namespaces. They do not address Maia's brand isolation requirement or hive's source Origin requirement. The opinion network's confidence model is applied at storage time; Engram requires it to propagate through retrieval to synthesis - especially for Deepak's strategic recommendations. |
| Amnesia types addressed | Confabulation (strong). Anterograde, retrograde (moderate). Source amnesia, interference, context-binding (weak). |

### 3.5 A-MEM - Agentic Memory with Zettelkasten Linking

| Property | Details |
| --- | --- |
| What it is | A-MEM (Xu et al., Feb 2025) organises interactions into Zettelkasten-like memory units: structured notes incrementally linked and refined as new interactions occur. |
| Key innovation | Associative encoding at write time: new memories connect to existing ones, building an associative network rather than a flat store. Memory refinement as existing notes update when contradicted. |
| What Engram inherits | Associative encoding at write time as a core principle - especially relevant for Deepak's competitive intelligence, where new observations should automatically link to prior observations about the same competitor. The Zettelkasten linking model maps to Engram's entity_links and causal_links fields. |
| Engram gap (production evidence) | A-MEM's retrieval relies on semantic embedding similarity, missing temporal and causal relationships - exactly Deepak's core need. For Maia and hive, the Zettelkasten metaphor also does not provide context namespace isolation. |
| Amnesia types addressed | Anterograde (strong, via associative encoding). Retrograde (moderate). Temporal, context-binding, interference (weak). |

### 3.6 MAGMA - Multi-Graph Agentic Memory (January 2026)

| Property | Details |
| --- | --- |
| What it is | MAGMA (Jiang et al., Jan 2026, arXiv 2601.03236). Each memory item represented across four orthogonal graphs: semantic, temporal, causal, entity. Retrieval as policy-guided traversal with Adaptive Traversal Policy. 45.5% higher reasoning accuracy; 95%+ token reduction; 40% faster latency vs prior methods. |
| Key innovation | Decoupling memory representation from retrieval logic. Causal graph explicitly models cause-effect relationships. Query-adaptive traversal routes retrieval based on query intent, pruning irrelevant graph regions. |
| What Engram inherits | Multi-relational retrieval across semantic, temporal, causal, and entity dimensions independently arrived at the same design as Engram's multi-path retrieval. MAGMA's causal graph is particularly relevant for Deepak's competitive intelligence (cause-effect between competitor actions and market responses). |
| Engram gap (production evidence) | MAGMA evaluates on LongMemEval and LoCoMo - single-user, single-purpose benchmark settings. It does not model context namespace isolation (critical for Maia's brand isolation and hive's people analytics isolation). No Origin model for confabulation prevention. MAGMA optimises retrieval accuracy; Engram diagnoses which failure type each graph dimension prevents. |
| Amnesia types addressed | Context-binding (strong). Temporal (strong). Source amnesia, confabulation, interference (weak - not primary design targets). |

### 3.7 The Benchmarks and Their Blind Spots

| Benchmark | What it measures and what it misses for our agents |
| --- | --- |
| LongMemEval (Wu et al. 2024) | Measures answer accuracy on memory-requiring questions over long conversation histories. Does not measure: which amnesia type caused a failure, whether brand contamination occurred, whether source Origin was maintained, or how memory quality degrades under multi-entity load. |
| LoCoMo | Temporal and causal reasoning over long sequences. Better than LongMemEval for Deepak's use case. Still a single-user, single-purpose benchmark - does not model Maia's multi-brand scenario or hive's multi-source attribution requirement. |
| Amnesia Score (Engram proposal) | Seven-dimensional diagnostic that measures which type of failure each agent exhibits. Designed specifically for multi-entity (multi-brand, multi-user, multi-source) agents. Intended to complement LongMemEval, not replace it: a system should report both its LongMemEval score (overall performance) and its Amnesia Score profile (which failure types remain). |

### 3.8 Summary: What Engram Inherits and What Is Novel

| Prior work | What Engram inherits | Gap filled by Engram (production evidence) |
| --- | --- | --- |
| Zhang et al. 2025 survey | Field landscape, three-form/function taxonomy | Pathology framework grounded in specific production agent failures |
| Mem0 | Memory-as-object, conflict detection, explicit forget | Context namespace isolation (Maia brands, hive people), temporal validity, confabulation Origin |
| Zep / Graphiti | Temporal validity windows, tri-tier graph storage | Context namespace isolation, multi-entity amnesia typing |
| MemGPT / Letta | Core memory blocks, explicit archival decisions | Automatic encoding for high-volume multi-source agents (hive), channel-scoped memory propagation (Maia) |
| Hindsight | Content taxonomy, confidence scoring on opinions | Confidence propagation through retrieval to synthesis (Deepak), context namespace model |
| A-MEM | Associative encoding at write time, Zettelkasten linking | Multi-path retrieval including temporal and causal (Deepak), context namespace isolation |
| MAGMA | Multi-relational retrieval, causal graph, query-adaptive traversal | Diagnostic framing, context namespace gating, Origin propagation, multi-entity isolation |
| LongMemEval / LoCoMo | Benchmark methodology and evaluation rigour | Amnesia Score: diagnostic complement measuring failure type, not just answer accuracy |

The novel contribution of Engram is not any single element. The novelty is the combination: a formal taxonomy of seven amnesia types from neuroscience, operationalised as measurable dimensions of a composite Amnesia Score, grounded in a three-mechanism functional model, with a retrieve-time synthesis principle and context hierarchy architecture. No prior work combines all of these into a diagnostic-first framework for multi-entity agents.

## 4. The Seven Amnesia Types (With Agent Evidence)

Each amnesia type is now grounded in observed production failures from Maia, hive, and/or Deepak. The clinical parallel, observed failure, root cause, targeted fix, and primary agent evidence are documented for each type.

### Type 1 - Anterograde Amnesia

| Property | Definition |
| --- | --- |
| Clinical parallel | Hippocampal damage prevents formation of new long-term memories. Patient HM (Henry Molaison) retained pre-surgery memories but could not form new ones after 1953. |
| Observable failure | Agent handles a session normally but retains nothing across session boundaries or channel switches. |
| Production evidence | Maia: voice session brand analysis not available in subsequent WhatsApp session. User must re-introduce the brand from scratch. |
| Root cause | Encoding pipeline does not produce durable storage records. Session data processed but not written to persistent memory store. |
| Targeted fix | Explicit end-of-session encoding. Every session produces structured EngramRecords. Cross-channel propagation with channel as metadata, not as storage namespace. |
| Most affected agent | Maia (channel-boundary anterograde is the highest-frequency failure). |

### Type 2 - Retrograde Amnesia

| Property | Definition |
| --- | --- |
| Clinical parallel | Existing memories become inaccessible after injury or through decay. Older memories degrade even though they were once clearly formed. |
| Observable failure | Agent correctly formed memories earlier but cannot access them later. Resolved issues re-surface as current. |
| Production evidence | hive: resolved escalation from three weeks ago re-appears in morning brief as if it were current. The resolution record decayed without reinforcement. |
| Root cause | Storage decay without reinforcement. Resolved or archived items not marked as superseded. |
| Targeted fix | Decay-aware storage with status tracking. Resolved/superseded records carry explicit status fields. Retrieval filters by status before ranking by recency. |
| Most affected agent | hive (high-volume daily data creates rapid decay of historical context). |

### Type 3 - Source Amnesia

| Property | Definition |
| --- | --- |
| Clinical parallel | The person remembers a fact but not where they learned it - who told them, in what context, how reliable the source was. |
| Observable failure | Agent retrieves a memory correctly but cannot attribute the source. Multi-source intelligence synthesised without Origin. |
| Production evidence | hive: morning brief correctly surfaces a pipeline risk but cannot tell the CEO whether the signal came from Pipedrive activity, Planhat health score, or a meeting transcript. Human override and verification are impossible. |
| Root cause | Multi-source ingestion without mandatory Origin metadata. Facts stored as content without source tags. |
| Targeted fix | Mandatory source metadata on every EngramRecord: source_system, source_record_id, ingestion_timestamp, confidence, Origin_type. |
| Most affected agent | hive (10+ source APIs with no unified Origin model). |

### Type 4 - Temporal Amnesia

| Property | Definition |
| --- | --- |
| Clinical parallel | Loss of the time dimension of memories - events remembered but not when, or in the wrong temporal order. |
| Observable failure | Agent treats outdated information as current. Cannot detect how things have changed because it has no memory of how they were. |
| Production evidence | Deepak: competitive intelligence observations stored as facts without timestamps. Deepak cannot identify when Competitor X shifted from feature-centric to outcomes-centric narrative, only that the shift happened. Pattern recognition requires knowing the sequence and timing. |
| Root cause | Memories stored without temporal validity windows. Retrieval ignores timestamps and recency. |
| Targeted fix | Temporal validity windows on every EngramRecord (Zep/Graphiti model). Retrieval weights recency. Competing temporal facts preserved with timestamps rather than overwritten. |
| Most affected agent | Deepak (temporal pattern recognition is its core value proposition; temporal amnesia is an existential failure). |

### Type 5 - Context-Binding Failure

| Property | Definition |
| --- | --- |
| Clinical parallel | Memory exists but is not linked to the right retrieval cue. Cannot surface the memory when it would be relevant. |
| Observable failure | Agent has the correct information in memory but cannot surface it because the retrieval context does not match the storage context. |
| Production evidence | Maia: brand voice constraints stored during brand analysis session not retrieved during tactic execution session. Constraints and execution are stored in different analysis contexts. Deepak: competitive pricing data stored during brand analysis not retrieved during pricing strategy analysis. |
| Root cause | Single-path semantic retrieval. Information stored with analysis-context encoding that does not match execution-context queries. |
| Targeted fix | Multi-path retrieval: semantic + entity + temporal + context-namespace gating in parallel. Information linked to multiple retrieval paths at encoding time (A-MEM / MAGMA principle). |
| Most affected agent | Both Maia and Deepak (workflow-spanning intelligence that is captured in one context and needed in another). |

### Type 6 - Confabulation

| Property | Definition |
| --- | --- |
| Clinical parallel | Brain fills memory gaps with plausible fabrications presented with full confidence. The patient genuinely believes the confabulated memory. |
| Observable failure | Agent states an inferred pattern as a validated finding with equal confidence to actually validated findings. |
| Production evidence | Deepak: user cohort preference inferred from one week of data three months ago presented in current strategic synthesis with the same confidence as a finding validated across 12 weeks of data. Strategic decisions made on this basis carry real commercial risk. |
| Root cause | No Origin distinction between inferred and validated findings at storage time. No confidence decay for aging inferences. Confidence not propagated from storage through retrieval to synthesis. |
| Targeted fix | Origin classification (stated, inferred, validated, agent_generated) stored at encoding time. Confidence decay for aging inferences. Origin and confidence propagated as metadata to every retrieved memory surfaced in the LLM's context. LLM instructed to express calibrated uncertainty. |
| Most affected agent | Deepak (strategic intelligence agent where inferred patterns are presented as analytical findings). |

### Type 7 - Interference

| Property | Definition |
| --- | --- |
| Clinical parallel | Strong memories from one context contaminate or suppress memories from another. Learning French makes Spanish retrieval worse. |
| Observable failure | High-frequency patterns from dominant entity contaminate retrieval for other entities of the same type. |
| Production evidence | Maia: Brand B (casual consumer) patterns contaminate Brand A (formal B2B) tactic suggestions after extended Brand B interactions. hive: large-team utilisation patterns contaminate individual assessment for small-team employees (the "should her holiday be approved" failure - assessed against wrong baseline). |
| Root cause | No context namespace isolation. Entities of the same class (brands, people, competitors) share a retrieval space without explicit boundaries. |
| Targeted fix | Context namespaces as first-class storage primitives. Brand-level, person-level, and competitor-level namespace isolation. Cross-namespace retrieval requires explicit relevance gates with configurable thresholds. |
| Most affected agents | Maia (brand isolation) and hive (people analytics isolation) - both high-stakes, both multi-entity-within-class. |

## 5. The EngramRecord Data Model

See Diagram 1 (EngramRecord Lifecycle) for the visual flow. Every field is motivated by a specific amnesia type.

```
EngramRecord {
  // Identity - Source amnesia prevention
  id, entity_type, entity_id, user_id, session_id, channel
  source_system, source_record_id    // hive: 'Pipedrive' | 'Planhat' | 'BambooHR'...

  // Content
  content: string
  type: fact | preference | pattern | belief | action | observation
  status: active | resolved | superseded | archived

  // Context namespace - Interference + Context-binding prevention
  // See Diagram 2 (Namespace Isolation) for visual model
  namespace: NamespaceRef            // Primary entity namespace (brand, person, competitor)
  contexts: Set<ContextRef>          // Additional relevant contexts
  context_relevance: Map<ContextRef, Float[0..1]>

  // Temporal - Temporal + Retrograde prevention (Zep/Graphiti model)
  created_at, last_reinforced, valid_from, valid_until, decay_rate, superseded_at

  // Origin - Confabulation prevention (Hindsight model extended)
  Origin: stated | inferred | validated | observed | agent_action
  confidence: Float[0..1]
  validation_count: Int              // How many times re-validated
  last_validated: Timestamp?

  // Multi-path retrieval - Context-binding failure prevention
  // See Diagram 3 (Multi-Path Retrieval) for visual model
  embedding: Vector                  // Semantic path
  entity_links: Set<EntityRef>       // Entity graph path (A-MEM model)
  causal_links: Set<EngramRef>       // Causal graph path (MAGMA model)
  keywords: Set<string>              // Keyword path
  activation_contexts: Set<ContextRef>
}
```

## 6. Amnesia Score: Formal Definition and Measurement Protocol

The Amnesia Score was introduced in v1 as a probabilistic definition. The v3 revision adds the operationalisation layer: the concrete labelling procedure, sampling method, inter-rater agreement protocol, and confidence interval computation that a reviewer would need to reproduce the measurement.

### 6.1 Formal Definition

For a ground-truth memory set M_ground established in past interactions:

```
S_anterograde(A)     = P(memory NOT in store | memory established in session N)
S_retrograde(A)      = P(memory NOT retrievable | memory correctly stored)
S_source(A)          = P(wrong attribution | memory correctly retrieved)
S_temporal(A)        = P(outdated memory retrieved | updated memory in store)
S_context_binding(A) = P(wrong-context memory surfaces | correct-context memory exists)
S_confabulation(A)   = P(inferred stated with validated-fact confidence)
S_interference(A)    = P(entity A memory in entity B namespace query)

AS(A) = weighted_mean(S_1..7)
  Perfect memory: AS(A) = 0.0    Total amnesia: AS(A) = 1.0
```

#### 6.1.1 What Each Dimension Costs in Production

| Amnesia type | Maia cost | hive cost | Deepak cost |
| --- | --- | --- | --- |
| Anterograde | Session restarts. Brand brief recreated per channel. No cross-session campaign learning. | Less critical - API data re-ingested daily. Meeting log gaps possible. | Less critical - analysis re-run on fresh data each session. |
| Retrograde | Historical campaign performance unavailable for optimisation. | Resolved escalations re-surface. Past data hygiene cycles forgotten. | Historical competitive observations unavailable for trend analysis. |
| Source amnesia | Low risk - single primary source per brand. | High risk - CEO cannot verify or override intelligence without knowing its source. | Medium risk - competitive signals from multiple sources merged without attribution. |
| Temporal amnesia | Campaign strategy drift undetected. Brief outdated but retrieved as current. | Low risk - daily API refresh provides natural recency. | Existential - competitive pattern recognition requires temporal sequence. |
| Context-binding | Brand constraints not surfaced during execution. Wrong workflow context. | Moderate - operational context generally matches retrieval context. | Competitive intel captured in brand-analysis context not retrieved in pricing context. |
| Confabulation | Inferred brand preferences stated as established constraints. | High stakes - AI-generated people assessments presented as factual. | High stakes - inferred patterns stated as validated strategic findings. |
| Interference | Brand B patterns contaminate Brand A copy. | Individual assessed against wrong team baseline (holiday approval failure). | Low current risk - fewer concurrent competitor analyses than Maia's brands. |

### 6.2 Operationalisation: How to Measure Each Dimension

The following protocol specifies how to measure each Amnesia Score dimension from real agent logs. It is designed to be reproducible with any agent that produces session logs.

**Step 1 - Ground Truth Establishment**

For each agent under evaluation, establish a ground-truth memory set M_ground through a structured interaction sequence:

- **Seed session:** conduct a controlled session in which a human annotator explicitly establishes N known facts, preferences, constraints, and temporal events. For Maia: brand voice rules, active campaign briefs, client constraints (N = 20 per brand). For hive: known escalation states, data sources for specific intelligence items, individual employee contexts (N = 20 per employee). For Deepak: competitor positioning snapshots at known timestamps, validated vs hypothesised user cohort findings (N = 20 per competitive entity).
- Label each M_ground item with: content, Origin (stated | inferred | observed), creation_timestamp, entity_namespace, and whether it has been updated since creation (temporal_update: true/false).
- Record the session_id, channel, and source_system for each item. This is the attribution ground truth used for source amnesia measurement.

**Step 2 - Sampling Method**

After the seed session, conduct a stratified sample of N = 200 probe interactions per agent, designed to elicit retrieval of ground-truth memories:

| Amnesia dimension | Probe design |
| --- | --- |
| Anterograde | Probe in a different channel and session than the seed (e.g., seed on voice, probe on WhatsApp). Measure: is the seeded memory available without re-statement? |
| Retrograde | Probe 30 days after seed. No reinforcing interactions in the interim. Measure: is the seeded memory retrievable? |
| Source | After retrieval, ask the agent to attribute the intelligence: "Where did you learn this?" Compare to ground-truth source_system label. |
| Temporal | Update a seeded memory in session N+1 (e.g., competitor shifts strategy). Probe in session N+3. Measure: does the agent retrieve the updated version, the original, or both? |
| Context-binding | Probe in a different workflow context than the seed (e.g., seed in brand analysis, probe in tactic execution). Measure: is the relevant constraint surfaced? |
| Confabulation | Ask the agent to state its confidence. Measure: does the stated confidence correspond to the memory's Origin label (stated vs inferred vs validated)? |
| Interference | After N high-volume interactions in entity namespace A, probe in entity namespace B. Measure: does any entity A memory surface in entity B retrieval? |

**Step 3 - Labelling Procedure**

Each probe response is independently labelled by two annotators using the following binary decision rule per dimension:

- **Anterograde:** 1 = memory unavailable (agent re-asks for information it was given in the seed session), 0 = memory available.
- **Retrograde:** 1 = memory unavailable after 30-day gap, 0 = memory available.
- **Source:** 1 = wrong attribution or cannot attribute, 0 = correct attribution matching ground-truth source_system.
- **Temporal:** 1 = outdated version retrieved as current, 0 = updated version retrieved (or both retrieved with correct temporal ordering).
- **Context-binding:** 1 = relevant constraint not surfaced in cross-context probe, 0 = constraint correctly surfaced.
- **Confabulation:** 1 = agent states inferred memory with the same or higher confidence as validated memory, 0 = correct confidence calibration. Measured using the agent's stated confidence against the Origin label.
- **Interference:** 1 = entity A memory surfaces in entity B probe, 0 = no contamination.

**Step 4 - Inter-Rater Agreement**

Two annotators independently label each probe. Inter-rater agreement is computed using Cohen's kappa (k) per dimension. Publication threshold: k > 0.70 for all seven dimensions before results are reported. Disagreements are resolved by a third annotator using majority rule.

Why kappa > 0.70? Landis and Koch (1977) classify k 0.61-0.80 as "substantial agreement" and > 0.80 as "almost perfect." We set 0.70 as the minimum threshold for any dimension to be reported in published results. If a dimension falls below 0.70, the labelling protocol for that dimension must be revised before results are included. The confabulation dimension is expected to be the hardest to agree on and may require a calibration round before formal labelling.

**Step 5 - Score Computation and Confidence Intervals**

For each dimension, the Amnesia Score is the proportion of probes labelled 1 (failure) out of total probes for that dimension. Confidence intervals are computed using the Wilson score interval at 95% confidence level.

```python
# Per dimension d, across N_d labelled probes:
S_d = (count of failure labels) / N_d

# Wilson score 95% CI (recommended over normal approximation for proportions):
n = N_d
p_hat = S_d
z = 1.96  # 95%
centre = (p_hat + z**2 / (2 * n)) / (1 + z**2 / n)
margin = z * sqrt(p_hat * (1 - p_hat) / n + z**2 / (4 * n**2)) / (1 + z**2 / n)
CI = [centre - margin, centre + margin]

# Report as: S_temporal(Deepak) = 0.74 [95% CI: 0.67, 0.80]

# Composite Amnesia Score:
AS(A) = sum(w_d * S_d) / sum(w_d)  # weights w_d sum to 1.0
# Report with propagated uncertainty across all dimensions
```

**Step 6 - Minimum Sample Size Calculation**

To detect a difference of 0.15 in any single Amnesia Score dimension (e.g., S_temporal = 0.74 vs 0.59) with 80% power at 0.05 significance, the minimum sample size per dimension per agent is:

```python
# Two-proportion z-test (comparing Engram to baseline system):
p1 = 0.74  # baseline (e.g., Deepak without Engram temporal fix)
p2 = 0.59  # target (Deepak with Engram temporal fix)
alpha = 0.05
power = 0.80
# Result: n = 132 probes per agent per dimension per system
# With 7 dimensions, 3 agents, 2 systems: 132 * 7 * 3 * 2 = 5,544 total probes
# Achievable given Maia + hive + Deepak combined session volume
```

### 6.3 The Undeniable Experiment

The most important single experiment for publication is demonstrating that two systems with similar LongMemEval scores have meaningfully different Amnesia Score profiles. This would prove that current benchmarks are insufficient to capture production failure modes. The design:

- Select Hindsight (91.4% LongMemEval) and a strong Mem0-based baseline (expected 85-90% LongMemEval) as comparison systems.
- Run both systems on the Maia brand-isolation scenario (interference test). Hypothesis: both systems score similarly on LongMemEval but Mem0 shows S_interference > 0.5 while Hindsight shows S_interference > 0.4 - both much higher than Engram's target S_interference < 0.1.
- Run both on the Deepak temporal scenario (temporal amnesia test). Hypothesis: Hindsight and Mem0 both show S_temporal > 0.6 on the competitive change-detection task.
- Report: [System A: LongMemEval 91.4%, AS_interference 0.42, AS_temporal 0.67] vs [Engram: LongMemEval 88%, AS_interference 0.08, AS_temporal 0.19].

If this experiment succeeds, the argument is irrefutable: a system can outscore Engram on LongMemEval and still fail in production because LongMemEval does not test multi-entity interference or temporal pattern recognition. Engram trades a small performance regression for a large pathology regression.

## 7. The Three-Mechanism Model and System Architecture

See the three diagrams for visual representations:

1. **EngramRecord Lifecycle** - six pipeline stages with amnesia checkpoints
2. **Namespace Isolation** - how Maia and hive isolate entity contexts with controlled bridges
3. **Multi-Path Retrieval** - how four parallel retrieval paths converge to a co-activated memory set

| Mechanism | What it does | Primary agent failure |
| --- | --- | --- |
| Encoding | Extracts entities, facts, patterns. Creates EngramRecords with temporal, context, and Origin metadata. Associative linking at write time (A-MEM principle). | Anterograde (Maia): encoding does not propagate across channel boundaries. Source amnesia (hive): multi-source data ingested without mandatory Origin. |
| Storage | Maintains memory structure with temporal validity windows, confidence levels, context namespaces, contradiction records, status fields. | Retrograde (hive): resolved records not marked as superseded. Interference (Maia/hive): entity-class namespace isolation not implemented. |
| Retrieval | Multi-path activation: semantic + entity + temporal + namespace gate in parallel. Co-activates related memories. Synthesis in LLM forward pass. | Context-binding (Maia/Deepak): single-path retrieval fails cross-workflow intelligence. Temporal amnesia (Deepak): retrieval does not weight recency or respect validity windows. |

> **Retrieve-time synthesis principle:** There is no background reflect() step. Synthesis happens at retrieval time in the LLM's forward pass with co-activated memories in context. For hive's morning brief: CEO-context, pipeline signals, customer health data, and escalation history are co-retrieved and synthesised in the LLM's forward pass - not pre-synthesised by a background agent that does not know what the morning will actually need.

## 8. Open Questions

### Q1 - Amnesia Type Independence

Are the seven types orthogonal? Temporal amnesia may co-present with confabulation (outdated memories retrieved with high confidence). Validation: measure whether architectural fixes targeting one type produce isolated improvements on that dimension only, using the measurement protocol in Section 6.2.

### Q2 - Origin Classification Accuracy

The confabulation dimension requires classifying memories as stated vs inferred. What is the achievable precision using current LLMs on real agent session logs? If precision falls below the threshold needed for reliable S_confabulation measurement (estimated kappa < 0.70), the fallback is to treat all non-direct-assertions as inferred - accepting higher abstention rates in exchange for lower confabulation risk.

### Q3 - Retrieve-Time Synthesis at Scale

hive ingests from 10+ APIs daily. At full ingestion rate, is there a scale threshold beyond which the co-activated memory set exceeds context window limits? If so, the minimum pre-synthesis that does not reintroduce confabulation or interference risks needs to be defined. Candidate: hierarchical co-retrieval - retrieve summaries of memory clusters first, then retrieve detail within the winning cluster. This preserves the retrieve-time synthesis principle while managing context window limits.

### Q4 - hive People Analytics: Ethical Constraints on Memory

The holiday approval incident reveals that some memory should not persist. An employee's engagement level on a given week should not be a durable memory that influences assessments three months later without re-validation. Are there EngramRecord types that require mandatory short validity windows regardless of reinforcement? Should some categories of people analytics data be excluded from long-term storage? This has legal (GDPR, labour law) and ethical dimensions that the data model must accommodate - and that no current memory framework addresses.

### Q5 - Composite Amnesia Score Weights

The composite Amnesia Score uses domain-calibrated weights. These weights should reflect impact on user experience - but they are domain-dependent: temporal amnesia is existential for Deepak but lower-stakes for hive (daily API refresh provides natural recency). Should Engram define universal weights with domain-specific calibration as an extension, or should all weights be domain-specific from the start?

### Q6 - Relationship to MAGMA

MAGMA (January 2026) independently arrived at multi-relational retrieval across semantic, temporal, causal, and entity graphs - closely parallel to Engram's multi-path retrieval design. The open question: can Engram's context namespace gating and Origin propagation be layered on top of MAGMA's graph infrastructure, or do the architectures require independent implementation? If MAGMA's graph traversal can be augmented with Engram's amnesia-typed metadata, the result may outperform either system independently on the interference and confabulation dimensions.

### Q7 - Positive Transfer vs Namespace Isolation: Controlled Leakage

The interference evidence from Maia and hive motivates strict entity namespace isolation. But peer review raised a legitimate counterpoint: isolation-first sacrifices positive transfer.

Maia serves multiple brands. Strict namespace isolation prevents Brand B patterns from contaminating Brand A - but it also prevents Maia from learning cross-brand patterns that would be genuinely useful: consumer brands in fashion verticals tend to respond to visual-first campaign structures; B2B software brands tend to respond to case-study-led narratives. This cross-brand learning is not contamination - it is generalisation that makes Maia more effective across its entire portfolio.

hive's people analytics isolation prevents individual assessment contamination - but prevents detection of org-wide patterns: if multiple employees show low engagement simultaneously, that is a company-level signal, not individual data. Deepak's competitor isolation prevents one competitive analysis from contaminating another - but prevents learning that certain competitor behaviour patterns (sudden pricing changes, marketing pivots) tend to co-occur.

**The design tension:** Isolation-first prevents interference (amnesia type 7). Controlled leakage enables positive transfer. These are not simply optimisation parameters - they represent a fundamental architectural choice about the relationship between entity namespaces. Engram's current design is isolation-first with an explicit relevance gate for cross-namespace retrieval. But the gate is binary (pass/block) rather than calibrated (pass with score penalty). A calibrated gate would allow controlled leakage at a defined precision-recall tradeoff.

Three architectural options under consideration:

| Option | Mechanism | Tradeoff |
| --- | --- | --- |
| 1. Binary isolation (current) | Hard namespace boundaries. Cross-namespace retrieval blocked by default. Explicit bridge required. | Eliminates interference. Eliminates positive transfer. |
| 2. Calibrated gate | Cross-namespace retrieval allowed at a configurable relevance threshold. Memories crossing the gate carry a relevance penalty in scoring. | Reduces interference. Preserves positive transfer above the threshold. Requires empirical calibration of threshold per entity type. |
| 3. Dual memory: entity + portfolio | Two co-existing stores per entity type: isolated entity-level store (brand A, brand B separately) and shared portfolio-level store (cross-brand patterns, abstracted from identifying details). | Separates the isolation and transfer problems. Adds storage complexity. Entity store prevents interference; portfolio store enables generalisation. |

The dual memory option (Option 3) is the most promising for Maia's use case. It mirrors how a skilled human account manager operates: they maintain strict brand-specific context for each client (entity store) while accumulating cross-client pattern knowledge over their career (portfolio store). The research question: does the portfolio store introduce interference at the portfolio level, or does the abstraction step (removing identifying brand details before storage) provide sufficient isolation? This requires empirical measurement using S_interference applied at the portfolio level as well as the entity level.

## 9. Validation Plan: Three Agents, Three Datasets

The validation plan uses the three production agents as independent datasets. Every experiment uses real operational data. The measurement protocol in Section 6.2 governs all labelling and scoring.

### 9.1 Datasets

- **Maia session logs:** multi-channel, multi-brand, multi-campaign. Anonymised per Protaige data policy. Ground truth: known brand voice constraints, campaign briefs, performance data.
- **hive session logs:** API ingestion events, morning brief generations, escalation records, people analytics outputs. Ground truth: resolved vs active escalations, source attribution for intelligence signals.
- **Deepak session logs:** competitive analysis sessions, user pattern analyses, strategic syntheses. Ground truth: competitor positioning at known timestamps, validated vs inferred user cohort findings.
- **Baseline memory systems:** Mem0 and Zep on the same datasets. MAGMA comparison if implementation is feasible.

### 9.2 Five Validation Experiments

| Experiment | Research question and method |
| --- | --- |
| A - Anterograde (Maia) | Does cross-channel encoding prevent channel-boundary anterograde failure? Ground truth: brand voice profiles established in voice sessions. Metric: S_anterograde per Section 6.2. Success: S_anterograde(Engram) < 0.1 vs S_anterograde(baseline) > 0.5. |
| B - Interference (Maia brands) | Does entity namespace isolation prevent brand contamination? Ground truth: Brand A and Brand B voice profiles. Metric: S_interference per Section 6.2. Success: S_interference(Engram) < 0.5x S_interference(Mem0). |
| C - Source Origin (hive) | Does mandatory source metadata enable CEO to verify intelligence origin? Ground truth: source API for each morning brief signal. Metric: S_source per Section 6.2. Success: S_source(Engram) < 0.15. |
| D - Temporal Pattern (Deepak) | Does temporal validity window model enable competitive change detection? Ground truth: competitor positioning shifts at known timestamps. Metric: S_temporal per Section 6.2. Success: S_temporal(Engram) < 0.3 vs S_temporal(baseline) > 0.6. |
| E - The Undeniable Experiment | Do Hindsight + Mem0 show similar LongMemEval scores but different Amnesia Score profiles? Method per Section 6.3. Success: both show S_interference > 0.35 and S_temporal > 0.55 while LongMemEval scores are within 10% of each other. |

## 10. What Would Need to Happen

- Instrument the three agents with Amnesia Score measurement. Establish baseline profiles across all 7 dimensions using the Section 6.2 protocol.
- Implement EngramRecord as the shared memory schema across Maia, hive, and Deepak. Add entity namespace isolation, mandatory Origin fields, and temporal validity windows.
- Run Experiments A-E. Complete inter-rater agreement study before reporting any dimension results.
- Run Mem0, Zep, and Hindsight on the same datasets. Publish comparative amnesia profiles. Run the Undeniable Experiment (E).
- Address Q7 (positive transfer): implement and test calibrated gate (Option 2) and dual memory (Option 3) for Maia. Measure S_interference at both entity and portfolio levels.
- Open-source the Amnesia Score evaluation harness with the three-agent dataset (anonymised). Publish the benchmark separately from the Engram paper to maximise adoption.
- Publish: "Current memory systems were designed for single-user, single-channel, single-purpose assistants. Here is what happens when you deploy them in multi-entity production agents. Here is the diagnostic standard. Here is the evidence."
