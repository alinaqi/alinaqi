# Lexon: Semantic Tool Binding for Adaptive Agentic Systems

**RFC v1.0 -- April 2026**

**Agent Architecture Series**

> Companion RFC to: **Mnemos** (Task-Scoped Agentic Memory), **iCPG** (Intent Computation Graph), **Engram** (Cross-Session Memory)

**INTERNAL RESEARCH DOCUMENT -- NOT FOR EXTERNAL DISTRIBUTION**

---

## Abstract

Large language model agents are increasingly expected to operate over large, dynamic tool registries -- sets of dozens to hundreds of callable functions, APIs, and capabilities. As tool count grows, a fundamental accuracy problem emerges: presenting all tools simultaneously overwhelms the model's context window, degrades selection accuracy, and produces tool call hallucinations. Existing approaches address fragments of this problem in isolation: embedding-based retrieval improves recall, tree-search planning improves multi-step accuracy, and reflection loops recover from errors. None addresses the full problem as encountered in production agentic systems: how does an agent correctly select and invoke tools when users speak in their own language, their own vocabulary, and their own terminology -- which may diverge substantially from the vocabulary in which tools are described?

This RFC introduces **Lexon** -- a semantic tool binding architecture for adaptive agentic systems. Lexon defines a two-tier routing pipeline, a multilingual embedding substrate, a structured disambiguation protocol, and a personalization layer that learns from user behavior over time. It is designed as a first-class component of the agent runtime and integrates directly with Mnemos (task memory), iCPG (intent computation), and Engram (cross-session memory) to form a complete agentic cognitive stack.

---

## 0. Position in the RFC Family

Lexon is the fourth RFC in the Agent Architecture Series. The series addresses distinct but interdependent problems in agentic cognition:

| RFC | Name | Scope | Key Concept |
| --- | --- | --- | --- |
| iCPG | Intent Computation Graph | How agents reason about what to do | Intent as a typed graph of goals and constraints |
| Mnemos | Agentic Task Memory | What agents remember within a task | Typed node lifecycle: Goal to Handoff |
| Engram | Cross-Session Memory | What agents carry across sessions | Memory pathology diagnosis + EngramRecord encoding |
| Lexon | Semantic Tool Binding | How agents find and invoke the right tool | Intent-to-tool resolution with adaptive personalization |

Mnemos governs what an agent remembers within a task. Engram governs what survives across sessions. iCPG governs how intent is structured and propagated. Lexon governs how intent reaches the correct tool. Without Lexon, the other three RFCs operate on a correctly understood, correctly remembered intent -- but then fail at the point of execution because the wrong capability is invoked.

### The Core Claim

> Tool selection is not an execution problem. It is a semantic translation problem. The gap between what a user intends and what a tool is named is the primary source of agentic tool hallucination in production systems. Lexon closes that gap.

---

## 1. Problem Statement

### 1.1 The Tool Overload Failure Mode

The naive approach to tool-enabled agents -- providing all available tool definitions in the system prompt -- degrades predictably as tool count grows. At 5-10 tools, models perform adequately. At 20-30 tools, confusion between similar-sounding tools begins to appear. At 50+ tools, accuracy collapses: the model selects plausible-sounding but incorrect tools, hallucinates parameter values, or conflates capabilities that share surface-level vocabulary.

This failure mode is structural, not a model quality issue. It reflects the fundamental tension between the breadth of capability a production agent must expose and the depth of reasoning a model can apply across a dense, undifferentiated tool list.

### 1.2 The Vocabulary Gap

Even with retrieval-based approaches that reduce the presented tool set, a second failure mode persists: the gap between how tools are described and how users actually speak. Tool descriptions are written by engineers in product vocabulary. Users speak in their own vocabulary -- shaped by their industry, their role, their language, and their personal shorthand.

Consider a concrete example from the Protaige context: the system exposes a `create_campaign` tool. A user says "I want to set up an email campaign." A naive retriever returns `create_campaign` correctly -- but another user says "I want to blast my leads" and the retriever, finding no lexical overlap, returns `bulk_email_send`. A third user says "Ich mochte eine Kampagne erstellen" and the English-only retriever fails entirely.

These are not edge cases. In a multilingual, multi-role product with sophisticated users, vocabulary divergence is the norm, not the exception.

### 1.3 The Ambiguity Problem

Beyond vocabulary gaps, genuine semantic ambiguity exists -- cases where a user phrase could legitimately map to multiple tools depending on context. "Create an email campaign" might mean a Protaige multi-step campaign sequence or a one-time bulk email send, depending on whether the user has a contact record active, what they did in the previous turn, and what their role in the organization is.

Silent resolution of ambiguous intents -- guessing the most likely tool -- produces errors that are particularly damaging: the agent confidently executes the wrong action, requiring user correction and eroding trust. Ambiguity must be surfaced, not suppressed.

### 1.4 The Personalization Gap

Tool registries are static. Users are not. Over time, individual users develop idiosyncratic shorthand, team-specific terminology, and workflow patterns that are invisible to a system relying solely on global tool descriptions. A user who consistently refers to "morning sequences" means a specific type of campaign. A team that calls lead outreach "pinging" means WhatsApp message. A sales organization that says "the blast" means a specific bulk send configuration. None of this is learnable from tool descriptions alone.

---

## 2. Prior Work and Limitations

The tool selection problem has received substantial research attention since 2023. We survey the primary approaches and identify the gaps that motivate Lexon.

| Approach | Key Paper / System | What It Does Well | What It Does Not Address |
| --- | --- | --- | --- |
| Full Context Loading | OpenAI Function Calling, early MCP | Simple, zero routing overhead | Context overload, accuracy collapses at 20+ tools |
| Embedding-Based Tool RAG | RAG-MCP (Anthropic, 2025) | Boosts accuracy 13% to 43% in large toolsets; halves prompt size | Lexical similarity gap; no personalization; English-only |
| Fine-Tuned Tool LLM | Gorilla (Berkeley, 2023) | Handles 1,600+ APIs; outperforms GPT-4 on API benchmarks | Requires retraining per toolset; no user personalization |
| Graph-Based Tool Retrieval | COLT (2024), Graph RAG-Tool Fusion (2025) | Models inter-tool relationships and collaborative dependencies | High complexity; no multilingual support; no user learning |
| Agentic Tool Discovery | MCP-Zero (2025) | Agent initiates tool discovery rather than passive ingestion | Adds latency; no synonym system; no multilingual support |
| Tree Search Planning | ToolTree (ICLR 2026) | +10% accuracy via MCTS-style pre/post evaluation and pruning | Addresses planning, not retrieval; no disambiguation layer |
| Reflection + Error Recovery | Tool-MVR (2025) | +24% accuracy via self-correction loops after failed tool calls | Corrects errors post-hoc; does not prevent mis-selection upstream |
| Lexon (This RFC) | Agent Architecture, 2026 | Two-tier routing (fast LLM + vector), multilingual embeddings, adaptive per-user personalization, confidence-gated clarification | First unified architecture combining retrieval, disambiguation, personalization, and multilingual support |

### 2.1 What the Literature Establishes

Three findings from prior work are well-established and Lexon adopts them as foundations rather than re-deriving them. First, retrieval-based tool selection reliably outperforms full-context loading at scale -- the RAG-MCP result (13% to 43% accuracy) is robust and directionally consistent across multiple papers. Second, tool descriptions written as example queries outperform descriptions written as functional summaries when used as embedding targets (Tool2Vec, 2024). Third, multi-step tool planning benefits from search-based approaches rather than greedy single-step selection (ToolTree, ICLR 2026).

### 2.2 What the Literature Does Not Address

No existing system addresses the combination of multilingual support, per-user personalization, and adaptive synonym learning in a unified production architecture. The research community has treated these as orthogonal problems. Lexon's contribution is a unified design that treats them as facets of a single semantic translation problem -- mapping diverse, multilingual, personalized user intent onto a typed tool registry -- and provides a coherent architecture for solving it.

---

## 3. The Lexon Architecture

Lexon is organized into five layers that operate sequentially on each incoming user message. The layers are: Language Normalization, Two-Tier Routing, Disambiguation, Execution, and Feedback. A sixth, persistent layer -- the Personalization Store -- is written by the Feedback layer and read by the Routing layer.

### 3.1 Layer 0: The LexonRecord

Before describing the pipeline, we define the primitive. A LexonRecord is the unit of semantic binding -- the data structure that flows through all five layers and is ultimately stored in the Personalization Store.

**LexonRecord Schema**

```json
{
  "lexon_id":          "string (uuid)",
  "phrase":            "string (original user phrase, pre-translation)",
  "phrase_normalized": "string (post-translation, lowercased)",
  "language":          "string (ISO 639-1 detected language)",
  "is_mixed":          "boolean (code-switching detected)",
  "candidate_tools":   "ToolCandidate[] [{tool_name, score, source}]",
  "selected_tool":     "string | null (null if clarification required)",
  "confidence":        "float (0.0-1.0)",
  "was_clarified":     "boolean",
  "correction":        "string | null (if user corrected post-execution)",
  "context_snapshot":  "ContextRef (pointer to Mnemos ContextNode at time of binding)",
  "user_id":           "string",
  "org_id":            "string",
  "created_at":        "timestamp"
}
```

The LexonRecord is the interface contract between Lexon and the broader RFC stack. The `context_snapshot` field is a reference to the Mnemos ContextNode active at the time of tool binding -- enabling replay, debugging, and cross-session learning via Engram.

### 3.2 Layer 1: Language Normalization

Every incoming user message passes through the Language Normalization layer before any routing occurs. This layer has three responsibilities: language detection, semantic translation, and code-switching handling.

**Language Detection**

Language is detected at the message level using a lightweight classifier. The detected language is stored in the LexonRecord and used to select the appropriate response language for Maia's reply. Critically, the user is never asked to switch languages. Maia always responds in the language the user wrote in.

**Semantic Translation for Routing**

The user message is translated to English for the purpose of tool routing only. The translation is not shown to the user and does not affect response generation. This separation -- route in English, respond in user language -- allows the tool registry to be maintained in a single language without sacrificing multilingual usability.

**Code-Switching**

Mixed-language input (e.g., "Ich will eine campaign erstellen fur meine leads") is handled by extracting English-language anchor terms -- technical vocabulary that users typically preserve across languages -- and using them as primary routing signals. The `is_mixed` flag is set on the LexonRecord, and the multilingual embedding model is invoked rather than the English-only fast path.

### 3.3 Layer 2: Two-Tier Routing

Routing is the core of Lexon. We define a two-tier architecture that combines the speed of a fast inference model with the semantic breadth of a multilingual embedding retriever.

**Tier A: Fast LLM Router**

A low-latency inference model (target: <300ms, e.g. a diffusion-based or speculative decoding model) receives the normalized user phrase alongside a compact tool manifest: tool names and single-line descriptions only, approximately 2-5 tokens per tool. At 80 tools, this is fewer than 400 tokens of tool context. The fast LLM performs intent-level reasoning -- not pattern matching -- and returns a ranked list of 5-7 candidate tool names with a brief rationale.

The fast LLM router is constrained to return only valid tool names via JSON schema with an enum constraint over the tool registry. This eliminates hallucinated tool names from propagating downstream.

**Tier B: Multilingual Semantic Retriever**

In parallel, a multilingual embedding model (e.g., multilingual-e5-large or paraphrase-multilingual-mpnet-base-v2) performs vector search over the full tool registry. Each tool in the registry is indexed by three embedding targets: the tool's formal description, a set of example user queries (the most discriminative signal, per Tool2Vec), and the tool's synonym list from the Personalization Store.

The retriever returns a ranked list of 5-7 candidates with cosine similarity scores.

**Union and Deduplication**

Candidates from both tiers are unioned and deduplicated. Tools appearing in both lists receive a score bonus. The resulting set of 5-8 candidates is passed to Layer 3. The union approach is deliberate: the fast LLM captures intent-level reasoning that embedding similarity misses; the retriever captures lexical variants and multilingual matches the LLM may miss. Each compensates for the other's failure mode.

### 3.4 Layer 3: The Terminology Map

Before the candidate set reaches the main agent LLM, it is filtered and re-ranked through the Terminology Map -- a structured, queryable table of known phrase-to-tool bindings maintained at three levels: system, organization, and user.

**Terminology Map Structure**

```json
{
  "phrase":      "string (e.g. 'blast my list')",
  "tool_name":   "string (e.g. 'bulk_email_send')",
  "params":      "object | null (default parameters if applicable)",
  "NOT":         "string[] (e.g. ['create_campaign'] -- explicitly not this tool)",
  "context":     "string | null (e.g. 'contact_selected' -- binding condition)",
  "level":       "system | org | user",
  "confidence":  "float (1.0 for explicit, <1.0 for learned)",
  "user_id":     "string | null (null for system/org level)"
}
```

The `NOT` field is critical. It encodes negative bindings -- cases where a phrase sounds like it maps to a tool but explicitly does not. This field is populated by correction events (when a user says "no, not that") and is the primary mechanism for preventing recurring mis-selections.

**Resolution Order**

When the Terminology Map is queried, entries are resolved in strict priority order: explicit user-level binding (confidence 1.0) overrides org-level which overrides system-level which overrides router inference. An explicit user preference is treated as ground truth and bypasses the confidence scoring layer entirely.

### 3.5 Layer 4: Disambiguation Protocol

After routing and terminology filtering, each candidate set carries a confidence score. Lexon defines a confidence-gated protocol for handling ambiguous results.

**Confidence Thresholds**

If the top candidate's confidence score exceeds 0.82 with no competing candidate within 0.15 of the top score, Lexon proceeds directly to execution. If confidence falls below 0.82, or if two candidates are within 0.15 of each other, the Disambiguation Protocol is triggered.

**The clarify_intent Tool**

Disambiguation is implemented as a first-class tool in the agent's tool set: `clarify_intent`. When invoked, it presents the user with 2-3 specific, concrete options rather than an open question. The options are derived from the top candidates and phrased in the user's own language.

Example: User writes "create an email campaign." Two candidates score within threshold: `create_campaign` (0.71) and `bulk_email_send` (0.68). Maia invokes `clarify_intent` and asks: "Are you setting up a multi-step outreach sequence, or sending a one-time email to a list?" -- not "what did you mean?"

The user's selection is captured as a high-confidence LexonRecord and immediately updates the Terminology Map at the user level, so the same phrase does not trigger disambiguation again for this user.

**Context as a Disambiguation Signal**

Before invoking `clarify_intent`, Lexon queries the Mnemos ContextNode for the current task. Active context -- a contact record open, a campaign in progress, a specific workflow step -- can collapse ambiguity without user interaction. The context-conditioned Terminology Map entry takes priority over ambiguous routing scores when a matching context condition is present.

---

## 4. The Lexon Personalization Layer

The Personalization Layer is the mechanism by which Lexon gets measurably more accurate for each user over time. It has two components: explicit preference management and implicit behavioral learning.

### 4.1 Explicit Preference Management

Users can directly teach Maia their vocabulary through a preference interface. The interaction model is simple: "When I say X, I mean Y." Explicit preferences are stored as user-level Terminology Map entries with confidence 1.0 and are never overridden by any other signal.

Default parameters can also be captured: "When I say 'morning sequence,' I mean create_campaign with time=09:00 and template=morning_outreach." This reduces the burden of parameter specification for repeated workflows.

### 4.2 Implicit Behavioral Learning

Lexon defines five behavioral signals that update the Personalization Store without requiring explicit user action:

| Signal | Trigger | Action |
| --- | --- | --- |
| Correction | User corrects tool after execution | Add NOT binding to corrected tool; add positive binding to actual tool |
| Affirmation | User confirms or proceeds without correction | Increment confidence on phrase-to-tool binding |
| Repetition | Same phrase to same tool 5+ times | Promote to high-confidence implicit synonym |
| Disambiguation Selection | User selects from clarify_intent options | Capture full context + selection as user-level binding |
| Clarification Repetition | Same phrase triggers disambiguation 3+ times | Flag phrase as persistently ambiguous; escalate to explicit preference prompt |

### 4.3 Org-Level Terminology

Between the system level and the user level sits an org-level Terminology Map. Org admins can define shared vocabulary for their team -- enabling consistent tool binding across all users in an organization without requiring each user to teach Maia independently. Org-level entries propagate to all users in the org as default bindings, overridable at the user level.

This tier is particularly important for industry-specific terminology: a legal firm where "send a letter" means a specific document workflow; a real estate team where "follow up" means a specific CRM update sequence; a sales organization where "blast" means a specific bulk send configuration.

### 4.4 Personalization and the RFC Stack

Personalization data is a natural candidate for cross-session persistence via Engram. User-level Terminology Map entries are EngramRecord-compatible: they carry a creation timestamp, a confidence score, and a context reference. High-confidence user bindings (confidence > 0.9, used > 10 times) are promoted to Engram for long-term retention and survive session boundaries. Lower-confidence bindings remain in the session-scoped Lexon store and decay if unused.

---

## 5. Multilingual Architecture

Multilingual support in Lexon is not a translation layer bolted onto a monolingual system. It is a first-class architectural property achieved through the choice of embedding model and the separation of routing language from response language.

### 5.1 The Embedding Model Choice

The tool registry is indexed using a multilingual sentence embedding model rather than an English-only model. Multilingual embedding models encode sentences from different languages into a shared semantic space -- meaning that "create a campaign," "Kampagne erstellen," and an Arabic equivalent produce embeddings that are close to each other and to the tool embeddings, without requiring translation.

This architectural choice has three consequences. First, users can query in any supported language and receive correctly routed tool candidates without a translation step. Second, code-switched input (mixing languages) is handled naturally by the shared embedding space. Third, the Personalization Store can store user synonyms in any language, and they will be correctly matched against tool embeddings regardless of language alignment.

### 5.2 The Routing/Response Language Separation

Lexon enforces a strict separation between routing language and response language. Routing always operates in the embedding space, which is language-agnostic. Response generation always uses the language detected in the user's message. The user is never exposed to the routing process, never required to use English, and never asked to repeat themselves in a different language.

### 5.3 Code-Switching Handling

Users with multilingual backgrounds -- a significant portion of Protaige's user base -- frequently mix languages within a single message, typically preserving English technical terms while writing surrounding context in their native language. Lexon handles this through two mechanisms: the multilingual embedding model naturally handles mixed-language input, and the English anchor extraction heuristic (identifying English-language technical terms in mixed-language input) provides a reliable fast path for the Tier A fast LLM router, which operates on English-normalized input.

---

## 6. Integration with the Agents RFC Stack

### 6.1 Lexon and Mnemos

The relationship between Lexon and Mnemos is bidirectional. Mnemos provides Lexon with the current task context -- specifically, the active ContextNode -- which is used as a disambiguation signal. When a Mnemos ContextNode indicates that a specific entity (contact, campaign, workflow) is active, Lexon uses this as a prior in confidence scoring, collapsing ambiguous routings toward the contextually appropriate tool.

In the reverse direction, Lexon writes a ToolCallNode into the Mnemos task graph for each tool invocation. This ToolCallNode captures the phrase, the LexonRecord ID, the selected tool, and the confidence score -- enabling Mnemos to track tool selection quality as a first-class property of task execution.

### 6.2 Lexon and iCPG

iCPG structures user intent as a typed computation graph of goals and constraints. Lexon operates downstream of iCPG: the intent has already been structured; Lexon's job is to translate a specific node in the iCPG into a concrete tool invocation. The iCPG ReasonNode provides Lexon with a structured representation of the sub-goal, which is a higher-quality routing signal than raw user text alone -- enabling Lexon to route on structured intent rather than raw language when the iCPG representation is available.

### 6.3 Lexon and Engram

Engram provides cross-session memory persistence. High-confidence Lexon personalization records -- user-level Terminology Map entries that have been confirmed multiple times -- are candidates for Engram encoding. This means a user's learned vocabulary survives session boundaries and does not need to be re-learned each time. The encoding follows the EngramRecord schema defined in the Engram RFC, with LexonRecord as the source type.

---

## 7. Novelty and Contribution

The table below maps Lexon's capabilities against the closest prior work. Capabilities marked with a checkmark are first introduced by this architecture.

| Capability | RAG-MCP | ToolTree | Tool-MVR | Lexon |
| --- | --- | --- | --- | --- |
| Two-tier routing (fast LLM + semantic vector) | -- | -- | -- | Yes |
| Multilingual + code-switching support | -- | -- | -- | Yes |
| Per-user synonym learning (implicit) | -- | -- | -- | Yes |
| Explicit user preference override | -- | -- | -- | Yes |
| Org-level terminology customization | -- | -- | -- | Yes |
| Confidence-gated clarification protocol | -- | -- | -- | Yes |
| Structured terminology map injection | Partial | -- | -- | Yes |
| Mnemos / iCPG / Engram integration | -- | -- | -- | Yes |

The individual techniques used in Lexon -- embedding-based retrieval, fast LLM routing, multilingual embeddings, synonym injection -- each have prior art. Lexon's contribution is not any individual technique but the unified architecture that combines them into a coherent system designed for production deployment in a multilingual, multi-user, personalization-first agentic product.

---

## 8. Honest Limitations

We document the following known limitations of the current Lexon design.

### 8.1 Cold Start

A new user has no personalization history. Lexon falls back to org-level and system-level bindings, which provide a reasonable baseline but will not reflect the user's specific vocabulary. The cold start period -- estimated at 20-50 interactions for meaningful personalization -- represents a window of elevated disambiguation rate and potential mis-selection.

### 8.2 Retriever Recall Ceiling

No retrieval system achieves perfect recall. A tool that is genuinely absent from the top-N candidates cannot be selected by the main LLM. Lexon mitigates this with the union of two independent retrieval tiers and a conservative top-N setting (5-8 candidates), but does not eliminate the failure mode. A fallback escape hatch -- a `request_additional_tools` capability that allows the agent to expand the candidate set -- is defined but its interaction with latency and user experience requires further design work.

### 8.3 Disambiguation Latency

The `clarify_intent` protocol adds a full conversation turn when triggered. For high-frequency workflows, repeated disambiguation represents a UX cost. The Personalization Layer is designed to eliminate this cost over time for established users, but the convergence rate depends on user engagement patterns and is not yet empirically characterized.

### 8.4 Multilingual Embedding Quality

Current multilingual embedding models show uneven quality across languages. High-resource languages (English, German, French, Spanish, Arabic) are well-supported. Lower-resource languages may show degraded retrieval quality.

---

## 9. Open Questions for RFC Review

The following questions are raised for discussion by reviewers of this RFC and are not resolved in v1.0:

- What is the correct confidence threshold for triggering `clarify_intent`? The 0.82 value is a design estimate. It should be empirically validated against a corpus of real Maia interactions.
- How should the Personalization Store handle conflicting signals? If a phrase maps to Tool A via org-level binding but the user has repeatedly corrected to Tool B, the resolution order (user > org) handles this -- but should conflicting org-level entries trigger a notification to the org admin?
- What is the appropriate Engram promotion threshold? High-confidence Lexon bindings are described as Engram candidates, but the specific confidence and frequency thresholds for promotion are not defined in this RFC and require joint design with the Engram working group.
- How does Lexon handle tool deprecation? When a tool is removed from the registry, user-level Terminology Map entries pointing to it become stale. The decay and invalidation protocol for deprecated tool bindings is not specified here.
- Should the Tier A fast LLM router be fine-tuned on Protaige's tool vocabulary over time? A fine-tuned router would likely outperform a general-purpose fast model for domain-specific routing, but introduces a training data curation and retraining overhead.

---

## 10. Conclusion

The gap between what users say and what agents can do is not a user education problem. It is an architectural problem. Users should not be required to learn the vocabulary of the system. The system should learn the vocabulary of the user.

Lexon defines the architecture by which this inversion is achieved in a production agentic system. Through a two-tier routing pipeline that combines semantic reasoning with embedding retrieval, a multilingual substrate that treats language diversity as a first-class constraint rather than an edge case, a structured disambiguation protocol that surfaces ambiguity rather than suppressing it, and a personalization layer that learns continuously from user behavior, Lexon closes the semantic gap between intent and tool invocation.

In the context of the Agents Architecture RFC stack, Lexon is the layer that makes the other RFCs executable. Mnemos can remember a goal, iCPG can structure an intent, Engram can persist a memory -- but without Lexon, the final step of turning structured intent into a correctly invoked tool capability remains the system's most frequent point of failure.

> **Positioning Statement:** A Lexon is the binding unit that maps a fragment of user intent -- a phrase, in any language, in any vocabulary -- to the precise agentic capability that fulfills it. It is the synaptic junction between what an agent understands and what it can do.

---

*Lexon RFC v1.0 -- Agent Architecture Series -- April 2026*

*Companion to Mnemos RFC v2 -- iCPG v8 -- Engram v3*
