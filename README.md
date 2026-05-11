## Hey, I'm Ali.

**Applying AI to push boundaries.**

CTO based in Berlin. I build things at the intersection of AI and product — autonomous agents that manage engineering teams, voice bots that handle real phone calls, developer tools that make Claude Code actually useful in production.

I ship fast, open source a lot, and believe the best software is built by small teams with high leverage.

---

### Research Papers

I write RFC-style research papers on the hard problems in agentic AI — memory, intent, tool selection, and autonomous engineering. These come from building production agents, not from theory.

| Paper | Topic | Summary |
|-------|-------|---------|
| **Engram** (v3, Mar 2026) | Agentic Memory Pathology | A pathology-first framework for diagnosing memory failure in AI agents. Defines an amnesia taxonomy (temporal, source, interference, encoding, retrieval, consolidation, prospective) validated against three production systems: Maia, hive, and Deepak. Introduces the RAG-Amnesia Scale and EngramRecord encoding. |
| **iCPG** (v8, Mar 2026) | Intent-Augmented Code Property Graph | Reframes a class of coding agent "hallucinations" as specification drift — measurable divergence from intent. Proposes ReasonNodes with formal contracts (preconditions, postconditions, invariants) and 6-dimension drift detection. Grounded in a live legacy migration (zenloop v1 → v2). |
| **Mnemos** (v1, Apr 2026) | Task-Scoped Agent Memory | A framework for how agents acquire, organize, compress, and hand off knowledge during a single task. Addresses context wall crashes in long-running Claude Code sessions with typed MnemoNodes, a 4-dimension fatigue model, tiered REM consolidation, and SkillNode promotion for reusable patterns. |
| **Lexon** (v1, Apr 2026) | Semantic Tool Binding | Solves tool selection accuracy collapse at scale. A two-tier routing pipeline with multilingual embeddings, structured disambiguation, and a personalization layer that learns user vocabulary over time. Integrates with Mnemos, iCPG, and Engram to form a complete agentic cognitive stack. |
| **Maggy** | Autonomous AI Engineering Agent | A local-first, self-improving engineering agent with multi-model orchestration (Claude, GPT-5, Gemini, Kimi, DeepSeek, Qwen), 5-level closed-loop control, process intelligence from CI/PR/deploy signals, and Maggy Mesh — a P2P network for sharing team learning across developer instances. |

These papers form a coherent **Agent Architecture Series**: iCPG governs intent, Mnemos governs task memory, Engram governs cross-session memory, Lexon governs tool resolution, and Maggy orchestrates all of it into an autonomous engineering platform.

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
- **AI as a multiplier, not a replacement.** I use Claude Code for ~80% of my implementation. The other 20% is architecture, judgment, and taste.
- **Ship first, abstract later.** Three similar lines of code is better than a premature abstraction.
- **TDD when it matters.** Tests are a design tool, not a checkbox. I write them first for complex logic, skip them for throwaway scripts.

---

### Tech I reach for

`Python` `TypeScript` `FastAPI` `Claude API` `Agent SDK` `MCP` `SQLite` `PostgreSQL` `React` `Next.js` `Shopify` `iTerm2 API` `WebSocket` `GraphQL`

---

<p align="center">
  <a href="https://github.com/alinaqi?tab=repositories&sort=stargazers">repos sorted by stars</a>
</p>
