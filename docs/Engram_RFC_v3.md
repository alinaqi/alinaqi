**RFC DRAFT<span class="Apple-converted-space">  </span>·<span class="Apple-converted-space">  </span>MARCH 2026<span class="Apple-converted-space">  </span>·<span class="Apple-converted-space">  </span>V3**

**Engram**

**The Amnesia Model for Agentic Memory Systems**

*A pathology-first framework for diagnosing, measuring, and eliminating memory failure in autonomous AI agents. Validated against three production agentic systems: Maia (Protaige),* <span class="s1">*hive*</span> *(*<span class="s1">*Ideaminer.io*</span>*), and Deepak (Protaige Chief of Staff). V3 adds operationalised measurement protocol, the positive transfer open question, and three system diagrams.*

\

\

|  |  |  |
|----|----|----|
| **Version** | **Date** | **Changes** |
| **v1.0** | **Dec 2025** | **Initial concept: amnesia taxonomy, RAG-Amnesia Scale, three-mechanism model, retrieve-time synthesis, context hierarchies.** |
| **v2.0** | **Mar 2026** | **Renamed to Engram. Prior research section. Three production agents (Maia,** <span class="s2">hive</span>**, Deepak) as validation dataset with observed failure evidence.** |
| **v3.0** | **Mar 2026** | **Operationalised Amnesia Score measurement protocol with concrete labelling procedure, sampling method, inter-rater agreement, and confidence intervals. New open question Q7: positive transfer vs namespace isolation tradeoff. Three system diagrams: EngramRecord lifecycle, namespace isolation architecture, multi-path retrieval.** |

\

**0.<span class="Apple-converted-space">  </span>Motivation: Three Agents, One Problem**

\

This paper is grounded in three production AI agents <span class="s1">-</span> Maia (Protaige), <span class="s1">hive</span> (<span class="s1">Ideaminer.io</span>), and Deepak (Protaige Chief of Staff) <span class="s1">-</span> that each exhibit distinct, observable memory failure patterns in daily operation. These failures are not edge cases. They are the central limitation preventing these agents from becoming more effective as their operational scope grows.

\

|  |  |  |
|----|----|----|
| **Agent** | **Role** | **Memory at stake** |
| **Maia** | **Protaige. Full marketing intelligence: brand analysis, campaign creation, execution, performance analysis, optimisation. 150+ tools. WhatsApp, voice, meetings via Recall.ai, phone.** | **Brand context across campaigns. User preference across channels. Campaign history and performance. Optimisation decisions across sessions. Client voice and constraint persistence.** |
| hive | <span class="s2">Ideaminer.io</span>**. AI-as-CEO OS: 10+ API integrations (Asana, Pipedrive, Planhat, GitHub, Render, BambooHR, SendGrid, Lemlist, ZenSurveys), meeting logs, morning briefs, escalation detection, people analytics, data hygiene. 9 sub-agents.** | **Company state continuity across daily briefs. Escalation history. Pipeline and customer health trends. People analytics without interference between individuals.** |
| **Deepak** | **Protaige. Chief of Staff: competitive intelligence, internal team analysis, user pattern analysis, strategic synthesis. Parallel to** <span class="s2">hive</span>**, optimised for analytical depth.** | **Competitive landscape over time. Team performance trends. User pattern changes across weeks and months. Strategic context across analysis sessions.** |

\

|  |
|----|
| *The Engram thesis: Memory is not a feature these agents happen to use. It is the mechanism by which they become genuinely useful rather than impressive demos. Every amnesia type in this RFC has a direct, observed manifestation in Maia,* <span class="s1">*hive*</span>*, or Deepak. The framework is distilled from production failure* <span class="s1">*-*</span> *not proposed in the abstract.* |

\

0.1<span class="Apple-converted-space">  </span>Why Existing Memory Systems Are Insufficient

Mem0, Zep, and similar systems were designed for a specific archetype: a conversational assistant that remembers user preferences across sessions. That archetype is a single user, single channel, single purpose. Our three agents violate all three constraints simultaneously.

<span class="Apple-converted-space"> </span>

|  |  |  |  |
|----|----|----|----|
| **Constraint** | **Maia** | **hive** | **Deepak** |
| Single user | No - serves multiple users across Protaige's client brands | No - serves the CEO but touches data about all employees, customers, and partners | No - synthesises intelligence from multiple internal and external sources |
| Single channel | No - WhatsApp, voice, meetings, phone, web chat | No - API integrations, meeting logs, morning briefs, alert emails | No - competitive data, team analytics, user patterns, strategic docs |
| Single purpose | No - brand analysis, campaign creation, execution, measurement, optimisation | No - pipeline, customer health, people analytics, data hygiene, escalations | No - competitive, internal, user pattern, strategic synthesis |

<span class="Apple-converted-space"> </span>

A memory system designed for single-user, single-channel, single-purpose assistants will produce interference failures in Maia (brand memories leaking across clients), source amnesia in hive (facts remembered without knowing which employee or customer they pertain to), and temporal amnesia in Deepak (competitive observations stored without timestamps, making it impossible to track how a competitor's positioning has evolved). These are not hypothetical risks. They are observed failure modes.

\

**1.<span class="Apple-converted-space">  </span>The Three Production Agents: Observed Memory Failures**

\

This section documents specific memory failure patterns from the three agents. Each maps to amnesia types defined formally in Section 4.

\

**1.1<span class="Apple-converted-space">  </span>Maia** <span class="s3">-</span> **Marketing Intelligence Agent**

<span class="s1">**Architecture:** </span>150+ tools. Claude Agent SDK core. Multi-channel: WhatsApp, voice calls, meeting participation via Recall.ai, phone. Full campaign lifecycle: brand analysis → strategy → tactic creation → execution → performance analysis → optimisation. Serves multiple client brands across Protaige's user base.

<span class="Apple-converted-space"> </span>

Memory is not incidental to Maia's function - it is constitutive of it. A Maia that cannot remember last month's campaign performance cannot optimise. A Maia that cannot remember a brand's voice constraints will generate copy that violates them. A Maia that confuses two clients' brand contexts is actively harmful.

\

**<span class="Apple-converted-space"> </span>**

<table class="t1" data-cellspacing="0" data-cellpadding="0">
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td class="td1" data-valign="middle"><p><strong>Observed failure - Anterograde: Maia correctly analyses a brand in session N and produces a detailed voice profile. In session N+1, called through a different channel (WhatsApp vs voice), the voice profile is unavailable. Maia starts the brand analysis from scratch.</strong></p>
<p>→ Root cause: Session data not persisted to long-term store. Channel-scoped memory that does not propagate across channels.</p>
<p><strong><span class="Apple-converted-space"> </span></strong></p>
<p><strong>Observed failure - Interference: Maia serves Brand A (formal, B2B software) and Brand B (casual, consumer lifestyle) for the same Protaige user. After extensive Brand B interactions, Maia's campaign suggestions for Brand A carry Brand B's casual register.</strong></p>
<p>→ Root cause: Brand contexts not isolated in separate namespaces. Shared embedding space allows Brand B's patterns to contaminate Brand A retrieval.</p>
<p><strong><span class="Apple-converted-space"> </span></strong></p>
<p><strong>Observed failure - Temporal: Campaign optimisation recommendations do not account for the fact that the baseline was established 6 weeks ago. Maia treats the original brief as current even after two rounds of performance review have updated the strategy.</strong></p>
<p>→ Root cause: No temporal validity windows on campaign context records. Strategy documents are retrieved without recency weighting.</p>
<p><strong><span class="Apple-converted-space"> </span></strong></p>
<p><strong>Observed failure - Context-binding: Maia has the correct brand voice guideline in memory but fails to surface it during tactic execution because the retrieval query (focused on creative output) does not semantically match the storage encoding (focused on brand constraints).</strong></p>
<p>→ Root cause: Single-path semantic retrieval. Brand constraints not linked to execution contexts at encoding time.</p></td>
</tr>
</tbody>
</table>

\

<span class="Apple-converted-space"> </span>

|  |  |
|----|----|
| **Amnesia type** | **Impact on Maia effectiveness** |
| Anterograde | Most critical: each new channel session or session gap starts from scratch. Maia cannot compound learning across the campaign lifecycle. |
| Interference | Most insidious: brand contamination is invisible until a client notices. High reputational risk in a multi-brand marketing context. |
| Temporal | Compounds over time: optimisation recommendations become increasingly disconnected from current strategy as temporal drift accumulates. |
| Context-binding | Limits autonomous execution: Maia has the right constraints but cannot surface them at the right moment in the workflow. |
| Confabulation | Lowest current risk: Maia's tool-use architecture reduces confabulation vs conversational agents. But inferred brand preferences presented as stated preferences are a real failure mode. |

\

**1.2<span class="Apple-converted-space">  </span>**<span class="s3">hive</span> <span class="s3">-</span> **CEO-Assistant Operating System**

**Architecture:** 10+ API integrations (Asana, Pipedrive, Planhat, GitHub, Render, BambooHR, SendGrid, Lemlist, ZenSurveys). Meeting log ingestion. Morning briefs. 9 specialised sub-agents: CEO, Customer Success, Sales, Project Management, Analyst, Advisor, Alert Detector, Reporter, Orchestrator. 47 tools. Full permission system (autonomous / requires approval / restricted).

<span class="Apple-converted-space"> </span>

hive operates at the highest stakes of the three agents: it makes or surfaces recommendations about employees, customers, pipeline, and operations. Its memory failures carry direct business and human consequences - as demonstrated in the 'should this employee's holiday be approved?' incident, where hive correctly identified low engagement but had no temporal context about why, no history of similar assessments, and no memory that this type of recommendation had caused cultural friction before.

<span class="Apple-converted-space"> </span>

<table class="t1" data-cellspacing="0" data-cellpadding="0">
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td class="td1" data-valign="middle"><p>Observed failure - Source amnesia: hive morning brief correctly surfaces a pipeline risk but cannot attribute whether the signal came from Pipedrive activity, Planhat health score, meeting transcript analysis, or a combination. The recommendation is correct; the reasoning is opaque. Human override is impossible without source tracing.</p>
<p>→ Root cause: Multi-source ingestion without Origin metadata propagation. Facts arrive from 10+ APIs and are stored without source tagging.</p>
<p><span class="Apple-converted-space"> </span></p>
<p>Observed failure - Interference (people context): hive analyses employee engagement patterns across the company. High-volume data from a large team contaminates pattern recognition for smaller teams. An employee in a 2-person team is assessed against whole-company utilisation patterns.</p>
<p>→ Root cause: No context namespace isolation for people analytics. Individual and team-level patterns share the same retrieval space.</p>
<p><span class="Apple-converted-space"> </span></p>
<p>Observed failure - Retrograde: hive loses track of escalation resolutions. An issue that was raised, escalated, and resolved re-surfaces in the morning brief three weeks later as if it were current.</p>
<p>→ Root cause: Storage decay without reinforcement. Resolved incidents are not marked as superseded; retrieval surfaces them without temporal validation.</p>
<p><span class="Apple-converted-space"> </span></p>
<p>Observed failure - Temporal: Data hygiene automation identifies stale Pipedrive leads from 2018-2022. hive can clean them up, but has no persistent memory of which leads were cleaned, why they were stale, or what patterns produced the staleness - so the same patterns can re-accumulate.</p>
<p>→ Root cause: Actions (data hygiene operations) not encoded as memory records. The agent acts but does not remember acting.</p></td>
</tr>
</tbody>
</table>

\

**<span class="Apple-converted-space"> </span>**

|  |  |
|----|----|
| **Amnesia type** | **Impact on hive effectiveness** |
| **Source amnesia** | **Most critical: a CEO-assistant that cannot explain where its intelligence came from cannot be trusted for decisions with human consequences.** |
| **Interference (people)** | **Most sensitive: people analytics requires the strictest context isolation of any agent we operate. Contamination across individuals or teams has legal and cultural implications.** |
| **Retrograde** | **Practical impact: re-surfacing resolved issues wastes executive attention and erodes trust in the morning brief format.** |
| **Temporal** | **Compounds at scale: as hive's dataset grows (more integrations, more meeting logs), temporal drift without validity windows becomes the dominant retrieval failure.** |

**<span class="Apple-converted-space"> </span>**

\

**1.3<span class="Apple-converted-space">  </span>Deepak** <span class="s3">-</span> **Chief of Staff Agent**

**Architecture:** Protaige's strategic intelligence agent. Competitive analysis (tracking competitor positioning, pricing, and product moves), internal team pattern analysis (identifying performance trends and bottlenecks), user pattern analysis (tracking how Protaige's users interact with the platform across cohorts), and strategic synthesis (connecting competitive signals to internal capabilities and user needs). Parallel architecture to hive but optimised for analytical depth over operational breadth.

<span class="Apple-converted-space"> </span>

Deepak's memory failure mode is distinct from Maia's or hive's. Its core function is temporal pattern recognition: how has a competitor's positioning shifted over the past quarter? Which user cohort behaviour change predicts churn? How does this week's internal team velocity compare to the same period last quarter? Without memory of how things were, Deepak cannot identify how things have changed - and pattern recognition without temporal context is just observation without intelligence.

<span class="Apple-converted-space"> </span>

<table class="t1" data-cellspacing="0" data-cellpadding="0">
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td class="td1" data-valign="middle"><p>Observed failure - Temporal amnesia (primary): Deepak correctly identifies that Competitor X has shifted from a feature-centric to an outcomes-centric narrative. But it cannot tell you when this shift began, what triggered it, or how it correlates with Competitor X's product releases. Every analysis starts from the current state with no accumulation of temporal signal.</p>
<p>→ Root cause: Competitive observations stored as facts, not as temporally-stamped events. No validity windows. No change-detection across observation records.</p>
<p><span class="Apple-converted-space"> </span></p>
<p>Observed failure - Confabulation: Deepak synthesises a user pattern analysis and presents 'users in this cohort consistently prefer X feature' as an established finding. The finding was inferred from a single week of behavioural data three months ago and has never been re-validated. Deepak presents it with the same confidence as a finding validated across 12 weeks of data.</p>
<p>→ Root cause: No Origin distinction between inferred patterns and validated patterns. Confidence scores not stored or propagated. Observation age not factored into retrieval ranking.</p>
<p><span class="Apple-converted-space"> </span></p>
<p>Observed failure - Context-binding: Deepak has competitive intelligence about Competitor X stored from a brand positioning analysis. When conducting a pricing strategy analysis, the relevant competitive price positioning is not retrieved because the storage encoding (brand analysis context) does not match the retrieval query (pricing strategy context).</p>
<p>→ Root cause: Intelligence stored in analysis-context silos. Multi-path retrieval not implemented - competitive facts are only retrievable in the context in which they were originally captured.</p></td>
</tr>
</tbody>
</table>

<span class="Apple-converted-space"> </span>

|  |  |
|----|----|
| **Amnesia type** | **Impact on Deepak effectiveness** |
| Temporal amnesia | Existential: Deepak's core value proposition is pattern recognition over time. Without temporal memory, it is a single-session analytical tool, not a strategic intelligence system. |
| Confabulation | High risk: strategic decisions made on inferred patterns presented as validated findings have direct commercial consequences. The confidence calibration problem is acute in an analytical agent. |
| Context-binding | Significant: intelligence captured in one analytical context (competitive analysis) should surface in related contexts (pricing, positioning, product strategy) but currently does not. |

<span class="Apple-converted-space"> </span>

1.4<span class="Apple-converted-space">  </span>Cross-Agent Memory Failure Patterns

**Examining the three agents together reveals structural failure patterns that are not agent-specific but architectural. These cross-agent patterns are the strongest evidence that the amnesia taxonomy is the right diagnostic frame:**

**<span class="Apple-converted-space"> </span>**

|  |  |  |
|----|----|----|
| **Cross-agent pattern** | **Affected agents** | **Structural cause** |
| **Channel-scoped memory that does not propagate across channels** | **Maia (WhatsApp vs voice vs meeting). hive (API data vs meeting logs). Deepak (web analysis vs meeting intelligence).** | **Memory encoded with channel as an implicit scope, not as an explicit metadata field. Cross-channel retrieval requires a namespace bridge that does not exist.** |
| **Actions not encoded as memory** | **hive (data hygiene operations). Maia (tactic execution). Deepak (competitive assessments delivered).** | **Agent-executed actions are logged for debugging but not encoded as EngramRecords. The agent knows what it decided; it does not remember that it decided it.** |
| **Confidence not propagated from encoding to synthesis** | **Maia (inferred brand preferences). Deepak (inferred user patterns). hive (multi-source intelligence synthesis).** | **Confidence is assessed at retrieval time by the LLM, not stored at encoding time and propagated. High-confidence generation masks low-confidence underlying data.** |
| **Context contamination under multi-entity load** | **Maia (brand contamination). hive (people analytics contamination). Deepak (competitor pattern contamination).** | **All three agents operate on multiple entities of the same type (brands, people, competitors) within a shared vector space. No explicit namespace isolation between entities of the same class.** |

**<span class="Apple-converted-space"> </span>**

\

**2.<span class="Apple-converted-space">  </span>The Core Reframe: From Optimisation to Diagnosis**

\

The memory systems field treats memory as a retrieval optimisation problem. The production evidence from Maia, <span class="s1">hive</span>, and Deepak demonstrates why this frame is insufficient: these agents' failures are encoding failures, storage attribution failures, temporal validity failures, and context isolation failures. Better retrieval algorithms cannot fix any of these <span class="s1">-</span> because the problem is upstream of retrieval.

\

<span class="Apple-converted-space"> </span>

|  |  |
|----|----|
| **Optimisation frame (current field)** | **Engram diagnostic frame** |
| How accurate is memory retrieval? | What type of amnesia does this agent exhibit? |
| Maximise recall on LongMemEval | Map observed failures to amnesia dimensions |
| Better retrieval algorithm | Targeted fix for specific pathology |
| Single performance score | Amnesia profile across 7 dimensions |
| System comparison on benchmark | Agent-specific amnesia diagnosis and treatment |

<span class="Apple-converted-space"> </span>

\

\

|  |
|----|
| **Amnesia Score:** 0.0 = perfect memory (no amnesia of any type). 1.0 = total amnesia. Every real system sits between these extremes across seven independent dimensions. Maia, <span class="s1">hive</span>, and Deepak each have a distinct amnesia profile <span class="s1">-</span> not a single memory quality score. The goal is to diagnose each dimension, trace it to its architectural cause, and apply the targeted fix. |

\

<span class="s4">**3.** </span>Prior Research: Standing on a Fragmented Field

\

The December 2025 survey 'Memory in the Age of AI Agents' (Zhang et al., arXiv 2512.13564) noted the field has become 'increasingly fragmented' with 'loosely defined terminologies and inconsistent taxonomies.' This section maps major prior works against Engram <span class="s1">-</span> identifying what each contributes and where it falls short for multi-entity, multi-channel, multi-purpose agents.

<span class="Apple-converted-space"> </span>

3.1<span class="Apple-converted-space">  </span>Mem0 - The Market Leader

|  |  |
|----|----|
| **Property** | **Details** |
| What it is | The dominant production memory layer. Dynamically extracts, consolidates, and retrieves memories. Graph-based variant with Conflict Detector and LLM-powered Update Resolver. \$24M Series A (October 2025). AWS exclusive memory provider for Agent SDK. 186M API calls in Q3 2025. |
| Key innovation | Memory as first-class objects with explicit write/forget operations. Conflict detection at write time. 26% accuracy improvement over plain vector retrieval; 91% lower p95 latency. |
| What Engram inherits | Memory-as-object mental model. Conflict detection as a write-time primitive. The explicit forget operation as an architectural necessity, not an afterthought. |
| Engram gap (production evidence) | Mem0's user-scoped storage helps Maia's brand contamination only partially - it scopes by user, not by brand within a user. hive's cross-source intelligence has no Origin propagation model in Mem0. Deepak's temporal pattern tracking has no validity window support. |
| Amnesia types addressed | Anterograde (strong). Source amnesia (partial). Temporal, interference, confabulation (weak). |

<span class="Apple-converted-space"> </span>

3.2<span class="Apple-converted-space">  </span>Zep / Graphiti - Temporal Knowledge Graphs

|  |  |
|----|----|
| **Property** | **Details** |
| What it is | Temporally-aware dynamic knowledge graph. Three tiers: episode, semantic entity, community subgraphs. Every graph edge carries explicit validity intervals. Temporal metadata used to update or invalidate without discarding outdated information. |
| Key innovation | Temporal validity as a first-class graph primitive. Tri-tier storage enables both episode-level retrieval and community-level pattern recognition. |
| What Engram inherits | The temporal validity window model. Preservation (not overwriting) of conflicting temporal facts. The tri-tier architecture as a model for Deepak's multi-resolution competitive intelligence. |
| Engram gap (production evidence) | Zep's temporal model is excellent for Deepak's competitive tracking use case. But context namespace isolation between brands (Maia) and between individuals (hive) is not a primary design target. The graph is structured around entities and time, not context hierarchies. |
| Amnesia types addressed | Temporal (strong). Retrograde (moderate). Context-binding, interference, source amnesia (weak). |

<span class="Apple-converted-space"> </span>

3.3<span class="Apple-converted-space">  </span>MemGPT / Letta - The OS Memory Metaphor

|  |  |
|----|----|
| **Property** | **Details** |
| What it is | OS-inspired memory hierarchy: main context as RAM, external storage as disk. Agents control memory through explicit function calls. Stateful memory server with always-available core memory blocks (goals, preferences, persona). |
| Key innovation | Memory management as an explicit agentic action. Agents decide what to remember and what to archive. Core memory blocks as reliable always-available context. |
| What Engram inherits | Core memory blocks as a model for hive's persistent CEO-context (company strategy, key relationships, ongoing escalations). The explicit archiving decision as a complement to Engram's decay-rate model. |
| Engram gap (production evidence) | Agent-controlled memory introduces a new failure mode: the agent may archive information it will need. Maia's cross-channel memory propagation requires system-level encoding, not agent-controlled archival. hive's 10+ API sources cannot rely on the agent to decide what is worth remembering - encoding must be automatic. |
| Amnesia types addressed | Working memory (strong). Anterograde (moderate). Confabulation, source amnesia, interference (weak). |

<span class="Apple-converted-space"> </span>

3.4<span class="Apple-converted-space">  </span>Hindsight - Current SOTA (December 2025)

|  |  |
|----|----|
| **Property** | **Details** |
| What it is | 91.4% on LongMemEval. Four logical networks: world (objective facts), bank (agent experiences), opinion (subjective judgments with updatable confidence), observation (preference-neutral entity summaries). |
| Key innovation | Separation of factual from opinionated memory. Confidence scores on the opinion network update as evidence accumulates. Reduces confabulation by marking beliefs as beliefs. |
| What Engram inherits | The four-network content taxonomy as a model for differentiating Deepak's intelligence types (facts vs inferences vs validated patterns). Confidence score model for Deepak's confabulation risk. |
| Engram gap (production evidence) | Hindsight's four networks are content categories, not functional mechanisms or context namespaces. They do not address Maia's brand isolation requirement or hive's source Origin requirement. The opinion network's confidence model is applied at storage time; Engram requires it to propagate through retrieval to synthesis - especially for Deepak's strategic recommendations. |
| Amnesia types addressed | Confabulation (strong). Anterograde, retrograde (moderate). Source amnesia, interference, context-binding (weak). |

<span class="Apple-converted-space"> </span>

3.5<span class="Apple-converted-space">  </span>A-MEM - Agentic Memory with Zettelkasten Linking

|  |  |
|----|----|
| **Property** | **Details** |
| What it is | A-MEM (Xu et al., Feb 2025) organises interactions into Zettelkasten-like memory units: structured notes incrementally linked and refined as new interactions occur. |
| Key innovation | Associative encoding at write time: new memories connect to existing ones, building an associative network rather than a flat store. Memory refinement as existing notes update when contradicted. |
| What Engram inherits | Associative encoding at write time as a core principle - especially relevant for Deepak's competitive intelligence, where new observations should automatically link to prior observations about the same competitor. The Zettelkasten linking model maps to Engram's entity_links and causal_links fields. |
| Engram gap (production evidence) | A-MEM's retrieval relies on semantic embedding similarity, missing temporal and causal relationships - exactly Deepak's core need. For Maia and hive, the Zettelkasten metaphor also does not provide context namespace isolation. |
| Amnesia types addressed | Anterograde (strong, via associative encoding). Retrograde (moderate). Temporal, context-binding, interference (weak). |

<span class="Apple-converted-space"> </span>

3.6<span class="Apple-converted-space">  </span>MAGMA - Multi-Graph Agentic Memory (January 2026)

|  |  |
|----|----|
| **Property** | **Details** |
| What it is | MAGMA (Jiang et al., Jan 2026, arXiv 2601.03236). Each memory item represented across four orthogonal graphs: semantic, temporal, causal, entity. Retrieval as policy-guided traversal with Adaptive Traversal Policy. 45.5% higher reasoning accuracy; 95%+ token reduction; 40% faster latency vs prior methods. |
| Key innovation | Decoupling memory representation from retrieval logic. Causal graph explicitly models cause-effect relationships. Query-adaptive traversal routes retrieval based on query intent, pruning irrelevant graph regions. |
| What Engram inherits | Multi-relational retrieval across semantic, temporal, causal, and entity dimensions independently arrived at the same design as Engram's multi-path retrieval. MAGMA's causal graph is particularly relevant for Deepak's competitive intelligence (cause-effect between competitor actions and market responses). |
| Engram gap (production evidence) | MAGMA evaluates on LongMemEval and LoCoMo - single-user, single-purpose benchmark settings. It does not model context namespace isolation (critical for Maia's brand isolation and hive's people analytics isolation). No Origin model for confabulation prevention. MAGMA optimises retrieval accuracy; Engram diagnoses which failure type each graph dimension prevents. |
| Amnesia types addressed | Context-binding (strong). Temporal (strong). Source amnesia, confabulation, interference (weak - not primary design targets). |

<span class="Apple-converted-space"> </span>

3.7<span class="Apple-converted-space">  </span>The Benchmarks and Their Blind Spots

|  |  |
|----|----|
| **Benchmark** | **What it measures and what it misses for our agents** |
| LongMemEval (Wu et al. 2024) | Measures answer accuracy on memory-requiring questions over long conversation histories. Does not measure: which amnesia type caused a failure, whether brand contamination occurred, whether source Origin was maintained, or how memory quality degrades under multi-entity load. |
| LoCoMo | Temporal and causal reasoning over long sequences. Better than LongMemEval for Deepak's use case. Still a single-user, single-purpose benchmark - does not model Maia's multi-brand scenario or hive's multi-source attribution requirement. |
| Amnesia Score (Engram proposal) | Seven-dimensional diagnostic that measures which type of failure each agent exhibits. Designed specifically for multi-entity (multi-brand, multi-user, multi-source) agents. Intended to complement LongMemEval, not replace it: a system should report both its LongMemEval score (overall performance) and its Amnesia Score profile (which failure types remain). |

<span class="Apple-converted-space"> </span>

3.8<span class="Apple-converted-space">  </span>Summary: What Engram Inherits and What Is Novel

|  |  |  |
|----|----|----|
| **Prior work** | **What Engram inherits** | **Gap filled by Engram (production evidence)** |
| Zhang et al. 2025 survey | Field landscape, three-form/function taxonomy | Pathology framework grounded in specific production agent failures |
| Mem0 | Memory-as-object, conflict detection, explicit forget | Context namespace isolation (Maia brands, hive people), temporal validity, confabulation Origin |
| Zep / Graphiti | Temporal validity windows, tri-tier graph storage | Context namespace isolation, multi-entity amnesia typing |
| MemGPT / Letta | Core memory blocks, explicit archival decisions | Automatic encoding for high-volume multi-source agents (hive), channel-scoped memory propagation (Maia) |
| Hindsight | Content taxonomy, confidence scoring on opinions | Confidence propagation through retrieval to synthesis (Deepak), context namespace model |
| A-MEM | Associative encoding at write time, Zettelkasten linking | Multi-path retrieval including temporal and causal (Deepak), context namespace isolation |
| MAGMA | Multi-relational retrieval, causal graph, query-adaptive traversal | Diagnostic framing, context namespace gating, Origin propagation, multi-entity isolation |
| LongMemEval / LoCoMo | Benchmark methodology and evaluation rigour | Amnesia Score: diagnostic complement measuring failure type, not just answer accuracy |

<span class="Apple-converted-space"> </span>

\

|  |
|----|
| *The novel contribution of Engram is not any single element. The novelty is the combination: a formal taxonomy of seven amnesia types from neuroscience, operationalised as measurable dimensions of a composite Amnesia Score, grounded in a three-mechanism functional model, with a retrieve-time synthesis principle and context hierarchy architecture. No prior work combines all of these into a diagnostic-first framework for multi-entity agents.* |

\

**4.<span class="Apple-converted-space">  </span>The Seven Amnesia Types (With Agent Evidence)**

\

Each amnesia type is now grounded in observed production failures from Maia, hive, and/or Deepak. The clinical parallel, observed failure, root cause, targeted fix, and primary agent evidence are documented for each type.

​​<span class="Apple-converted-space"> </span>

Type 1 - Anterograde Amnesia

|  |  |
|----|----|
| **Property** | **Definition** |
| Clinical parallel | Hippocampal damage prevents formation of new long-term memories. Patient HM (Henry Molaison) retained pre-surgery memories but could not form new ones after 1953. |
| Observable failure | Agent handles a session normally but retains nothing across session boundaries or channel switches. |
| Production evidence | Maia: voice session brand analysis not available in subsequent WhatsApp session. User must re-introduce the brand from scratch. |
| Root cause | Encoding pipeline does not produce durable storage records. Session data processed but not written to persistent memory store. |
| Targeted fix | Explicit end-of-session encoding. Every session produces structured EngramRecords. Cross-channel propagation with channel as metadata, not as storage namespace. |
| Most affected agent | Maia (channel-boundary anterograde is the highest-frequency failure). |

<span class="Apple-converted-space"> </span>

Type 2 - Retrograde Amnesia

|  |  |
|----|----|
| **Property** | **Definition** |
| Clinical parallel | Existing memories become inaccessible after injury or through decay. Older memories degrade even though they were once clearly formed. |
| Observable failure | Agent correctly formed memories earlier but cannot access them later. Resolved issues re-surface as current. |
| Production evidence | hive: resolved escalation from three weeks ago re-appears in morning brief as if it were current. The resolution record decayed without reinforcement. |
| Root cause | Storage decay without reinforcement. Resolved or archived items not marked as superseded. |
| Targeted fix | Decay-aware storage with status tracking. Resolved/superseded records carry explicit status fields. Retrieval filters by status before ranking by recency. |
| Most affected agent | hive (high-volume daily data creates rapid decay of historical context). |

<span class="Apple-converted-space"> </span>

Type 3 - Source Amnesia

|  |  |
|----|----|
| **Property** | **Definition** |
| Clinical parallel | The person remembers a fact but not where they learned it - who told them, in what context, how reliable the source was. |
| Observable failure | Agent retrieves a memory correctly but cannot attribute the source. Multi-source intelligence synthesised without Origin. |
| Production evidence | hive: morning brief correctly surfaces a pipeline risk but cannot tell the CEO whether the signal came from Pipedrive activity, Planhat health score, or a meeting transcript. Human override and verification are impossible. |
| Root cause | Multi-source ingestion without mandatory Origin metadata. Facts stored as content without source tags. |
| Targeted fix | Mandatory source metadata on every EngramRecord: source_system, source_record_id, ingestion_timestamp, confidence, Origin_type. |
| Most affected agent | hive (10+ source APIs with no unified Origin model). |

<span class="Apple-converted-space"> </span>

Type 4 - Temporal Amnesia

|  |  |
|----|----|
| **Property** | **Definition** |
| Clinical parallel | Loss of the time dimension of memories - events remembered but not when, or in the wrong temporal order. |
| Observable failure | Agent treats outdated information as current. Cannot detect how things have changed because it has no memory of how they were. |
| Production evidence | Deepak: competitive intelligence observations stored as facts without timestamps. Deepak cannot identify when Competitor X shifted from feature-centric to outcomes-centric narrative, only that the shift happened. Pattern recognition requires knowing the sequence and timing. |
| Root cause | Memories stored without temporal validity windows. Retrieval ignores timestamps and recency. |
| Targeted fix | Temporal validity windows on every EngramRecord (Zep/Graphiti model). Retrieval weights recency. Competing temporal facts preserved with timestamps rather than overwritten. |
| Most affected agent | Deepak (temporal pattern recognition is its core value proposition; temporal amnesia is an existential failure). |

<span class="Apple-converted-space"> </span>

Type 5 - Context-Binding Failure

|  |  |
|----|----|
| **Property** | **Definition** |
| Clinical parallel | Memory exists but is not linked to the right retrieval cue. Cannot surface the memory when it would be relevant. |
| Observable failure | Agent has the correct information in memory but cannot surface it because the retrieval context does not match the storage context. |
| Production evidence | Maia: brand voice constraints stored during brand analysis session not retrieved during tactic execution session. Constraints and execution are stored in different analysis contexts. Deepak: competitive pricing data stored during brand analysis not retrieved during pricing strategy analysis. |
| Root cause | Single-path semantic retrieval. Information stored with analysis-context encoding that does not match execution-context queries. |
| Targeted fix | Multi-path retrieval: semantic + entity + temporal + context-namespace gating in parallel. Information linked to multiple retrieval paths at encoding time (A-MEM / MAGMA principle). |
| Most affected agent | Both Maia and Deepak (workflow-spanning intelligence that is captured in one context and needed in another). |

<span class="Apple-converted-space"> </span>

Type 6 - Confabulation

|  |  |
|----|----|
| **Property** | **Definition** |
| Clinical parallel | Brain fills memory gaps with plausible fabrications presented with full confidence. The patient genuinely believes the confabulated memory. |
| Observable failure | Agent states an inferred pattern as a validated finding with equal confidence to actually validated findings. |
| Production evidence | Deepak: user cohort preference inferred from one week of data three months ago presented in current strategic synthesis with the same confidence as a finding validated across 12 weeks of data. Strategic decisions made on this basis carry real commercial risk. |
| Root cause | No Origin distinction between inferred and validated findings at storage time. No confidence decay for aging inferences. Confidence not propagated from storage through retrieval to synthesis. |
| Targeted fix | Origin classification (stated \| inferred \| validated \| agent_generated) stored at encoding time. Confidence decay for aging inferences. Origin and confidence propagated as metadata to every retrieved memory surfaced in the LLM's context. LLM instructed to express calibrated uncertainty. |
| Most affected agent | Deepak (strategic intelligence agent where inferred patterns are presented as analytical findings). |

<span class="Apple-converted-space"> </span>

Type 7 - Interference

|  |  |
|----|----|
| **Property** | **Definition** |
| Clinical parallel | Strong memories from one context contaminate or suppress memories from another. Learning French makes Spanish retrieval worse. |
| Observable failure | High-frequency patterns from dominant entity contaminate retrieval for other entities of the same type. |
| Production evidence | Maia: Brand B (casual consumer) patterns contaminate Brand A (formal B2B) tactic suggestions after extended Brand B interactions. hive: large-team utilisation patterns contaminate individual assessment for small-team employees (the 'should her holiday be approved' failure - assessed against wrong baseline). |
| Root cause | No context namespace isolation. Entities of the same class (brands, people, competitors) share a retrieval space without explicit boundaries. |
| Targeted fix | Context namespaces as first-class storage primitives. Brand-level, person-level, and competitor-level namespace isolation. Cross-namespace retrieval requires explicit relevance gates with configurable thresholds. |
| Most affected agents | Maia (brand isolation) and hive (people analytics isolation) - both high-stakes, both multi-entity-within-class. |

<span class="Apple-converted-space"> </span>

\

**5.<span class="Apple-converted-space">  </span>The EngramRecord Data Model**

\

See Diagram 1 (EngramRecord Lifecycle) for the visual flow. Every field is motivated by a specific amnesia type.

\

<table class="t1" data-cellspacing="0" data-cellpadding="0">
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td class="td1" data-valign="middle"><p>EngramRecord {</p>
<p><span class="Apple-converted-space">  </span>// Identity - Source amnesia prevention</p>
<p><span class="Apple-converted-space">  </span>id, entity_type, entity_id, user_id, session_id, channel</p>
<p><span class="Apple-converted-space">  </span>source_system, source_record_id<span class="Apple-converted-space">    </span>// hive: 'Pipedrive' | 'Planhat' | 'BambooHR'...</p>
<p><br />
</p>
<p><span class="Apple-converted-space">  </span>// Content</p>
<p><span class="Apple-converted-space">  </span>content: string</p>
<p><span class="Apple-converted-space">  </span>type: fact | preference | pattern | belief | action | observation</p>
<p><span class="Apple-converted-space">  </span>status: active | resolved | superseded | archived</p>
<p><br />
</p>
<p><span class="Apple-converted-space">  </span>// Context namespace - Interference + Context-binding prevention</p>
<p><span class="Apple-converted-space">  </span>// See Diagram 2 (Namespace Isolation) for visual model</p>
<p><span class="Apple-converted-space">  </span>namespace: NamespaceRef<span class="Apple-converted-space">            </span>// Primary entity namespace (brand, person, competitor)</p>
<p><span class="Apple-converted-space">  </span>contexts: Set&lt;ContextRef&gt;<span class="Apple-converted-space">          </span>// Additional relevant contexts</p>
<p><span class="Apple-converted-space">  </span>context_relevance: Map&lt;ContextRef, Float[0..1]&gt;</p>
<p><br />
</p>
<p><span class="Apple-converted-space">  </span>// Temporal - Temporal + Retrograde prevention (Zep/Graphiti model)</p>
<p><span class="Apple-converted-space">  </span>created_at, last_reinforced, valid_from, valid_until, decay_rate, superseded_at</p>
<p><br />
</p>
<p><span class="Apple-converted-space">  </span>// Origin - Confabulation prevention (Hindsight model extended)</p>
<p><span class="Apple-converted-space">  </span>Origin: stated | inferred | validated | observed | agent_action</p>
<p><span class="Apple-converted-space">  </span>confidence: Float[0..1]</p>
<p><span class="Apple-converted-space">  </span>validation_count: Int<span class="Apple-converted-space">              </span>// How many times re-validated</p>
<p><span class="Apple-converted-space">  </span>last_validated: Timestamp?</p>
<p><br />
</p>
<p><span class="Apple-converted-space">  </span>// Multi-path retrieval - Context-binding failure prevention</p>
<p><span class="Apple-converted-space">  </span>// See Diagram 3 (Multi-Path Retrieval) for visual model</p>
<p><span class="Apple-converted-space">  </span>embedding: Vector<span class="Apple-converted-space">                  </span>// Semantic path</p>
<p><span class="Apple-converted-space">  </span>entity_links: Set&lt;EntityRef&gt; <span class="Apple-converted-space">      </span>// Entity graph path (A-MEM model)</p>
<p><span class="Apple-converted-space">  </span>causal_links: Set&lt;EngramRef&gt; <span class="Apple-converted-space">      </span>// Causal graph path (MAGMA model)</p>
<p><span class="Apple-converted-space">  </span>keywords: Set&lt;string&gt;<span class="Apple-converted-space">              </span>// Keyword path</p>
<p><span class="Apple-converted-space">  </span>activation_contexts: Set&lt;ContextRef&gt;</p>
<p>}</p></td>
</tr>
</tbody>
</table>

\

**6.<span class="Apple-converted-space">  </span>Amnesia Score: Formal Definition and Measurement Protocol**

\

The Amnesia Score was introduced in v1 as a probabilistic definition. The v3 revision adds the operationalisation layer: the concrete labelling procedure, sampling method, inter-rater agreement protocol, and confidence interval computation that a reviewer would need to reproduce the measurement.

\

**6.1<span class="Apple-converted-space">  </span>Formal Definition**

<table class="t1" data-cellspacing="0" data-cellpadding="0">
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td class="td1" data-valign="middle"><p>For a ground-truth memory set M_ground established in past interactions:</p>
<p><br />
</p>
<p>S_anterograde(A) <span class="Apple-converted-space">    </span>= P(memory NOT in store | memory established in session N)</p>
<p>S_retrograde(A)<span class="Apple-converted-space">      </span>= P(memory NOT retrievable | memory correctly stored)</p>
<p>S_source(A)<span class="Apple-converted-space">          </span>= P(wrong attribution | memory correctly retrieved)</p>
<p>S_temporal(A)<span class="Apple-converted-space">        </span>= P(outdated memory retrieved | updated memory in store)</p>
<p>S_context_binding(A) = P(wrong-context memory surfaces | correct-context memory exists)</p>
<p>S_confabulation(A) <span class="Apple-converted-space">  </span>= P(inferred stated with validated-fact confidence)</p>
<p>S_interference(A)<span class="Apple-converted-space">    </span>= P(entity A memory in entity B namespace query)</p>
<p><br />
</p>
<p>AS(A) = weighted_mean(S_1..7)</p>
<p><span class="Apple-converted-space">  </span>Perfect memory: AS(A) = 0.0<span class="Apple-converted-space">    </span>Total amnesia: AS(A) = 1.0</p></td>
</tr>
</tbody>
</table>

\

6.1.1<span class="Apple-converted-space">  </span>What Each Dimension Costs in Production

|  |  |  |  |
|----|----|----|----|
| **Amnesia type** | **Maia cost** | **hive cost** | **Deepak cost** |
| Anterograde | Session restarts. Brand brief recreated per channel. No cross-session campaign learning. | Less critical - API data re-ingested daily. Meeting log gaps possible. | Less critical - analysis re-run on fresh data each session. |
| Retrograde | Historical campaign performance unavailable for optimisation. | Resolved escalations re-surface. Past data hygiene cycles forgotten. | Historical competitive observations unavailable for trend analysis. |
| Source amnesia | Low risk - single primary source per brand. | High risk - CEO cannot verify or override intelligence without knowing its source. | Medium risk - competitive signals from multiple sources merged without attribution. |
| Temporal amnesia | Campaign strategy drift undetected. Brief outdated but retrieved as current. | Low risk - daily API refresh provides natural recency. | Existential - competitive pattern recognition requires temporal sequence. |
| Context-binding | Brand constraints not surfaced during execution. Wrong workflow context. | Moderate - operational context generally matches retrieval context. | Competitive intel captured in brand-analysis context not retrieved in pricing context. |
| Confabulation | Inferred brand preferences stated as established constraints. | High stakes - AI-generated people assessments presented as factual. | High stakes - inferred patterns stated as validated strategic findings. |
| Interference | Brand B patterns contaminate Brand A copy. | Individual assessed against wrong team baseline (holiday approval failure). | Low current risk - fewer concurrent competitor analyses than Maia's brands. |

<span class="Apple-converted-space"> </span>

\

**6.2<span class="Apple-converted-space">  </span>Operationalisation: How to Measure Each Dimension**

The following protocol specifies how to measure each Amnesia Score dimension from real agent logs. It is designed to be reproducible with any agent that produces session logs.

\

**Step 1** <span class="s3">-</span> **Ground Truth Establishment**

For each agent under evaluation, establish a ground-truth memory set M_ground through a structured interaction sequence:

\

- ****<span class="s5">***Seed session: conduct a controlled session in which a human annotator explicitly establishes N known facts, preferences, constraints, and temporal events. For Maia: brand voice rules, active campaign briefs, client constraints (N = 20 per brand). For*** </span><span class="s3">hive</span><span class="s5">***: known escalation states, data sources for specific intelligence items, individual employee contexts (N = 20 per employee). For Deepak: competitor positioning snapshots at known timestamps, validated vs hypothesised user cohort findings (N = 20 per competitive entity).***</span>
- ****<span class="s5">***Label each M_ground item with: content,*** </span><span class="s3">Origin</span><span class="s5"> ***(stated \| inferred \| observed), creation_timestamp, entity_namespace, and whether it has been updated since creation (temporal_update: true/false).***</span>
- ****<span class="s5">***Record the session_id, channel, and source_system for each item. This is the attribution ground truth used for source amnesia measurement.***</span>

\

**Step 2** <span class="s3">-</span> **Sampling Method**

After the seed session, conduct a stratified sample of N = 200 probe interactions per agent, designed to elicit retrieval of ground-truth memories:

\

|  |  |
|----|----|
| **Amnesia dimension** | **Probe design** |
| **Anterograde** | **Probe in a different channel and session than the seed (e.g., seed on voice, probe on WhatsApp). Measure: is the seeded memory available without re-statement?** |
| **Retrograde** | **Probe 30 days after seed. No reinforcing interactions in the interim. Measure: is the seeded memory retrievable?** |
| **Source** | **After retrieval, ask the agent to attribute the intelligence: 'Where did you learn this?' Compare to ground-truth source_system label.** |
| **Temporal** | **Update a seeded memory in session N+1 (e.g., competitor shifts strategy). Probe in session N+3. Measure: does the agent retrieve the updated version, the original, or both?** |
| **Context-binding** | **Probe in a different workflow context than the seed (e.g., seed in brand analysis, probe in tactic execution). Measure: is the relevant constraint surfaced?** |
| **Confabulation** | **Ask the agent to state its confidence. Measure: does the stated confidence correspond to the memory's** <span class="s2">Origin</span> **label (stated vs inferred vs validated)?** |
| **Interference** | **After N high-volume interactions in entity namespace A, probe in entity namespace B. Measure: does any entity A memory surface in entity B retrieval?** |

\

**Step 3** <span class="s3">-</span> **Labelling Procedure**

Each probe response is independently labelled by two annotators using the following binary decision rule per dimension:

\

- ****<span class="s5">***Anterograde: 1 = memory unavailable (agent re-asks for information it was given in the seed session), 0 = memory available.***</span>
- ****<span class="s5">***Retrograde: 1 = memory unavailable after 30-day gap, 0 = memory available.***</span>
- ****<span class="s5">***Source: 1 = wrong attribution or cannot attribute, 0 = correct attribution matching ground-truth source_system.***</span>
- ****<span class="s5">***Temporal: 1 = outdated version retrieved as current, 0 = updated version retrieved (or both retrieved with correct temporal ordering).***</span>
- ****<span class="s5">***Context-binding: 1 = relevant constraint not surfaced in cross-context probe, 0 = constraint correctly surfaced.***</span>
- ****<span class="s5">***Confabulation: 1 = agent states inferred memory with the same or higher confidence as validated memory, 0 = correct confidence calibration. Measured using the agent's stated confidence against the*** </span><span class="s3">Origin</span><span class="s5"> ***label.***</span>
- ****<span class="s5">***Interference: 1 = entity A memory surfaces in entity B probe, 0 = no contamination.***</span>

\

**Step 4** <span class="s3">-</span> **Inter-Rater Agreement**

Two annotators independently label each probe. Inter-rater agreement is computed using Cohen's kappa (κ) per dimension. Publication threshold: κ \> 0.70 for all seven dimensions before results are reported. Disagreements are resolved by a third annotator using majority rule.

\

|  |
|----|
| Why kappa \> 0.70? Landis and Koch (1977) classify κ 0.61–0.80 as 'substantial agreement' and \> 0.80 as 'almost perfect.' We set 0.70 as the minimum threshold for any dimension to be reported in published results. If a dimension falls below 0.70, the labelling protocol for that dimension must be revised before results are included. The confabulation dimension is expected to be the hardest to agree on and may require a calibration round before formal labelling. |

\

**Step 5** <span class="s3">-</span> **Score Computation and Confidence Intervals**

For each dimension, the Amnesia Score is the proportion of probes labelled 1 (failure) out of total probes for that dimension. Confidence intervals are computed using the Wilson score interval at 95% confidence level.

\

<table class="t1" data-cellspacing="0" data-cellpadding="0">
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td class="td1" data-valign="middle"><p># Per dimension d, across N_d labelled probes:</p>
<p>S_d = (count of failure labels) / N_d</p>
<p><br />
</p>
<p># Wilson score 95% CI (recommended over normal approximation for proportions):</p>
<p>n = N_d</p>
<p>p_hat = S_d</p>
<p>z = 1.96<span class="Apple-converted-space">  </span># 95%</p>
<p>centre = (p_hat + z²/(2n)) / (1 + z²/n)</p>
<p>margin = z * sqrt(p_hat*(1-p_hat)/n + z²/(4n²)) / (1 + z²/n)</p>
<p>CI = [centre - margin, centre + margin]</p>
<p><br />
</p>
<p># Report as: S_temporal(Deepak) = 0.74 [95% CI: 0.67, 0.80]</p>
<p><br />
</p>
<p># Composite Amnesia Score:</p>
<p>AS(A) = sum(w_d * S_d) / sum(w_d)<span class="Apple-converted-space">  </span># weights w_d sum to 1.0</p>
<p># Report with propagated uncertainty across all dimensions</p></td>
</tr>
</tbody>
</table>

\

**Step 6** <span class="s3">-</span> **Minimum Sample Size Calculation**

To detect a difference of 0.15 in any single Amnesia Score dimension (e.g., S_temporal = 0.74 vs 0.59) with 80% power at 0.05 significance, the minimum sample size per dimension per agent is:

\

<table class="t1" data-cellspacing="0" data-cellpadding="0">
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td class="td1" data-valign="middle"><p># Two-proportion z-test (comparing Engram to baseline system):</p>
<p>p1 = 0.74<span class="Apple-converted-space">  </span># baseline (e.g., Deepak without Engram temporal fix)</p>
<p>p2 = 0.59<span class="Apple-converted-space">  </span># target (Deepak with Engram temporal fix)</p>
<p>alpha = 0.05, power = 0.80</p>
<p># Result: n = 132 probes per agent per dimension per system</p>
<p># With 7 dimensions, 3 agents, 2 systems: 132 * 7 * 3 * 2 = 5,544 total probes</p>
<p># Achievable given Maia + hive + Deepak combined session volume</p></td>
</tr>
</tbody>
</table>

\

**6.3<span class="Apple-converted-space">  </span>The Undeniable Experiment<span class="Apple-converted-space"> </span>**

The most important single experiment for publication is demonstrating that two systems with similar LongMemEval scores have meaningfully different Amnesia Score profiles. This would prove that current benchmarks are insufficient to capture production failure modes. The design:

\

- ****<span class="s5">***Select Hindsight (91.4% LongMemEval) and a strong Mem0-based baseline (expected 85-90% LongMemEval) as comparison systems.***</span>
- ****<span class="s5">***Run both systems on the Maia brand-isolation scenario (interference test). Hypothesis: both systems score similarly on LongMemEval but Mem0 shows S_interference \> 0.5 while Hindsight shows S_interference \> 0.4*** </span><span class="s3">-</span><span class="s5"> ***both much higher than Engram's target S_interference \< 0.1.***</span>
- ****<span class="s5">***Run both on the Deepak temporal scenario (temporal amnesia test). Hypothesis: Hindsight and Mem0 both show S_temporal \> 0.6 on the competitive change-detection task.***</span>
- ****<span class="s5">***Report: \[System A: LongMemEval 91.4%, AS_interference 0.42, AS_temporal 0.67\] vs \[Engram: LongMemEval 88%, AS_interference 0.08, AS_temporal 0.19\].***</span>

\

|  |
|----|
| *If this experiment succeeds, the argument is irrefutable: a system can outscore Engram on LongMemEval and still fail in production because LongMemEval does not test multi-entity interference or temporal pattern recognition. Engram trades a small performance regression for a large pathology regression.* |

\

**7.<span class="Apple-converted-space">  </span>The Three-Mechanism Model and System Architecture**

\

See the three diagrams for visual representations:<span class="Apple-converted-space"> </span>

\(1\) EngramRecord Lifecycle <span class="s1">-</span> six pipeline stages with amnesia checkpoints

<span class="Apple-converted-space"> </span>

\(2\) Namespace Isolation <span class="s1">-</span> how Maia and <span class="s1">hive</span> isolate entity contexts with controlled bridges,<span class="Apple-converted-space"> </span>

\

\(3\) Multi-Path Retrieval <span class="s1">-</span> how four parallel retrieval paths converge to a co-activated memory set.

\

\

|  |  |  |
|----|----|----|
| **Mechanism** | **What it does** | **Primary agent failure** |
| **Encoding** | **Extracts entities, facts, patterns. Creates EngramRecords with temporal, context, and** <span class="s2">Origin</span> **metadata. Associative linking at write time (A-MEM principle).** | **Anterograde (Maia): encoding does not propagate across channel boundaries. Source amnesia (**<span class="s2">hive</span>**): multi-source data ingested without mandatory** <span class="s2">Origin</span>**.** |
| **Storage** | **Maintains memory structure with temporal validity windows, confidence levels, context namespaces, contradiction records, status fields.** | **Retrograde (**<span class="s2">hive</span>**): resolved records not marked as superseded. Interference (Maia/**<span class="s2">hive</span>**): entity-class namespace isolation not implemented.** |
| **Retrieval** | **Multi-path activation: semantic + entity + temporal + namespace gate in parallel. Co-activates related memories. Synthesis in LLM forward pass.** | **Context-binding (Maia/Deepak): single-path retrieval fails cross-workflow intelligence. Temporal amnesia (Deepak): retrieval does not weight recency or respect validity windows.** |

\

|  |
|----|
| *Retrieve-time synthesis principle: There is no background reflect() step. Synthesis happens at retrieval time in the LLM's forward pass with co-activated memories in context. For* <span class="s1">*hive*</span>*'s morning brief: CEO-context, pipeline signals, customer health data, and escalation history are co-retrieved and synthesised in the LLM's forward pass* <span class="s1">*-*</span> *not pre-synthesised by a background agent that does not know what the morning will actually need.* |

\

**8.<span class="Apple-converted-space">  </span>Open Questions**

\

\

**Q1** <span class="s3">-</span> **Amnesia Type Independence**

Are the seven types orthogonal? Temporal amnesia may co-present with confabulation (outdated memories retrieved with high confidence). Validation: measure whether architectural fixes targeting one type produce isolated improvements on that dimension only, using the measurement protocol in Section 6.2.

\

**Q2** <span class="s3">-</span> <span class="s3">Origin</span> **Classification Accuracy**

The confabulation dimension requires classifying memories as stated vs inferred. What is the achievable precision using current LLMs on real agent session logs? If precision falls below the threshold needed for reliable S_confabulation measurement (estimated kappa \< 0.70), the fallback is to treat all non-direct-assertions as inferred <span class="s1">-</span> accepting higher abstention rates in exchange for lower confabulation risk.

\

**Q3** <span class="s3">-</span> **Retrieve-Time Synthesis at Scale**

<span class="s1">hive</span> ingests from 10+ APIs daily. At full ingestion rate, is there a scale threshold beyond which the co-activated memory set exceeds context window limits? If so, the minimum pre-synthesis that does not reintroduce confabulation or interference risks needs to be defined. Candidate: hierarchical co-retrieval <span class="s1">-</span> retrieve summaries of memory clusters first, then retrieve detail within the winning cluster. This preserves the retrieve-time synthesis principle while managing context window limits.

\

**Q4** <span class="s3">-</span> <span class="s3">hive</span> **People Analytics: Ethical Constraints on Memory**

The holiday approval incident reveals that some memory should not persist. An employee's engagement level on a given week should not be a durable memory that influences assessments three months later without re-validation. Are there EngramRecord types that require mandatory short validity windows regardless of reinforcement? Should some categories of people analytics data be excluded from long-term storage? This has legal (GDPR, labour law) and ethical dimensions that the data model must accommodate <span class="s1">-</span> and that no current memory framework addresses.

\

**Q5** <span class="s3">-</span> **Composite Amnesia Score Weights**

The composite Amnesia Score uses domain-calibrated weights. These weights should reflect impact on user experience <span class="s1">-</span> but they are domain-dependent: temporal amnesia is existential for Deepak but lower-stakes for <span class="s1">hive</span> (daily API refresh provides natural recency). Should Engram define universal weights with domain-specific calibration as an extension, or should all weights be domain-specific from the start?

\

**Q6** <span class="s3">-</span> **Relationship to MAGMA**

MAGMA (January 2026) independently arrived at multi-relational retrieval across semantic, temporal, causal, and entity graphs <span class="s1">-</span> closely parallel to Engram's multi-path retrieval design. The open question: can Engram's context namespace gating and <span class="s1">Origin</span> propagation be layered on top of MAGMA's graph infrastructure, or do the architectures require independent implementation? If MAGMA's graph traversal can be augmented with Engram's amnesia-typed metadata, the result may outperform either system independently on the interference and confabulation dimensions.

\

**Q7** <span class="s3">-</span> **Positive Transfer vs Namespace Isolation: Controlled Leakage**

The interference evidence from Maia and <span class="s1">hive</span> motivates strict entity namespace isolation. But peer review raised a legitimate counterpoint: isolation-first sacrifices positive transfer.

\

Maia serves multiple brands. Strict namespace isolation prevents Brand B patterns from contaminating Brand A <span class="s1">-</span> but it also prevents Maia from learning cross-brand patterns that would be genuinely useful: consumer brands in fashion verticals tend to respond to visual-first campaign structures; B2B software brands tend to respond to case-study-led narratives. This cross-brand learning is not contamination <span class="s1">-</span> it is generalisation that makes Maia more effective across its entire portfolio.

\

<span class="s1">hive</span>'s people analytics isolation prevents individual assessment contamination <span class="s1">-</span> but prevents detection of org-wide patterns: if multiple employees show low engagement simultaneously, that is a company-level signal, not individual data. Deepak's competitor isolation prevents one competitive analysis from contaminating another <span class="s1">-</span> but prevents learning that certain competitor behaviour patterns (sudden pricing changes, marketing pivots) tend to co-occur.

\

|  |
|----|
| **The design tension:** Isolation-first prevents interference (amnesia type 7). Controlled leakage enables positive transfer. These are not simply optimisation parameters <span class="s1">-</span> they represent a fundamental architectural choice about the relationship between entity namespaces. Engram's current design is isolation-first with an explicit relevance gate for cross-namespace retrieval. But the gate is binary (pass/block) rather than calibrated (pass with score penalty). A calibrated gate would allow controlled leakage at a defined precision-recall tradeoff. |

\

Three architectural options under consideration:

\

|  |  |  |
|----|----|----|
| **Option** | **Mechanism** | **Tradeoff** |
| **1. Binary isolation (current)** | **Hard namespace boundaries. Cross-namespace retrieval blocked by default. Explicit bridge required.** | **Eliminates interference. Eliminates positive transfer.** |
| **2. Calibrated gate** | **Cross-namespace retrieval allowed at a configurable relevance threshold. Memories crossing the gate carry a relevance penalty in scoring.** | **Reduces interference. Preserves positive transfer above the threshold. Requires empirical calibration of threshold per entity type.** |
| **3. Dual memory: entity + portfolio** | **Two co-existing stores per entity type: isolated entity-level store (brand A, brand B separately) and shared portfolio-level store (cross-brand patterns, abstracted from identifying details).** | **Separates the isolation and transfer problems. Adds storage complexity. Entity store prevents interference; portfolio store enables generalisation.** |

\

The dual memory option (Option 3) is the most promising for Maia's use case. It mirrors how a skilled human account manager operates: they maintain strict brand-specific context for each client (entity store) while accumulating cross-client pattern knowledge over their career (portfolio store). The research question: does the portfolio store introduce interference at the portfolio level, or does the abstraction step (removing identifying brand details before storage) provide sufficient isolation? This requires empirical measurement using S_interference applied at the portfolio level as well as the entity level.

\

**9.<span class="Apple-converted-space">  </span>Validation Plan: Three Agents, Three Datasets**

\

The validation plan uses the three production agents as independent datasets. Every experiment uses real operational data. The measurement protocol in Section 6.2 governs all labelling and scoring.

\

**9.1<span class="Apple-converted-space">  </span>Datasets**

- ****<span class="s5">***Maia session logs: multi-channel, multi-brand, multi-campaign. Anonymised per Protaige data policy. Ground truth: known brand voice constraints, campaign briefs, performance data.***</span>
- <span class="s3">hive</span><span class="s5"> ***session logs: API ingestion events, morning brief generations, escalation records, people analytics outputs. Ground truth: resolved vs active escalations, source attribution for intelligence signals.***</span>
- ****<span class="s5">***Deepak session logs: competitive analysis sessions, user pattern analyses, strategic syntheses. Ground truth: competitor positioning at known timestamps, validated vs inferred user cohort findings.***</span>
- ****<span class="s5">***Baseline memory systems: Mem0 and Zep on the same datasets. MAGMA comparison if implementation is feasible.***</span>

\

**9.2<span class="Apple-converted-space">  </span>Five Validation Experiments**

|  |  |
|----|----|
| **Experiment** | **Research question and method** |
| **A** <span class="s2">-</span> **Anterograde (Maia)** | **Does cross-channel encoding prevent channel-boundary anterograde failure? Ground truth: brand voice profiles established in voice sessions. Metric: S_anterograde per Section 6.2. Success: S_anterograde(Engram) \< 0.1 vs S_anterograde(baseline) \> 0.5.** |
| **B** <span class="s2">-</span> **Interference (Maia brands)** | **Does entity namespace isolation prevent brand contamination? Ground truth: Brand A and Brand B voice profiles. Metric: S_interference per Section 6.2. Success: S_interference(Engram) \< 0.5x S_interference(Mem0).** |
| **C** <span class="s2">-</span> **Source** <span class="s2">Origin</span> **(**<span class="s2">hive</span>**)** | **Does mandatory source metadata enable CEO to verify intelligence origin? Ground truth: source API for each morning brief signal. Metric: S_source per Section 6.2. Success: S_source(Engram) \< 0.15.** |
| **D** <span class="s2">-</span> **Temporal Pattern (Deepak)** | **Does temporal validity window model enable competitive change detection? Ground truth: competitor positioning shifts at known timestamps. Metric: S_temporal per Section 6.2. Success: S_temporal(Engram) \< 0.3 vs S_temporal(baseline) \> 0.6.** |
| **E** <span class="s2">-</span> **The Undeniable Experiment** | **Do Hindsight + Mem0 show similar LongMemEval scores but different Amnesia Score profiles? Method per Section 6.3. Success: both show S_interference \> 0.35 and S_temporal \> 0.55 while LongMemEval scores are within 10% of each other.** |

\

**10.<span class="Apple-converted-space">  </span>What Would Need to Happen**

\

- ****<span class="s5">***Instrument the three agents with Amnesia Score measurement. Establish baseline profiles across all 7 dimensions using the Section 6.2 protocol.***</span>
- ****<span class="s5">***Implement EngramRecord as the shared memory schema across Maia,*** </span><span class="s3">hive</span><span class="s5">***, and Deepak. Add entity namespace isolation, mandatory*** </span><span class="s3">Origin</span><span class="s5"> ***fields, and temporal validity windows.***</span>
- ****<span class="s5">***Run Experiments A–E. Complete inter-rater agreement study before reporting any dimension results.***</span>
- ****<span class="s5">***Run Mem0, Zep, and Hindsight on the same datasets. Publish comparative amnesia profiles. Run the Undeniable Experiment (E).***</span>
- ****<span class="s5">***Address Q7 (positive transfer): implement and test calibrated gate (Option 2) and dual memory (Option 3) for Maia. Measure S_interference at both entity and portfolio levels.***</span>
- ****<span class="s5">***Open-source the Amnesia Score evaluation harness with the three-agent dataset (anonymised). Publish the benchmark separately from the Engram paper to maximise adoption.***</span>
- ****<span class="s5">***Publish: 'Current memory systems were designed for single-user, single-channel, single-purpose assistants. Here is what happens when you deploy them in multi-entity production agents. Here is the diagnostic standard. Here is the evidence.'***</span>

\

\
