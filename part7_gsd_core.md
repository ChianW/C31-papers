# The Agent Harness Papers, Part 7: GSD Core — The Last Line of Defense Against Context Rot

*Series: The Agent Harness Papers — 7 frameworks, 1 personal AI operating system, 5 months of production use*

---

![Context Rot — AI Output Quality Over Conversation Length](images/final_part7_context_rot.jpg)

---

Have you noticed that AI gets dumber the longer you talk to it?

Not gradually. Not gracefully. It's more like a slow leak. After an hour of complex work, the AI starts contradicting decisions it made 20 minutes ago. It forgets the naming convention you agreed on. It proposes approaches you explicitly rejected earlier. It hallucinates function names that don't exist — or worse, function names that *used to* exist before you refactored them out.

This isn't a model bug. It isn't a capability limitation. It's a structural property of how transformers process attention over long sequences.

GSD Core is the only framework that treats this as its **central design problem**. And it gives the problem a name: **Context Rot**.

---

## Naming the Enemy

Context Rot is the progressive degradation of AI output quality as the context window fills up. The mechanism is well-understood:

1. **Attention diffusion**: Transformer attention spreads across all tokens. As the context grows, attention per-token decreases. Early instructions get less attention weight.
2. **Recency bias**: The model disproportionately attends to recent tokens. Your carefully crafted system prompt from 50,000 tokens ago? It's background noise now.
3. **Contradictory accumulation**: Long conversations accumulate corrections, changes of direction, and updated decisions. The model sees all of them — including the outdated ones — and can't reliably distinguish "current truth" from "superseded decision."

The result: after enough back-and-forth, your AI assistant is working from a corrupted understanding of your project. It's not lying. It's not lazy. It's literally losing access to the context that would make it effective.

Most developers respond to context rot the same way: `/clear`. Start over. Re-establish context. Lose everything from the previous session.

GSD Core's thesis: this is the wrong response. The right response is **architectural**.

---

## The .planning/ Directory

GSD Core's most concrete contribution is a directory pattern: `.planning/`.

```
my-project/
├── .planning/
│   ├── spec.md           ← what we're building (survives /clear)
│   ├── plan.md           ← how we're building it (survives /clear)  
│   ├── progress.md       ← what's done, what's next (survives /clear)
│   ├── decisions.md      ← architectural decisions log
│   └── concerns.md       ← known risks and open questions
├── src/
├── tests/
└── package.json
```

The insight is deceptively simple: **conversation history is volatile; files are persistent.**

When you `/clear` a conversation, the chat history disappears. But `.planning/spec.md` remains. When a new session starts, the AI reads the spec, plan, and progress log, then resumes work from *exactly* where you left off.

No re-explaining. No context rebuilding. No re-making decisions.

### What Goes in .planning/

GSD Core is prescriptive about what deserves to be persisted:

| File | Contents | Update Frequency |
|------|---------|-----------------|
| `spec.md` | Requirements, acceptance criteria, scope boundaries | Set once, rarely changed |
| `plan.md` | Implementation approach, component breakdown, dependencies | Updated when approach changes |
| `progress.md` | Completed items, current work, blockers | Updated after each work unit |
| `decisions.md` | Architectural decisions and their rationale | Append-only |
| `concerns.md` | Risks, open questions, unresolved tradeoffs | Updated as discovered |

The most important file is `progress.md`. It's the **recovery point**. After `/clear`, after a crash, after sleeping — `progress.md` tells the next session exactly where the work stands.

---

## Artifacts Over Memory

GSD Core's second principle is behavioral:

> **Write results to files, not to conversation history.**

This sounds obvious. It isn't. The default behavior for most developers (and most AI agents) is to keep results in the conversation:

```
User: "Can you analyze the performance bottlenecks?"
AI: "Here are the 5 bottlenecks I found: [detailed analysis]"
User: "Good. Now fix the top 3."
AI: [fixes]
```

The analysis exists in the conversation. After `/clear`, it's gone. Open a new session tomorrow — that analysis doesn't exist.

GSD Core's alternative:

```
User: "Can you analyze the performance bottlenecks?"
AI: [writes analysis to .planning/analysis-perf.md]
    "I've saved the analysis to .planning/analysis-perf.md. 
     Here's a summary: [brief overview]"
User: "Good. Now fix the top 3."
AI: [reads .planning/analysis-perf.md, fixes]
```

The analysis persists. The next session can reference it. The next developer can read it. The next AI can build on it.

**Files are permanent records. Conversations are ephemeral.**

In retrospect, this principle seems obvious. But look at your actual AI coding sessions. How much critical information lives only in the chat? How much have you lost?

---

## Fresh-Context Subagent Delegation

GSD Core's third mechanism attacks Context Rot directly: **delegate heavy work to subagents with fresh context windows.**

The pattern:

1. **Main session** handles coordination, planning, and lightweight decisions
2. **Heavy research, implementation, or analysis** is delegated to a subagent
3. Subagent starts with a fresh context window — no Context Rot
4. Subagent results are written to files (Artifacts Over Memory)
5. Main session reads the files and continues

```
Main Session (context: 60% full, yellowing)
│
├── "Research the 3 authentication libraries"
│   └── Subagent A (fresh context) → writes .planning/auth-research.md
│
├── "Implement option B from the research"  
│   └── Subagent B (fresh context) → writes code + tests
│
└── "Review the implementation"
    └── Main session reads code, provides lightweight review
```

The main session stays lean. Heavy work gets done in clean context. Files act as the bridge.

This is particularly effective for long-running projects. Instead of a single conversation that runs for 4 hours and gradually rots, you have a coordinator that delegates to fresh executors. The coordinator's context stays manageable. The executors start from zero.

---

## The Five-Phase Cycle

GSD Core organizes work into five phases:

```
Discuss → Plan → Execute → Verify → Ship
```

Each phase has explicit entry/exit criteria:

| Phase | Entry Condition | Exit Condition |
|-------|----------------|----------------|
| **Discuss** | User has a vague idea | Requirements are clear enough to plan |
| **Plan** | Requirements are clear | Implementation plan written to `.planning/plan.md` |
| **Execute** | Plan exists | Code written and compiling |
| **Verify** | Code compiles | Tests passing, review done |
| **Ship** | Tests passing | Changes merged/deployed |

The phases are **enforced**. You can't skip Discuss and go straight to Execute. You can't skip Verify and go straight to Ship. Each phase gate prevents the AI from shortcutting.

This isn't as novel as GSD Core's context management — most frameworks have some version of a phase cycle. But GSD Core's contribution is binding the phases **to .planning/ artifacts**: each phase updates specific files, and those files themselves constitute the phase gate (no `plan.md` = can't enter Execute).

---

## What C31 Takes From GSD Core

GSD Core's influence on C31 is pervasive:

### Context Health 🟢🟡🟠🔴

GSD Core named the problem (Context Rot) and pointed toward the solution (proactive monitoring). C31 merged this with ECC's color system to create four-state monitoring:

| Status | Threshold | Automatic Behavior |
|--------|-----------|-------------------|
| 🟢 Green | <50% | Normal operation |
| 🟡 Yellow | 50–70% | Start compressing completed work into summaries |
| 🟠 Orange | 70–85% | Active compression: move decisions to files, archive assumptions |
| 🔴 Red | >85% | Forced checkpoint: write all state to files before continuing |

The key innovation is **behavior that automatically senses degradation**. The AI doesn't wait for the developer to notice quality declining. It proactively monitors its own context utilization and takes corrective action.

### Artifacts Over Memory

This became a core C31 principle. Session results go into `session_state.json`. Instinct evolution goes into `instincts/`. Diary entries go into `diary/`. Solutions go into `docs/solutions/`.

Conversations are ephemeral. The filesystem is truth.

### Plan Quality Gate

Before spawning a subagent, C31 checks three conditions (adapted from GSD Core):

```
- [ ] Can a brand-new agent execute this plan without additional questions?
- [ ] Does each step have clear verification criteria (definition of done)?
- [ ] Is the context payload minimized (no unnecessary information)?
```

If any check fails, the plan needs more work before it can be delegated.

---

## Honest Limitations

### 1. Smallest Community

~6,200 stars. Compare to Superpowers (249k) or ECC (225k). GSD Core's ideas are valuable, but the adoption surface is limited. Fewer users means fewer battle-tested case studies, fewer community-contributed improvements, less ecosystem support.

### 2. No Knowledge Compounding

Like Archon, GSD Core has no Compound step. `.planning/` persists project state, but it doesn't build institutional knowledge. Solved problems aren't indexed for future reference. There's no equivalent to a `docs/solutions/` directory.

### 3. .planning/ Doesn't Scale to Teams

`.planning/` works beautifully for individual developers. For teams, it raises questions: who owns `plan.md`? What happens when two developers update `progress.md` simultaneously? GSD Core doesn't address multi-developer workflows.

### 4. Context Monitoring is Approximate

The 🟢🟡🟠🔴 thresholds are heuristics, not precise measurements. LLMs don't expose "context utilization" as a queryable metric. Monitoring is based on message counts, token estimates, and behavioral signals — useful but imprecise.

### 5. Fresh-Context Delegation Has Overhead

Spawning a subagent with a fresh context requires re-establishing project context. The subagent reads `.planning/` files, but it still needs to understand the codebase. That startup time isn't free. For small tasks, delegation overhead might exceed the cost of Context Rot.

---

## The Underrated Framework

GSD Core has the fewest stars of any framework in this series. But it arguably has **the most universally applicable insight**. Context Rot affects *every* developer using *any* AI tool, in *every* session longer than 30 minutes.

The `.planning/` pattern costs zero to implement. No npm install. No framework dependencies. Create a directory. Put your spec in it. Update your progress log. Done.

That this simple idea — files outlast conversations — needed to be formalized as a principle says something about how poorly we collectively think about persistence. We treat AI sessions as stateless interactions when they should be treated as **chapters in an ongoing process**.

GSD Core named the disease and prescribed the simplest possible cure.

---

*Next: [Part 8 — The Ultimate Framework Comparison](part8_comparison.md)*

---

## 🧵 Twitter Thread

**1/** Have you noticed AI gets dumber the longer you talk to it?

It's not a bug. It's Context Rot — the structural degradation of AI output quality as the context window fills.

GSD Core is the only framework that treats this as its *central* design problem. 🧵

**2/** Context Rot mechanism:
- Transformer attention spreads thinner as context grows
- Your system prompt from 50k tokens ago? Background noise
- Accumulated corrections + outdated decisions = corrupted understanding

Most dev response: `/clear`. GSD Core says: wrong direction.

**3/** The fix: `.planning/` directory
```
├── spec.md     ← survives /clear
├── plan.md     ← survives /clear
├── progress.md ← survives /clear
```
Conversations are ephemeral. Files are permanent.
When you /clear, nothing is lost.

**4/** Core principle: Artifacts Over Memory
Write results to files, not to conversation history.

Look at your actual AI sessions. How much critical info lives only in chat? How much have you already lost?

**5/** Fresh-context subagent delegation:
- Main session: coordination (stays lean)
- Heavy work → subagent (fresh context)
- Results → written to files

Your coordinator never rots. Your executors start from zero.

**6/** C31 took from GSD Core:
- Context Health 🟢🟡🟠🔴 monitoring
- Artifacts Over Memory as architectural law
- Plan Quality Gate (3 checks before spawning subagents)

6k stars. Smallest framework. Most universal insight.
github.com/open-gsd/gsd-core

---

# 中文版

# Agent Harness 系列文章，第七篇：GSD Core —— 对抗 Context Rot 的最后防线

*系列：Agent Harness 论文集 —— 7 个框架，1 套个人 AI 操作系统，5 个月生产环境实战*

---

你有没有发现，跟 AI 聊得越久，它就越蠢？

不是渐渐变笨。不是优雅地退化。更像是一种缓慢的泄漏。经过一小时复杂的工作，AI 开始自相矛盾——推翻它 20 分钟前刚做的决定。它忘了你们约定好的命名规范。它提出你明确拒绝过的方案。它开始幻觉出不存在的函数名——或者更糟，幻觉出那些*曾经存在*但你在重构时已经删掉的函数名。

这不是模型的 bug。这不是能力限制。这是 transformer 在处理长序列注意力时的**结构性特征**。

GSD Core 是唯一一个把这件事当作**核心设计问题**来对待的框架。它还给这个问题起了个名字：**Context Rot（上下文腐化）**。

---

## 命名敌人

Context Rot 是指随着上下文窗口逐渐填满，AI 输出质量逐步退化的过程。其机制已被充分理解：

1. **注意力扩散**：Transformer 的注意力分布在所有 token 上。随着上下文增长，每个 token 获得的注意力权重下降。早期指令获得的关注越来越少。
2. **近因偏差**：模型会不成比例地关注近期 token。你 50,000 token 之前精心编写的 system prompt？现在只是背景噪音。
3. **矛盾堆积**：长对话中会不断积累修正、方向变化和决策更新。模型看到所有这些——包括已过时的版本——却无法可靠地区分"当前事实"和"已被取代的决定"。

结果就是：经过足够多的来回之后，你的 AI 助手基于一个**被污染的理解**在工作。它不是在说谎。它不是在偷懒。它确确实实在失去让它保持高效所需的上下文。

大多数开发者面对 Context Rot 的反应都一样：`/clear`。重新开始。重建上下文。丢失上一轮的所有内容。

GSD Core 的论点是：这是错误的应对方式。正确的应对方式应该是**架构层面的**。

---

## .planning/ 目录

GSD Core 最具体的贡献是一个目录模式：`.planning/`。

```
my-project/
├── .planning/
│   ├── spec.md           ← what we're building (survives /clear)
│   ├── plan.md           ← how we're building it (survives /clear)  
│   ├── progress.md       ← what's done, what's next (survives /clear)
│   ├── decisions.md      ← architectural decisions log
│   └── concerns.md       ← known risks and open questions
├── src/
├── tests/
└── package.json
```

这个洞察看似简单，实则深刻：**对话历史是易失的；文件是持久的。**

当你 `/clear` 一个对话时，聊天记录就消失了。但 `.planning/spec.md` 留了下来。当新的会话开始时，AI 读取 spec、plan、progress log，然后从你上次离开的地方**精确**地接续工作。不需要重新解释。不需要重建上下文。不需要重新做决定。

---

## Artifacts Over Memory

GSD Core 的第二条原则是一个行为准则：

> **把结果写进文件，而不是对话历史。**

最关键的文件是 `progress.md`。它是**恢复点**。在 `/clear` 之后，在崩溃之后，在睡了一夜之后——`progress.md` 告诉下一个会话工作到底处于什么状态。

---

## Fresh-Context Subagent 委派

GSD Core 的第三个机制直接针对 Context Rot：**将重型工作委派给拥有全新上下文窗口的 subagent。**

主会话保持精简。重活在干净的上下文中完成。文件充当桥梁。这在长期运行的项目中尤其有效——你不再需要一个持续 4 小时并逐渐腐化的单一对话，而是用一个协调者委派给全新的执行者。

---

## C31 从 GSD Core 吸收了什么

### Context Health 🟢🟡🟠🔴

| 状态 | 阈值 | 自动行为 |
|-------|-----------|-----------------|
| 🟢 Green | <50% | 正常运行 |
| 🟡 Yellow | 50–70% | 开始将已完成的工作压缩为摘要 |
| 🟠 Orange | 70–85% | 主动压缩：将决策移入文件，归档假设 |
| 🔴 Red | >85% | 强制 checkpoint：先将所有状态写入文件，再继续 |

### Artifacts Over Memory

这成为 C31 的核心原则。会话结果存入 `session_state.json`，Instinct 演化存入 `instincts/`，解决方案存入 `docs/solutions/`。对话是临时的，文件系统才是真相。

### Plan Quality Gate

在启动 subagent 之前，C31 检查三个条件：
- 全新 agent 能否无需追问地执行此计划？
- 每个步骤是否有明确的验收标准？
- Context payload 是否最小化？

---

## 坦诚的局限性

1. **最小的社区**：约 6,200 stars，实战案例有限。
2. **没有 Knowledge Compounding**：执行项目状态但不构建机构知识。
3. **.planning/ 无法扩展到团队**：多开发者协作场景未被解决。
4. **Context 监控是近似的**：阈值是启发式的，不是精确测量。
5. **Fresh-Context 委派有开销**：小任务中委派成本可能超过收益。

---

## 被低估的框架

GSD Core 是本系列所有框架中 star 数最少的。但它拥有的，可以说是**最具普遍适用性的洞察**。Context Rot 影响着使用*任何* AI 工具的*每一位*开发者，在*每一个*超过 30 分钟的会话中。

`.planning/` 模式的实施成本为零。不需要 npm install。不需要框架依赖。创建一个目录。把你的 spec 放进去。更新你的 progress log。搞定。

GSD Core 命名了这种疾病，并开出了最简单的药方。

---

*下一篇：[第八篇 —— 七大框架终极对比](part8_comparison.md)*

---

## 推特线程

```
1/ 🧵 GSD Core：最被低估的 AI Agent 框架

你有没有发现跟 AI 聊越久它就越蠢？
这不是 bug。这是 Context Rot（上下文腐化）。
GSD Core 是唯一把这个当核心问题来解决的框架。👇

2/ Context Rot = 随着上下文填满，输出质量逐步退化。
机制：transformer 的注意力在长序列上被稀释。
你 5 万 token 之前写的 system prompt？现在只是背景噪音。
大多数开发者的应对方式是 /clear。GSD Core 说：方向错了。

3/ 正确的做法：.planning/ 目录
├── spec.md     ← /clear 之后还在
├── plan.md     ← /clear 之后还在
├── progress.md ← /clear 之后还在
对话是临时的。文件是持久的。你 /clear 的时候，什么都不会丢失。

4/ 核心原则：Artifacts Over Memory
把结果写进文件，而不是对话历史。
观察一下你自己的 AI 会话。有多少关键信息只活在聊天记录里？

5/ Fresh-context subagent 委派：
主会话负责协调（保持精简）。
重活 → 委派给 subagent（全新上下文）。
结果 → 写入文件。
你的协调者永远不会腐化。你的执行者从零开始。

6/ C31 从 GSD Core 吸收了：
• Context Health 🟢🟡🟠🔴 监控
• Artifacts Over Memory 作为架构铁律
• Plan Quality Gate（启动 subagent 前的 3 项检查）
6k stars。最小的框架。最具普遍性的洞察。
github.com/open-gsd/gsd-core
```
