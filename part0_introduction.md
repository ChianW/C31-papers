# The Agent Harness Papers, Part 0: Why You Need an Agent Harness

*Series: The Agent Harness Papers — 7 frameworks, 1 personal AI operating system, 5 months of production use*

---

![The Agent Harness Papers — Build AI Systems That Don't Break](images/final_hero_v5.jpg)

![Agent Harness Architecture — From Chaos to Structure](images/hero_v2_architecture.jpg)

---

You wrote 1,000 prompts for your AI. Carefully crafted system instructions. Detailed coding guidelines. A beautiful `CLAUDE.md` file with your team's conventions.

Then the conversation hit the context limit. You started a new session.

And the AI forgot everything.

Not just your prompts — your decisions. Your architectural context. The three approaches you already tried and rejected. The naming convention you spent 20 minutes agreeing on. The bug pattern you discovered last Tuesday.

Gone.

This is the fundamental problem with prompt collections: **they are stateless**. Every session starts from zero. The AI never gets smarter. You never compound your experience.

This is the problem that Agent Harness systems solve.

---

## Prompt Collection vs. Agent Harness

A prompt collection gives you one-shot instructions. Copy, paste, use, forget.

An agent harness gives you a **persistent operating layer** between you and your AI tool. It remembers. It learns. It enforces quality. It compounds knowledge.

| | Prompt Collection | Agent Harness |
|---|---|---|
| **Memory** | None — every session is fresh | Persistent — `session_state.json`, diary, instincts |
| **Learning** | Static — same quality forever | Evolving — patterns graduate from candidate → instinct |
| **Quality** | Hope-based — you trust the AI did it right | Gate-based — Critic Gate, review passes, verification evidence |
| **Knowledge** | Lost — solutions disappear with the conversation | Compounded — `docs/solutions/` with searchable INDEX |
| **Context** | Unmanaged — degrades as conversation grows | Monitored — 🟢🟡🟠🔴 four-state health system |

The difference is not incremental. It's architectural.

---

## The Three Pain Points

Every developer who uses AI coding tools daily hits the same three walls:

### 1. No Memory

Your AI doesn't remember that you prefer functional components over class components. It doesn't know you switched from Jest to Vitest last week. It doesn't recall the three-hour debugging session where you discovered your ORM silently swallows connection errors.

Every session, you re-explain. Re-establish context. Re-make decisions you already made.

This isn't a prompt problem. It's a **state persistence** problem.

### 2. No Quality Gates

Your AI writes code that looks correct. Sometimes it is. Sometimes it introduces subtle bugs that pass your initial review because you're tired and the code is plausible.

There's no automated self-review. No Critic Gate that triggers when the AI makes inferred conclusions. No behavioral verification that demands test evidence for every code change. No anti-sycophancy mechanism that prevents the AI from agreeing with your bad ideas just to avoid conflict.

This isn't a capability problem. It's a **process engineering** problem.

### 3. No Knowledge Compounding

You solved a tricky CORS configuration issue on Monday. On Thursday, your teammate hits the same issue. The AI doesn't know. Your solution lives in a Slack thread that nobody will find.

Six months later, you hit the same issue yourself. You don't remember the fix. The AI doesn't either.

Every solved problem should make the next occurrence faster. But without a compounding mechanism, your team's institutional knowledge has a half-life of about two weeks.

This isn't a documentation problem. It's a **knowledge architecture** problem.

---

## The 2026 Consensus

In 2025-2026, seven independent open-source projects attacked these problems from different angles. They had different authors, different philosophies, different star counts. But they converged on the same conclusions:

**1. Architecture beats prompts.**

Reliability doesn't come from better wording. It comes from structure — explicit control flow, quality gates, state management. You don't prompt your way to production-grade agents. You engineer them.

**2. Agents need persistent state.**

Session-scoped memory is not enough. Instincts, learnings, and decisions must survive across conversations. The agent should get smarter over time, not reset to baseline every session.

**3. Multi-agent orchestration beats monolithic agents.**

A specialized small agent that does one thing well outperforms a single agent trying to brainstorm, code, test, review, and document all in one context window. Delegation is not a feature — it's an architectural requirement.

**4. Human-in-the-loop is a first-class operation.**

Irreversible actions need approval gates. This isn't a safety afterthought — it's a core design primitive. The best systems make the boundary between "AI decides autonomously" and "AI pauses for human input" explicit and configurable.

**5. Knowledge compounds — if you capture it.**

The final step of every workflow should leave the system better than it found it. Document the solution. Index it. Make it searchable. The next time the same class of problem appears, it should take minutes, not hours.

These aren't theoretical principles. They emerged from thousands of hours of real-world production use across seven independent teams.

---

## The Seven Frameworks

This series examines each framework in depth — its philosophy, its unique mechanism, its honest limitations, and what it contributed to the broader ecosystem:

| Part | Framework | Stars | The One-Line Pitch |
|------|-----------|-------|-------------------|
| **1** | [12-Factor Agents](https://github.com/humanlayer/12-factor-agents) | ~24k | The architectural manifesto — `f(context) → action` |
| **2** | [Superpowers](https://github.com/obra/superpowers) | ~249k | Cialdini's psychology applied to LLM behavioral compliance |
| **3** | [Everything Claude Code](https://github.com/affaan-m/ecc) | ~225k | The most comprehensive agent operating layer — 246 skills |
| **4** | [Agent Skills](https://github.com/addyosmani/agent-skills) | ~70k | Anti-Rationalization Tables that stop AI from cutting corners |
| **5** | [Compound Engineering](https://github.com/everyinc/compound-engineering-plugin) | ~23k | The "Compound" step — making future work easier, every time |
| **6** | [Archon](https://github.com/coleam00/Archon) | ~23k | YAML-defined workflows — Dockerfiles for AI coding |
| **7** | [GSD Core](https://github.com/open-gsd/gsd-core) | ~6k | Context Rot as a named, first-class engineering problem |

Part 8 compares all seven. Part 9 tells the story of how one developer audited all of them in 48 hours and forged them into a single system called [C31](https://github.com/ChianW/C31).

---

## Who This Series Is For

- **AI-assisted developers** who use Claude Code, Cursor, Gemini CLI, or similar tools daily and want to move beyond one-shot prompting
- **Engineering leads** evaluating how to standardize AI tooling across a team
- **Framework authors** looking for cross-pollination ideas
- **Anyone curious** about what happens when you treat AI not as a chatbot, but as a software system that needs engineering

Let's begin.

→ **[Part 1: 12-Factor Agents — The Architectural Manifesto](part1_12_factor_agents.md)**

---

## Twitter Version

> You wrote 1,000 prompts for your AI. Then the conversation reset. It forgot everything.
>
> This is why you need an Agent Harness — not better prompts, but a persistent operating layer.
>
> I spent 5 months building one. Audited 7 open-source frameworks. Wrote 9 articles about it.
>
> The series starts now: "The Agent Harness Papers" 🧵
>
> Part 1: 12-Factor Agents (the manifesto)
> Part 2: Superpowers (the psychology hack)
> Part 3: ECC (the operating layer)
> Part 4: Agent Skills (the anti-laziness framework)
> Part 5: CEP (the compounding engine)
> Part 6: Archon (the workflow Dockerfile)
> Part 7: GSD Core (the context rot fighter)
> Part 8: All 7 compared
> Part 9: How I built C31 from all of them
>
> github.com/ChianW/C31


---

# 中文版

# Agent Harness 系列文章，第 0 篇：为什么你需要一个 Agent Harness

*系列：The Agent Harness Papers — 7 个框架，1 个个人 AI 操作系统，5 个月的生产环境实战*

---

你为你的 AI 写了 1,000 条提示词。精心打磨的系统指令。详细的编码规范。一份漂漂亮亮的 `CLAUDE.md` 文件，里面写满了团队约定。

然后对话撞上了上下文限制。你开了一个新会话。

AI 什么都忘了。

不只是你的提示词——还有你的决策、你的架构上下文、你已经试过并否决的三种方案、你花了 20 分钟才敲定的命名约定、上周二你发现的那个 bug 模式。

全没了。

这就是提示词集合的根本问题：**它们是无状态的**。每次会话都从零开始。AI 永远不会变聪明。你的经验永远无法复利积累。

这就是 Agent Harness 系统要解决的问题。

---

## 提示词集合 vs. Agent Harness

提示词集合给你的是一次性指令。复制、粘贴、用完、忘掉。

Agent Harness 给你的是一个**持久化的操作层**，架在你和 AI 工具之间。它记得。它学习。它强制质量把关。它让知识复利增长。

| | 提示词集合 | Agent Harness |
|---|---|---|
| **记忆** | 无 — 每次会话都是全新的 | 持久化 — `session_state.json`、日记、本能 |
| **学习** | 静态 — 质量永远不变 | 进化 — 模式从 candidate → instinct 逐步晋级 |
| **质量** | 靠信念 — 你信任 AI 做对了 | 靠门控 — Critic Gate、审查轮次、验证证据 |
| **知识** | 丢失 — 解决方案随对话消失 | 复利积累 — `docs/solutions/` 带可搜索的 INDEX |
| **上下文** | 无管理 — 随对话增长而退化 | 受监控 — 🟢🟡🟠🔴 四级健康状态系统 |

差别不是渐进式的。是架构级的。

---

## 三大痛点

每个日常使用 AI 编码工具的开发者，都会撞上同样三堵墙：

### 1. 没有记忆

你的 AI 不记得你偏好函数式组件而不是类组件。它不知道你上周刚从 Jest 切换到了 Vitest。它不记得你花了三小时调试、最终发现你的 ORM 会默默吞掉连接错误的那次经历。

每次会话，你都在重新解释、重新建立上下文、重新做出你早已做过的决策。

这不是提示词的问题。这是一个**状态持久化**问题。

### 2. 没有质量门控

你的 AI 写出来的代码看起来是对的。有时确实对了。有时它引入了微妙的 bug，通过了你的初步审查——因为你累了，而代码看起来很合理。

没有自动化的自我审查。没有 Critic Gate 在 AI 做出推断性结论时触发。没有行为验证机制要求每次代码变更都必须有测试证据。没有反谄媚机制阻止 AI 为了避免冲突而附和你的坏主意。

这不是能力问题。这是一个**流程工程**问题。

### 3. 没有知识复利

你在周一解决了一个棘手的 CORS 配置问题。周四，你的队友遇到了同样的问题。AI 不知道。你的解决方案躺在一条没人会找到的 Slack 消息里。

六个月后，你自己又遇到了同样的问题。你不记得怎么修了。AI 也不记得。

每个已解决的问题都应该让下一次遇到时更快。但没有复利机制，你团队的机构知识半衰期大约只有两周。

这不是文档问题。这是一个**知识架构**问题。

---

## 2026 年的共识

在 2025-2026 年间，七个独立的开源项目从不同角度攻克了这些问题。它们有不同的作者、不同的哲学、不同的 star 数。但它们得出了相同的结论：

**1. 架构胜过提示词。**

可靠性不来自更好的措辞。它来自结构——显式控制流、质量门控、状态管理。你不可能靠堆提示词堆出生产级的 agent。你需要工程化地构建它们。

**2. Agent 需要持久化状态。**

会话级的记忆不够。本能、学习成果和决策必须跨对话存活。Agent 应该随时间变得更聪明，而不是每次会话都重置到初始水平。

**3. 多 Agent 编排胜过单体 Agent。**

一个做好一件事的小型专业 Agent，胜过一个试图在同一个上下文窗口里同时做头脑风暴、编码、测试、审查和文档的大而全 Agent。委托不是一个功能——它是一个架构需求。

**4. Human-in-the-loop 是一等公民操作。**

不可逆操作需要审批门控。这不是安全层面的事后补救——它是核心设计原语。最好的系统会将「AI 自主决策」和「AI 暂停等待人类输入」之间的边界做得明确且可配置。

**5. 知识会复利——如果你去捕获它的话。**

每个工作流的最后一步，都应该让系统比开始时更好。记录解决方案。索引它。让它可搜索。下次同类问题出现时，应该只需几分钟，而不是几小时。

这些不是理论原则。它们从七个独立团队数千小时的真实生产使用中浮现。

---

## 七大框架

本系列深入剖析每个框架——它的哲学、独特机制、诚实的局限性，以及它对更广泛生态系统的贡献：

| 篇章 | 框架 | Stars | 一句话卖点 |
|------|-----------|-------|-------------------|
| **1** | [12-Factor Agents](https://github.com/humanlayer/12-factor-agents) | ~24k | 架构宣言 — `f(context) → action` |
| **2** | [Superpowers](https://github.com/obra/superpowers) | ~249k | 将西奥迪尼的心理学应用于 LLM 行为合规 |
| **3** | [Everything Claude Code](https://github.com/affaan-m/ecc) | ~225k | 最全面的 Agent 操作层 — 246 个技能 |
| **4** | [Agent Skills](https://github.com/addyosmani/agent-skills) | ~70k | 反合理化表格，阻止 AI 偷工减料 |
| **5** | [Compound Engineering](https://github.com/everyinc/compound-engineering-plugin) | ~23k | 「复利」步骤 — 每次都让未来的工作更轻松 |
| **6** | [Archon](https://github.com/coleam00/Archon) | ~23k | YAML 定义的工作流 — AI 编码的 Dockerfile |
| **7** | [GSD Core](https://github.com/open-gsd/gsd-core) | ~6k | 将 Context Rot 命名为一等工程问题 |

第 8 篇对比全部七个框架。第 9 篇讲述一个开发者如何在 48 小时内审计了所有框架，并将它们锻造成一个叫 [C31](https://github.com/ChianW/C31) 的统一系统。

---

## 本系列适合谁

- **AI 辅助开发者**：每天使用 Claude Code、Cursor、Gemini CLI 或类似工具，想要超越一次性提示词模式
- **工程负责人**：正在评估如何在团队中标准化 AI 工具链
- **框架作者**：寻找跨项目交叉借鉴的灵感
- **任何好奇的人**：想知道当你不把 AI 当聊天机器人，而是当作一个需要工程化的软件系统时，会发生什么

让我们开始。

→ **[第 1 篇：12-Factor Agents — 架构宣言](part1_12_factor_agents.md)**

---

## Twitter 版

> 你为你的 AI 写了 1,000 条提示词。然后对话重置了。它什么都忘了。
>
> 这就是为什么你需要一个 Agent Harness — 不是更好的提示词，而是一个持久化的操作层。
>
> 我花了 5 个月构建了一个。审计了 7 个开源框架。写了 9 篇文章来讲述它。
>
> 系列现在开始：「The Agent Harness Papers」🧵
>
> 第 1 篇：12-Factor Agents（架构宣言）
> 第 2 篇：Superpowers（心理学黑客）
> 第 3 篇：ECC（操作层）
> 第 4 篇：Agent Skills（反偷懒框架）
> 第 5 篇：CEP（复利引擎）
> 第 6 篇：Archon（工作流 Dockerfile）
> 第 7 篇：GSD Core（上下文腐化战士）
> 第 8 篇：全部 7 个对比
> 第 9 篇：我如何从它们中构建出 C31
>
> github.com/ChianW/C31
