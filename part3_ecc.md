# Agent Harness Papers — Part 3: Everything Claude Code (ECC)

*225k stars, 246 skills, 61 agents. This isn't a framework — it's AI agent Linux.*

![ECC Architecture — 4-Layer System](images/final_part3_ecc.jpg)

---

## The Scale That Demands Explanation

Let's start with the numbers because the numbers are the argument.

Everything Claude Code (ECC) has accumulated roughly 225,000 GitHub stars. It ships 246 skills. It orchestrates 61 distinct sub-agents. It wires 28 hooks into the development lifecycle. And it was built by Affaan Mustafa, who won an Anthropic hackathon and then apparently decided that winning wasn't enough — he wanted to rewrite how AI agents think about software engineering.

To put this in perspective: most "AI coding frameworks" ship a system prompt template and a README that says "customize to your needs." ECC ships an operating system. The comparison to Linux isn't hyperbole — it's architectural. Linux doesn't tell you how to write code. It provides primitives (processes, filesystems, pipes) and lets an ecosystem emerge. ECC does the same thing for AI agents: it provides primitives (skills, agents, hooks, memory layers) and lets engineering teams compose them into workflows that match their actual development culture.

But scale alone is noise. The interesting question is *why* this particular project attracted 225k stars when dozens of alternatives exist. The answer lives in one philosophical commitment that ECC got right before anyone else articulated it clearly.

---

## The Cold-Start Problem: Why Your AI Agent Is Worse On Monday Morning

Here's the dirty secret of AI-assisted development: every session starts from zero.

You open your IDE. You invoke Claude, or GPT, or Gemini. The model knows nothing about your project's conventions. It doesn't know that your team uses `snake_case` for database columns and `camelCase` for API responses. It doesn't know that `utils/` is a graveyard of dead code that nobody has cleaned in three years. It doesn't know that the last developer who touched the authentication module was fired for committing secrets to the repo.

This is the cold-start problem, and ECC's core thesis is that it's the *only* problem worth solving.

Mustafa's insight was that AI agents aren't bad at coding — they're bad at *knowing what they should already know*. A human developer who joins your team spends weeks absorbing context: reading docs, attending standups, getting code reviews rejected, learning where the bodies are buried. An AI agent gets none of this onboarding. Every session, it's a new hire on day one.

ECC's solution is to eliminate cold starts entirely. When an ECC-configured agent begins a session, it doesn't start with a blank context window. It starts with your project's standards, your team's conventions, your codebase's known failure modes, and your organization's decision history — all pre-loaded through a layered architecture of skills, memory, and hooks.

The philosophical position is radical in its simplicity: **the agent should never need to ask a question that your codebase already answers.**

---

## The 4-Layer Architecture: Memory as Infrastructure

ECC's architecture isn't a monolith. It's four layers that compose vertically, each layer serving a different temporal scope of knowledge.

### Layer 1: Skills (Static Knowledge)

The 246 skills are the bedrock. Each skill is a self-contained instruction set — a SKILL.md file with YAML frontmatter for trigger matching, a markdown body with execution instructions, and optional supporting directories (`scripts/`, `examples/`, `references/`, `resources/`).

Skills are *static* in the sense that they represent codified best practices that don't change per-session. A skill like `ce-debug` encodes a systematic debugging methodology: reproduce → hypothesize → isolate → fix → verify → document. A skill like `ce-commit` encodes commit message conventions. These aren't suggestions — they're executable protocols.

The critical design decision is **progressive disclosure**. Skills aren't dumped into the context window wholesale. The YAML frontmatter contains trigger keywords, and skills are loaded *only when relevant to the current task*. This keeps the context window clean and the agent focused. The agent doesn't need to know about the `ce-code-review` protocol while it's in the middle of debugging a failing test.

### Layer 2: Agents (Dynamic Orchestration)

The 61 sub-agents are the muscle. While skills define *what* to do, agents define *who* does it. ECC decomposes complex workflows into specialized roles: a Context Analyzer agent, a Solution Extractor agent, a Related Docs Finder agent, a Security Reviewer agent, an Adversarial Reviewer agent.

This isn't just organizational neatness — it's a context management strategy. Each sub-agent receives only the minimal context required for its specific task. The Context Analyzer doesn't need to see the full codebase; it needs to see the files changed in the current diff. The Security Reviewer doesn't need to understand business logic; it needs to see authentication flows and input handling.

This is **sub-agent context delegation**, and it solves one of the hardest problems in LLM orchestration: context window pollution. When you throw everything at a single agent, signal-to-noise ratio degrades. When you decompose into specialized sub-agents with curated context, each agent operates at peak effectiveness within its narrow domain.

### Layer 3: Hooks (Event-Driven Automation)

The 28 hooks are the nervous system. Hooks fire on events — pre-commit, post-save, on-error, on-test-failure — and trigger automated responses without human intervention.

A hook might auto-format code on save. Another might detect common error patterns in test output and suggest fixes. Another might enforce that no file is committed without a corresponding test. Hooks are the layer that makes ECC feel *alive* — the agent isn't just responding to prompts, it's actively monitoring the development lifecycle and intervening when its standards are violated.

The hook system is where ECC's "operating system" metaphor becomes most literal. Linux has systemd, cron, inotify. ECC has hooks. Both serve the same purpose: making the environment reactive to events rather than waiting for explicit commands.

### Layer 4: Memory (Temporal Knowledge)

Memory is the layer that makes cold starts impossible. ECC implements a 4-layer memory consolidation system:

1. **Session State** (`session_state.json`): Active projects, open TODOs, pending decisions, last topic. Flushed at session end, loaded at session start. This is working memory — what you were doing five minutes ago.

2. **Diary** (`diary/YYYY-MM-DD.md`): Daily logs of what happened. 1-5 lines per session, appended chronologically. This is episodic memory — what happened on Tuesday.

3. **Instincts** (`instincts/instinct-XXX-name.md`): Learned behavioral patterns with confidence scores. These evolve through a promotion pipeline: candidate → verifying → instinct. This is procedural memory — the reflexes that get faster with practice.

4. **Solutions** (`docs/solutions/[category]/`): Documented problem-solution pairs, indexed for retrieval. When the agent encounters a problem, it searches the solutions store before attempting a fresh solution. This is institutional memory — the knowledge that survives employee turnover.

The consolidation flow is deliberate: session state is volatile, diary is append-only, instincts are evolutionary, solutions are permanent. Each layer has different write patterns, different retention policies, and different retrieval strategies. This isn't accidental — it mirrors how human teams actually accumulate knowledge, from Slack messages (volatile) to wiki pages (permanent).

---

## Sub-Agent Context Delegation: The Real Innovation

Most people look at ECC's 246 skills and think that's the innovation. It's not. The innovation is how ECC manages context across 61 sub-agents.

Here's the problem: LLMs have finite context windows. Even with 200k tokens, you can't fit an entire codebase, all project conventions, all historical decisions, and the current task into a single prompt. Something has to give.

ECC's answer is delegation with minimal context. When the main agent encounters a code review task, it doesn't try to review the code itself with the full project context loaded. Instead, it spawns specialized sub-agents:

- A **Style Reviewer** gets the diff + style guide
- A **Logic Reviewer** gets the diff + related tests + function signatures
- A **Security Reviewer** gets the diff + auth flows + input handling patterns
- An **Adversarial Reviewer** gets the diff + a mandate to find everything the other reviewers missed

Each sub-agent operates in a clean context window, focused on its specific domain. Results are collected, deduplicated, and merged by the orchestrating agent.

This pattern — which ECC calls the **Accumulator Pattern** — is deceptively powerful. It means that the *effective* context available to an ECC review exceeds any single context window. The Style Reviewer uses its full window for style concerns. The Security Reviewer uses its full window for security concerns. The total analytical capacity is multiplied across sub-agents while each individual agent remains focused and accurate.

The Accumulator Pattern also introduces a natural **GateGuard** mechanism. Before spawning sub-agents, the orchestrating agent evaluates whether the task actually requires decomposition. A single-file formatting change doesn't need four reviewers. A critical authentication refactor does. GateGuard prevents over-engineering simple tasks while ensuring complex tasks get the scrutiny they deserve.

---

## Continuous Learning v2.1: The Instinct System

ECC's most forward-looking feature is its Continuous Learning system, currently at version 2.1. This is where the framework stops being a tool and starts being an entity that *evolves*.

The core concept is the **instinct** — a behavioral pattern with a confidence score between 0.1 and 0.95. Instincts are born as candidates (confidence ~0.3), promoted to verifying status after repeated successful application (confidence ~0.6), and elevated to full instincts after three successful verifications (confidence ~0.9).

Here's what makes this interesting: instincts can also *die*. When the user provides negative feedback — "that's wrong," "stop doing that," "you misunderstood" — the relevant instinct's confidence is immediately dropped to below 0.3. If confidence hits the floor, the instinct is flagged as a deprecated anti-pattern and actively avoided in future sessions.

This creates a genuine evolutionary pressure. Good patterns survive and strengthen. Bad patterns weaken and die. The agent's behavior genuinely changes over time based on accumulated feedback, not just per-session prompt engineering.

The seed instincts in ECC illustrate the system's philosophy:

- `instinct-001-no-overwrite` (confidence: 0.95) — Never overwrite existing files without explicit confirmation. This is a high-confidence instinct because file destruction is irreversible.
- `instinct-002-research-first` (confidence: 0.9) — Always research before coding. High confidence because cold-start failures are consistently traced to skipping research.
- `instinct-003-surgical-changes` (confidence: 0.9) — Modify only what the task requires. High confidence because scope creep is the most common failure mode in AI-assisted development.
- `instinct-004-ce-compound-trigger` (confidence: 0.85) — Proactively document solutions after solving problems. Slightly lower confidence because not every solution warrants documentation.

The instinct system is effectively a prototype of what AI agent personality could look like at scale — not a fixed character, but an evolving set of learned behaviors calibrated by real-world outcomes.

---

## Cross-Tool Reality: The Name Is a Lie (And That's the Point)

Despite being called "Everything Claude Code," ECC works across Claude, Cursor, Codex, OpenCode, Gemini, Zed, and GitHub Copilot.

This isn't an accident. Mustafa designed ECC around a universal abstraction: the agent configuration layer. Every modern AI coding tool supports some form of system prompt injection, file-based configuration, and tool/function calling. ECC targets this common substrate rather than any specific model's API.

The practical implication is significant: teams using ECC can switch between AI providers without losing their accumulated knowledge, conventions, and behavioral instincts. The skills, memory, and hooks are provider-agnostic. The 4-layer architecture is a *portable development culture* that travels with the team, not with the tool.

This cross-tool portability is also what makes ECC an "operating system" rather than a "framework." Frameworks are coupled to their runtime. Operating systems provide an abstraction layer that lets applications run regardless of the underlying hardware. ECC provides an abstraction layer that lets engineering practices run regardless of the underlying AI model.

---

## C31 Integration: The Art of Selective Extraction

Here's where the story gets interesting for the Agent Harness Papers series.

C31 — the compound engineering system this series has been building towards — looked at ECC's 246 skills, 61 agents, and 28 hooks, and extracted exactly **three mechanisms**. Not three skills. Not three agents. Three *mechanisms* — structural patterns that could be repurposed independent of ECC's specific implementation.

### Mechanism 1: Context Health Color System (🟢🟡🟠🔴)

ECC introduced a brilliant visual metaphor for context window utilization:

| Status | Threshold | Behavior |
|--------|-----------|----------|
| 🟢 Green | <50% | Normal operation |
| 🟡 Yellow | 50–70% | Begin compressing completed work into summaries |
| 🟠 Orange | 70–85% | Aggressively compress: move decisions to files, archive assumptions |
| 🔴 Red | >85% | Forced checkpoint: write state to disk before continuing |

C31 adopted this wholesale because it solves a problem that every long-running agent session eventually hits: context rot. As conversations grow, early instructions get diluted by later content. The color system makes this degradation visible and actionable, turning an invisible failure mode into a managed process.

### Mechanism 2: Assumption Tracking Template

ECC's assumption tracking format:

```
Assumption: [What we're assuming]
Basis: [Why we think it's true]
Risk: [What happens if wrong]
Verification: [How to confirm]
Status: Active / Validated / Invalidated
```

C31 adopted this because assumption drift is the number one cause of debugging rabbit holes. An agent that makes an unstated assumption about data formats, API behavior, or environment configuration will happily build an entire solution on that assumption — and the failure won't be detected until the assumption collapses under production load. Making assumptions explicit and trackable converts silent failures into early warnings.

### Mechanism 3: Instinct Evolution (candidate → verifying → instinct)

The three-stage promotion pipeline for learned behaviors. C31 adopted this because it provides a structured alternative to the binary choice between "always do X" and "never do X." A candidate instinct is a hypothesis. A verifying instinct is an experiment. A confirmed instinct is a proven practice. The pipeline creates space for behavioral experimentation without risking production quality.

### Why Only Three?

The question isn't why C31 extracted three mechanisms — it's why C31 *only* extracted three from a pool of 246 skills.

The answer is philosophical: **C31 optimizes for minimal effective configuration.** ECC's 246 skills represent the full complexity of every engineering workflow Mustafa has encountered. C31 doesn't need all of that complexity. It needs the *structural patterns* that make any skill more effective, regardless of what the skill does.

Context Health makes every skill work better by preventing context degradation. Assumption Tracking makes every debugging session more effective by surfacing hidden premises. Instinct Evolution makes every behavioral rule improvable by providing a maturation pathway.

These three mechanisms are *meta-skills* — they improve the quality of other skills rather than doing any specific work themselves. That's why they survived the extraction process while 243 skills did not.

---

## Limitations: Where ECC Breaks Down

No honest analysis skips the failure modes. ECC has several.

**Complexity ceiling.** 246 skills is a lot of skills. New contributors face a genuine onboarding problem — the very cold-start problem ECC solves for AI agents, it *creates* for human developers trying to understand the framework. The documentation is extensive but the sheer surface area is intimidating.

**Skill interaction effects.** With 246 skills that can be loaded in arbitrary combinations, emergent interactions are inevitable. Skill A might assume formatting conventions that Skill B modifies. Skill C might spawn sub-agents that conflict with Skill D's resource assumptions. ECC doesn't have a formal skill compatibility system — conflicts are discovered empirically.

**Memory management overhead.** The 4-layer memory system requires maintenance. Session states need to be flushed correctly. Diaries need to stay concise. Instincts need to be pruned when they accumulate. Solutions need to be indexed. In practice, the memory system drifts toward entropy unless actively maintained — the same problem that plagues every knowledge management system ever built.

**Model dependency variance.** While ECC is theoretically cross-tool, the quality of execution varies significantly across models. Skills designed for Claude's 200k context window behave differently in Gemini's context. Sub-agent delegation patterns that work smoothly with Claude's tool-calling API may stutter with other providers. The abstraction layer leaks in practice.

**Instinct overfitting.** The Continuous Learning system can overfit to specific users or projects. An instinct that achieves high confidence in one codebase might be counterproductive in another. The system doesn't currently support per-project instinct profiles, meaning instincts are global across all projects — a design choice that trades precision for simplicity.

---

## The Verdict

ECC is the most ambitious attempt to date at making AI agents *institutionally intelligent* — not just individually capable, but embedded in a development culture with memory, standards, and evolutionary learning.

Its weakness is the same as its strength: comprehensive ambition. 246 skills cover everything, but everything is a lot to maintain. 61 agents provide thorough coverage, but thorough coverage means complex orchestration. 28 hooks automate the lifecycle, but lifecycle automation means lifecycle coupling.

For C31, ECC's value isn't in its specific skills — it's in its structural innovations. Context Health, Assumption Tracking, and Instinct Evolution are mechanisms that improve any agent system, regardless of scale. They're the operating system primitives that survived the extraction process because they're genuinely universal.

ECC proved that agent harnesses can be operating systems, not just configuration files. Whether you need the full operating system or just its best kernel modules — that's a team-by-team decision.

---

## 🧵 Twitter Thread

**1/** 225k stars. 246 skills. 61 agents. 28 hooks.

Everything Claude Code isn't a framework — it's an operating system for AI agents. Here's what @AffaanMustafa built and why it matters. 🧵

**2/** The core thesis: AI agents have a cold-start problem. Every session starts from zero — no knowledge of your conventions, your codebase, your team's decisions.

ECC eliminates cold starts entirely. When an agent begins a session, your project's standards are already loaded.

**3/** The 4-layer memory architecture:
- Session state (working memory)
- Diary (episodic memory)
- Instincts (procedural memory)
- Solutions (institutional memory)

Each layer has different retention policies. Just like how human teams actually store knowledge.

**4/** The killer feature: Sub-Agent Context Delegation.

Instead of cramming everything into one context window, ECC spawns specialized agents — Style Reviewer, Security Reviewer, Logic Reviewer — each with minimal, focused context.

Total analytical capacity exceeds any single model's window.

**5/** Continuous Learning v2.1 introduces instincts — behavioral patterns with confidence scores.

Good patterns strengthen over time. Bad patterns weaken and die. The agent literally evolves based on accumulated feedback.

This is what AI personality looks like at scale.

**6/** Despite the name, ECC works across Claude, Cursor, Codex, Gemini, Zed, and GitHub Copilot.

It targets the universal substrate: system prompts + file configs + tool calling. Your development culture becomes portable across AI providers.

**7/** C31 looked at all 246 skills and extracted exactly 3 mechanisms:
- Context Health color system (🟢🟡🟠🔴)
- Assumption Tracking template
- Instinct Evolution pipeline

Why only 3? Because these are meta-skills — they make other skills better.

**8/** The limitation: 246 skills is a lot. The complexity ceiling creates its own cold-start problem — for humans trying to learn the framework.

But ECC proved that agent harnesses can be operating systems, not just config files. That idea changes everything downstream.


---

# 中文版

# Agent Harness 论文系列 — 第三篇：Everything Claude Code (ECC)

*225k star，246 个 skill，61 个 agent。这不是一个框架——这是 AI agent 的 Linux。*

---

## 需要解释的规模

先看数字，因为数字本身就是论点。

Everything Claude Code（ECC）积累了大约 225,000 个 GitHub star。它搭载了 246 个 skill，编排 61 个独立的 sub-agent，将 28 个 hook 接入开发生命周期。而它的作者 Affaan Mustafa，赢得了 Anthropic 黑客松之后，显然觉得赢还不够——他要重新定义 AI agent 对软件工程的理解方式。

为了直观感受这个规模：大多数"AI 编程框架"交付的是一个 system prompt 模板和一份写着"根据需要自定义"的 README。ECC 交付的是一个操作系统。与 Linux 的类比不是夸张——而是架构层面的精确对应。Linux 不教你怎么写代码，它提供原语（进程、文件系统、管道），让生态自然涌现。ECC 对 AI agent 做了同样的事：它提供原语（skill、agent、hook、memory 层），让工程团队将其组合成匹配自身开发文化的工作流。

但规模本身只是噪音。真正有趣的问题是：当几十个替代品存在时，*为什么*偏偏这个项目吸引了 225k star？答案藏在一个哲学承诺中——ECC 在其他人清晰表述之前就已经做对了的事。

---

## Cold-Start Problem：为什么你的 AI Agent 周一早上更蠢

这是 AI 辅助开发的肮脏秘密：每个 session 都从零开始。

你打开 IDE，调用 Claude、GPT 或 Gemini。模型对你的项目规范一无所知。它不知道你的团队用 `snake_case` 命名数据库列，用 `camelCase` 命名 API 响应。它不知道 `utils/` 是一个三年没人清理的死代码墓地。它不知道上一个碰过认证模块的开发者因为把 secret 提交到仓库而被开除了。

这就是 Cold-Start Problem，而 ECC 的核心论点是：这是*唯一*值得解决的问题。

Mustafa 的洞察在于：AI agent 不是编程能力不行——它们是*不知道自己本应已知的东西*。一个新加入团队的人类开发者会花几周时间吸收上下文：读文档，参加站会，被 code review 打回，学会哪里埋了雷。AI agent 什么入职培训都没有。每个 session，它都是第一天上班的新人。

ECC 的解决方案是彻底消除冷启动。当一个配置了 ECC 的 agent 开始 session 时，它不是从空白的 context window 启动的。它启动时已经加载了你的项目标准、团队规范、代码库已知的故障模式和组织的决策历史——全部通过 skill、memory 和 hook 的分层架构预加载。

这个哲学立场极其朴素却又激进：**agent 永远不应该提出一个你的代码库已经回答了的问题。**

---

## 四层架构：Memory 即基础设施

ECC 的架构不是单体的。它是四个垂直组合的层级，每一层服务于不同时间尺度的知识。

### 第一层：Skill（静态知识）

246 个 skill 是基岩。每个 skill 是一个自包含的指令集——一个带 YAML frontmatter 的 SKILL.md 文件用于触发匹配，一个 markdown 正文包含执行指令，以及可选的支持目录（`scripts/`、`examples/`、`references/`、`resources/`）。

Skill 是*静态的*，意味着它们代表已编纂的最佳实践，不会按 session 变化。像 `ce-debug` 这样的 skill 编码了一套系统化的调试方法论：复现 → 假设 → 隔离 → 修复 → 验证 → 文档化。像 `ce-commit` 这样的 skill 编码了 commit message 规范。这些不是建议——它们是可执行的协议。

关键的设计决策是**渐进式披露**。Skill 不会被整体塞进 context window。YAML frontmatter 包含触发关键词，skill *仅在与当前任务相关时*才被加载。这保持了 context window 的整洁和 agent 的专注。当 agent 正在调试一个失败的测试时，它不需要知道 `ce-code-review` 协议。

### 第二层：Agent（动态编排）

61 个 sub-agent 是执行肌肉。Skill 定义*做什么*，agent 定义*谁来做*。ECC 将复杂工作流分解为专业角色：Context Analyzer agent、Solution Extractor agent、Related Docs Finder agent、Security Reviewer agent、Adversarial Reviewer agent。

这不仅仅是组织上的整洁——而是一种上下文管理策略。每个 sub-agent 只接收完成其特定任务所需的最小上下文。Context Analyzer 不需要看到整个代码库；它只需要看到当前 diff 中改动的文件。Security Reviewer 不需要理解业务逻辑；它只需要看到认证流程和输入处理。

这就是 **Sub-Agent Context Delegation**，它解决了 LLM 编排中最难的问题之一：context window 污染。当你把所有东西都扔给一个 agent 时，信噪比会退化。当你分解为拥有精选上下文的专业 sub-agent 时，每个 agent 都能在自己的窄领域内以最佳状态运行。

### 第三层：Hook（事件驱动自动化）

28 个 hook 是神经系统。Hook 在事件触发时运行——pre-commit、post-save、on-error、on-test-failure——无需人工干预即可触发自动响应。

一个 hook 可能在保存时自动格式化代码。另一个可能检测测试输出中的常见错误模式并建议修复。还有一个可能强制要求每个文件在提交时必须有对应的测试。Hook 是让 ECC 感觉*活着*的那一层——agent 不仅仅是在响应 prompt，它在主动监控开发生命周期，并在标准被违反时进行干预。

Hook 系统是 ECC 的"操作系统"隐喻最真实的地方。Linux 有 systemd、cron、inotify。ECC 有 hook。两者服务于同一目的：让环境对事件做出反应，而不是等待显式命令。

### 第四层：Memory（时间性知识）

Memory 是让冷启动变得不可能的那一层。ECC 实现了一个四层 memory 固化系统：

1. **Session State**（`session_state.json`）：活跃项目、待办事项、待决事项、上一个话题。session 结束时刷出，session 开始时加载。这是工作记忆——你五分钟前在做什么。

2. **Diary**（`diary/YYYY-MM-DD.md`）：每日发生了什么的日志。每个 session 追加 1-5 行，按时间顺序。这是情节记忆——周二发生了什么。

3. **Instincts**（`instincts/instinct-XXX-name.md`）：带置信度分数的学习型行为模式。它们通过晋升管道演化：candidate → verifying → instinct。这是程序性记忆——随着练习变得更快的条件反射。

4. **Solutions**（`docs/solutions/[category]/`）：文档化的问题-解决方案对，建立索引以供检索。当 agent 遇到问题时，它会先搜索 solutions store，然后再尝试从头解决。这是机构记忆——能在人员流动中幸存的知识。

固化流程是刻意设计的：session state 是易失的，diary 是只追加的，instincts 是演化的，solutions 是永久的。每一层有不同的写入模式、不同的保留策略和不同的检索方式。这不是偶然的——它反映了人类团队实际积累知识的方式，从 Slack 消息（易失）到 wiki 页面（永久）。

---

## Sub-Agent Context Delegation：真正的创新

大多数人看到 ECC 的 246 个 skill 会以为那就是创新。不是的。创新在于 ECC 如何跨 61 个 sub-agent 管理上下文。

问题是这样的：LLM 的 context window 是有限的。即使有 200k token，你也无法把整个代码库、所有项目规范、所有历史决策和当前任务塞进一个 prompt 里。必须有所取舍。

ECC 的答案是最小上下文委托。当主 agent 遇到代码审查任务时，它不会在加载完整项目上下文的情况下自己审查代码。相反，它会生成专业的 sub-agent：

- **Style Reviewer** 获得 diff + 风格指南
- **Logic Reviewer** 获得 diff + 相关测试 + 函数签名
- **Security Reviewer** 获得 diff + 认证流程 + 输入处理模式
- **Adversarial Reviewer** 获得 diff + 一个使命：找到其他审查者遗漏的一切

每个 sub-agent 在一个干净的 context window 中运行，专注于自己的特定领域。结果被收集、去重并由编排 agent 合并。

这个模式——ECC 称之为 **Accumulator Pattern**——有着欺骗性的强大力量。它意味着 ECC 审查可用的*有效*上下文超越了任何单一 context window。Style Reviewer 用它的全部窗口处理风格问题。Security Reviewer 用它的全部窗口处理安全问题。总分析能力在 sub-agent 之间被乘数放大，而每个单独的 agent 仍然保持专注和准确。

Accumulator Pattern 还引入了一个自然的 **GateGuard** 机制。在生成 sub-agent 之前，编排 agent 会评估任务是否真的需要分解。一个单文件的格式修改不需要四个审查者。一个关键的认证重构需要。GateGuard 防止对简单任务过度工程化，同时确保复杂任务得到它们应有的审视。

---

## Continuous Learning v2.1：Instinct 系统

ECC 最具前瞻性的特性是它的 Continuous Learning 系统，目前版本为 2.1。这是框架不再只是工具、开始成为一个*演化实体*的地方。

核心概念是 **instinct**——一个带有 0.1 到 0.95 之间置信度分数的行为模式。Instinct 诞生为 candidate（置信度约 0.3），在反复成功应用后晋升为 verifying 状态（置信度约 0.6），三次成功验证后升级为完全的 instinct（置信度约 0.9）。

有趣的是：instinct 也可以*死亡*。当用户给出负面反馈——"那是错的"、"别这样做"、"你理解错了"——相关 instinct 的置信度会立即降到 0.3 以下。如果置信度触底，该 instinct 会被标记为已弃用的反模式，在未来的 session 中被主动规避。

这创造了一种真正的演化压力。好的模式存活并强化。坏的模式弱化并消亡。Agent 的行为确实会随着时间的推移基于累积的反馈而改变，而不仅仅是每个 session 的 prompt 工程。

ECC 中的种子 instinct 体现了这套系统的哲学：

- `instinct-001-no-overwrite`（置信度：0.95）——未经明确确认永不覆盖现有文件。这是高置信度的 instinct，因为文件销毁是不可逆的。
- `instinct-002-research-first`（置信度：0.9）——编码前先调研。高置信度，因为冷启动失败始终可以追溯到跳过了调研。
- `instinct-003-surgical-changes`（置信度：0.9）——只修改任务要求的部分。高置信度，因为范围蔓延是 AI 辅助开发中最常见的失败模式。
- `instinct-004-ce-compound-trigger`（置信度：0.85）——解决问题后主动记录解决方案。置信度略低，因为不是每个解决方案都值得文档化。

Instinct 系统实际上是 AI agent 人格在规模化场景下可能样貌的原型——不是固定的角色设定，而是一组由真实世界结果校准的、不断演化的学习型行为。

---

## 跨工具现实：名字是个谎言（但这正是关键）

尽管名字叫"Everything Claude Code"，ECC 可以跨 Claude、Cursor、Codex、Gemini、Zed 和 GitHub Copilot 工作。

这不是偶然的。Mustafa 围绕一个通用抽象来设计 ECC：agent 配置层。每个现代 AI 编程工具都支持某种形式的 system prompt 注入、基于文件的配置和 tool/function calling。ECC 瞄准的是这个公共基底，而不是任何特定模型的 API。

实际意义非常重大：使用 ECC 的团队可以在 AI 供应商之间切换，而不会丢失他们积累的知识、规范和行为 instinct。Skill、memory 和 hook 是供应商无关的。四层架构是一种*可移植的开发文化*，它跟着团队走，而不是跟着工具走。

这种跨工具可移植性也正是让 ECC 成为"操作系统"而非"框架"的原因。框架耦合于其运行时。操作系统提供抽象层，让应用程序无论底层硬件如何都能运行。ECC 提供抽象层，让工程实践无论底层 AI 模型如何都能运行。

---

## C31 整合：选择性萃取的艺术

这里是故事对 Agent Harness 论文系列变得有趣的地方。

C31——这个系列一直在构建的复合工程系统——审视了 ECC 的 246 个 skill、61 个 agent 和 28 个 hook，然后精确地萃取了**三个机制**。不是三个 skill。不是三个 agent。三个*机制*——可以独立于 ECC 具体实现而复用的结构性模式。

### 机制 1：Context Health 色彩系统（🟢🟡🟠🔴）

ECC 引入了一个精妙的视觉隐喻来表示 context window 的使用率：

| 状态 | 阈值 | 行为 |
|------|------|------|
| 🟢 绿色 | <50% | 正常运行 |
| 🟡 黄色 | 50–70% | 开始将已完成工作压缩为摘要 |
| 🟠 橙色 | 70–85% | 积极压缩：将决策移入文件，归档假设 |
| 🔴 红色 | >85% | 强制 checkpoint：写状态到磁盘后再继续 |

C31 全盘采纳了这个机制，因为它解决了每个长时间运行的 agent session 最终都会遇到的问题：Context Rot。随着对话增长，早期的指令会被后续内容稀释。色彩系统让这种退化变得可见且可操作，将一个隐形的失败模式转化为一个被管理的过程。

### 机制 2：假设追踪模板

ECC 的假设追踪格式：

```
Assumption: [What we're assuming]
Basis: [Why we think it's true]
Risk: [What happens if wrong]
Verification: [How to confirm]
Status: Active / Validated / Invalidated
```

C31 采纳这个机制是因为假设漂移是调试兔子洞的头号原因。一个对数据格式、API 行为或环境配置做出未声明假设的 agent，会愉快地基于这个假设构建整个解决方案——而故障直到假设在生产负载下崩溃时才会被检测到。让假设显式化和可追踪，将静默故障转化为早期预警。

### 机制 3：Instinct 演化（candidate → verifying → instinct）

学习型行为的三阶段晋升管道。C31 采纳它是因为它提供了一种结构化的替代方案，避免了"始终做 X"和"永不做 X"之间的二元选择。Candidate instinct 是一个假设。Verifying instinct 是一个实验。已确认的 instinct 是一个被证明的实践。这个管道为行为实验创造了空间，而不会冒影响生产质量的风险。

### 为什么只有三个？

问题不是为什么 C31 萃取了三个机制——而是为什么 C31 从 246 个 skill 的池子里*只*萃取了三个。

答案是哲学性的：**C31 优化的目标是最小有效配置。** ECC 的 246 个 skill 代表了 Mustafa 遇到过的所有工程工作流的完整复杂度。C31 不需要所有这些复杂度。它需要的是让任何 skill 都更有效的*结构性模式*，无论该 skill 具体做什么。

Context Health 通过防止上下文退化让每个 skill 工作得更好。Assumption Tracking 通过浮现隐藏前提让每次调试 session 更有效。Instinct Evolution 通过提供成熟路径让每条行为规则可改进。

这三个机制是*元技能*——它们提升其他 skill 的质量，而不是自身做任何具体工作。这就是为什么它们在萃取过程中存活下来，而其他 243 个 skill 没有。

---

## 局限性：ECC 在哪里失灵

诚实的分析不会跳过失败模式。ECC 有几个。

**复杂度天花板。** 246 个 skill 是很多 skill。新贡献者面临一个真实的入门问题——ECC 为 AI agent 解决的冷启动问题，它反过来为试图理解这个框架的人类开发者*制造*了冷启动问题。文档很详尽，但庞大的表面积令人生畏。

**Skill 交互效应。** 有 246 个可以任意组合加载的 skill，涌现性的交互是不可避免的。Skill A 可能假设的格式规范被 Skill B 修改了。Skill C 可能生成的 sub-agent 与 Skill D 的资源假设冲突。ECC 没有正式的 skill 兼容性系统——冲突只能通过经验发现。

**Memory 管理开销。** 四层 memory 系统需要维护。Session state 需要正确刷出。Diary 需要保持精简。Instinct 累积时需要剪枝。Solutions 需要建索引。实践中，memory 系统会向熵的方向漂移，除非被积极维护——这和人类有史以来建立的每个知识管理系统面临的问题一样。

**模型依赖差异。** 虽然 ECC 理论上是跨工具的，但执行质量在不同模型间差异显著。为 Claude 的 200k context window 设计的 skill 在 Gemini 的 context 中表现不同。在 Claude 的 tool-calling API 上运行流畅的 sub-agent 委托模式，在其他供应商那里可能会卡顿。抽象层在实践中会泄漏。

**Instinct 过拟合。** Continuous Learning 系统可能对特定用户或项目过拟合。在一个代码库中达到高置信度的 instinct 在另一个代码库中可能适得其反。系统目前不支持按项目的 instinct 配置，这意味着 instinct 在所有项目中是全局的——这是一个用精度换简单性的设计选择。

---

## 结论

ECC 是迄今为止最雄心勃勃的尝试，让 AI agent 具备*机构智能*——不仅仅是个体能力强，而是嵌入到一个拥有记忆、标准和演化学习的开发文化中。

它的弱点与优点相同：全面的雄心。246 个 skill 覆盖了一切，但一切意味着大量的维护工作。61 个 agent 提供了全面覆盖，但全面覆盖意味着复杂的编排。28 个 hook 自动化了生命周期，但生命周期自动化意味着生命周期耦合。

对于 C31 而言，ECC 的价值不在于它的具体 skill——而在于它的结构性创新。Context Health、Assumption Tracking 和 Instinct Evolution 是能改善任何 agent 系统的机制，无关规模。它们是在萃取过程中存活下来的操作系统原语，因为它们是真正通用的。

ECC 证明了 Agent Harness 可以是操作系统，而不仅仅是配置文件。你是需要完整的操作系统还是只需要它最好的内核模块——那是每个团队各自的决定。

---

## 🧵 推特长帖

**1/** 225k star。246 个 skill。61 个 agent。28 个 hook。

Everything Claude Code 不是一个框架——它是 AI agent 的操作系统。以下是 @AffaanMustafa 构建了什么，以及为什么它很重要。🧵

**2/** 核心论点：AI agent 有一个 Cold-Start Problem。每个 session 从零开始——不了解你的规范、你的代码库、你的团队的决策。

ECC 彻底消除了冷启动。当 agent 开始 session 时，你的项目标准已经加载好了。

**3/** 四层 memory 架构：
- Session state（工作记忆）
- Diary（情节记忆）
- Instincts（程序性记忆）
- Solutions（机构记忆）

每一层有不同的保留策略。就像人类团队实际存储知识的方式一样。

**4/** 杀手锏特性：Sub-Agent Context Delegation。

不是把所有东西塞进一个 context window，ECC 生成专业 agent——Style Reviewer、Security Reviewer、Logic Reviewer——每个都拥有最小化的、聚焦的上下文。

总分析能力超越了任何单个模型的窗口。

**5/** Continuous Learning v2.1 引入了 instinct——带有置信度分数的行为模式。

好的模式随时间强化。坏的模式弱化并消亡。Agent 真的会基于累积的反馈进行演化。

这就是 AI 人格在规模化场景下的样子。

**6/** 尽管名字如此，ECC 可以跨 Claude、Cursor、Codex、Gemini、Zed 和 GitHub Copilot 工作。

它瞄准的是通用基底：system prompt + 文件配置 + tool calling。你的开发文化变得可以在 AI 供应商之间移植。

**7/** C31 审视了全部 246 个 skill，精确萃取了 3 个机制：
- Context Health 色彩系统（🟢🟡🟠🔴）
- Assumption Tracking 模板
- Instinct Evolution 管道

为什么只有 3 个？因为这些是元技能——它们让其他 skill 变得更好。

**8/** 局限：246 个 skill 真的很多。复杂度天花板为试图学习框架的人类制造了它自己的冷启动问题。

但 ECC 证明了 Agent Harness 可以是操作系统，而不仅仅是配置文件。这个理念改变了下游的一切。
