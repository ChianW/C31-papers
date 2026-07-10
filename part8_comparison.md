# Agent Harness Papers, Part 8: Seven Frameworks Compared — The State of AI Engineering, Mid-2026

**Series:** Agent Harness Papers
**Author:** Research notes compiled from public repositories and documentation
**Subject:** Comparative analysis of seven AI agent engineering frameworks

![7 AI Agent Frameworks Compared](images/final_part8_comparison.jpg)

---

> *"7 frameworks, 7 philosophies, 1 goal: stop AI from being a disposable tool."*

---

## Introduction

Over the course of this series, we've examined seven frameworks that each attempt to answer the same fundamental question: **how do you make AI agents reliably useful for real engineering work?**

Each framework comes from a different starting point, serves a different audience, and makes different bets about what matters most. But as we'll see, they've converged — independently, without coordination — on a set of shared principles that amount to a 2026 consensus on what AI-assisted engineering should look like.

This article maps the full landscape. We'll compare all seven frameworks across eight dimensions, trace the philosophical spectrum from manifesto to implementation, provide a practical selection guide, identify the five points of consensus, and — perhaps most importantly — name what's still missing from all of them.

No favorites. No rankings. Just an honest assessment of what each framework does well, where it falls short, and where the field is heading.

---

## 1. The Frameworks at a Glance

Before diving into the comparison, here's the field:

| Framework | Stars | Type | Primary Focus | Unique Mechanism |
|-----------|-------|------|---------------|------------------|
| **Superpowers** | ~249k | Skills/Methodology | Engineering discipline | Cialdini persuasion psychology |
| **ECC** | ~225k | Agent Harness | Comprehensive ops layer | Sub-agent context delegation |
| **Agent Skills** | ~70k | Skill Library | Anti-shortcutting | Anti-Rationalization Tables |
| **12-Factor Agents** | ~24k | Design Manifesto | Architecture principles | Stateless Reducer pattern |
| **CEP** | ~23k | Plugin/Workflow | Knowledge compounding | Compound step + Institutional Memory |
| **Archon** | ~23k | Workflow Engine | Deterministic execution | YAML workflow definitions |
| **GSD Core** | ~6k | Context Framework | Session management | Context rot prevention + `.planning/` |

The star counts tell one story: mass appeal correlates with accessibility. Superpowers and ECC are approachable — you can start using them in minutes. 12-Factor Agents and GSD Core require deeper architectural investment.

But stars don't tell you which framework solves *your* problem. For that, we need a richer comparison.

---

## 2. The Full Comparison: Eight Dimensions

### Dimension 1: Philosophy

What does the framework believe is the root problem?

| Framework | Core Belief |
|-----------|------------|
| **Superpowers** | The AI needs *motivation* and *discipline* — treat it like a talented but easily distracted engineer |
| **ECC** | The AI needs *infrastructure* — context management, session state, error classification are engineering problems |
| **Agent Skills** | The AI needs *guardrails* — without them, it will rationalize shortcuts every time |
| **12-Factor Agents** | The AI needs *architecture* — the same way web apps needed the 12-factor methodology |
| **CEP** | The AI needs *memory* — solving problems repeatedly without learning is the core waste |
| **Archon** | The AI needs *determinism* — creative freedom leads to unpredictable results |
| **GSD Core** | The AI needs *continuity* — without session management, every conversation starts from zero |

These beliefs aren't mutually exclusive. In fact, a complete AI engineering setup probably needs all of them. But each framework prioritizes its core belief, and this prioritization shapes every design decision downstream.

### Dimension 2: Architecture Model

How does the framework organize the AI's behavior?

| Framework | Architecture | Key Pattern |
|-----------|-------------|------------|
| **Superpowers** | Flat skill library with behavioral rules | Rules injected into system prompt; skills triggered by context |
| **ECC** | Hierarchical agent orchestration | Parent agent delegates to specialized sub-agents with minimal context |
| **Agent Skills** | Modular skill definitions with guard tables | Skills + anti-rationalization tables that explicitly list forbidden shortcuts |
| **12-Factor Agents** | Stateless reducer + tool loop | `f(context) → action` — no hidden state, every decision derived from context |
| **CEP** | Sequential workflow pipeline | Brainstorm → Plan → Work → Simplify → Review → Compound |
| **Archon** | YAML-defined workflow engine | Workflows are data (YAML), not code — deterministic step execution |
| **GSD Core** | Phase-based session management | `.planning/` directory + context health monitoring + session state |

### Dimension 3: Multi-Agent Strategy

How does the framework handle multiple AI agents?

| Framework | Multi-Agent Approach | Coordination Model |
|-----------|---------------------|-------------------|
| **Superpowers** | Implicit — single agent with multiple personas | Persona switching within one context |
| **ECC** | Explicit parent-child delegation | Parent sends minimal context; child returns results; parent integrates |
| **Agent Skills** | Minimal — single agent focus | Skills compose but don't orchestrate multiple agents |
| **12-Factor Agents** | Prescribed — stateless agents communicate through shared state | Each agent is a pure function; coordination through external state |
| **CEP** | Parallel reviewers in Simplify + Review steps | 3 simplification agents + multi-persona review agents run concurrently |
| **Archon** | Workflow-defined agent chains | YAML specifies which agents run, in what order, with what inputs |
| **GSD Core** | Lightweight — subagents for specific phases | Phase-specific subagents; context rot monitoring applies to all |

### Dimension 4: State Management

How does the framework handle persistence across sessions?

| Framework | State Strategy | Persistence Mechanism |
|-----------|---------------|----------------------|
| **Superpowers** | Minimal — relies on rules being re-injected each session | System prompt + rules files |
| **ECC** | Comprehensive — session state, project state, agent memory | `session_state.json` + memory files + diary entries |
| **Agent Skills** | Light — skill definitions persist, execution state doesn't | Skill files in `.claude/` or equivalent |
| **12-Factor Agents** | Zero in-agent state — everything external | All state in files/databases; agent derives everything from context |
| **CEP** | Solutions store + Repo Profile Cache | `docs/solutions/` with INDEX.md + git-keyed profile cache |
| **Archon** | Workflow state tracked in YAML | Workflow execution state persisted between runs |
| **GSD Core** | Session state + planning artifacts | `session_state.json` + `.planning/` directory + diary |

### Dimension 5: Quality Assurance

How does the framework prevent bad output?

| Framework | QA Mechanism | Verification Approach |
|-----------|-------------|----------------------|
| **Superpowers** | Persuasion psychology — make the AI *want* to write quality code | Cialdini principles (consistency, authority, social proof) embedded in prompts |
| **ECC** | Multi-tier error handling + confidence scoring | Recoverable → Fixable → Escalate; confidence scores on all assertions |
| **Agent Skills** | Anti-Rationalization Tables — pre-enumerate forbidden shortcuts | Explicit "if you're tempted to do X, the correct action is Y" tables |
| **12-Factor Agents** | Architectural constraints — bad patterns are structurally impossible | Stateless design prevents hidden state bugs; tool schemas prevent malformed calls |
| **CEP** | Behavior Change Contract + Deletion Test | No verification_evidence = untrustworthy; remove instructions that don't change output |
| **Archon** | Deterministic workflow execution | YAML-defined workflows can't deviate; each step has explicit success/failure criteria |
| **GSD Core** | Context health monitoring | Color-coded context usage (🟢🟡🟠🔴); forced checkpoint at >85% |

### Dimension 6: Learning & Adaptation

Does the framework get better over time?

| Framework | Learning Mechanism | Scope |
|-----------|-------------------|-------|
| **Superpowers** | None — static rules | No adaptation; same behavior session after session |
| **ECC** | Instinct system with confidence scoring | Instincts promoted from candidate → verifying → instinct based on repeated validation |
| **Agent Skills** | None — static skill definitions | Skills can be manually updated but don't self-improve |
| **12-Factor Agents** | None — deliberately stateless | Learning happens outside the agent, in external systems |
| **CEP** | Compound loop — institutional memory accumulation | Solutions store grows over time; pre-search becomes more effective |
| **Archon** | Workflow refinement — YAML can be updated based on results | Workflows can be versioned and improved, but manually |
| **GSD Core** | Session state carryover + diary system | Completed tasks and open todos persist; diary captures daily learnings |

### Dimension 7: Ease of Adoption

How hard is it to start using the framework?

| Framework | Time to First Value | Adoption Complexity |
|-----------|-------------------|-------------------|
| **Superpowers** | Minutes | Copy rules into system prompt. Immediate benefit. |
| **ECC** | Hours | Install configuration files. Learn session management. Moderate setup. |
| **Agent Skills** | Minutes | Copy skill definitions. Immediate benefit. |
| **12-Factor Agents** | Days–Weeks | Rearchitect agent infrastructure. Significant investment. |
| **CEP** | Hours | Install skills. Create initial `docs/solutions/` structure. Moderate setup. |
| **Archon** | Hours–Days | Define YAML workflows. Learn workflow syntax. Moderate-to-high investment. |
| **GSD Core** | Hours | Set up `.planning/` directory. Configure session state. Moderate setup. |

### Dimension 8: Composability

Can the framework be combined with others?

| Framework | Composability | Integration Pattern |
|-----------|--------------|-------------------|
| **Superpowers** | High — rules can be mixed with any system | Rules layer on top of any existing setup |
| **ECC** | Medium — comprehensive but modular | Individual components (error tiers, session state) can be extracted |
| **Agent Skills** | High — skills are independent units | Individual skills can be adopted without the full library |
| **12-Factor Agents** | High — principles apply to any architecture | Principles guide design without requiring specific implementation |
| **CEP** | High — skills are modular and self-contained | Individual skills (especially ce-compound) can be used independently |
| **Archon** | Low — workflow engine is an all-or-nothing commitment | Switching to YAML workflows requires significant refactoring |
| **GSD Core** | Medium — session management integrates but requires conventions | `.planning/` pattern can be adopted; context health monitoring is portable |

---

## 3. The Philosophy Spectrum: From Manifesto to Implementation

The seven frameworks occupy different positions on a spectrum from *abstract principles* to *concrete implementation*:

```
Abstract ◄────────────────────────────────────────────► Concrete

12-Factor    Superpowers    Agent Skills    GSD Core    ECC    CEP    Archon
 Agents                                                              
(Principles) (Behaviors)   (Guardrails)   (Session)  (Ops) (Workflow)(Engine)
```

### Left Side: Principles and Behaviors

**12-Factor Agents** is the most abstract. It provides architectural principles — "Agent as Stateless Reducer," "Own Your Context Window" — but doesn't ship runnable code. It's a design manifesto, not a product. This is both its strength (universally applicable) and its weakness (requires significant interpretation).

**Superpowers** is slightly more concrete. It provides specific behavioral rules that can be directly injected into system prompts. But the rules are still at the behavior level — they tell the AI *how to act*, not *what to do*.

### Middle: Guardrails and Sessions

**Agent Skills** occupies the middle ground. Skill definitions are concrete and executable, but the Anti-Rationalization Tables are at the guardrail level — they constrain behavior rather than prescribing it.

**GSD Core** adds session management infrastructure — the `.planning/` directory, context health monitoring, session state persistence. This is concrete infrastructure, but it's still a *framework* that developers use to organize their own workflows.

### Right Side: Operations and Workflows

**ECC** provides a comprehensive operational layer — error classification, confidence scoring, sub-agent delegation patterns. This is ready-to-use infrastructure with minimal customization needed.

**CEP** provides a complete workflow pipeline. The six-step loop (brainstorm → plan → work → simplify → review → compound) is a concrete, opinionated process that the team follows.

**Archon** is the most concrete: YAML-defined workflows where every step, every branch, every decision point is specified in data. There's almost no room for the AI to deviate — which is either a feature or a bug, depending on your perspective.

### The Tradeoff

The spectrum reveals a fundamental tradeoff: **flexibility vs. predictability**.

Frameworks on the left give the AI maximum creative latitude — which is great when the AI is competent and terrible when it's not. Frameworks on the right constrain the AI to predetermined paths — which is great for reliability and terrible for novel problems.

Most mature teams end up using a combination: abstract principles for overall guidance, concrete workflows for routine tasks, and guardrails for known failure modes.

---

## 4. Selection Guide: If You Want X, Use Y

This is the practical section. Based on the analysis above, here's a decision tree:

### "I want to start with something simple and see immediate results."
→ **Superpowers** or **Agent Skills**. Both can be adopted in minutes by copying rules/skills into your agent configuration. Superpowers is better for general behavioral improvement; Agent Skills is better for preventing specific known failure modes.

### "I want a complete operational framework for a production AI agent."
→ **ECC**. It's the most comprehensive operational layer, covering session management, error handling, sub-agent delegation, and confidence scoring. The setup investment is moderate, and the payoff is substantial.

### "I want my team to stop solving the same problems over and over."
→ **CEP**. The compound loop is specifically designed for institutional memory. If knowledge loss between sessions is your primary pain point, CEP directly addresses it.

### "I'm building a new AI agent system from scratch and want to get the architecture right."
→ **12-Factor Agents**. The architectural principles will guide your design decisions and prevent common structural mistakes. Complement with a more concrete framework (CEP, ECC) for day-to-day operations.

### "I need deterministic, repeatable workflows — the same input must produce the same output."
→ **Archon**. YAML-defined workflows provide the strongest guarantees about execution order and decision points. This is the right choice for regulated environments, compliance-sensitive domains, and production pipelines where predictability trumps flexibility.

### "I'm working on a long-running project with many sessions, and I keep losing context."
→ **GSD Core**. Context rot prevention and session state management are its core strengths. The `.planning/` directory and diary system maintain continuity across sessions.

### "I want all of the above."
→ **Start with a composition.** CEP's compound loop + ECC's operational layer + Superpowers' behavioral rules + 12-Factor's architectural principles. This is essentially what C31 did, and it works — but the integration effort is significant.

---

## 5. The 2026 Consensus: Five Points of Agreement

Despite their different philosophies, all seven frameworks have converged — independently — on five shared principles. This convergence is significant because it wasn't coordinated. These principles emerged from the practical experience of building AI agent systems at scale.

### Consensus 1: Architecture Over Prompts

Every framework in this comparison has moved beyond "just write a better prompt." All seven recognize that reliable AI behavior requires *structural* interventions — configuration files, workflow definitions, state management systems, review pipelines — not just clever instructions.

This is the 2026 equivalent of the software industry's shift from "just write better code" to "use design patterns, testing frameworks, and CI/CD pipelines." The individual instruction is still important, but the system around it is what determines reliability.

**Evidence across frameworks:**
- Superpowers: behavioral rules, not ad-hoc prompts
- ECC: session state infrastructure, error classification tiers
- Agent Skills: structured skill definitions with guard tables
- 12-Factor: explicit architectural principles
- CEP: six-step workflow pipeline
- Archon: YAML workflow definitions
- GSD Core: `.planning/` directory structure, context health monitoring

### Consensus 2: Multi-Agent Orchestration

Six of the seven frameworks (all except 12-Factor Agents, which prescribes it without implementing it) provide some form of multi-agent orchestration. The consensus is clear: a single monolithic AI agent is insufficient for complex engineering work.

The patterns differ — ECC uses hierarchical delegation, CEP uses parallel reviewers, Archon uses workflow-defined chains — but the principle is the same: decompose complex tasks into specialized roles, let each role operate with focused context, and merge the results.

This mirrors the evolution of software architecture from monoliths to microservices. The AI agent ecosystem is going through the same transition, for the same reasons: specialization enables higher quality, and bounded contexts enable better reasoning.

### Consensus 3: Human-in-the-Loop as Standard

None of these frameworks advocate for fully autonomous AI operation. All seven include explicit human checkpoints — decision boundaries, verification gates, approval requirements, escalation protocols.

The consensus position is nuanced: AI agents should be **autonomous within a bounded scope** and **pause for human input at scope boundaries**. The scope varies by framework (Archon's scope is narrow and rigid; Superpowers' scope is broad and fluid), but the principle is universal.

This is a meaningful departure from the "AGI will do everything" narrative. The frameworks that have survived real-world usage all agree: the human is not optional.

### Consensus 4: Persistent State Across Sessions

Every framework except Superpowers (which is purely behavioral) provides some mechanism for persisting state across sessions. The consensus is that a "memoryless" AI agent — one that starts fresh every conversation — is fundamentally limited.

The persistence mechanisms vary widely:
- ECC/GSD Core: `session_state.json` + diary files
- CEP: `docs/solutions/` with INDEX.md
- Archon: workflow state in YAML
- 12-Factor: external state stores (prescribed but not implemented)

But the underlying principle is the same: knowledge, context, and progress must survive the session boundary.

### Consensus 5: Knowledge Compounding

This is the newest consensus point and arguably the most important. While CEP made it explicit (it's in the name), every framework has some version of it:

- **ECC**: instinct system where patterns are promoted based on repeated validation
- **Agent Skills**: skill library that grows over time
- **GSD Core**: diary system that accumulates daily learnings
- **CEP**: compound loop with structured documentation and pre-search
- **Archon**: workflow refinement based on execution results

The consensus is that **AI-assisted engineering should be a learning system**, not a static one. Each unit of work should contribute to a growing base of knowledge that makes future work easier.

This is the compounding principle, and the fact that all seven frameworks implement some version of it — despite their very different architectures — suggests it's not just a nice idea but a structural necessity.

---

## 6. What's Missing from ALL of Them

Consensus tells us what the field has figured out. The gaps tell us where it's going.

### Gap 1: Cross-Team Knowledge Sharing

All seven frameworks operate within a single team or single developer's context. None of them provide mechanisms for sharing knowledge *across* teams or across organizational boundaries.

CEP's compound loop builds institutional memory for one project. But what if five teams in the same company are all solving similar problems? There's no cross-project knowledge network. The solutions store is local, not distributed.

This is a hard problem — it involves permissions, relevance filtering, and trust — but it's also an enormous opportunity. The company that cracks cross-team knowledge compounding will have a genuine competitive advantage.

### Gap 2: Cost-Awareness

None of the seven frameworks incorporate cost as a first-class constraint. How many tokens did this operation consume? How much did this sub-agent delegation cost? Is the Simplify step worth its token cost for a one-line change?

AI compute is expensive, and costs scale with usage. A framework that optimizes for *value per token* — not just output quality — would be genuinely useful. Today, none of them do this.

### Gap 3: Quantitative Self-Assessment

The frameworks provide qualitative self-assessment (confidence scoring, Critic Gates) but no quantitative measurement. How much time did the compound loop save over the last month? How often did the pre-search find relevant prior art? What's the false-positive rate of the Anti-Rationalization Tables?

Without metrics, it's impossible to know whether the framework overhead is worth its cost. This is ironic — these are engineering frameworks, and engineering demands measurement.

### Gap 4: Model-Agnostic Design

Most frameworks are implicitly designed for a specific class of models (large-context LLMs with strong instruction-following). None of them address how their principles should adapt to different model capabilities.

What happens when context windows are 10x larger? When models have native tool use? When multi-modal models can directly observe code execution? The current frameworks will need significant adaptation, and none of them have a clear migration path.

### Gap 5: Security and Trust Boundaries

The frameworks address code quality, process discipline, and knowledge management — but none of them seriously address security. What happens when a sub-agent is given access to sensitive code? How do you prevent prompt injection through the solutions store? What are the trust boundaries between agents in a multi-agent system?

CEP's Behavior Change Contract ensures that changes are verified — but verified by whom? By another AI agent, whose judgment could also be compromised. The security model of AI agent systems is still largely undefined.

### Gap 6: Graceful Degradation

What happens when the AI model degrades — slower responses, lower quality output, rate limiting? None of the seven frameworks have a graceful degradation strategy. They assume the underlying model is always available and always capable.

In production environments, this assumption doesn't hold. A framework that could detect model degradation and adjust its workflow accordingly (e.g., simplifying the Review step when response quality is low, or batching Compound operations during rate limiting) would be significantly more robust.

---

## 7. Future Predictions

Based on the patterns observed across all seven frameworks, here's where the field is heading:

### Prediction 1: Convergence into Composable Standards

The seven frameworks will not remain seven separate projects. They'll converge — either through explicit integration (as C31 integrated CEP) or through the emergence of interoperability standards. Within 18 months, we'll see a de facto standard for skill definition format, state persistence schema, and inter-agent communication protocol.

### Prediction 2: Cost-Aware Execution

Framework-level cost tracking and optimization will become table stakes. Agents will be able to report "this task cost $X in compute" and frameworks will provide cost budgets with automatic degradation strategies when budgets are exceeded.

### Prediction 3: Continuous Learning Becomes Default

CEP's compound loop will be recognized as the minimum viable learning mechanism, and more sophisticated approaches — automated solution quality scoring, semantic search over institutional memory, cross-project knowledge graphs — will emerge.

### Prediction 4: Security-First Frameworks

The first serious AI agent security incident will trigger a wave of security-focused framework development. Trust boundaries, agent sandboxing, and adversarial robustness testing will become mandatory features, not afterthoughts.

### Prediction 5: The "Standard Stack" Emerges

Just as the web development world converged on a standard stack (React + Node + PostgreSQL, or equivalent), the AI agent world will converge on a standard stack:
- **Behavioral layer**: Superpowers-style rules
- **Operational layer**: ECC-style infrastructure
- **Workflow layer**: CEP-style pipeline with Archon-style determinism for critical paths
- **Memory layer**: CEP-style compound loop with cross-project sharing
- **Architecture layer**: 12-Factor principles governing the overall design

This stack doesn't exist as a unified product today, but all the pieces are present in the seven frameworks analyzed here.

---

## 8. Conclusion: The Field Matures

The existence of seven serious frameworks for AI agent engineering — all independently developed, all battle-tested, all converging on shared principles — is itself the most important finding.

It means the field has moved past the "prompt engineering is all you need" phase. Past the "AI agents are magic" phase. Into the "AI agents are engineering systems that require engineering discipline" phase.

This is maturity. Not glamorous, not revolutionary, not the singularity. Just the quiet, persistent work of making a powerful tool actually reliable.

The seven frameworks disagree on many things — how much autonomy to give the AI, how to structure multi-agent systems, how to handle state, how deterministic workflows should be. But they agree on the fundamentals: architecture matters, memory matters, humans matter, and the quality of today's work should make tomorrow's work easier.

That consensus, hard-won through real-world failure and iteration, is the most valuable output of the entire AI agent engineering movement so far.

---

## Appendix: Quick Reference Matrix

For teams evaluating which framework(s) to adopt:

| If Your Priority Is... | Primary Choice | Secondary Choice | Avoid |
|------------------------|---------------|-----------------|-------|
| Quick wins, low investment | Superpowers | Agent Skills | 12-Factor (too abstract for quick wins) |
| Production reliability | ECC | Archon | Superpowers (too behavioral for production) |
| Team knowledge retention | CEP | GSD Core | 12-Factor (no implementation) |
| Architectural correctness | 12-Factor | ECC | Archon (too rigid for architectural thinking) |
| Session continuity | GSD Core | ECC | Agent Skills (no session management) |
| Deterministic pipelines | Archon | CEP | Superpowers (too flexible for pipelines) |
| Preventing AI shortcuts | Agent Skills | Superpowers | Archon (constraints are structural, not behavioral) |
| Everything (kitchen sink) | C31 integration | Build your own composition | Any single framework alone |

---

## Twitter Thread: 7 Frameworks Compared in 12 Tweets

**🧵 1/12**
I spent months studying 7 AI agent engineering frameworks: Superpowers (249k⭐), ECC (225k⭐), Agent Skills (70k⭐), 12-Factor (24k⭐), CEP (23k⭐), Archon (23k⭐), GSD Core (6k⭐).

Here's what I found. Thread 👇

**🧵 2/12**
Each framework believes the AI's root problem is different:
• Superpowers: needs discipline
• ECC: needs infrastructure
• Agent Skills: needs guardrails
• 12-Factor: needs architecture
• CEP: needs memory
• Archon: needs determinism
• GSD Core: needs continuity

**🧵 3/12**
They sit on a spectrum from abstract to concrete:

Abstract ◄──────────────────────► Concrete
12-Factor → Superpowers → Agent Skills → GSD → ECC → CEP → Archon

Left = flexible but unpredictable. Right = predictable but rigid. Pick your tradeoff.

**🧵 4/12**
SELECTION GUIDE:

Want quick wins? → Superpowers or Agent Skills
Production reliability? → ECC
Stop solving same bugs? → CEP
Architecting from scratch? → 12-Factor
Losing context between sessions? → GSD Core
Need deterministic pipelines? → Archon

**🧵 5/12**
The surprise: despite zero coordination, all 7 converged on 5 shared principles. This is the 2026 consensus on AI engineering. Real convergence from real-world usage.

**🧵 6/12**
Consensus #1: Architecture over prompts.

All 7 moved beyond "write a better prompt." Reliable AI needs structural interventions — config files, workflow defs, state management. The prompt is still important, but the system around it determines reliability.

**🧵 7/12**
Consensus #2: Multi-agent orchestration.

6 of 7 implement some form of multi-agent work. Single monolithic agents can't handle complex tasks. Decompose → specialize → merge. Same evolution as monoliths → microservices.

**🧵 8/12**
Consensus #3: Human-in-the-loop is NOT optional.

Zero frameworks advocate full autonomy. All include human checkpoints. The consensus: autonomous within bounded scope, pause at scope boundaries.

The "AGI does everything" crowd lost. The engineering crowd won.

**🧵 9/12**
Consensus #4: Persistent state across sessions.

"Memoryless" AI agents are fundamentally limited. Every mature framework persists knowledge, context, and progress across session boundaries. The mechanisms differ, the principle is universal.

**🧵 10/12**
Consensus #5: Knowledge compounding.

CEP made it explicit, but ALL 7 implement some version. AI-assisted engineering should be a learning system. Each unit of work should make the next one easier.

This is compound interest applied to software engineering.

**🧵 11/12**
What's MISSING from ALL 7:
• Cross-team knowledge sharing
• Cost-awareness ($/token tracking)
• Quantitative self-assessment
• Model-agnostic design
• Security & trust boundaries
• Graceful degradation

These are the next frontier.

**🧵 12/12**
The big picture: 7 frameworks independently converging = the field is maturing.

We're past "prompts are all you need." Past "AI is magic." Into "AI agents are engineering systems that need engineering discipline."

Not glamorous. Not revolutionary. Just reliable. And that's the point. 🔧



---

# 中文版

# Agent Harness 论文系列，第八篇：七大框架横评 — 2026年中 AI 工程的行业全景

**系列：** Agent Harness Papers
**作者：** 基于公开代码仓库与文档整理的研究笔记
**主题：** 七个 AI Agent 工程框架的横向对比分析

![7 AI Agent Frameworks Compared](images/final_part8_comparison.jpg)

---

> *"7 个框架，7 种哲学，1 个目标：让 AI 不再是用完即弃的工具。"*

---

## 引言

在这个系列的前几篇中，我们逐一考察了七个框架，它们各自尝试回答同一个根本问题：**如何让 AI Agent 在真实的工程工作中可靠地发挥作用？**

每个框架的出发点不同，服务的受众不同，对"什么最重要"的押注也不同。但正如我们即将看到的，它们独立地、在没有任何协调的情况下，趋同于一组共享原则——这组原则构成了 2026 年关于"AI 辅助工程应该是什么样子"的行业共识。

本文将绘制完整的全景地图。我们将从八个维度对全部七个框架进行横向对比，追溯从宣言到实现的哲学光谱，提供实用的选型指南，识别五个共识点，以及——也许最重要的——指出它们所有人都还缺失的东西。

没有偏爱。没有排名。只有对每个框架的优势、短板和行业走向的诚实评估。

---

## 1. 框架一览

在深入对比之前，先看全貌：

| 框架 | Stars | 类型 | 核心聚焦 | 独特机制 |
|-----------|-------|------|---------------|---------------------|
| **Superpowers** | ~249k | 技能/方法论 | 工程纪律 | Cialdini 说服心理学 |
| **ECC** | ~225k | Agent Harness | 综合运维层 | 子代理上下文委派 |
| **Agent Skills** | ~70k | 技能库 | 反走捷径 | Anti-Rationalization Tables |
| **12-Factor Agents** | ~24k | 设计宣言 | 架构原则 | Stateless Reducer 模式 |
| **CEP** | ~23k | 插件/工作流 | 知识复利 | Compound 步骤 + 机构记忆 |
| **Archon** | ~23k | 工作流引擎 | 确定性执行 | YAML 工作流定义 |
| **GSD Core** | ~6k | 上下文框架 | 会话管理 | Context Rot 预防 + `.planning/` |

Star 数讲述了一个故事：大众吸引力与易用性正相关。Superpowers 和 ECC 门槛低——几分钟就能上手。12-Factor Agents 和 GSD Core 则需要更深层的架构投入。

但 star 数并不能告诉你哪个框架能解决*你的*问题。为此，我们需要更丰富的对比。

---

## 2. 全面对比：八个维度

### 维度 1：哲学

框架认为根本问题是什么？

| 框架 | 核心信念 |
|-----------|------------|
| **Superpowers** | AI 需要*动力*和*纪律*——把它当作一个有才华但容易分心的工程师对待 |
| **ECC** | AI 需要*基础设施*——上下文管理、会话状态、错误分类都是工程问题 |
| **Agent Skills** | AI 需要*护栏*——没有护栏，它每次都会合理化各种走捷径的行为 |
| **12-Factor Agents** | AI 需要*架构*——就像 Web 应用当年需要 12-factor 方法论一样 |
| **CEP** | AI 需要*记忆*——反复解决同样的问题却不学习，才是核心浪费 |
| **Archon** | AI 需要*确定性*——创造性自由会导致不可预测的结果 |
| **GSD Core** | AI 需要*连续性*——没有会话管理，每次对话都从零开始 |

这些信念并不互斥。事实上，一个完整的 AI 工程设置可能需要它们全部。但每个框架都优先其核心信念，而这种优先级决定了下游所有的设计决策。

### 维度 2：架构模型

框架如何组织 AI 的行为？

| 框架 | 架构 | 关键模式 |
|-----------|-------------|------------|
| **Superpowers** | 扁平技能库 + 行为规则 | 规则注入系统提示词；技能按上下文触发 |
| **ECC** | 分层 Agent 编排 | 父代理以最小上下文委派给专业子代理 |
| **Agent Skills** | 模块化技能定义 + 护栏表 | 技能 + Anti-Rationalization Tables，显式列出禁止的捷径 |
| **12-Factor Agents** | Stateless Reducer + 工具循环 | `f(context) → action` — 无隐藏状态，所有决策均从上下文推导 |
| **CEP** | 顺序工作流管线 | Brainstorm → Plan → Work → Simplify → Review → Compound |
| **Archon** | YAML 定义的工作流引擎 | 工作流是数据（YAML），不是代码——确定性步骤执行 |
| **GSD Core** | 基于阶段的会话管理 | `.planning/` 目录 + 上下文健康监控 + 会话状态 |

### 维度 3：多 Agent 策略

框架如何处理多个 AI Agent？

| 框架 | 多 Agent 方式 | 协调模型 |
|-----------|---------------------|-------------------|
| **Superpowers** | 隐式——单 Agent 多角色 | 同一上下文内的角色切换 |
| **ECC** | 显式的父子委派 | 父代理发送最小上下文；子代理返回结果；父代理整合 |
| **Agent Skills** | 极简——聚焦单 Agent | 技能可组合但不编排多 Agent |
| **12-Factor Agents** | 规定式——无状态 Agent 通过共享状态通信 | 每个 Agent 是纯函数；通过外部状态协调 |
| **CEP** | Simplify + Review 步骤中的并行审查者 | 3 个简化代理 + 多角色审查代理并发运行 |
| **Archon** | 工作流定义的 Agent 链 | YAML 指定哪些 Agent 运行、以什么顺序、使用什么输入 |
| **GSD Core** | 轻量级——特定阶段的子代理 | 阶段特定的子代理；Context Rot 监控适用于所有 |

### 维度 4：状态管理

框架如何处理跨会话的持久化？

| 框架 | 状态策略 | 持久化机制 |
|-----------|---------------|----------------------|
| **Superpowers** | 极简——依赖每次会话重新注入规则 | 系统提示词 + 规则文件 |
| **ECC** | 全面——会话状态、项目状态、Agent 记忆 | `session_state.json` + 记忆文件 + 日记条目 |
| **Agent Skills** | 轻量——技能定义持久化，执行状态不持久化 | `.claude/` 或等效目录中的技能文件 |
| **12-Factor Agents** | Agent 内零状态——一切外部化 | 所有状态存储在文件/数据库中；Agent 从上下文推导一切 |
| **CEP** | Solutions Store + Repo Profile 缓存 | `docs/solutions/` 配合 INDEX.md + 基于 git 的 Profile 缓存 |
| **Archon** | 工作流状态以 YAML 追踪 | 工作流执行状态在运行之间持久化 |
| **GSD Core** | 会话状态 + 规划产物 | `session_state.json` + `.planning/` 目录 + 日记 |

### 维度 5：质量保证

框架如何防止糟糕的输出？

| 框架 | QA 机制 | 验证方式 |
|-----------|-------------|----------------------|
| **Superpowers** | 说服心理学——让 AI *想要*写高质量代码 | Cialdini 原则（一致性、权威、社会认同）嵌入提示词 |
| **ECC** | 多层错误处理 + 置信度评分 | Recoverable → Fixable → Escalate；所有断言附带置信度评分 |
| **Agent Skills** | Anti-Rationalization Tables——预先列举禁止的捷径 | 显式的"如果你想做 X，正确的做法是 Y"表格 |
| **12-Factor Agents** | 架构约束——糟糕的模式在结构上不可能发生 | 无状态设计防止隐藏状态 bug；工具 schema 防止格式错误的调用 |
| **CEP** | Behavior Change Contract + Deletion Test | 没有 verification_evidence = 不可信；删除不改变输出的指令 |
| **Archon** | 确定性工作流执行 | YAML 定义的工作流不会偏离；每个步骤有明确的成功/失败标准 |
| **GSD Core** | 上下文健康监控 | 颜色编码的上下文使用率（🟢🟡🟠🔴）；>85% 时强制 checkpoint |

### 维度 6：学习与适应

框架是否会随时间变得更好？

| 框架 | 学习机制 | 范围 |
|-----------|-------------------|-------|
| **Superpowers** | 无——静态规则 | 无适应性；每次会话行为相同 |
| **ECC** | Instinct 系统 + 置信度评分 | Instinct 基于反复验证从 candidate → verifying → instinct 升级 |
| **Agent Skills** | 无——静态技能定义 | 技能可手动更新但不会自我改进 |
| **12-Factor Agents** | 无——刻意无状态 | 学习发生在 Agent 外部的外部系统中 |
| **CEP** | Compound 循环——机构记忆积累 | Solutions Store 随时间增长；预搜索变得更有效 |
| **Archon** | 工作流优化——YAML 可根据结果更新 | 工作流可以版本化和改进，但需手动操作 |
| **GSD Core** | 会话状态延续 + 日记系统 | 已完成任务和待办事项持久化；日记捕获每日学习 |

### 维度 7：上手难度

开始使用框架有多难？

| 框架 | 首次获得价值的时间 | 采用复杂度 |
|-----------|-------------------|-------------------|
| **Superpowers** | 几分钟 | 复制规则到系统提示词。即时见效。 |
| **ECC** | 几小时 | 安装配置文件。学习会话管理。中等设置。 |
| **Agent Skills** | 几分钟 | 复制技能定义。即时见效。 |
| **12-Factor Agents** | 几天到几周 | 重新架构 Agent 基础设施。重大投入。 |
| **CEP** | 几小时 | 安装技能。创建初始 `docs/solutions/` 结构。中等设置。 |
| **Archon** | 几小时到几天 | 定义 YAML 工作流。学习工作流语法。中高投入。 |
| **GSD Core** | 几小时 | 设置 `.planning/` 目录。配置会话状态。中等设置。 |

### 维度 8：可组合性

框架能否与其他框架组合使用？

| 框架 | 可组合性 | 集成模式 |
|-----------|--------------|-------------------|
| **Superpowers** | 高——规则可与任何系统混用 | 规则层叠加在任何现有设置之上 |
| **ECC** | 中——全面但模块化 | 可提取单个组件（错误分层、会话状态） |
| **Agent Skills** | 高——技能是独立单元 | 可单独采用个别技能而无需完整库 |
| **12-Factor Agents** | 高——原则适用于任何架构 | 原则指导设计但不要求特定实现 |
| **CEP** | 高——技能模块化且自包含 | 可独立使用个别技能（尤其是 ce-compound） |
| **Archon** | 低——工作流引擎是全有或全无的承诺 | 切换到 YAML 工作流需要大规模重构 |
| **GSD Core** | 中——会话管理可集成但需要约定 | `.planning/` 模式可被采用；上下文健康监控可移植 |

---

## 3. 哲学光谱：从宣言到实现

七个框架在从*抽象原则*到*具体实现*的光谱上占据着不同的位置：

```
抽象 ◄────────────────────────────────────────────► 具体

12-Factor    Superpowers    Agent Skills    GSD Core    ECC    CEP    Archon
 Agents                                                              
(原则)      (行为)         (护栏)         (会话)     (运维) (工作流)(引擎)
```

### 左侧：原则与行为

**12-Factor Agents** 是最抽象的。它提供架构原则——"Agent as Stateless Reducer"、"Own Your Context Window"——但不提供可运行的代码。它是设计宣言，不是产品。这既是它的优势（普遍适用），也是它的劣势（需要大量解读）。

**Superpowers** 稍微具体一些。它提供可以直接注入系统提示词的具体行为规则。但这些规则仍然停留在行为层面——它们告诉 AI *如何行动*，而不是*做什么*。

### 中段：护栏与会话

**Agent Skills** 占据中间地带。技能定义是具体且可执行的，但 Anti-Rationalization Tables 处于护栏层面——它们约束行为而非规定行为。

**GSD Core** 增加了会话管理基础设施——`.planning/` 目录、上下文健康监控、会话状态持久化。这是具体的基础设施，但它仍然是一个*框架*，开发者用它来组织自己的工作流。

### 右侧：运维与工作流

**ECC** 提供了全面的运维层——错误分类、置信度评分、子代理委派模式。这是开箱即用的基础设施，只需最少的定制。

**CEP** 提供了完整的工作流管线。六步循环（Brainstorm → Plan → Work → Simplify → Review → Compound）是一个具体的、有明确观点的流程，团队按此执行。

**Archon** 是最具体的：YAML 定义的工作流，每一步、每一个分支、每一个决策点都在数据中指定。AI 几乎没有偏离的余地——这到底是特性还是缺陷，取决于你的立场。

### 权衡

这个光谱揭示了一个根本性的权衡：**灵活性 vs. 可预测性**。

光谱左侧的框架给予 AI 最大的创造自由度——当 AI 能力强时很棒，不行时就很糟。右侧的框架将 AI 约束在预定义的路径上——对可靠性很好，但对新问题很糟。

大多数成熟团队最终会使用组合方案：用抽象原则做整体指导，用具体工作流处理常规任务，用护栏应对已知的失败模式。

---

## 4. 选型指南：如果你想要 X，就用 Y

这是实操部分。基于以上分析，这里是一个决策树：

### "我想从简单的东西开始，立刻看到效果。"
→ **Superpowers** 或 **Agent Skills**。两者都可以在几分钟内通过复制规则/技能到你的 Agent 配置中来采用。Superpowers 更适合全面的行为改善；Agent Skills 更适合防止特定的已知失败模式。

### "我想要一个完整的运维框架来支撑生产环境的 AI Agent。"
→ **ECC**。它是最全面的运维层，涵盖会话管理、错误处理、子代理委派和置信度评分。设置投入适中，回报可观。

### "我想让团队不再反复解决同样的问题。"
→ **CEP**。Compound 循环专门为机构记忆设计。如果会话间的知识流失是你的主要痛点，CEP 直接解决它。

### "我正在从零构建一个新的 AI Agent 系统，想把架构做对。"
→ **12-Factor Agents**。架构原则将指导你的设计决策，防止常见的结构性错误。配合一个更具体的框架（CEP、ECC）来处理日常操作。

### "我需要确定性的、可重复的工作流——同样的输入必须产生同样的输出。"
→ **Archon**。YAML 定义的工作流对执行顺序和决策点提供最强的保证。这是受监管环境、合规敏感领域和生产管线的正确选择——在这些场景中，可预测性优先于灵活性。

### "我在做一个长期项目，有很多会话，而且我总是丢失上下文。"
→ **GSD Core**。Context Rot 预防和会话状态管理是它的核心优势。`.planning/` 目录和日记系统在会话之间维持连续性。

### "我全都要。"
→ **从组合方案开始。** CEP 的 Compound 循环 + ECC 的运维层 + Superpowers 的行为规则 + 12-Factor 的架构原则。这基本上就是 C31 所做的事情，而且它是有效的——但集成工作量很大。

---

## 5. 2026 共识：五个共同点

尽管哲学各异，全部七个框架独立地趋同于五个共享原则。这种趋同意义重大，因为它不是协调的结果。这些原则来自于大规模构建 AI Agent 系统的实践经验。

### 共识 1：架构优于提示词

本次对比中的每一个框架都已经超越了"写个更好的提示词就行了"的阶段。全部七个都认识到，可靠的 AI 行为需要*结构性*的干预——配置文件、工作流定义、状态管理系统、审查管线——而不仅仅是巧妙的指令。

这是 2026 年版的软件行业从"写更好的代码"到"使用设计模式、测试框架和 CI/CD 管线"的转变。单条指令仍然重要，但围绕它的系统才决定了可靠性。

**跨框架证据：**
- Superpowers：行为规则，而非临时提示词
- ECC：会话状态基础设施、错误分类层级
- Agent Skills：结构化的技能定义 + 护栏表
- 12-Factor：显式的架构原则
- CEP：六步工作流管线
- Archon：YAML 工作流定义
- GSD Core：`.planning/` 目录结构、上下文健康监控

### 共识 2：多 Agent 编排

七个框架中的六个（除了 12-Factor Agents——它规定了多 Agent 但没有实现）提供了某种形式的多 Agent 编排。共识很明确：单一的单体 AI Agent 不足以应对复杂的工程工作。

模式各不相同——ECC 使用分层委派，CEP 使用并行审查者，Archon 使用工作流定义的链——但原则是一样的：将复杂任务分解为专业角色，让每个角色在聚焦的上下文中运行，然后合并结果。

这镜像了软件架构从单体到微服务的演进。AI Agent 生态正在经历同样的转变，出于同样的原因：专业化实现更高质量，边界明确的上下文实现更好的推理。

### 共识 3：Human-in-the-Loop 是标配

这些框架中没有一个主张完全自主的 AI 运行。全部七个都包含显式的人类检查点——决策边界、验证门控、审批要求、上报协议。

共识立场是细腻的：AI Agent 应该**在有限范围内自主运行**，并**在范围边界处暂停等待人类输入**。范围因框架而异（Archon 的范围窄而刚性；Superpowers 的范围宽而弹性），但原则是普遍的。

这是对"AGI 将做一切"叙事的有意义的背离。经受住真实世界使用考验的框架一致认为：人类不是可选的。

### 共识 4：跨会话持久状态

除 Superpowers（纯行为性的）之外的每一个框架都提供了某种跨会话持久化状态的机制。共识是："无记忆"的 AI Agent——每次对话都从零开始的——从根本上是受限的。

持久化机制差异很大：
- ECC/GSD Core：`session_state.json` + 日记文件
- CEP：`docs/solutions/` 配合 INDEX.md
- Archon：YAML 中的工作流状态
- 12-Factor：外部状态存储（规定但未实现）

但底层原则是一样的：知识、上下文和进度必须跨越会话边界存活下来。

### 共识 5：Knowledge Compounding

这是最新的共识点，也可以说是最重要的。虽然 CEP 将它显式化了（它就在名字里），但每个框架都有某种版本：

- **ECC**：Instinct 系统，模式基于反复验证被提升
- **Agent Skills**：随时间增长的技能库
- **GSD Core**：积累每日学习的日记系统
- **CEP**：Compound 循环 + 结构化文档 + 预搜索
- **Archon**：基于执行结果的工作流优化

共识是：**AI 辅助工程应该是一个学习系统**，而不是一个静态系统。每一个工作单元都应该为不断增长的知识库做出贡献，使未来的工作更轻松。

这就是复利原则，而所有七个框架都实现了某种版本——尽管它们的架构截然不同——这表明它不只是一个好想法，而是一种结构性的必需。

---

## 6. 它们所有人都缺失了什么

共识告诉我们这个领域已经搞清楚了什么。缺口告诉我们它要往哪里走。

### 缺口 1：跨团队知识共享

全部七个框架都在单个团队或单个开发者的上下文中运行。没有一个提供*跨*团队或跨组织边界共享知识的机制。

CEP 的 Compound 循环为一个项目建立机构记忆。但如果同一家公司的五个团队都在解决类似的问题呢？没有跨项目的知识网络。Solutions Store 是本地的，不是分布式的。

这是一个困难的问题——涉及权限、相关性过滤和信任——但它也是一个巨大的机会。第一个攻克跨团队 Knowledge Compounding 的公司将拥有真正的竞争优势。

### 缺口 2：成本感知

七个框架中没有一个将成本作为一等约束。这个操作消耗了多少 token？这次子代理委派花了多少钱？对于一行代码的改动，Simplify 步骤值得它的 token 成本吗？

AI 计算是昂贵的，成本随使用量扩展。一个优化*每 token 价值*——而不仅仅是输出质量——的框架将是真正有用的。目前，没有一个框架做到了这一点。

### 缺口 3：定量自我评估

这些框架提供定性的自我评估（置信度评分、Critic Gate），但没有定量度量。Compound 循环在过去一个月节省了多少时间？预搜索多久能找到相关的历史记录？Anti-Rationalization Tables 的误报率是多少？

没有指标，就不可能知道框架的开销是否值得。这很讽刺——这些是工程框架，而工程要求度量。

### 缺口 4：模型无关设计

大多数框架隐式地为特定类型的模型（大上下文 LLM + 强指令跟随能力）设计。没有一个框架讨论其原则应该如何适应不同的模型能力。

当上下文窗口扩大 10 倍会怎样？当模型原生支持工具使用时呢？当多模态模型可以直接观察代码执行时呢？当前的框架将需要重大调整，但没有一个有清晰的迁移路径。

### 缺口 5：安全与信任边界

这些框架处理了代码质量、流程纪律和知识管理——但没有一个认真地处理安全问题。当子代理被赋予访问敏感代码的权限时会发生什么？如何防止通过 Solutions Store 进行的 Prompt 注入？多 Agent 系统中 Agent 之间的信任边界是什么？

CEP 的 Behavior Change Contract 确保变更被验证——但被谁验证？被另一个 AI Agent，而它的判断也可能被污染。AI Agent 系统的安全模型目前仍然基本上是未定义的。

### 缺口 6：优雅降级

当 AI 模型性能下降时会发生什么——响应变慢、输出质量下降、速率限制？七个框架中没有一个有优雅降级策略。它们假设底层模型总是可用的、总是有能力的。

在生产环境中，这个假设不成立。一个能够检测模型退化并相应调整工作流的框架（例如，当响应质量低时简化 Review 步骤，或在速率限制期间批量处理 Compound 操作）将显著更加健壮。

---

## 7. 未来预测

基于全部七个框架中观察到的模式，以下是这个领域的走向：

### 预测 1：趋同为可组合的标准

七个框架不会永远保持七个独立的项目。它们将趋同——要么通过显式集成（如 C31 集成了 CEP），要么通过互操作性标准的出现。在 18 个月内，我们将看到技能定义格式、状态持久化 schema 和 Agent 间通信协议的事实标准。

### 预测 2：成本感知执行

框架级的成本跟踪和优化将成为标配。Agent 将能够报告"这个任务消耗了 $X 的算力"，框架将提供成本预算，并在预算超支时自动降级。

### 预测 3：持续学习成为默认

CEP 的 Compound 循环将被认定为最小可行学习机制，更复杂的方法——自动化方案质量评分、机构记忆的语义搜索、跨项目知识图谱——将会出现。

### 预测 4：安全优先的框架

第一个严重的 AI Agent 安全事件将触发一波安全导向的框架开发。信任边界、Agent 沙箱和对抗性鲁棒性测试将成为必备功能，而非事后补充。

### 预测 5："标准技术栈"的出现

正如 Web 开发领域趋同于标准技术栈（React + Node + PostgreSQL，或等效方案），AI Agent 领域也将趋同于一个标准技术栈：
- **行为层**：Superpowers 风格的规则
- **运维层**：ECC 风格的基础设施
- **工作流层**：CEP 风格的管线 + Archon 风格的确定性（用于关键路径）
- **记忆层**：CEP 风格的 Compound 循环 + 跨项目共享
- **架构层**：12-Factor 原则治理整体设计

这个技术栈目前还不存在于一个统一的产品中，但所有的组件都已经存在于此次分析的七个框架中。

---

## 8. 结语：领域走向成熟

七个严肃的 AI Agent 工程框架的存在——全部独立开发、全部经过实战检验、全部趋同于共享原则——本身就是最重要的发现。

这意味着这个领域已经走过了"提示词工程就是一切"的阶段。走过了"AI Agent 是魔法"的阶段。进入了"AI Agent 是需要工程纪律的工程系统"的阶段。

这就是成熟。不华丽，不革命性，不是奇点。只是让一个强大工具变得真正可靠的安静、持久的工作。

七个框架在很多事情上有分歧——给 AI 多少自主权、如何构建多 Agent 系统、如何处理状态、工作流应该有多确定。但它们在基本面上达成了一致：架构很重要，记忆很重要，人类很重要，今天的工作质量应该让明天的工作更轻松。

这个共识，通过真实世界的失败和迭代艰难赢得，是整个 AI Agent 工程运动迄今为止最有价值的产出。

---

## 附录：快速参考矩阵

供评估采用哪个框架的团队参考：

| 如果你的优先级是... | 首选 | 备选 | 避免 |
|------------------------|---------------|-----------------|-------|
| 快速见效、低投入 | Superpowers | Agent Skills | 12-Factor（对于快速见效过于抽象） |
| 生产可靠性 | ECC | Archon | Superpowers（对于生产环境过于行为化） |
| 团队知识留存 | CEP | GSD Core | 12-Factor（无实现） |
| 架构正确性 | 12-Factor | ECC | Archon（对于架构思维过于刚性） |
| 会话连续性 | GSD Core | ECC | Agent Skills（无会话管理） |
| 确定性管线 | Archon | CEP | Superpowers（对于管线过于灵活） |
| 防止 AI 走捷径 | Agent Skills | Superpowers | Archon（约束是结构性的，不是行为性的） |
| 全都要（厨房水槽） | C31 集成方案 | 自建组合方案 | 单独使用任何一个框架 |

---

## Twitter 帖子：12 条推文对比 7 大框架

**🧵 1/12**
我花了几个月时间研究 7 个 AI Agent 工程框架：Superpowers (249k⭐)、ECC (225k⭐)、Agent Skills (70k⭐)、12-Factor (24k⭐)、CEP (23k⭐)、Archon (23k⭐)、GSD Core (6k⭐)。

以下是我的发现。长帖👇

**🧵 2/12**
每个框架认为 AI 的根本问题不同：
• Superpowers：需要纪律
• ECC：需要基础设施
• Agent Skills：需要护栏
• 12-Factor：需要架构
• CEP：需要记忆
• Archon：需要确定性
• GSD Core：需要连续性

**🧵 3/12**
它们在从抽象到具体的光谱上各有位置：

抽象 ◄──────────────────────► 具体
12-Factor → Superpowers → Agent Skills → GSD → ECC → CEP → Archon

左 = 灵活但不可预测。右 = 可预测但刚性。选择你的权衡。

**🧵 4/12**
选型指南：

想快速见效？→ Superpowers 或 Agent Skills
生产可靠性？→ ECC
不想重复修同样的 bug？→ CEP
从零开始做架构？→ 12-Factor
会话之间总丢上下文？→ GSD Core
需要确定性管线？→ Archon

**🧵 5/12**
惊人的发现：尽管零协调，全部 7 个趋同于 5 个共享原则。这就是 2026 年 AI 工程的共识。来自真实世界使用的真正趋同。

**🧵 6/12**
共识 #1：架构优于提示词。

全部 7 个都已超越"写个更好的提示词"。可靠的 AI 需要结构性干预——配置文件、工作流定义、状态管理。提示词仍然重要，但围绕它的系统决定了可靠性。

**🧵 7/12**
共识 #2：多 Agent 编排。

7 个中的 6 个实现了某种形式的多 Agent 协作。单一单体 Agent 无法胜任复杂任务。分解 → 专业化 → 合并。与单体 → 微服务的演进如出一辙。

**🧵 8/12**
共识 #3：Human-in-the-loop 不是可选的。

零个框架主张完全自主。全部包含人类检查点。共识是：在有限范围内自主运行，在范围边界处暂停。

"AGI 做一切"阵营输了。工程阵营赢了。

**🧵 9/12**
共识 #4：跨会话持久状态。

"无记忆"的 AI Agent 从根本上受限。每一个成熟框架都在会话边界之间持久化知识、上下文和进度。机制各异，原则统一。

**🧵 10/12**
共识 #5：Knowledge Compounding。

CEP 将它显式化了，但全部 7 个都实现了某种版本。AI 辅助工程应该是一个学习系统。每一个工作单元都应该让下一个更轻松。

这就是复利效应应用于软件工程。

**🧵 11/12**
全部 7 个都缺失的：
• 跨团队知识共享
• 成本感知（$/token 追踪）
• 定量自我评估
• 模型无关设计
• 安全与信任边界
• 优雅降级

这些就是下一个前沿。

**🧵 12/12**
大局观：7 个框架独立趋同 = 这个领域正在走向成熟。

我们已走过"提示词就是一切"的阶段。走过"AI 是魔法"的阶段。进入了"AI Agent 是需要工程纪律的工程系统"的阶段。

不华丽。不革命性。只是可靠。而这就是重点。🔧
