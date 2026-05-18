## Hey, I'm Ali.

**Bringing personal super intelligence to every worker in the world.**

CTO based in Berlin. I build things at the intersection of AI and product — autonomous agents that manage engineering teams, voice bots that handle real phone calls, developer tools that make Claude Code actually useful in production.

I ship fast, open source a lot, and believe the best software is built by small teams with high leverage.

---

### Research Papers

Here is some of my work on hard problems in agentic AI — memory, intent, tool selection, and autonomous engineering. These come from building production agents, not from theory.

| Paper | Topic | Summary |
|-------|-------|---------|
| [**Engram**](docs/Engram_RFC_v3.md) (v3, Mar 2026) | Agentic Memory Pathology | A pathology-first framework for diagnosing memory failure in AI agents. Defines an amnesia taxonomy (temporal, source, interference, encoding, retrieval, consolidation, prospective) validated against three production systems: Maia, hive, and Deepak. Introduces the RAG-Amnesia Scale and EngramRecord encoding. |
| [**iCPG**](docs/iCPG_RFC_v8.md) (v8, Mar 2026) | Intent-Augmented Code Property Graph | Reframes a class of coding agent "hallucinations" as specification drift — measurable divergence from intent. Proposes ReasonNodes with formal contracts (preconditions, postconditions, invariants) and 6-dimension drift detection. I did it primarily to make claude code work better.  |
| [**Mnemos**](docs/Mnemos_RFC_v1.md) (v1, Apr 2026) | Task-Scoped Agent Memory | A framework for how agents acquire, organize, compress, and hand off knowledge during a single task. Addresses context wall crashes in long-running Claude Code sessions with typed MnemoNodes, a 4-dimension fatigue model, tiered REM consolidation, and SkillNode promotion for reusable patterns. |
| [**Lexon**](docs/Lexon_RFC_v1.md) (v1, Apr 2026) | Semantic Tool Binding | Solves tool selection accuracy collapse at scale. A two-tier routing pipeline with multilingual embeddings, structured disambiguation, and a personalization layer that learns user vocabulary over time. Integrates with Mnemos, iCPG, and Engram to form a complete agentic cognitive stack. |
| [**Telos**](docs/Telos_RFC_v1.1.md) (v1.1, May 2026) | Intent-Grounded Testing | Reframes testing from "does the output match the spec?" to "does the artifact serve the intent?" Models the lossy chain from real intent to behavior, defines 8 intent-failure modes (IF-1 through IF-8), and runs three test planes autonomously — including whether the spec itself is wrong. Cross-references iCPG (intent governance), Engram (cross-session memory), and Polyphony (decomposition closure). Operational layer: DAE (Dynamic Autonomous Evaluation). |
| [**DAE**](docs/DAE_RFC_v1.md) (v1, May 2026) | Dynamic Autonomous Evaluation | Operational evaluation substrate for autonomous agents. Continuous 5-stage pipeline (Capture→Score→Compare→Gate→Learn) with dimensional rubric scoring, regression baselines, risk-tiered adaptive sampling, and multi-judge panels. Agent-evaluator isolation as a structural design constraint. Operational layer for Telos's intent-grounded testing. |
| [**Maggy**](docs/Maggy_RFC.md) | Autonomous AI Engineering Agent | Released at [claude-bootstrap](https://github.com/alinaqi/claude-bootstrap). A local-first, self-improving engineering agent with multi-model orchestration (Claude, GPT-5, Gemini, Kimi, DeepSeek, Qwen), 5-level closed-loop control, process intelligence from CI/PR/deploy signals, and Maggy Mesh — a P2P network for sharing team learning across developer instances. |

These papers form a coherent **Agent Architecture Series**: iCPG governs intent in code, Mnemos governs task memory, Engram governs cross-session memory, Lexon governs tool resolution, Telos governs intent-grounded autonomous testing with DAE as its operational evaluation layer, and Maggy orchestrates all of it into an autonomous engineering platform.

---

### What I'm working on

**Autonomous AI Systems** — Most of my recent work is about making AI agents that actually do real work, not demos. [Zoro](https://github.com/alinaqi/zoro) is an autonomous engineering manager that runs as an iTerm2 extension — it monitors tickets, routes work to Claude Code sessions, detects error loops, and runs a web cockpit for oversight. [claude-bootstrap](https://github.com/alinaqi/claude-bootstrap) (529+ stars) is the opinionated project scaffold I use to make Claude Code reliable across all my projects.

**AI-Native Developer Tools** — [Hive](https://github.com/alinaqi/Hive-Standalone-Specs) is a standalone AI command center for SaaS — it manages budgets, creates tasks, makes strategic decisions, and coordinates between AI agents and humans. [Halo](https://github.com/alinaqi/halo) brings Claude Code to the desktop. [voxy](https://github.com/alinaqi/voxy) is a voice-controlled terminal assistant.

**Voice & Conversational AI** — [AIVoiceBot](https://github.com/alinaqi/AIVoiceBot) is a complete voice bot service handling inbound and outbound calls. [realtime-transcription](https://github.com/alinaqi/realtime-transcription) does live audio-to-text across languages. [voiceover](https://github.com/alinaqi/voiceover) generates AI narrations for screen recordings.

**MCP & Integrations** — [mcp-linkedin-server](https://github.com/alinaqi/mcp-linkedin-server) (50+ stars) is an MCP server for LinkedIn automation. I've built crawlers, search engines, proposal generators, and various connectors between AI and the tools people already use.

---

### Private work

Alongside open source, I have worked on...

**Enterprise CX Platform** — a customer experience platform processing millions of survey responses. I managed/contributed to the full stack: backend services, frontend apps, Shopify integrations, and a migration from legacy to a modern multi-tenant architecture. Multi-provider integrations (Salesforce, HubSpot, Intercom). Team of engineers across backend, frontend, and DevOps.

**End-to-End AI Marketing Agents** — an AI-native marketing platform built around a fully autonomous AI agent that controls the entire system. The agent runs campaigns end-to-end: strategy, brief co-creation, content generation, creative production, and analytics — all via streaming chat, autopilot mode, or proactive nudges. It conducts interviews and outreach over email, WhatsApp, and live voice meetings with real-time turn-taking and TTS. Multi-agent architecture with hired sub-agents per brand, automated email cadences, MCP integrations (HubSpot, Salesforce, Google Ads, social platforms), and a full copilot toolkit with tools, skills, and context-aware planning.

**AI-Powered Learning & Transformation Platform** — an ed-tech platform with a suite of AI products: synthetic interviews, knowledge sprints, AI-generated podcasts, micro-learning, chat-based tutoring, content generation, onboarding companions, and a full transformation suite for organizational change. Simulation agents, ITSM process automation, and a unified platform serving it all.

---

### How I think about building

I wrote a [No-Agile Agile Manifesto](https://github.com/alinaqi/no-agile-agile-manifesto) and an [Organization Consciousness Protocol](https://github.com/alinaqi/ocp) because I think most process is theater. What actually works:

- **Small teams, high autonomy.** One engineer with good tools beats a squad with a Jira board.
- **Multi-model, not single-model.** I don't use one AI — I built a [9-tier routing system](https://github.com/alinaqi/maggy) that classifies every task and delegates to the cheapest capable model. Qwen3 for lookups (~$0), DeepSeek Pro for implementation (~$0.44/M), Kimi for review, Gemini for multimodal and deep research, Codex for bulk generation, Claude for architecture and security. DeepSeek handles ~80% of coding; Claude is reserved for what actually needs judgment. This isn't cost-cutting — it's about using the right tool for the job. Why pay $15/M for a typo fix?
- **Memory is the moat.** Every AI coding tool loses context on compaction. [Mnemos](docs/Mnemos_RFC_v1.md) doesn't compress blindly — it tracks *why* each memory node exists with typed eviction policies, measures fatigue across 4 dimensions, and writes checkpoints *before* things go wrong. Codex and Claude Code fire-and-forget; Mnemos preserves intent.
- **Autonomous, not assisted.** [Maggy](https://github.com/alinaqi/maggy) doesn't wait for me to ask. It auto-discovers untested code and generates test suites. Background heartbeats scan competitors and refresh the task inbox. After significant changes, a Stop hook asks qwen3 whether a multi-model review (DeepSeek + Kimi + Codex in parallel) is warranted — and triggers it autonomously. The agent decides when it needs help, not me.
- **Ship first, abstract later.** Three similar lines of code is better than a premature abstraction.
- **Test intent, not just output.** Every testing framework today asks "does the output match the spec?" [Telos](docs/Telos_RFC_v1.1.md) asks a harder question: "does the artifact serve the intent?" A green test suite doesn't mean the spec was right — it means you built the wrong thing correctly. Telos tests the spec itself, scores how much intent survives each translation link, and detects when the tests themselves have been captured by a proxy. Autonomous testing that tests the test.

---

### Tech I reach for

`Python` `TypeScript` `FastAPI` `Claude API` `Agent SDK` `MCP` `SQLite` `PostgreSQL` `React` `Next.js` `Shopify` `iTerm2 API` `WebSocket` `GraphQL`

---

<p align="center">
  <a href="https://github.com/alinaqi?tab=repositories&sort=stargazers">repos sorted by stars</a>
</p>

