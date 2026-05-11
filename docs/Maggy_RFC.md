Maggy: An Autonomous AI Engineering Agent

\

\

1\. Executive Summary

<span class="s1">***Maggy is a local-first, self-improving AI engineering*** </span><span class="s2">agent</span><span class="s1"> ***that transforms how development teams build software. Unlike code assistants that wait for prompts, Maggy is an autonomous agent that observes, learns, and optimizes — continuously improving its own effectiveness across models, workflows, and team knowledge.***</span>

<span class="s1">***What makes Maggy different:***</span>

- ****<span class="s1">***Multi-model orchestration — Maggy routes tasks to the best model (Claude,*** </span><span class="s2">gpt-5</span><span class="s1">***, Gemini, Kimi, DeepSeek, local Qwen) based on learned performance data, not static rules. When one model hits quota, work continues seamlessly on the next.***</span>
- ****<span class="s1">***Self-improving closed-loop control — Every task Maggy completes generates reward signals that improve its future decisions. Model routing, inbox ordering, workflow steps, and fatigue management all optimize automatically.***</span>
- ****<span class="s1">***Process intelligence — Maggy doesn’t just write code. It learns from CI results, PR reviews, CodeRabbit findings, and merge patterns to preemptively fix issues before they reach reviewers.***</span>
- ****<span class="s1">***Maggy Mesh — A peer-to-peer network connecting Maggy instances across a team. One developer’s hard-won CI fix becomes the entire team’s knowledge. Autonomously. Instantly.***</span>
- ****<span class="s1">***Local-first, no vendor lock-in — All data stays on developer machines. No cloud dependency. No vendor seeing your code. Works offline with local models.***</span>

<span class="s1">***The value proposition: A team of 5 developers running Maggy Mesh for 6 months accumulates 4x the learning of a solo developer. New team members inherit collective intelligence on day one. CI pass rates go up, review rounds go down, and the system gets smarter every week — without anyone configuring it.***</span>

\

2\. Vision: Autonomous Engineering, Not Code Generation

<span class="s1">***The current generation of AI coding tools — Copilot, Cursor, Devin — are fundamentally reactive. They complete code when prompted, suggest edits when asked, and run tasks when instructed. They’re sophisticated typeaheads, not engineers.***</span>

<span class="s1">***An engineer doesn’t just write code. An engineer:***</span>

- ****<span class="s1">***Prioritizes — Which ticket matters most right now?***</span>
- ****<span class="s1">***Plans — What’s the blast radius? What could break?***</span>
- ****<span class="s1">***Validates — Does this feature align with the market? Do competitors have it?***</span>
- ****<span class="s1">***Executes — Write the code, with the right model for the task***</span>
- ****<span class="s1">***Verifies — Did CI pass? Did reviewers approve? Did it deploy cleanly?***</span>
- ****<span class="s1">***Learns — What worked? What didn’t? How do I do it better next time?***</span>

<span class="s1">***Maggy does all of this. It’s the first AI platform designed around the full software development lifecycle, not just the “write code” step.***</span>

The Autonomy Spectrum

<span class="s1">***Level 0: Autocomplete (Copilot, TabNine)***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">  </span>→ Completes the current line***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">  </span>→ No context beyond the file***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">  </span>→ No learning***</span><span class="s3">***\
\***
</span><span class="s1">***Level 1: Chat Assistant (ChatGPT, Claude)***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">  </span>→ Answers questions about code***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">  </span>→ No project context***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">  </span>→ No memory between sessions***</span><span class="s3">***\
\***
</span><span class="s1">***Level 2: Project-Aware Assistant (Cursor, Continue)***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">  </span>→ Understands the codebase***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">  </span>→ Can edit multiple files***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">  </span>→ Limited memory (rules, preferences)***</span><span class="s3">***\
\***
</span><span class="s1">***Level 3: Task Agent (Devin, Claude Code Agent)***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">  </span>→ Executes multi-step tasks***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">  </span>→ Uses tools (terminal, browser)***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">  </span>→ Single-model, single-project***</span><span class="s3">***\
\***
</span><span class="s1">***Level 4: Autonomous Engineering Platform (Maggy) ← WE ARE HERE***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">  </span>→ Multi-model, multi-project orchestration***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">  </span>→ Self-improving from every task***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">  </span>→ Process intelligence (learns from CI, reviews, deploys)***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">  </span>→ Team intelligence via P2P mesh***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">  </span>→ Market validation before engineering***</span>

\

3\. Architecture Overview

The Component Map

<span class="s1">***┌─────────────────────────────────────────────────────────────┐***</span><span class="s3">***\***
</span><span class="s1">***│<span class="Apple-converted-space">                    </span>MAGGY WEB DASHBOARD<span class="Apple-converted-space">                        </span>│***</span><span class="s3">***\***
</span><span class="s1">***│<span class="Apple-converted-space">  </span>┌──────────┐ ┌─────────┐ ┌────────┐ ┌───────┐ ┌────────┐ │***</span><span class="s3">***\***
</span><span class="s1">***│<span class="Apple-converted-space">  </span>│<span class="Apple-converted-space">  </span>Inbox <span class="Apple-converted-space">  </span>│ │ Budget<span class="Apple-converted-space">  </span>│ │ Agents │ │<span class="Apple-converted-space">  </span>CIKG │ │Process │ │***</span><span class="s3">***\***
</span><span class="s1">***│<span class="Apple-converted-space">  </span>│ (ranked) │ │ (live)<span class="Apple-converted-space">  </span>│ │(status)│ │ (gaps)│ │(health)│ │***</span><span class="s3">***\***
</span><span class="s1">***│<span class="Apple-converted-space">  </span>└──────────┘ └─────────┘ └────────┘ └───────┘ └────────┘ │***</span><span class="s3">***\***
</span><span class="s1">***└──────────────────────────┬──────────────────────────────────┘***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">                           </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">              </span>┌────────────┴────────────┐***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">              </span>│<span class="Apple-converted-space">    </span>ORCHESTRATOR LAYER<span class="Apple-converted-space">    </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">              </span>│ <span class="Apple-converted-space">                        </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">              </span>│<span class="Apple-converted-space">  </span>Pi Agent (universal<span class="Apple-converted-space">    </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">              </span>│<span class="Apple-converted-space">  </span>harness, RPC mode) <span class="Apple-converted-space">    </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">              </span>│ <span class="Apple-converted-space">                        </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">              </span>│<span class="Apple-converted-space">  </span>Token Budget Manager <span class="Apple-converted-space">  </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">              </span>│<span class="Apple-converted-space">  </span>Model Router (learned) │***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">              </span>│<span class="Apple-converted-space">  </span>Dual-Model Planner <span class="Apple-converted-space">    </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">              </span>└────────┬────────────────┘***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">                       </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">        </span>┌──────────────┼──────────────┐***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">        </span>│<span class="Apple-converted-space">              </span>│<span class="Apple-converted-space">              </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">   </span>┌────▼────┐ <span class="Apple-converted-space">  </span>┌────▼────┐ <span class="Apple-converted-space">  </span>┌────▼────┐***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">   </span>│Container│ <span class="Apple-converted-space">  </span>│Container│ <span class="Apple-converted-space">  </span>│Container│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">   </span>│<span class="Apple-converted-space">  </span>1<span class="Apple-converted-space">      </span>│ <span class="Apple-converted-space">  </span>│<span class="Apple-converted-space">  </span>2<span class="Apple-converted-space">      </span>│ <span class="Apple-converted-space">  </span>│<span class="Apple-converted-space">  </span>3<span class="Apple-converted-space">      </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">   </span>│ Claude<span class="Apple-converted-space">  </span>│ <span class="Apple-converted-space">  </span>│*** </span><span class="s2">gpt-5</span><span class="s1">***<span class="Apple-converted-space">  </span>│ <span class="Apple-converted-space">  </span>│<span class="Apple-converted-space">  </span>Qwen <span class="Apple-converted-space">  </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">   </span>│ (auth)<span class="Apple-converted-space">  </span>│ <span class="Apple-converted-space">  </span>│ (front) │ <span class="Apple-converted-space">  </span>│ (docs)<span class="Apple-converted-space">  </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">   </span>└─────────┘ <span class="Apple-converted-space">  </span>└─────────┘ <span class="Apple-converted-space">  </span>└─────────┘***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">        </span>│<span class="Apple-converted-space">              </span>│<span class="Apple-converted-space">              </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">   </span>┌────┴──────────────┴──────────────┴────┐***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">   </span>│ <span class="Apple-converted-space">        </span>INTELLIGENCE LAYER <span class="Apple-converted-space">            </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">   </span>│<span class="Apple-converted-space">                                        </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">   </span>│<span class="Apple-converted-space">  </span>iCPG — blast radius, drift, intent<span class="Apple-converted-space">    </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">   </span>│<span class="Apple-converted-space">  </span>Mnemos — memory, fatigue, checkpoints │***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">   </span>│<span class="Apple-converted-space">  </span>codebase-memory-mcp — code graph<span class="Apple-converted-space">      </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">   </span>│<span class="Apple-converted-space">  </span>CIKG — competitive intelligence <span class="Apple-converted-space">      </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">   </span>│<span class="Apple-converted-space">  </span>Process Intelligence — CI/PR/deploy <span class="Apple-converted-space">  </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">   </span>│<span class="Apple-converted-space">  </span>MCP Forge — capability expansion<span class="Apple-converted-space">      </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">   </span>│<span class="Apple-converted-space">  </span>Maggy Mesh — P2P team learning<span class="Apple-converted-space">        </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">   </span>└────────────────────────────────────────┘***</span>

Pi: The Universal Agent Harness

<span class="s1">***Pi replaces per-CLI adapters with a single interface to every model. It runs inside Polyphony containers in RPC mode over stdin/stdout. The same PiAdapter code controls Claude,*** </span><span class="s2">gpt-5</span><span class="s1">***, Gemini, Kimi, DeepSeek, or a local Qwen — with identical tool interfaces.***</span>

<span class="s1">***Model fallback chain:***</span>

<span class="s1">***Claude →*** </span><span class="s2">gpt-5</span><span class="s1"> ***→ Gemini → Kimi → DeepSeek → Qwen (local, unlimited)***</span>

<span class="s1">***When a model hits quota or rate limits:<span class="Apple-converted-space"> </span>***</span>

<span class="s1">***1. Mnemos writes a structured checkpoint (goal, constraints, progress, state)<span class="Apple-converted-space"> </span>***</span>

<span class="s1">***2. Pi switches to the next model<span class="Apple-converted-space"> </span>***</span>

<span class="s1">***3. The checkpoint is injected as context<span class="Apple-converted-space"> </span>***</span>

<span class="s1">***4. The new model verifies it understands the task before continuing<span class="Apple-converted-space"> </span>***</span>

<span class="s1">***5. If verification fails, escalate to the next tier — don’t retry on a weaker model***</span>

<span class="s1">***The user never notices the switch. Work continues. That’s the wow.***</span>

Token Budget Manager

<span class="s1">***providers***</span><span class="s4">***:***</span><span class="s5">***\***
</span><span class="s6">***<span class="Apple-converted-space">  </span>***</span><span class="s1">***anthropic***</span><span class="s4">***:***</span><span class="s5">***\***
</span><span class="s6">***<span class="Apple-converted-space">    </span>***</span><span class="s1">***daily_limit_usd***</span><span class="s4">***:***</span><span class="s6"> **** </span><span class="s7">***50.00***</span><span class="s5">***\***
</span><span class="s6">***<span class="Apple-converted-space">    </span>***</span><span class="s1">***used_today_usd***</span><span class="s4">***:***</span><span class="s6"> **** </span><span class="s7">***32.15***</span><span class="s5">***\***
</span><span class="s6">***<span class="Apple-converted-space">    </span>***</span><span class="s1">***model_preference***</span><span class="s4">***:***</span><span class="s6"> ***claude-sonnet-4***</span><span class="s5">***\***
</span><span class="s6">***<span class="Apple-converted-space">  </span>***</span><span class="s1">***openai***</span><span class="s4">***:***</span><span class="s5">***\***
</span><span class="s6">***<span class="Apple-converted-space">    </span>***</span><span class="s1">***daily_limit_usd***</span><span class="s4">***:***</span><span class="s6"> **** </span><span class="s7">***30.00***</span><span class="s5">***\***
</span><span class="s6">***<span class="Apple-converted-space">    </span>***</span><span class="s1">***used_today_usd***</span><span class="s4">***:***</span><span class="s6"> **** </span><span class="s7">***5.20***</span><span class="s5">***\***
</span><span class="s6">***<span class="Apple-converted-space">    </span>***</span><span class="s1">***model_preference***</span><span class="s4">***:***</span><span class="s6"> **** </span><span class="s8">gpt-5</span><span class="s5">***\***
</span><span class="s6">***<span class="Apple-converted-space">  </span>***</span><span class="s1">***local***</span><span class="s4">***:***</span><span class="s5">***\***
</span><span class="s6">***<span class="Apple-converted-space">    </span>***</span><span class="s1">***daily_limit_usd***</span><span class="s4">***:***</span><span class="s6"> **** </span><span class="s7">***0***</span><span class="s9">***<span class="Apple-converted-space">  </span>\# free***</span><span class="s5">***\***
</span><span class="s6">***<span class="Apple-converted-space">    </span>***</span><span class="s1">***model_preference***</span><span class="s4">***:***</span><span class="s6"> ***qwen2.5-coder:32b***</span>

<span class="s1">***The budget manager prevents runaway costs. When anthropic hits \$50, Maggy doesn’t stop — it routes to OpenAI. When OpenAI hits \$30, it routes to local Qwen. Work never stops.***</span>

\

4\. Self-Improvement: Multi-Level Closed-Loop Control

<span class="s1">***This is Maggy’s core differentiator. Every task teaches Maggy something. Every CI failure, every review comment, every deploy result feeds back into the system. Maggy gets smarter every day — without anyone configuring it.***</span>

The Objective Function

<span class="s1">***efficiency = (value_delivered / time_spent) x quality_multiplier***</span><span class="s3">***\
\***
</span><span class="s1">***where:***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">  </span>value_delivered <span class="Apple-converted-space">  </span>= tickets landed + features shipped + bugs fixed***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">  </span>time_spent<span class="Apple-converted-space">        </span>= wall clock from ticket selection to merge***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">  </span>quality_multiplier = 1.0 - (bug_escape_rate + revert_rate + incident_rate)***</span>

Five Control Levels

|  |  |  |
|----|----|----|
| <span class="s1">***Level***</span> | <span class="s1">***Frequency***</span> | <span class="s1">***What It Does***</span> |
| <span class="s1">***L0 — Real-time***</span> | <span class="s1">***Seconds***</span> | <span class="s1">***Catches tool failures, test failures, fatigue spikes, scope drift as they happen. Switches models mid-task when quality degrades.***</span> |
| <span class="s1">***L1 — Task***</span> | <span class="s1">***Minutes***</span> | <span class="s1">***Computes task reward score. Updates model performance table. Logs process signals.***</span> |
| <span class="s1">***L2 — Daily***</span> | <span class="s1">***Hours***</span> | <span class="s1">***Catches operational degradation: CI pass rate drops, model failure spikes, budget burn rate anomalies. Disables failing models.***</span> |
| <span class="s1">***L3 — Weekly***</span> | <span class="s1">***Days***</span> | <span class="s1">***Strategic optimization: evolves skill files, adjusts workflow steps, triggers MCP Forge for capability gaps, patches prompts.***</span> |
| <span class="s1">***L4 — Monthly***</span> | <span class="s1">***Weeks***</span> | <span class="s1">***Meta-optimization: recalibrates reward signals, adjusts tier boundaries, tunes exploration rate, changes the improvement process itself.***</span> |

<span class="s1">***Key principle: Inner loops provide stability. Outer loops provide optimization. L0 catches a failing model in seconds — the user barely notices. L3 makes routing smarter over weeks — the system quietly improves. L4 makes the improvement process itself better over months.***</span>

What Gets Optimized

<span class="s1">***Model routing — Maggy tracks reward per*** </span><span class="s10">***(model x task_type x blast_tier)***</span><span class="s1"> ***triple. After 50+ tasks, routing outperforms random assignment by 20%+.***</span>

<span class="s1">***(claude, auth, high): <span class="Apple-converted-space">      </span>+0.92<span class="Apple-converted-space">  </span>← claude excels at auth***</span><span class="s3">***\***
</span><span class="s1">***(qwen, docs, low):<span class="Apple-converted-space">          </span>+0.85<span class="Apple-converted-space">  </span>← qwen is fast and free for docs***</span><span class="s3">***\***
</span><span class="s1">***(***</span><span class="s2">gpt-5</span><span class="s1">***, frontend, medium): +0.78<span class="Apple-converted-space">  </span>←*** </span><span class="s2">gpt-5</span><span class="s1"> ***is strong on frontend***</span>

<span class="s1">***Inbox ordering — Learns which tickets the user actually picks first. Adjusts urgency weights to match user behavior.***</span>

<span class="s1">***Workflow steps — Drops steps that never catch issues (e.g., Codex counter-check on blast \< 3). Re-enables them when they become valuable again.***</span>

<span class="s1">***Fatigue management — Learns each user’s optimal session length and pre-checkpoints at the right moment. Not at a generic threshold — at your threshold.***</span>

\

5\. Process Intelligence: Learning from the Full SDLC

<span class="s1">***Most AI tools optimize code generation. Maggy optimizes the entire development process.***</span>

Environment Discovery

<span class="s1">***On first run per project, Maggy auto-discovers the developer’s workflow — no configuration:***</span>

- ****<span class="s1">***Ticketing: GitHub Issues, Asana, Linear, Jira***</span>
- ****<span class="s1">***CI/CD: GitHub Actions, Jenkins, CircleCI***</span>
- ****<span class="s1">***Code quality: ESLint, ruff, mypy, pre-commit, coverage***</span>
- ****<span class="s1">***Review process: Required reviewers, CODEOWNERS, branch protection***</span>
- ****<span class="s1">***Integrations: CodeRabbit, Dependabot, Renovate, Vercel***</span>

Signal Collection

<span class="s1">***Maggy continuously collects signals from the SDLC:***</span>

|  |  |
|----|----|
| <span class="s1">***Signal Source***</span> | <span class="s1">***What Maggy Learns***</span> |
| <span class="s1">***CI results***</span> | <span class="s1">***Which code patterns cause test failures***</span> |
| <span class="s1">***PR review comments***</span> | <span class="s1">***What reviewers consistently flag***</span> |
| <span class="s1">***CodeRabbit findings***</span> | <span class="s1">***Security and quality issues by pattern***</span> |
| <span class="s1">***Merge patterns***</span> | <span class="s1">***How many rounds of review, time to merge***</span> |
| <span class="s1">***Deploy results***</span> | <span class="s1">***Which changes cause deploy failures***</span> |

Preemptive Fixes

<span class="s1">***The pattern engine correlates*** </span><span class="s10">***(code_pattern, review_feedback)***</span><span class="s1"> ***pairs:***</span>

<span class="s1">***“Your reviewer always flags missing error handling in API routes. Maggy added it before the PR was created. Review rounds dropped from 2.8 to 1.1.”***</span>

<span class="s1">***This is not prompt engineering. This is autonomous process optimization — Maggy observed a pattern, validated it statistically, and changed its behavior to prevent the issue. No human told it to.***</span>

\

6\. Engram: Cross-Session Memory

The Amnesia Problem

<span class="s1">***Every AI coding tool today is an amnesiac. When a session ends, everything the agent learned — project conventions, reviewer preferences, codebase idioms, tool configurations — evaporates. The next session starts from scratch. This isn’t a minor inconvenience; it’s the fundamental bottleneck preventing AI agents from becoming genuinely useful over time.***</span>

<span class="s1">***Engram identifies seven distinct amnesia pathologies:***</span>

|  |  |  |
|----|----|----|
| <span class="s1">***Amnesia Type***</span> | <span class="s1">***What Gets Lost***</span> | <span class="s1">***Impact***</span> |
| <span class="s1">***Anterograde***</span> | <span class="s1">***New memories fail to form across sessions***</span> | <span class="s1">***Every session restarts from zero***</span> |
| <span class="s1">***Retrograde***</span> | <span class="s1">***Existing memories degrade over time***</span> | <span class="s1">***Learned patterns fade***</span> |
| <span class="s1">***Temporal***</span> | <span class="s1">***When something happened is lost***</span> | <span class="s1">***Can’t track how things changed***</span> |
| <span class="s1">***Source***</span> | <span class="s1">***Where a fact came from is lost***</span> | <span class="s1">***Can’t trust or audit memories***</span> |
| <span class="s1">***Interference***</span> | <span class="s1">***Memories from one context contaminate another***</span> | <span class="s1">***Project A’s patterns leak into Project B***</span> |
| <span class="s1">***Context-binding***</span> | <span class="s1">***Right memory, wrong retrieval context***</span> | <span class="s1">***Conventions exist but aren’t surfaced when needed***</span> |
| <span class="s1">***Confabulation***</span> | <span class="s1">***Inferred patterns presented as confirmed facts***</span> | <span class="s1">***Agent “remembers” things it actually guessed***</span> |

The Memory Lifecycle

<span class="s1">***Engram completes Maggy’s memory stack:***</span>

<span class="s1">***Mnemos (within-task) <span class="Apple-converted-space">    </span>→ What the agent remembers during a single task***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">     </span>↓ promote (confidence \> 0.8, evidence \>= 3)***</span><span class="s3">***\***
</span><span class="s1">***Engram (cross-session) <span class="Apple-converted-space">  </span>→ What survives between sessions, per machine***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">     </span>↓ distill to typed memory***</span><span class="s3">***\***
</span><span class="s1">***Mesh (cross-machine) <span class="Apple-converted-space">    </span>→ What's shared across the team, P2P***</span>

<span class="s1">***Without Engram, Maggy has a 10-minute memory. With Engram, knowledge compounds across every session. After 100 sessions, Maggy knows your project’s conventions, your reviewers’ preferences, your CI failure patterns — and applies them automatically.***</span>

Three-Tier Namespace Model

<span class="s1">***Memory is organized into three tiers to prevent both cross-project contamination and useful-pattern siloing:***</span>

- ****<span class="s1">***Local — project-specific memories (strict isolation). A Python FastAPI project’s conventions never contaminate a React project’s patterns.***</span>
- ****<span class="s1">***Portfolio — abstracted cross-project patterns. When a local pattern proves useful across 3+ projects, it’s promoted — but only after de-contextualization (stripping project-specific names and paths).***</span>
- ****<span class="s1">***Mesh — peer-derived memories (quarantined on arrival). Must be locally validated before promotion to portfolio.***</span>

<span class="s1">***This three-tier model means Engram gets smarter across projects without cross-contamination.***</span>

Engram as Improvement Substrate

<span class="s1">***Engram absorbs the improvement ledger. The ledger is the mutation log (what changed), Engram is the memory substrate (persists it across sessions), and the reward registry tracks whether it worked. Every self-modification becomes a persistent, queryable memory — Maggy remembers not just what it learned, but what it tried and what failed.***</span>

Amnesia Score

<span class="s1">***Each project gets a 7-dimension diagnostic score (0.0 = perfect retention, 1.0 = total amnesia). The L3 weekly loop analyzes Amnesia Scores and adjusts encoding rules: if anterograde score is high, lower the promotion threshold; if interference is high, tighten namespace isolation.***</span>

Research Basis

<span class="s1">***Engram builds on validated research: Mem0 (186M API calls, memory-as-object model), Zep/Graphiti (temporal validity windows), Hindsight (91.4% on LongMemEval, fact vs opinion separation), MAGMA (multi-graph retrieval with 45.5% higher reasoning accuracy), and A-MEM (Zettelkasten-style associative encoding). What none of these systems address is the combination of namespace isolation, origin tracking, temporal validity, and amnesia diagnosis in a single architecture designed for multi-project AI agents.***</span>

\

7\. Maggy Mesh: Peer-to-Peer Team Intelligence

The Problem

<span class="s1">***A solo developer’s Maggy learns from their tasks. But teams have 5, 10, 50 developers — each independently discovering the same CI fixes, the same reviewer preferences, the same model performance patterns. That’s wasted learning.***</span>

The Solution

<span class="s1">***Maggy Mesh connects instances across a team into a peer-to-peer network. Each Maggy autonomously shares learned intelligence with other Maggys in the same organization.***</span>

<span class="s1">***┌──────────────────────────────────────────────────────────┐***</span><span class="s3">***\***
</span><span class="s1">***│<span class="Apple-converted-space">                    </span>ORGANIZATION<span class="Apple-converted-space">                            </span>│***</span><span class="s3">***\***
</span><span class="s1">***│ <span class="Apple-converted-space">                                                          </span>│***</span><span class="s3">***\***
</span><span class="s1">***│<span class="Apple-converted-space">  </span>┌─────────┐<span class="Apple-converted-space">    </span>┌─────────┐<span class="Apple-converted-space">    </span>┌─────────┐<span class="Apple-converted-space">              </span>│***</span><span class="s3">***\***
</span><span class="s1">***│<span class="Apple-converted-space">  </span>│ Maggy-A │◄──►│ Maggy-B │◄──►│ Maggy-C │<span class="Apple-converted-space">              </span>│***</span><span class="s3">***\***
</span><span class="s1">***│<span class="Apple-converted-space">  </span>│ (Ali) <span class="Apple-converted-space">  </span>│<span class="Apple-converted-space">    </span>│ (Sarah) │<span class="Apple-converted-space">    </span>│ (John)<span class="Apple-converted-space">  </span>│<span class="Apple-converted-space">              </span>│***</span><span class="s3">***\***
</span><span class="s1">***│<span class="Apple-converted-space">  </span>│ Python<span class="Apple-converted-space">  </span>│<span class="Apple-converted-space">    </span>│ React <span class="Apple-converted-space">  </span>│<span class="Apple-converted-space">    </span>│ DevOps<span class="Apple-converted-space">  </span>│<span class="Apple-converted-space">              </span>│***</span><span class="s3">***\***
</span><span class="s1">***│<span class="Apple-converted-space">  </span>└─────────┘<span class="Apple-converted-space">    </span>└─────────┘<span class="Apple-converted-space">    </span>└─────────┘<span class="Apple-converted-space">              </span>│***</span><span class="s3">***\***
</span><span class="s1">***│ <span class="Apple-converted-space">      </span>▲<span class="Apple-converted-space">              </span>▲<span class="Apple-converted-space">              </span>▲<span class="Apple-converted-space">                    </span>│***</span><span class="s3">***\***
</span><span class="s1">***│ <span class="Apple-converted-space">      </span>└──────────────┴──────────────┘<span class="Apple-converted-space">                    </span>│***</span><span class="s3">***\***
</span><span class="s1">***│<span class="Apple-converted-space">            </span>Full mesh — everyone sees<span class="Apple-converted-space">                      </span>│***</span><span class="s3">***\***
</span><span class="s1">***│<span class="Apple-converted-space">            </span>everyone's learnings <span class="Apple-converted-space">                          </span>│***</span><span class="s3">***\***
</span><span class="s1">***└──────────────────────────────────────────────────────────┘***</span>

What Gets Shared

<span class="s1">***Not everything. Maggy Mesh shares typed memory classes with different merge rules:***</span>

|  |  |  |
|----|----|----|
| <span class="s1">***Type***</span> | <span class="s1">***Example***</span> | <span class="s1">***Merge Rule***</span> |
| <span class="s1">***Scores***</span> | <span class="s1">***“Claude scores 0.92 on auth tasks”***</span> | <span class="s1">***Weighted average by sample count***</span> |
| <span class="s1">***Patterns***</span> | <span class="s1">***“Add error handling before PR”***</span> | <span class="s1">***Union-merge with frequency tracking***</span> |
| <span class="s1">***Policies***</span> | <span class="s1">***“Route blast 7+ to premium only”***</span> | <span class="s1">***Backtest-gated — must pass on local data***</span> |
| <span class="s1">***Gaps***</span> | <span class="s1">***“No Linear integration”***</span> | <span class="s1">***Additive accumulation***</span> |

Provenance

<span class="s1">***Every shared memory carries full provenance:***</span>

- ****<span class="s1">***Who: peer_id, peer_name***</span>
- ****<span class="s1">***Where: project_key, language, toolchain***</span>
- ****<span class="s1">***When: created_at, last_verified***</span>
- ****<span class="s1">***How much: evidence_count, confidence (decays with age)***</span>

<span class="s1">***This enables intelligent filtering: “Only accept Python patterns from peers working on Python projects.”***</span>

Quarantine System

<span class="s1">***Incoming peer data doesn’t go live immediately. It enters quarantine:***</span>

- ****<span class="s1">***Self-confirmed: Local data validates the pattern within 30 days***</span>
- ****<span class="s1">***Crowd-confirmed: 3+ peers independently report the same pattern***</span>
- ****<span class="s1">***Human override: Developer manually promotes or rejects***</span>

<span class="s1">***This prevents poisoning, stale data propagation, and context collapse. A bad pattern from one node can’t silently corrupt the entire team.***</span>

Cold Start

<span class="s1">***A new team member installs Maggy, discovers peers via mDNS, and receives the entire team’s collective intelligence — quarantined until locally validated. Day one, they have the benefit of months of team learning.***</span>

The Compound Effect

<span class="s1">***Individual Maggy:<span class="Apple-converted-space">    </span>knowledge = learning_rate x time***</span><span class="s3">***\***
</span><span class="s1">***Team Mesh (n peers): knowledge = n x learning_rate x time x sharing_factor***</span><span class="s3">***\
\***
</span><span class="s1">***5 developers, 6 months:***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">  </span>Solo:<span class="Apple-converted-space">  </span>1 x 1.0 x 180 = 180 learning units***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">  </span>Mesh:<span class="Apple-converted-space">  </span>5 x 1.0 x 180 x 0.8 = 720 learning units (4x multiplier)***</span>

<span class="s1">***The sharing_factor (0.8) accounts for context mismatch and quarantine filtering. The effect is superlinear because peers validate each other’s patterns through crowd confirmation.***</span>

\

8\. Lexon: Semantic Tool Binding

The Tool Overload Problem

<span class="s1">***As Maggy’s capabilities grow — MCP Forge auto-generates servers, Process Intelligence adds signal collectors, each project adds environment-specific tools — the tool count will cross 50, then 100. Research shows tool selection accuracy collapses at this scale: RAG-MCP demonstrated accuracy dropping from 87% to 13% as tools grew from 10 to 100.***</span>

<span class="s1">***A second failure mode persists even with retrieval: the vocabulary gap. Tool descriptions are written by engineers. Users speak in their own vocabulary. “I want to blast my leads” doesn’t match*** </span><span class="s10">***create_campaign***</span><span class="s1"> ***by any lexical metric. Maggy needs to learn that for this user, “blast” means bulk email send.***</span>

Two-Tier Routing

<span class="s1">***Lexon solves this with a two-tier pipeline that runs in parallel:***</span>

- **Tier A — Fast LLM Router** (\<300ms): A compact tool manifest (name + 1-line description, ~400 tokens for 80 tools) fed to a fast model. Returns 5-7 candidates with rationale. JSON schema constrained to valid tool names — no hallucinated tools.
- **Tier B — Multilingual Semantic Retriever**: Vector search over the full tool registry, indexed by description, example queries, and learned synonyms. Multilingual embedding model ensures queries in any language match correctly.

<span class="s1">***Candidates from both tiers are unioned and deduplicated. Each tier compensates for the other’s failure mode: the LLM captures intent-level reasoning; the retriever captures lexical variants and multilingual matches.***</span>

Terminology Map

<span class="s1">***A three-level vocabulary store that learns over time:***</span>

- ****<span class="s1">***System level: Built-in tool descriptions (baseline)***</span>
- ****<span class="s1">***Org level: Team-shared vocabulary, propagated via Mesh (e.g., “follow up” = specific CRM workflow)***</span>
- ****<span class="s1">***User level: Personal shortcuts and preferences (e.g., “morning sequence” = campaign with time=09:00)***</span>

<span class="s1">***Resolution: user overrides org overrides system. NOT bindings encode negative matches — “blast” is explicitly NOT “delete_all” — preventing recurring mis-selections.***</span>

Dual-Mode Disambiguation

<span class="s1">***When confidence is ambiguous, Lexon has two resolution modes:***</span>

<span class="s1">***Self-clarify (default, autonomous): Lexon resolves ambiguity without asking the user by consulting iCPG’s structured intent, Mnemos context, Engram’s past bindings, process history, and Mesh consensus. If any source resolves confidence above threshold, proceed silently. The goal: 95%+ resolutions via self-clarify after 50+ interactions.***</span>

<span class="s1">***User-clarify (irreversible actions only): Triggered only for destructive, expensive, or irreversible actions (delete, deploy, billing changes). Presents 2-3 concrete options. The user’s selection becomes a permanent binding.***</span>

<span class="s1">***Autonomous agents should almost never trigger user-clarify. This is what separates Maggy from tools that interrupt you constantly.***</span>

Personalization

<span class="s1">***Five implicit learning signals update the Terminology Map without user effort: 1. Correction → add NOT binding + positive binding 2. Affirmation → increment confidence 3. Repetition (5+) → promote to high-confidence synonym 4. Disambiguation selection → capture as user-level binding 5. Clarification repetition (3+) → escalate to explicit preference prompt***</span>

<span class="s1">***High-confidence bindings persist via Engram across sessions and propagate to the org via Mesh.***</span>

Tool Contract Binding

<span class="s1">***Lexon doesn’t just bind phrases to tool names — it binds to tool contracts. Each LexonRecord records the tool version and schema hash at bind time. When a tool’s API changes, Lexon detects the schema drift and re-evaluates bindings rather than silently calling a tool with a different interface. This matters because MCP Forge auto-generates tools from API docs that evolve.***</span>

Outcome-Bearing Records

<span class="s1">***Every LexonRecord carries an outcome reward (-1.0 to 1.0): did the binding produce good results? Corrections are tracked with their source (user explicit, CI failure, review comment). This transforms Lexon from a static lookup table into a reward-bearing learning system that gets measurably better at tool selection over time.***</span>

Research Basis

<span class="s1">***Lexon builds on: RAG-MCP (Anthropic, 2025 — retrieval-based tool selection), Tool2Vec (2024 — example queries as embedding targets), ToolTree (ICLR 2026 — MCTS-style tool planning), Tool-MVR (2025 — self-correction loops), and Gorilla (Berkeley, 2023 — fine-tuned tool LLMs). Lexon’s contribution is the unified architecture combining retrieval, disambiguation, multilingual support, and adaptive personalization — no prior system addresses all four.***</span>

\

9\. Event Spine: The Nervous System

Why an Event Spine

<span class="s1">***Maggy’s components — iCPG, Mnemos, Lexon, Engram, Process Intelligence, Mesh — each generate events in their own formats. Without a canonical event spine, correlating “user said X → Lexon bound tool Y → execution failed → memory Z was created → mutation W was proposed” requires stitching together six different log formats.***</span>

<span class="s1">***The Event Spine defines a single ordered event stream that every component writes to:***</span>

<span class="s1">***IntentEvent → BindingEvent → ExecutionEvent → MemoryEvent***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">                                                   </span>↓***</span><span class="s3">***\***
</span><span class="s1">***MeshEvent ← MutationEvent ← OutcomeEvent ← PersistenceEvent***</span>

<span class="s1">***Eight typed events, each carrying a common header (event_id, task_id, project_id, agent_id, model_id, confidence, namespace, policy_version, reward_delta). This enables:***</span>

- ****<span class="s1">***End-to-end tracing: follow a task_id across all 8 event types***</span>
- ****<span class="s1">***Reward attribution: OutcomeEvent.reward propagates back to BindingEvent (was tool selection good?) and MutationEvent (was self-modification good?)***</span>
- ****<span class="s1">***Replay debugging: reproduce failures from the event stream without re-executing***</span>
- ****<span class="s1">***Amnesia diagnosis: compare MemoryEvent → PersistenceEvent conversion rate per project***</span>
- ****<span class="s1">***Self-improvement validation: MutationEvent + OutcomeEvent = evidence for whether L3/L4 changes helped***</span>

The Positioning Statement

<span class="s1">***Maggy understands intent through iCPG. Maggy survives task execution through Mnemos. Maggy chooses the right capability through Lexon. Maggy remembers consequences through Engram. Maggy evolves behavior through rewards. Maggy spreads successful mutations through Mesh.***</span>

<span class="s1">***The Event Spine connects all six into a single typed, correlated, reward-bearing event stream. This is the nervous system of an autonomous engineering agent.***</span>

\

10\. Competitive Landscape

<span class="s1">***The AI coding tool market has exploded into distinct categories. Understanding where Maggy fits — and where it doesn’t compete — is critical for positioning.***</span>

10.1 Market Taxonomy

<span class="s1">***The landscape breaks into five categories, each with different value propositions:***</span>

<span class="s1">***┌─────────────────────────────────────────────────────────────────┐***</span><span class="s3">***\***
</span><span class="s1">***│ <span class="Apple-converted-space">                  </span>AI CODING TOOL TAXONOMY (2026)<span class="Apple-converted-space">                  </span>│***</span><span class="s3">***\***
</span><span class="s1">***│<span class="Apple-converted-space">                                                                  </span>│***</span><span class="s3">***\***
</span><span class="s1">***│<span class="Apple-converted-space">  </span>1. CLOUD AGENT PLATFORMS (autonomous, cloud-hosted) <span class="Apple-converted-space">            </span>│***</span><span class="s3">***\***
</span><span class="s1">***│ <span class="Apple-converted-space">    </span>Codex (OpenAI), Devin (Cognition), Copilot Cloud Agent<span class="Apple-converted-space">      </span>│***</span><span class="s3">***\***
</span><span class="s1">***│ <span class="Apple-converted-space">    </span>Claude Managed Agents<span class="Apple-converted-space">                                        </span>│***</span><span class="s3">***\***
</span><span class="s1">***│<span class="Apple-converted-space">                                                                  </span>│***</span><span class="s3">***\***
</span><span class="s1">***│<span class="Apple-converted-space">  </span>2. AI-NATIVE IDEs (editor-first, multi-model) <span class="Apple-converted-space">                  </span>│***</span><span class="s3">***\***
</span><span class="s1">***│ <span class="Apple-converted-space">    </span>Cursor, Windsurf (Codeium/Cognition) <span class="Apple-converted-space">                        </span>│***</span><span class="s3">***\***
</span><span class="s1">***│<span class="Apple-converted-space">                                                                  </span>│***</span><span class="s3">***\***
</span><span class="s1">***│<span class="Apple-converted-space">  </span>3. CLI AGENTS (terminal-first, model-agnostic)<span class="Apple-converted-space">                  </span>│***</span><span class="s3">***\***
</span><span class="s1">***│ <span class="Apple-converted-space">    </span>Claude Code, Codex CLI, Aider, OpenCode, Cline<span class="Apple-converted-space">              </span>│***</span><span class="s3">***\***
</span><span class="s1">***│<span class="Apple-converted-space">                                                                  </span>│***</span><span class="s3">***\***
</span><span class="s1">***│<span class="Apple-converted-space">  </span>4. APP BUILDERS (prompt-to-app, no-code/low-code) <span class="Apple-converted-space">              </span>│***</span><span class="s3">***\***
</span><span class="s1">***│ <span class="Apple-converted-space">    </span>Lovable, Bolt.new, Replit Agent, v0 (Vercel) <span class="Apple-converted-space">                </span>│***</span><span class="s3">***\***
</span><span class="s1">***│<span class="Apple-converted-space">                                                                  </span>│***</span><span class="s3">***\***
</span><span class="s1">***│<span class="Apple-converted-space">  </span>5. AUTONOMOUS ENGINEERING PLATFORMS <span class="Apple-converted-space">                            </span>│***</span><span class="s3">***\***
</span><span class="s1">***│ <span class="Apple-converted-space">    </span>Maggy ← ONLY ENTRY <span class="Apple-converted-space">                                          </span>│***</span><span class="s3">***\***
</span><span class="s1">***│ <span class="Apple-converted-space">    </span>(self-improving + process intelligence + team mesh)<span class="Apple-converted-space">          </span>│***</span><span class="s3">***\***
</span><span class="s1">***└─────────────────────────────────────────────────────────────────┘***</span>

<span class="s1">***Maggy is not competing with Lovable (app builders) or Cursor (IDE experience). Maggy competes on a different axis: autonomous improvement over time. The question isn’t “which tool writes better code today?” — it’s “which tool writes better code next month than it did this month?”***</span>

10.2 Cloud Agent Platforms

OpenAI Codex (Cloud)

<span class="s1">***Codex is OpenAI’s cloud-hosted autonomous coding agent, launched May 2025. Each task runs in its own sandboxed cloud environment preloaded with your GitHub repository. It can write features, fix bugs, run tests, and submit PRs — all in parallel.***</span>

|  |  |  |
|----|----|----|
| <span class="s1">***Capability***</span> | <span class="s1">***Codex Cloud***</span> | <span class="s1">***Maggy***</span> |
| <span class="s1">***Execution model***</span> | <span class="s1">***Cloud sandbox (internet disabled)***</span> | <span class="s1">***Local containers (full network)***</span> |
| <span class="s1">***Model***</span> | <span class="s1">***codex-1 (o3 variant), GPT-5.3-Codex***</span> | <span class="s1">***6+ models, learned routing***</span> |
| <span class="s1">***Parallel tasks***</span> | <span class="s1">***Yes (multiple cloud sandboxes)***</span> | <span class="s1">***Yes (Polyphony containers)***</span> |
| <span class="s1">***Self-improvement***</span> | <span class="s1">***No***</span> | <span class="s1">***5-level closed-loop control***</span> |
| <span class="s1">***Process intelligence***</span> | <span class="s1">***No***</span> | <span class="s1">***Full SDLC learning***</span> |
| <span class="s1">***Team learning***</span> | <span class="s1">***No cross-instance learning***</span> | <span class="s1">***Mesh (P2P, autonomous)***</span> |
| <span class="s1">***SWE-bench Verified***</span> | <span class="s1">***85% (GPT-5.3-Codex)***</span> | <span class="s1">***Model-dependent (routes to best)***</span> |
| <span class="s1">***Cost***</span> | <span class="s1">***ChatGPT Pro/Enterprise subscription***</span> | <span class="s1">***Self-hosted, pay-per-model-use***</span> |
| <span class="s1">***Data privacy***</span> | <span class="s1">***Code sent to OpenAI cloud***</span> | <span class="s1">***Local-first, code stays on machine***</span> |
| <span class="s1">***Trigger automation***</span> | <span class="s1">***Codex Jobs (on GitHub push)***</span> | <span class="s1">***Process Intelligence (on any signal)***</span> |

<span class="s1">***Codex’s strength: Cloud-native parallel execution with strong sandboxing. The upcoming Codex Jobs feature (automated triggers on git events) is compelling for CI/CD workflows.***</span>

<span class="s1">***Maggy’s edge: Codex treats each task as independent — it doesn’t learn from past tasks, doesn’t track reviewer patterns, and doesn’t share knowledge across team members. Maggy’s L1-L4 control loops mean task \#100 is handled significantly better than task \#1.***</span>

Devin (Cognition)

<span class="s1">***Devin is an autonomous cloud-based AI software engineer. It reached \$73M ARR by early 2026, with 67% of PRs merged autonomously. Cognition also acquired Windsurf for ~\$250M.***</span>

|  |  |  |
|----|----|----|
| <span class="s1">***Capability***</span> | <span class="s1">***Devin***</span> | <span class="s1">***Maggy***</span> |
| <span class="s1">***Execution model***</span> | <span class="s1">***Cloud VM with browser***</span> | <span class="s1">***Local containers***</span> |
| <span class="s1">***Knowledge system***</span> | <span class="s1">***Playbooks + Knowledge docs (manual)***</span> | <span class="s1">***Dynamic typed memory (automatic)***</span> |
| <span class="s1">***Cross-instance learning***</span> | <span class="s1">***No — knowledge is per-org, manually curated***</span> | <span class="s1">***Yes — Mesh shares automatically***</span> |
| <span class="s1">***Multi-model***</span> | <span class="s1">***Limited***</span> | <span class="s1">***6+ models with auto-routing***</span> |
| <span class="s1">***Self-improvement***</span> | <span class="s1">***Playbooks improve via manual updates***</span> | <span class="s1">***5-level automatic control loops***</span> |
| <span class="s1">***Process intelligence***</span> | <span class="s1">***No***</span> | <span class="s1">***CI, reviews, deploys, merge patterns***</span> |
| <span class="s1">***Managed Devins***</span> | <span class="s1">***Yes (parallel orchestration)***</span> | <span class="s1">***Yes (Polyphony containers)***</span> |
| <span class="s1">***SWE-bench Verified***</span> | <span class="s1">***45.8% (Devin 2.0, unassisted)***</span> | <span class="s1">***Model-dependent***</span> |
| <span class="s1">***Cost***</span> | <span class="s1">***\$500/mo Teams, custom Enterprise***</span> | <span class="s1">***Self-hosted***</span> |
| <span class="s1">***Scheduling***</span> | <span class="s1">***Recurring/one-time scheduled sessions***</span> | <span class="s1">***Continuous background operation***</span> |

<span class="s1">***Devin’s strength: Enterprise organization structure, admin controls, playbook management. The acquisition of Windsurf gives them an IDE play too.***</span>

<span class="s1">***Maggy’s edge: Devin’s knowledge system is manually curated — someone writes playbooks and knowledge docs. Maggy’s intelligence is learned automatically from task outcomes. Devin doesn’t share learnings across team members’ instances; Maggy Mesh does this autonomously.***</span>

Claude Managed Agents

<span class="s1">***Anthropic’s cloud agent platform, updated May 2026 with three significant features: dreaming, outcomes, and multi-agent orchestration.***</span>

|  |  |  |
|----|----|----|
| <span class="s1">***Capability***</span> | <span class="s1">***Claude Managed Agents***</span> | <span class="s1">***Maggy***</span> |
| <span class="s1">***Execution model***</span> | <span class="s1">***Secure cloud containers***</span> | <span class="s1">***Local containers***</span> |
| <span class="s1">***Dreaming***</span> | <span class="s1">***Yes — reviews past sessions, extracts patterns***</span> | <span class="s1">***Similar to L3/L4 loops***</span> |
| <span class="s1">***Memory***</span> | <span class="s1">***Per-agent + cross-agent via dreaming***</span> | <span class="s1">***Typed memory (scores, patterns, policies, gaps)***</span> |
| <span class="s1">***Multi-agent***</span> | <span class="s1">***Orchestration + webhooks***</span> | <span class="s1">***Polyphony containers + cross-agent delegation***</span> |
| <span class="s1">***Self-improvement***</span> | <span class="s1">***Dreaming (research preview)***</span> | <span class="s1">***5-level closed-loop control (designed in)***</span> |
| <span class="s1">***Process intelligence***</span> | <span class="s1">***No***</span> | <span class="s1">***Full SDLC learning***</span> |
| <span class="s1">***Team learning***</span> | <span class="s1">***Cross-agent dreaming (same org)***</span> | <span class="s1">***Mesh (P2P, cross-machine)***</span> |
| <span class="s1">***Local execution***</span> | <span class="s1">***No (cloud only)***</span> | <span class="s1">***Yes (local-first)***</span> |

<span class="s1">***Claude Managed Agents’ strength: Dreaming is the closest any competitor comes to Maggy’s self-improvement concept. Harvey (legal AI) saw 6x task completion improvement after implementing dreaming. The cross-agent pattern extraction is genuinely novel.***</span>

<span class="s1">***Maggy’s edge: Dreaming is cloud-only and Anthropic-locked. Maggy’s control loops work locally, across any model, and share learnings across developer machines — not just across agent sessions in the cloud.***</span>

GitHub Copilot (Cloud Agent + Agent Mode)

<span class="s1">***Copilot evolved from autocomplete to a multi-layered platform: inline suggestions, chat, agent mode (IDE), and cloud agent (autonomous).***</span>

|  |  |  |
|----|----|----|
| <span class="s1">***Capability***</span> | <span class="s1">***Copilot***</span> | <span class="s1">***Maggy***</span> |
| <span class="s1">***Code completion***</span> | <span class="s1">***Best-in-class inline suggestions***</span> | <span class="s1">***Via Pi (any model)***</span> |
| <span class="s1">***Cloud agent***</span> | <span class="s1">***Yes — autonomous PRs from issues***</span> | <span class="s1">***Yes — local containers***</span> |
| <span class="s1">***Agent mode***</span> | <span class="s1">***IDE-integrated (VS Code, Visual Studio)***</span> | <span class="s1">***CLI + web dashboard***</span> |
| <span class="s1">***Custom agents***</span> | <span class="s1">***User-level + repo-level definitions***</span> | <span class="s1">***Skills + iCPG + Mnemos***</span> |
| <span class="s1">***Multi-model***</span> | <span class="s1">***Yes (***</span><span class="s2">gpt-5</span><span class="s1">***, Claude, Gemini via settings)***</span> | <span class="s1">***Yes (6+ models, learned routing)***</span> |
| <span class="s1">***Security tools***</span> | <span class="s1">***Security Reviewer agent (beta)***</span> | <span class="s1">***iCPG drift detection***</span> |
| <span class="s1">***Self-improvement***</span> | <span class="s1">***No***</span> | <span class="s1">***5-level closed-loop control***</span> |
| <span class="s1">***Process intelligence***</span> | <span class="s1">***No***</span> | <span class="s1">***Full SDLC learning***</span> |
| <span class="s1">***Team learning***</span> | <span class="s1">***Spaces (cloud-mediated, admin-controlled)***</span> | <span class="s1">***Mesh (P2P, autonomous)***</span> |
| <span class="s1">***Debugger agent***</span> | <span class="s1">***Yes (Visual Studio, runtime validation)***</span> | <span class="s1">***L0 real-time control***</span> |
| <span class="s1">***Ecosystem***</span> | <span class="s1">***GitHub-native (Issues, PRs, Actions)***</span> | <span class="s1">***GitHub API + any ticketing system***</span> |

<span class="s1">***Copilot’s strength: Deepest IDE integration. The debugger agent validating fixes against runtime behavior is unique. GitHub ecosystem integration is unmatched. Custom agents with workspace awareness, MCP connections, and model selection are powerful.***</span>

<span class="s1">***Maggy’s edge: Copilot doesn’t learn from its mistakes. It doesn’t track which model does best on which task type. It doesn’t observe CI results to preemptively fix reviewer complaints. And Spaces is admin-curated knowledge — not automatically learned intelligence.***</span>

10.3 AI-Native IDEs

Cursor

<span class="s1">***Cursor is the leading AI-native IDE (~\$100M+ ARR), a fork of VS Code with deep AI integration.***</span>

|  |  |  |
|----|----|----|
| <span class="s1">***Capability***</span> | <span class="s1">***Cursor***</span> | <span class="s1">***Maggy***</span> |
| <span class="s1">***IDE experience***</span> | <span class="s1">***Native (fork of VS Code)***</span> | <span class="s1">***CLI + web dashboard***</span> |
| <span class="s1">***Background agents***</span> | <span class="s1">***8 parallel cloud agents***</span> | <span class="s1">***Polyphony local containers***</span> |
| <span class="s1">***Memories***</span> | <span class="s1">***Project-scoped, persisted across sessions***</span> | <span class="s1">***Typed memory with provenance***</span> |
| <span class="s1">***Rules***</span> | <span class="s10">***.cursorrules***</span><span class="s1">***, project rules***</span> | <span class="s1">***Skills (***</span><span class="s10">***.md***</span><span class="s1">***), iCPG, Mnemos***</span> |
| <span class="s1">***Security review***</span> | <span class="s1">***Always-on PR security agents (beta)***</span> | <span class="s1">***iCPG constraints + drift***</span> |
| <span class="s1">***Team features***</span> | <span class="s1">***Centralized billing, usage analytics***</span> | <span class="s1">***Mesh (P2P intelligence sharing)***</span> |
| <span class="s1">***Model routing***</span> | <span class="s1">***Manual selection***</span> | <span class="s1">***Learned from reward data***</span> |
| <span class="s1">***Self-improvement***</span> | <span class="s1">***Memories (passive)***</span> | <span class="s1">***5-level active control loops***</span> |
| <span class="s1">***Process intelligence***</span> | <span class="s1">***No***</span> | <span class="s1">***Full SDLC learning***</span> |
| <span class="s1">***Context management***</span> | <span class="s1">***Rules, skills, MCPs, subagents***</span> | <span class="s1">***Skills, iCPG, Mnemos, code graph***</span> |

<span class="s1">***Cursor’s strength: UX polish, background agents at scale (8 parallel), and the always-on security review agents. The context usage breakdown (rules, skills, MCPs) shows mature observability.***</span>

<span class="s1">***Maggy’s edge: Cursor’s memories are passive (“remember this fact”). Maggy’s memory is active — it observes outcomes and adjusts behavior. Cursor doesn’t learn from CI failures, doesn’t track reviewer patterns, and doesn’t share intelligence P2P.***</span>

Windsurf (Codeium → Cognition)

<span class="s1">***Windsurf’s Cascade agent plans and executes multi-file edits with a dedicated planning agent running in the background. Acquired by Cognition (Devin) for ~\$250M in December 2025.***</span>

|  |  |  |
|----|----|----|
| <span class="s1">***Capability***</span> | <span class="s1">***Windsurf***</span> | <span class="s1">***Maggy***</span> |
| <span class="s1">***Agent***</span> | <span class="s1">***Cascade (plan + execute)***</span> | <span class="s1">***Multi-level control loops***</span> |
| <span class="s1">***Codemaps***</span> | <span class="s1">***AI-annotated visual code maps***</span> | <span class="s1">***codebase-memory-mcp graph***</span> |
| <span class="s1">***Built-in browser***</span> | <span class="s1">***Yes (web context for Cascade)***</span> | <span class="s1">***Process Intelligence API hooks***</span> |
| <span class="s1">***Self-improvement***</span> | <span class="s1">***No***</span> | <span class="s1">***5-level closed-loop control***</span> |
| <span class="s1">***Cost***</span> | <span class="s1">***\$15/mo Pro***</span> | <span class="s1">***Self-hosted***</span> |

10.4 CLI Agents

Claude Code

<span class="s1">***Anthropic’s terminal-first coding agent. Runs locally, supports multi-agent orchestration via Task tool with teams.***</span>

|  |  |  |
|----|----|----|
| <span class="s1">***Capability***</span> | <span class="s1">***Claude Code***</span> | <span class="s1">***Maggy***</span> |
| <span class="s1">***Multi-agent***</span> | <span class="s1">***Task tool, teams, SendMessage***</span> | <span class="s1">***Polyphony containers + Pi***</span> |
| <span class="s1">***Model***</span> | <span class="s1">***Claude only***</span> | <span class="s1">***6+ models with auto-routing***</span> |
| <span class="s1">***IDE integration***</span> | <span class="s1">***VS Code, JetBrains, desktop app***</span> | <span class="s1">***CLI + web dashboard***</span> |
| <span class="s1">***Hooks***</span> | <span class="s1">***PreToolUse, PostToolUse, Stop***</span> | <span class="s1">***Skills + hooks + L0 real-time***</span> |
| <span class="s1">***Self-improvement***</span> | <span class="s1">***No***</span> | <span class="s1">***5-level closed-loop control***</span> |
| <span class="s1">***MCP support***</span> | <span class="s1">***Native***</span> | <span class="s1">***Native + MCP Forge (auto-generate)***</span> |

<span class="s1">***Note: Maggy is built on Claude Code’s infrastructure (skills, hooks, MCP). It extends Claude Code with self-improvement, multi-model routing, process intelligence, and team mesh.***</span>

Codex CLI (OpenAI)

<span class="s1">***Open-source (Apache-2.0), Rust-based terminal agent. 81K+ GitHub stars. Runs locally, authenticates via ChatGPT account or API key.***</span>

|  |  |  |
|----|----|----|
| <span class="s1">***Capability***</span> | <span class="s1">***Codex CLI***</span> | <span class="s1">***Maggy***</span> |
| <span class="s1">***Open source***</span> | <span class="s1">***Yes (Apache-2.0, 81K stars)***</span> | <span class="s1">***Yes***</span> |
| <span class="s1">***Language***</span> | <span class="s1">***Rust (96.3%)***</span> | <span class="s1">***Python***</span> |
| <span class="s1">***Model***</span> | <span class="s1">***OpenAI models only***</span> | <span class="s1">***6+ providers***</span> |
| <span class="s1">***Self-improvement***</span> | <span class="s1">***No***</span> | <span class="s1">***5-level closed-loop control***</span> |
| <span class="s1">***Team learning***</span> | <span class="s1">***No***</span> | <span class="s1">***Mesh (P2P)***</span> |

Aider

<span class="s1">***Open-source CLI pair programmer. 39K+ GitHub stars, 4.1M+ installations. Model-agnostic with an architect/editor dual-model approach.***</span>

|  |  |  |
|----|----|----|
| <span class="s1">***Capability***</span> | <span class="s1">***Aider***</span> | <span class="s1">***Maggy***</span> |
| <span class="s1">***Open source***</span> | <span class="s1">***Yes (39K stars)***</span> | <span class="s1">***Yes***</span> |
| <span class="s1">***Multi-model***</span> | <span class="s1">***Yes (75+ providers)***</span> | <span class="s1">***Yes (6+ with auto-routing)***</span> |
| <span class="s1">***Architect mode***</span> | <span class="s1">***Dual-model: strong planner + cheap editor***</span> | <span class="s1">***Dual-model planning (Phase 6)***</span> |
| <span class="s1">***Git integration***</span> | <span class="s1">***Every edit = reviewable commit***</span> | <span class="s1">***iCPG + Polyphony branches***</span> |
| <span class="s1">***Auto-lint/test***</span> | <span class="s1">***Yes (on every change)***</span> | <span class="s1">***L0 real-time control***</span> |
| <span class="s1">***Self-improvement***</span> | <span class="s1">***No***</span> | <span class="s1">***5-level closed-loop control***</span> |
| <span class="s1">***Team learning***</span> | <span class="s1">***No***</span> | <span class="s1">***Mesh (P2P)***</span> |

<span class="s1">***Aider’s strength: The architect/editor mode is clever cost optimization — expensive model plans, cheap model executes. Maggy’s Phase 6 dual-model planning is similar but adds conflict resolution and outcome tracking.***</span>

OpenCode

<span class="s1">***Was a Go-based CLI with TUI (Bubble Tea), 12K+ stars. Archived September 2025, now continued as “Crush” by the original author (Charm team). Supported 75+ LLM providers, SQLite session storage, LSP integration.***</span>

10.5 App Builders

<span class="s1">***These tools target a different audience (non-developers, designers, rapid prototyping) but are worth understanding as they represent the “opposite end” of the autonomy spectrum.***</span>

Lovable

<span class="s1">***Prompt-to-full-stack-app builder. 2.3M users, \$100M ARR, \$6.6B valuation (Series B, Dec 2025, backed by Nvidia/Salesforce).***</span>

|  |  |  |
|----|----|----|
| <span class="s1">***Capability***</span> | <span class="s1">***Lovable***</span> | <span class="s1">***Maggy***</span> |
| <span class="s1">***Target user***</span> | <span class="s1">***Non-developers, designers***</span> | <span class="s1">***Professional developers***</span> |
| <span class="s1">***Output***</span> | <span class="s1">***Full-stack app from prompt***</span> | <span class="s1">***Code changes to existing codebase***</span> |
| <span class="s1">***Stack***</span> | <span class="s1">***React + TypeScript + Supabase***</span> | <span class="s1">***Any stack***</span> |
| <span class="s1">***Agent mode***</span> | <span class="s1">***Autonomous development mode***</span> | <span class="s1">***Multi-level control loops***</span> |
| <span class="s1">***GitHub sync***</span> | <span class="s1">***Yes***</span> | <span class="s1">***Native (git-first)***</span> |
| <span class="s1">***Self-improvement***</span> | <span class="s1">***No***</span> | <span class="s1">***5-level closed-loop control***</span> |

Bolt.new, Replit Agent, v0

- ****<span class="s1">***Bolt.new — Browser-based JS app generator. 1M+ websites generated in 5 months.***</span>
- ****<span class="s1">***Replit Agent 4 (March 2026) — Handles auth, databases, parallel task execution, Design Mode, checkpoint rollback. Richest ecosystem (50+ languages).***</span>
- ****<span class="s1">***v0 (Vercel) — Specializes in React components with Tailwind/shadcn/ui. Precision frontend generation.***</span>

<span class="s1">***These are complementary to Maggy, not competitive. A developer might use Lovable to prototype, then bring the codebase into Maggy for professional development with CI integration, code quality tracking, and team collaboration.***</span>

10.6 Summary Comparison Matrix

|  |  |  |  |  |  |  |  |  |
|----|----|----|----|----|----|----|----|----|
| <span class="s1">***Capability***</span> | <span class="s1">***Codex Cloud***</span> | <span class="s1">***Devin***</span> | <span class="s1">***Claude Managed***</span> | <span class="s1">***Copilot***</span> | <span class="s1">***Cursor***</span> | <span class="s1">***Claude Code***</span> | <span class="s1">***Aider***</span> | <span class="s1">***Maggy***</span> |
| <span class="s1">***Self-improvement***</span> | <span class="s1">***-***</span> | <span class="s1">***-***</span> | <span class="s1">***Dreaming (preview)***</span> | <span class="s1">***-***</span> | <span class="s1">***-***</span> | <span class="s1">***-***</span> | <span class="s1">***-***</span> | <span class="s1">***5-level control***</span> |
| <span class="s1">***Process intelligence***</span> | <span class="s1">***-***</span> | <span class="s1">***-***</span> | <span class="s1">***-***</span> | <span class="s1">***-***</span> | <span class="s1">***-***</span> | <span class="s1">***-***</span> | <span class="s1">***-***</span> | <span class="s1">***Full SDLC***</span> |
| <span class="s1">***Team learning***</span> | <span class="s1">***-***</span> | <span class="s1">***-***</span> | <span class="s1">***Cross-agent dreaming***</span> | <span class="s1">***Spaces***</span> | <span class="s1">***Org memories***</span> | <span class="s1">***-***</span> | <span class="s1">***-***</span> | <span class="s1">***P2P Mesh***</span> |
| <span class="s1">***Multi-model routing***</span> | <span class="s1">***-***</span> | <span class="s1">***Limited***</span> | <span class="s1">***-***</span> | <span class="s1">***Manual***</span> | <span class="s1">***Manual***</span> | <span class="s1">***-***</span> | <span class="s1">***Manual***</span> | <span class="s1">***Learned***</span> |
| <span class="s1">***Local-first***</span> | <span class="s1">***-***</span> | <span class="s1">***-***</span> | <span class="s1">***-***</span> | <span class="s1">***-***</span> | <span class="s1">***Partial***</span> | <span class="s1">***Yes***</span> | <span class="s1">***Yes***</span> | <span class="s1">***Yes***</span> |
| <span class="s1">***Cloud agents***</span> | <span class="s1">***Yes***</span> | <span class="s1">***Yes***</span> | <span class="s1">***Yes***</span> | <span class="s1">***Yes***</span> | <span class="s1">***Yes***</span> | <span class="s1">***-***</span> | <span class="s1">***-***</span> | <span class="s1">***-***</span> |
| <span class="s1">***IDE integration***</span> | <span class="s1">***VS Code***</span> | <span class="s1">***Browser***</span> | <span class="s1">***-***</span> | <span class="s1">***Native***</span> | <span class="s1">***Native***</span> | <span class="s1">***VS Code***</span> | <span class="s1">***Terminal***</span> | <span class="s1">***Dashboard***</span> |
| <span class="s1">***Open source***</span> | <span class="s1">***CLI only***</span> | <span class="s1">***-***</span> | <span class="s1">***-***</span> | <span class="s1">***-***</span> | <span class="s1">***-***</span> | <span class="s1">***-***</span> | <span class="s1">***Yes***</span> | <span class="s1">***Yes***</span> |
| <span class="s1">***Vendor lock-in***</span> | <span class="s1">***OpenAI***</span> | <span class="s1">***Cognition***</span> | <span class="s1">***Anthropic***</span> | <span class="s1">***GitHub***</span> | <span class="s1">***Cursor***</span> | <span class="s1">***Anthropic***</span> | <span class="s1">***None***</span> | <span class="s1">***None***</span> |

10.7 Where Maggy Wins

- ****<span class="s1">***Self-improvement is the product — No other tool has a formal multi-level control system. Claude’s dreaming is the closest, but it’s cloud-only and single-vendor.***</span>
- ****<span class="s1">***Process intelligence is unique — Nobody else learns from CI results, reviewer comments, and merge patterns to preemptively fix code.***</span>
- ****<span class="s1">***Autonomous team learning — Mesh shares typed, provenanced intelligence P2P without a central server. Everyone else’s “team features” are admin-curated knowledge or cloud-mediated memory.***</span>
- ****<span class="s1">***Model-agnostic by design — Not locked to any provider. Learns which model is best for which task type automatically.***</span>
- ****<span class="s1">***Local-first with no compromises — Code never leaves developer machines. Works offline with local models. No vendor sees your proprietary codebase.***</span>

10.8 Where Competitors Win Today

- ****<span class="s1">***Copilot: Deepest IDE integration, GitHub ecosystem, largest user base***</span>
- ****<span class="s1">***Cursor: Best editor UX, background agents at scale, security review agents***</span>
- ****<span class="s1">***Devin: Enterprise controls, playbooks, \$73M ARR proves market demand***</span>
- ****<span class="s1">***Claude Managed Agents: Dreaming is genuinely novel, cloud scalability***</span>
- ****<span class="s1">***Codex Cloud: Parallel cloud sandboxes, upcoming Codex Jobs automation***</span>
- ****<span class="s1">***Lovable: Prompt-to-app for non-developers, \$6.6B validates the broader market***</span>
- ****<span class="s1">***Aider: Open-source community (39K stars), architect/editor cost optimization***</span>

\

11\. Migration Roadmap

Phase Dependencies

<span class="s1">***Phase 1: PiAdapter + Token Budget ──────────────────┐***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">    </span>│ <span class="Apple-converted-space">                                                </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">    </span>├── Phase 2: Model Routing (blast→model)<span class="Apple-converted-space">          </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">    </span>├── Phase 3: Mnemos Multi-Model Fatigue <span class="Apple-converted-space">          </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">    </span>├── Phase 6: Dual-Model Planning<span class="Apple-converted-space">                  </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">    </span>│ <span class="Apple-converted-space">                                                </span>│***</span><span class="s3">***\***
</span><span class="s1">***Phase 4: CIKG Extract ────────────────┐ <span class="Apple-converted-space">              </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">    </span>│<span class="Apple-converted-space">                                  </span>│<span class="Apple-converted-space">              </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">    </span>└───────────┬──────────────────────┘<span class="Apple-converted-space">              </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">                </span>│ <span class="Apple-converted-space">                                    </span>│***</span><span class="s3">***\***
</span><span class="s1">***Phase 5: Maggy v2 Dashboard ◄─────────────────────────┘***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">    </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">    </span>├── Phase 7: Vercel Deploy Containers (Docker)***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">    </span>├── Phase 8: Process Intelligence ──────┐***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">    </span>├── Phase 9: MCP Forge<span class="Apple-converted-space">                  </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">    </span>│ <span class="Apple-converted-space">                                      </span>│***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">    </span>└── Phase 11: Maggy Mesh ◄──────────────┘***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">                                            </span>│***</span><span class="s3">***\***
</span><span class="s1">***Phase 10: Integration Testing ◄─────────────┘***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">                                            </span>│***</span><span class="s3">***\***
</span><span class="s1">***Phase 3 + Phase 5 ──► Phase 12: Engram ─────┘***</span><span class="s3">***\***
</span><span class="s1">***<span class="Apple-converted-space">                                    </span>│***</span><span class="s3">***\***
</span><span class="s1">***Phase 9 + Phase 12 ─► Phase 13: Lexon***</span>

Phase Summary

|  |  |  |  |  |
|----|----|----|----|----|
| <span class="s1">***Phase***</span> | <span class="s1">***What***</span> | <span class="s1">***Priority***</span> | <span class="s1">***Effort***</span> | <span class="s1">***Dependencies***</span> |
| <span class="s1">***1***</span> | <span class="s1">***PiAdapter + token budget***</span> | <span class="s1">***P0***</span> | <span class="s1">***Large***</span> | <span class="s1">***Pi installed***</span> |
| <span class="s1">***2***</span> | <span class="s1">***Model routing (blast→model)***</span> | <span class="s1">***P0***</span> | <span class="s1">***Medium***</span> | <span class="s1">***Phase 1 + iCPG***</span> |
| <span class="s1">***3***</span> | <span class="s1">***Mnemos multi-model fatigue***</span> | <span class="s1">***P1***</span> | <span class="s1">***Medium***</span> | <span class="s1">***Phase 1***</span> |
| <span class="s1">***4***</span> | <span class="s1">***CIKG extraction***</span> | <span class="s1">***P1***</span> | <span class="s1">***Medium***</span> | <span class="s1">***Supabase***</span> |
| <span class="s1">***5***</span> | <span class="s1">***Maggy v2 dashboard***</span> | <span class="s1">***P0***</span> | <span class="s1">***Large***</span> | <span class="s1">***Phases 1-4***</span> |
| <span class="s1">***6***</span> | <span class="s1">***Dual-model planning***</span> | <span class="s1">***P2***</span> | <span class="s1">***Medium***</span> | <span class="s1">***Phase 1***</span> |
| <span class="s1">***7***</span> | <span class="s1">***Vercel deploy containers***</span> | <span class="s1">***P2***</span> | <span class="s1">***Medium***</span> | <span class="s1">***Docker***</span> |
| <span class="s1">***8***</span> | <span class="s1">***Process intelligence***</span> | <span class="s1">***P1***</span> | <span class="s1">***Large***</span> | <span class="s1">***Phase 5 + GitHub API***</span> |
| <span class="s1">***9***</span> | <span class="s1">***MCP Forge***</span> | <span class="s1">***P2***</span> | <span class="s1">***Large***</span> | <span class="s1">***Phase 5***</span> |
| <span class="s1">***10***</span> | <span class="s1">***Integration testing + docs***</span> | <span class="s1">***P1***</span> | <span class="s1">***Large***</span> | <span class="s1">***All phases***</span> |
| <span class="s1">***11***</span> | <span class="s1">***Maggy Mesh (P2P)***</span> | <span class="s1">***P2***</span> | <span class="s1">***XL***</span> | <span class="s1">***Phase 5 + Phase 8***</span> |
| <span class="s1">***12***</span> | <span class="s1">***Engram (cross-session memory)***</span> | <span class="s1">***P1***</span> | <span class="s1">***Large***</span> | <span class="s1">***Phase 3 + Phase 5***</span> |
| <span class="s1">***13***</span> | <span class="s1">***Lexon (semantic tool binding)***</span> | <span class="s1">***P2***</span> | <span class="s1">***Large***</span> | <span class="s1">***Phase 9 + Phase 12***</span> |

\

12\. Research Foundations & Prior Art

<span class="s1">***Maggy’s architecture draws from five distinct research streams. This isn’t a tool assembled from hype — each component maps to validated research with production evidence.***</span>

12.1 Self-Evolving Agent Systems

<span class="s1">***The field of self-improving AI agents has exploded in 2025-2026. Papers mentioning “AI Agent” or “Agentic AI” in 2025 exceeded the total from 2020-2024 combined by more than twofold.***</span>

<span class="s1">***Key papers and systems:***</span>

- **SICA — Self-Improving Coding Agent (ICLR 2025 Workshop)** — An agent that autonomously edits its own codebase, climbing from 17% to 53% on SWE-bench Verified through self-modification. This validates Maggy’s core thesis: agents that modify their own behavior based on outcomes dramatically outperform static agents. (<span class="s11">Paper</span>)
- **Godel Agent (ACL 2025)** — Uses runtime monkey-patching with safety verification. The agent modifies both its task-solving policy and its own learning algorithm, guided by high-level objectives while formal invariant checking prevents unsafe changes. Maggy’s L3/L4 control loops use a similar principle: change the improvement process itself, but with rollback safeguards.
- **SAGE — Skill Augmented GRPO (December 2025)** — Agents accumulate reusable function libraries across task chains, achieving 8.9% goal completion gains while reducing output tokens by 59%. This directly parallels Maggy’s skill evolution in L3, where successful patterns get codified into reusable skills.
- **HyperAgents (2026)** — Makes the meta-level itself editable. Agents improve *how they improve*, discovering domain-general skills (memory management, prompt engineering, exploration strategies) that transfer across coding, mathematics, and scientific domains. Maggy’s L4 monthly evolution loop is designed for exactly this: improving the improvement process.
- **SWE-RL (Meta, 2025)** — Uses self-play where agents alternate between bug injection and fixing roles, gaining +10.4 points on SWE-bench Verified without human-labeled data. This reinforcement-based approach validates Maggy’s reward registry concept.
- **AlphaEvolve (Google DeepMind)** — Recovered 0.7% of Google’s worldwide compute through automated algorithm optimization. This is the first evidence of hyperscale ROI from self-improving agents — validating that autonomous optimization can deliver measurable economic value.

<span class="s1">***Maggy’s position: Maggy applies self-evolution at the operational level (routing, workflows, process patterns) rather than at the model-weight level. This is more practical for a local-first system — you don’t need GPU clusters to improve model routing decisions based on task rewards.***</span>

12.2 Agent Memory Systems

<span class="s1">***Memory has emerged as the central bottleneck for autonomous agents. A comprehensive 2025-2026 survey (“Memory in the Age of AI Agents”) offers a structured taxonomy of how memory is designed, implemented, and evaluated in modern LLM-based agents.***</span>

<span class="s1">***Key developments:***</span>

- **Mem0 (2025-2026)** — Dominates commercially with 186 million API calls quarterly. The graph-enhanced variant (Mem0g) builds a directed, labeled knowledge graph alongside the vector store. Maggy’s typed memory system (scores, patterns, policies, gaps) is similarly structured but uses domain-specific merge rules rather than a general-purpose graph.
- **Collaborative Memory (2025)** — A framework for multi-user, multi-agent environments with asymmetric, time-evolving access controls. Maintains private memory (per-user) and shared memory (selectively shared). This directly validates Maggy Mesh’s approach of personal memory + team memory with provenance-based filtering.
- **MAGMA: Multi-Graph Agentic Memory Architecture (2026)** — Uses multiple graph structures for different memory types. Parallels Maggy’s typed memory classes where scores, patterns, and policies each have different storage and merge semantics.
- **SimpleMem (2025)** — Achieved 26.4% average F1 improvement over baselines with 30x token reduction. Demonstrates that structured memory management produces dramatically better results than naive context stuffing.

<span class="s1">***Maggy’s position: Most memory systems are passive stores. Maggy’s memory is active — the L1-L4 control loops continuously update, prune, and evolve stored knowledge based on outcomes. The Mesh adds a distributed dimension that no other agent memory system currently implements.***</span>

12.3 Federated & Distributed AI

- **Federated AI Agents** — Intelligent software systems that learn collaboratively across multiple devices while keeping data localized. This is the theoretical foundation for Maggy Mesh: share learned intelligence, not raw data.
- **Agentic Federated Learning (ICML 2025)** — Autonomous agents collaborate on distributed learning tasks, each contributing local expertise to a shared model. Maggy adapts this from model training to operational intelligence: instead of sharing gradients, Maggy shares typed memory (scores, patterns, policies) with provenance.
- **Multi-Agent Collaboration Surveys (ACM DEAI 2025)** — A unified taxonomy decomposing AI agents into Perception, Brain, Planning, Action, Tool Use, and Collaboration subsystems. Surveys show collaborative architectures outperform isolated agents by 30-60% on complex tasks. Gartner reported a 1,445% surge in multi-agent system inquiries from Q1 2024 to Q2 2025.
- **CRDT-inspired merge** — Conflict-free replicated data types allow distributed systems to merge state without coordination. Maggy uses type-specific merge rules (weighted average for scores, union for patterns, backtest-gated for policies) inspired by CRDT semantics.

12.4 Self-Improving Coding in Production

<span class="s1">***The research isn’t just theoretical. Production deployments validate that self-improving agents deliver measurable value:***</span>

|  |  |  |
|----|----|----|
| <span class="s1">***System***</span> | <span class="s1">***Result***</span> | <span class="s1">***Relevance to Maggy***</span> |
| <span class="s1">***Meta’s REA***</span> | <span class="s1">***Doubled model accuracy; 3 engineers improved 8 models simultaneously***</span> | <span class="s1">***Multi-model optimization works at scale***</span> |
| <span class="s1">***Cognition (Devin)***</span> | <span class="s1">***\$73M ARR, 67% of PRs merged autonomously***</span> | <span class="s1">***Market demand for autonomous engineering is real***</span> |
| <span class="s1">***Harvey + Claude Dreaming***</span> | <span class="s1">***6x task completion improvement***</span> | <span class="s1">***Cross-session pattern extraction works***</span> |
| <span class="s1">***Karpathy’s autoresearch***</span> | <span class="s1">***630-line script, 700 experiments in 2 days, 20 optimizations, 11% efficiency gain***</span> | <span class="s1">***Automated experimentation finds real improvements***</span> |
| <span class="s1">***AlphaEvolve***</span> | <span class="s1">***0.7% of Google’s worldwide compute recovered***</span> | <span class="s1">***Self-improvement produces hyperscale ROI***</span> |

<span class="s1">***Claude Managed Agents — Dreaming (May 2026): Anthropic’s most relevant competitive move. Dreaming is a scheduled process that reviews past agent sessions, extracts patterns, and curates memories so agents improve over time. It surfaces insights no single session could see: recurring mistakes, workflows that multiple agents converge on, and team-shared preferences. This is the closest any competitor comes to Maggy’s L3/L4 control loops — but it’s cloud-only, Anthropic-locked, and doesn’t include process intelligence (CI/review/deploy learning).***</span>

12.5 Control Theory Foundations

- **Inner-outer loop control** — Industrial control systems use fast inner loops for stability and slow outer loops for optimization. Maggy’s L0 (seconds) through L4 (months) hierarchy mirrors this established engineering pattern. The key insight: outer loops NEVER override inner loop stability. L3 can change routing policy, but L0 still catches in-task failures regardless.
- **Reinforcement learning from task outcomes** — Maggy’s reward registry applies RLHF principles at the system level, using task outcomes (CI pass, review rounds, deploy success) and user behavior (overrides, re-dos, reverts) as reward signals. Unlike RLHF for model training, this operates at the operational level without any model fine-tuning.

12.6 Local-First Software

- **Local-first principles (Ink & Switch, 2019)** — Software that works offline, keeps data on user devices, and syncs peer-to-peer. Maggy’s architecture is explicitly local-first: SQLite databases, local filesystem storage, optional P2P sync.
- **Privacy-first trend (2026)** — Multiple tools now emphasize data privacy. OpenCode stores no code or context data. Aider runs entirely locally. The market is moving toward local execution as enterprises grow wary of sending proprietary code to cloud services. Maggy was designed local-first from day one — this isn’t a retrofit.

12.7 Market Context

<span class="s1">***The AI coding tool market is at an inflection point:***</span>

- ****<span class="s1">***Gartner predicts 40% of enterprise apps will include task-specific AI agents by 2026, up from less than 5% in 2025.***</span>
- ****<span class="s1">***57% of organizations report measurable impact from AI agents in software development (2025 industry survey).***</span>
- ****<span class="s1">***The explosion of coding CLIs (30+ tools in 2026) reflects a shift from IDE-native AI to terminal-first agents that understand codebases, git history, and development workflows.***</span>
- ****<span class="s1">***SWE-bench scores continue to climb: Claude Mythos Preview hits 93.9% on Verified, 77.8% on Pro. But raw coding ability is becoming commoditized. The differentiation is moving to what surrounds the model: memory, learning, process integration, and team collaboration.***</span>

<span class="s1">***The implication for Maggy: Raw code generation quality is converging across models. The next competitive frontier is what happens around the generation: learning from outcomes, optimizing processes, sharing intelligence across teams. This is exactly where Maggy’s architecture is positioned.***</span>

\

13\. How to Get Started

Installation

<span class="s12">***git***</span><span class="s1"> ***clone https://github.com/alinaqi/maggy.git***</span><span class="s3">***\***
</span><span class="s13">***cd***</span><span class="s1"> ***maggy***</span><span class="s3">***\***
</span><span class="s1">***./install.sh***</span>

Current State (v4.0)

<span class="s1">***Today, Maggy includes: - Skills system — Markdown-based instructions for AI agents (TDD, security, iCPG, Mnemos, etc.) - Polyphony — Container-isolated multi-agent orchestration (173 tests, 14 modules) - iCPG — Intent-augmented code property graph with blast radius scoring - Mnemos — Task-scoped memory lifecycle with typed MnemoGraph - Cross-agent delegation — Complexity-based task routing to Codex, Kimi, etc. - Skill-lint — Quality gates for skill files - Behavioral evals — Test framework for skill effectiveness***</span>

Roadmap to v5.0

<span class="s1">***The 11-phase migration path takes Maggy from a single-project, single-model toolkit to the multi-project, multi-model, self-improving, team-learning platform described in this RFC.***</span>

\
