# Agent Harness Papers, Part 1: 12-Factor Agents

**The design manifesto that treats AI agents as software systems — not magic.**

![12-Factor Agents — Design Principles](images/final_part1_12factor.jpg)

---

In 2011, Adam Wiggins published *The Twelve-Factor App* while working at Heroku. It wasn't a framework. It wasn't a library you could `npm install`. It was a set of opinions — twelve of them — about how web applications should be built if you wanted them to survive contact with production. It became the backbone of cloud-native thinking. Every PaaS, every container orchestrator, every "serverless" pitch deck since then has been, in some way, a response to those twelve factors.

In 2025, Dex Horthy did the same thing for AI agents.

His project, [12-Factor Agents](https://github.com/humanlayer/12-factor-agents), sits at ~24k GitHub stars. There is no package to install. No SDK. No `pip install twelve-factor-agents`. It's a design manifesto — a set of hard-won opinions about why your agent keeps failing in production, and what to do about it.

This is Part 1 of the **Agent Harness Papers** series. We're starting here because everything else in the series — the frameworks, the orchestrators, the evaluation harnesses — exists in the gravitational field of this document. You can't evaluate what you haven't designed well. And right now, most agents are not designed well.

---

## The Origin: 100 Founders and a Pattern of Failure

Horthy didn't write these factors in a vacuum. As the founder of [HumanLayer](https://humanlayer.dev) — a platform for human-in-the-loop AI workflows — he had a front-row seat to how production agents actually fail. He interviewed over 100 SaaS founders who were building agents for real businesses: customer support, sales automation, document processing, internal tooling.

The pattern was consistent. The failure mode wasn't what you'd expect.

> Agents don't fail because the LLM is too dumb. They fail because developers treat the LLM as the control plane instead of a component.

The founders weren't struggling with prompt engineering. They weren't blocked on model capability. They were drowning in the *software engineering* problems that surround the model:

- State management across multi-step workflows
- Error recovery when a tool call fails on step 7 of 12
- Context windows ballooning with irrelevant conversation history
- No way to pause a workflow, get human approval, and resume
- Agents that worked in demos but hallucinated their way through edge cases in production

These aren't AI problems. These are software architecture problems. And Horthy's core insight was deceptively simple:

**Stop building "autonomous agents." Start building software systems that use LLMs for specific decisions.**

---

## The Core Insight: LLM as Router, Not Autopilot

The dominant mental model in 2024 was the *agentic loop*: give the LLM a goal, some tools, and let it figure out the steps. AutoGPT, BabyAGI, and their descendants all followed this pattern. The LLM plans. The LLM decides what to do next. The LLM is the control flow.

Horthy's manifesto inverts this. In 12-Factor Agents, the LLM's job is narrow and specific:

**Convert natural language into structured tool calls.**

That's Factor 1, and it's the foundation everything else builds on. The LLM doesn't decide *what to do* — your code decides what to do. The LLM decides *how to express* a decision in structured output. It's a router, not a pilot.

This is a philosophical shift with enormous practical consequences. When the LLM is the autopilot, every failure is an AI problem — inscrutable, non-reproducible, and fixed by "better prompting." When the LLM is a router inside a well-architected system, failures become software bugs — traceable, testable, and fixable with regular engineering.

---

## The 12 Factors at a Glance

Before we deep-dive into three critical factors, here's the complete map:

| # | Factor | One-Liner |
|---|--------|-----------|
| 1 | Natural Language → Tool Calls | The LLM's job is to convert intent into structured actions. |
| 2 | Own Your Prompts | No framework-injected system prompts. You write every token. |
| 3 | Own Your Context Window | Curate what goes in. Don't just append. |
| 4 | Tools Are Just Structured Outputs | A "tool call" is just JSON. The execution is your code. |
| 5 | Unify Execution and Business State | Your agent's state *is* your application state. One source of truth. |
| 6 | Launch/Pause/Resume | Agents must support interruption as a first-class operation. |
| 7 | Contact Humans with Tool Calls | HITL isn't an escape hatch — it's a tool the agent can invoke. |
| 8 | Own Your Control Flow | Explicit state machines, not open-ended loops. |
| 9 | Compact Errors into Context | Errors aren't fatal — they're information for the next step. |
| 10 | Small, Focused Agents | 3–10 steps. If it's bigger, decompose. |
| 11 | Trigger from Anywhere | Webhooks, cron, API calls, Slack — the entry point shouldn't matter. |
| 12 | Agent as Stateless Reducer | `reduce(state, event) → new_state`. Pure function. No hidden state. |
| 13 | Pre-fetch Context *(bonus)* | Gather relevant data before the LLM sees the prompt. |

Each factor is a constraint. And like the best constraints, they're liberating once you accept them. Let's go deep on the three that matter most.

---

## Deep Dive: Factor 3 — Own Your Context Window

This is where most agent architectures silently rot.

The default behavior in every agent framework is *append*. User says something → append to messages. Tool returns something → append to messages. Error happens → append to messages. The context window grows monotonically until you hit the token limit, at which point the framework either truncates from the front (losing critical instructions) or summarizes (losing critical details).

Factor 3 says: **stop appending. Start curating.**

Your context window is not a conversation log. It's a *working memory* that you engineer for each LLM call. You decide what goes in, what gets compressed, and what gets evicted. The metaphor isn't a chat history — it's a surgical briefing.

### What This Looks Like in Practice

Instead of:

```
messages = [system_prompt] + all_previous_messages + [new_user_message]
```

You build each context window deliberately:

```python
def build_context(state, current_step):
    return [
        system_prompt_for_step(current_step),
        relevant_state_summary(state),
        recent_errors_if_any(state),
        current_task_description(state),
        available_tools_for_step(current_step),
    ]
```

Notice what's missing: all the intermediate chatter. The "Okay, I'll do that" responses. The tool calls from step 2 that are irrelevant to step 7. The verbose error stack traces that have already been parsed and compacted.

### Context Health: A Color System

In production systems that adopt this factor, context health becomes a monitorable metric:

| Status | Threshold | Behavior |
|--------|-----------|----------|
| 🟢 Green | <50% capacity | Normal operation |
| 🟡 Yellow | 50–70% | Begin compressing completed work into summaries |
| 🟠 Orange | 70–85% | Active compression: move decisions to external state, archive assumptions |
| 🔴 Red | >85% | Force checkpoint: serialize state to storage, then continue with fresh context |

This isn't theoretical. The C31 engineering system (which we'll cover later in this series) implements exactly this color system as an operational protocol. When context hits orange, agents proactively externalize their working state — not because they ran out of tokens, but because *attention quality degrades long before the hard limit*.

### Why This Factor Is Hard

Because it requires you to understand your domain deeply enough to know what's relevant at each step. You can't just throw everything at the LLM and hope attention mechanisms sort it out. You need to make editorial decisions about information hierarchy. That's engineering work, not prompt engineering.

---

## Deep Dive: Factor 8 — Own Your Control Flow

This is the factor that most directly attacks the "agentic loop" paradigm.

In a typical agent framework, control flow looks like this:

```
while not done:
    response = llm.chat(messages)
    if response.has_tool_calls():
        results = execute_tools(response.tool_calls)
        messages.append(results)
    else:
        done = True
```

It's elegant. It's general. And it's a production nightmare.

The problem is that `while not done` is doing all the work, and the LLM is making all the decisions about when to stop, what to do next, and how to recover from errors. You've abdicated control flow to a stochastic system. Your agent's behavior is now *emergent* rather than *designed*.

Factor 8 says: **make the control flow explicit.** Use state machines. Use directed graphs. Use plain `if/else` statements. Whatever makes the transitions visible and testable.

### The State Machine Alternative

```python
class AgentState(Enum):
    GATHERING_REQUIREMENTS = "gathering_requirements"
    ANALYZING_DATA = "analyzing_data"
    GENERATING_REPORT = "generating_report"
    AWAITING_APPROVAL = "awaiting_approval"
    EXECUTING_ACTION = "executing_action"
    DONE = "done"

def step(state: AgentState, context: dict) -> AgentState:
    match state:
        case AgentState.GATHERING_REQUIREMENTS:
            # LLM extracts structured requirements from user input
            requirements = llm_extract_requirements(context)
            context["requirements"] = requirements
            return AgentState.ANALYZING_DATA

        case AgentState.ANALYZING_DATA:
            # Deterministic code fetches and processes data
            data = fetch_data(context["requirements"])
            context["analysis"] = analyze(data)
            return AgentState.GENERATING_REPORT

        case AgentState.AWAITING_APPROVAL:
            # Agent pauses. Resumes when human approves.
            if context.get("approved"):
                return AgentState.EXECUTING_ACTION
            return AgentState.AWAITING_APPROVAL  # stay paused

        # ... and so on
```

Look at what changed:

1. **Every possible state is enumerated.** No surprise emergent states.
2. **Transitions are explicit.** You can draw a diagram of this system. You can test each transition independently.
3. **The LLM is used surgically.** It extracts requirements in one step. It generates a report in another. It doesn't decide the overall flow.
4. **Pausing is trivial.** `AWAITING_APPROVAL` is just a state. Serialize the context, shut down the process, resume when the human responds. No WebSocket connections to maintain, no long-running processes to babysit.

### The 3–10 Step Heuristic

Factor 10 reinforces this: if your agent needs more than 10 steps, it's too big. Break it into smaller agents that each own a sub-workflow. This isn't about capability — modern LLMs can handle complex reasoning. It's about *debuggability*. When your 47-step agent fails on step 31, good luck figuring out what went wrong. When your 7-step agent fails on step 4, the blast radius is contained and the fix is obvious.

---

## Deep Dive: Factor 12 — Agent as Stateless Reducer

This is the architectural capstone. If you accept only one factor, make it this one.

```
new_state = reduce(current_state, event)
```

Your agent is a pure function. It takes the current state and an event (user message, tool result, timer tick, webhook payload), and it returns a new state. No hidden variables. No in-memory caches that survive between invocations. No global `conversation_history` list that accumulates across calls.

### Why Stateless?

Three reasons, all of them operational:

**1. Reproducibility.** If `reduce(state, event)` is pure, you can replay any failure by feeding the same state and event. This is impossible with stateful agents because the failure depends on the accumulated memory of every previous interaction.

**2. Scalability.** Stateless reducers can run anywhere. Spin up a new container, feed it the state, get the result. No sticky sessions. No affinity requirements. No "this agent must run on the same machine because it has in-memory state."

**3. Durability.** When you serialize state between every step, you get free crash recovery. The process dies on step 5? No problem — read the state from storage, re-dispatch the event. The work already done isn't lost because it was persisted as state, not held in memory.

### What State Looks Like

```json
{
  "agent_id": "invoice-processor-abc123",
  "current_step": "awaiting_approval",
  "created_at": "2025-06-15T10:30:00Z",
  "context": {
    "invoice_id": "INV-2025-0847",
    "extracted_fields": {
      "vendor": "Acme Corp",
      "amount": 14250.00,
      "due_date": "2025-07-15"
    },
    "validation_results": {
      "vendor_match": true,
      "amount_within_policy": true,
      "duplicate_check": "clear"
    }
  },
  "pending_action": "human_approval",
  "error_log": []
}
```

This is the entire truth of the agent. No hidden state. No in-memory conversation buffer. A fresh process can pick this up and know exactly what to do next. This is what Horthy means by "Agent as Stateless Reducer" — the agent is a function, the state is a document, and every execution is a fresh reduction.

### The Anti-Pattern: "I Remember You Said..."

If your agent ever says "I remember from earlier that you preferred X" and that "memory" comes from anything other than serialized external state — you have hidden state. And hidden state is where production bugs go to hide.

---

## C31 Integration: How a Real System Adopted These Principles

The 12 Factors are abstract by design — they're principles, not implementations. The C31 engineering system (a compound engineering framework used in production by several teams) provides a concrete case study of how these factors translate into operational protocols.

### F3 → Context Health Color System

C31 implements Factor 3 as a literal traffic-light monitoring system for context windows. Agents track their context utilization in real-time and trigger automated compression behaviors at each threshold. At 🟡 Yellow, completed work is compressed into one-line summaries. At 🟠 Orange, decisions are externalized to files. At 🔴 Red, a full checkpoint is forced — state is serialized to `session_state.json` before proceeding.

This is F3 operationalized: context isn't just curated, it's *governed*.

### F5 → Session State Writeback

C31's `session_state.json` is the single source of truth for cross-session continuity. When a skill (a reusable workflow unit) completes execution, its results — document paths, modified files, new knowledge artifacts — are written back to `session_state.json` under `completed_tasks`. This directly implements Factor 5's unification of execution and business state: the agent's progress *is* the application state, and it lives in one place.

```json
{
  "last_topic": "refactoring auth module",
  "open_todos": ["update API docs", "add integration test for edge case"],
  "completed_tasks": [
    {
      "skill": "ce-compound",
      "output_path": "docs/solutions/auth/token-refresh-fix.md",
      "files_modified": ["src/auth/refresh.ts", "tests/auth/refresh.test.ts"]
    }
  ],
  "last_flush": "2025-06-15T14:30:00Z"
}
```

### F7 → Decision Boundary Structured Pause

Factor 7 says human contact should be a tool call, not an escape hatch. C31 goes further with its **Decision Boundary** protocol: every action is classified as either *Execution Layer* (reversible within 10 minutes — agent proceeds autonomously) or *Decision Layer* (irreversible or high-impact — agent pauses for human confirmation).

The pause isn't a vague "are you sure?" dialog. It's structured:

```
⏸️ [Type: File Overwrite] [Impact: Will replace production config] [Need: Confirm/Modify/Cancel]
```

This is HITL as a first-class citizen — not an afterthought bolted onto an autonomous loop, but a designed interaction pattern with clear triggers and structured output.

### F12 → Stateless Reducer as Architectural Law

C31 treats F12 as its root architectural principle. Every agent execution is `f(context) → action` with no hidden state. Cross-session continuity comes exclusively from `session_state.json` and external files. Subagents receive only the minimum context needed for their task. Any agent can be restarted from its serialized state and produce the same result.

The anti-pattern is explicitly called out: if the agent says "I remember you said earlier that..." and that memory isn't sourced from an external state file, it's unreliable and should be treated as such.

---

## Limitations and Honest Critiques

No manifesto is complete without its blind spots, and the 12-Factor Agents design has several worth acknowledging:

### 1. The State Machine Trap

Factor 8 (Own Your Control Flow) can be taken too far. Explicit state machines are great for well-understood workflows. But some domains are genuinely exploratory — research, open-ended analysis, creative work — where you *don't know* the states upfront. Forcing a state machine onto an inherently open-ended process can produce brittle, over-specified systems that are just as bad as the agentic loops they replace.

The mitigation: use state machines for the *outer workflow* and allow bounded agentic loops for *inner steps*. Let the LLM explore within a step, but don't let it choose the next step.

### 2. No Framework = No Guardrails

The manifesto's deliberate refusal to provide a reference implementation is both its strength and its weakness. Developers who understand the principles can implement them in any language, any stack. But developers who are new to agent architecture may struggle to translate principles into code without concrete examples. There's a gap between "own your context window" and knowing *how* to own it.

### 3. The Complexity Cost of Statelessness

Factor 12's stateless reducer pattern adds real complexity. Serializing and deserializing state between every step, managing state storage, handling version migrations when your state schema changes — these are non-trivial engineering costs. For a quick internal tool or a prototype, this overhead may not be justified. The factors are optimized for production systems that need to run reliably for months, not for hackathon projects.

### 4. HITL Latency

Factor 7 (Contact Humans with Tool Calls) introduces human response time into the critical path. For workflows where speed matters — real-time customer support, time-sensitive trading — waiting for human approval at a decision boundary can be a dealbreaker. The manifesto acknowledges this implicitly (Factor 6's pause/resume mechanism is designed for it), but doesn't offer much guidance on *when* to automate vs. when to ask.

### 5. The "Small Agents" Decomposition Problem

Factor 10's 3–10 step heuristic is useful, but it pushes complexity from *within* agents to *between* agents. Agent orchestration — deciding which sub-agent to invoke, how to pass state between them, how to handle partial failures in a chain — is its own circle of engineering hell. The manifesto doesn't address multi-agent coordination in depth.

---

## The Bottom Line

The 12-Factor Agents manifesto succeeds because it asks the right question. Not "how do we make LLMs more autonomous?" but "how do we build reliable software systems that happen to use LLMs?"

It's not the answer to every agent architecture challenge. But it's the starting point. If you're building agents for production, you should read it the same way you'd read *The Twelve-Factor App* before deploying to the cloud — not as scripture, but as a set of constraints that will save you from the most common and most expensive failure modes.

The agents that survive production won't be the cleverest. They'll be the most well-engineered.

---

## 🧵 Twitter Thread Version (8 tweets)

> **1/** In 2011, Heroku's Twelve-Factor App changed how we build web services. In 2025, @dexhorthy's 12-Factor Agents is doing the same for AI agents. 24k GitHub stars. No code to install. Just opinions that will save your production agent. 🧵

> **2/** The core insight: agents don't fail because LLMs are dumb. They fail because developers treat the LLM as the control plane instead of a component. Stop building "autonomous agents." Start building software that uses LLMs for specific decisions.

> **3/** Factor 3 — Own Your Context Window. Stop appending everything to your message list. Your context window is a surgical briefing, not a chat log. Curate what goes in. Compress completed work. Evict irrelevant history. Monitor context health like you monitor CPU.

> **4/** Factor 8 — Own Your Control Flow. The `while not done: ask_llm()` loop is elegant and a production nightmare. Use explicit state machines. Enumerate your states. Make transitions testable. Let the LLM make *decisions within steps*, not decide the steps themselves.

> **5/** Factor 12 — Agent as Stateless Reducer. `new_state = reduce(state, event)`. Pure function. No hidden memory. Serialize state between every step. Get free crash recovery, reproducible debugging, and horizontal scaling. This is the architectural capstone.

> **6/** The 3–10 step rule: if your agent needs more than 10 steps, decompose it. Not because LLMs can't handle complexity — because *you* can't debug a 47-step failure at 3am. Small agents = contained blast radius.

> **7/** HITL isn't an escape hatch — it's Factor 7. "Contact Humans with Tool Calls." Human approval is a first-class tool the agent can invoke, with structured pause/resume. Design for interruption, not just automation.

> **8/** No pip install. No npm package. Just 12 constraints that will keep your agent alive in production. If you're building agents for real workloads, read this before writing another line of code. github.com/humanlayer/12-factor-agents

---

*Next in the Agent Harness Papers series: Part 2 — [TBD]*

*This is Part 1 of a multi-part series examining the foundational documents, frameworks, and tools shaping production AI agent architecture in 2025.*


---

# 中文版

# Agent Harness 论文系列，第一篇：12-Factor Agents

**把 AI Agent 当软件系统来设计的宣言——而不是当魔法。**

---

2011 年，Adam Wiggins 在 Heroku 工作期间发布了 *The Twelve-Factor App*。它不是一个框架，不是一个你可以 `npm install` 的库。它是一组观点——十二条——关于 Web 应用应该怎么构建，如果你希望它们在生产环境中活下来的话。它后来成了云原生思维的脊梁。此后每一个 PaaS、每一个容器编排器、每一份"无服务器"路演材料，都在某种程度上是对那十二条因素的回应。

2025 年，Dex Horthy 为 AI Agent 做了同样的事。

他的项目 [12-Factor Agents](https://github.com/humanlayer/12-factor-agents) 目前在 GitHub 上有约 24k 星。没有可安装的包。没有 SDK。没有 `pip install twelve-factor-agents`。它是一份设计宣言——一组来之不易的观点，告诉你为什么你的 Agent 在生产环境里总是出问题，以及该怎么办。

这是 **Agent Harness Papers** 系列的第一篇。我们从这里开始，因为本系列中的其他一切——框架、编排器、评估工具——都存在于这份文档的引力场中。你无法评估一个设计糟糕的东西。而现在，大多数 Agent 的设计都很糟糕。

---

## 起源：100 位创始人与一个失败模式

Horthy 不是在真空中写出这些因素的。作为 [HumanLayer](https://humanlayer.dev) 的创始人——一个面向人在回路（HITL）AI 工作流的平台——他拥有第一排座位，亲眼目睹了生产级 Agent 是如何失败的。他访谈了超过 100 位正在为真实业务构建 Agent 的 SaaS 创始人：客户支持、销售自动化、文档处理、内部工具。

模式是一致的。失败方式不是你预想的那样。

> Agent 失败的原因不是 LLM 太蠢。而是开发者把 LLM 当成了控制面（control plane），而不是一个组件。

创始人们挣扎的不是提示工程（prompt engineering）。他们也不是被模型能力卡住了。他们淹没在围绕模型的*软件工程*问题中：

- 多步骤工作流中的状态管理
- 当工具调用在第 12 步中的第 7 步失败时的错误恢复
- 上下文窗口被无关的对话历史撑到爆
- 没有办法暂停工作流、获取人工审批、然后继续
- 在演示中好好工作但在生产环境的边缘情况下一路幻觉的 Agent

这些不是 AI 问题。这些是软件架构问题。Horthy 的核心洞察看似简单，实则深刻：

**别再构建"自主 Agent"了。开始构建利用 LLM 做特定决策的软件系统。**

---

## 核心洞察：LLM 是路由器，不是自动驾驶

2024 年的主流心智模型是*Agentic 循环*：给 LLM 一个目标、一些工具，让它自己想办法走完所有步骤。AutoGPT、BabyAGI 及其后继者都遵循这种模式。LLM 做计划。LLM 决定下一步做什么。LLM 就是控制流。

Horthy 的宣言颠覆了这一点。在 12-Factor Agents 中，LLM 的任务是狭窄而具体的：

**将自然语言转化为结构化的工具调用。**

这是 Factor 1，也是其他一切的基础。LLM 不决定*做什么*——你的代码决定做什么。LLM 决定的是*如何将*决策表达为结构化输出。它是一个路由器，不是一个飞行员。

这是一个具有重大实际影响的哲学转变。当 LLM 是自动驾驶时，每个失败都是一个 AI 问题——难以理解、无法复现、用"更好的提示"来修复。当 LLM 是一个架构良好的系统中的路由器时，失败就变成了软件 Bug——可追踪、可测试、用常规工程手段可修复。

---

## 12 个因素一览

在深入三个关键因素之前，先看完整地图：

| # | 因素 | 一句话解释 |
|---|------|-----------|
| 1 | 自然语言 → 工具调用 | LLM 的任务是把意图转换成结构化操作。 |
| 2 | 掌控你的提示词 | 不要用框架注入的系统提示。你自己写每一个 token。 |
| 3 | 掌控你的上下文窗口 | 策展放进去的内容。不要只是追加。 |
| 4 | 工具只是结构化输出 | "工具调用"只是 JSON。执行是你的代码。 |
| 5 | 统一执行状态和业务状态 | 你的 Agent 状态*就是*你的应用状态。唯一真相来源。 |
| 6 | 启动/暂停/恢复 | Agent 必须把中断作为一等公民来支持。 |
| 7 | 用工具调用联系人类 | HITL 不是逃生出口——它是 Agent 可以调用的一个工具。 |
| 8 | 掌控你的控制流 | 显式的状态机，而不是开放式循环。 |
| 9 | 把错误压缩进上下文 | 错误不是致命的——它们是下一步的信息。 |
| 10 | 小型、专注的 Agent | 3–10 步。如果更大，就分解。 |
| 11 | 从任何地方触发 | Webhook、cron、API 调用、Slack——入口点不应该重要。 |
| 12 | Agent 即 Stateless Reducer | `reduce(state, event) → new_state`。纯函数。没有隐藏状态。 |
| 13 | 预取上下文 *(附加)* | 在 LLM 看到提示之前收集相关数据。 |

每一个因素都是一个约束。而就像最好的约束一样，一旦你接受它们，它们就是解放的。让我们深入最重要的三个。

---

## 深入解析：Factor 3 — 掌控你的上下文窗口

这是大多数 Agent 架构悄无声息地腐烂的地方。

每个 Agent 框架的默认行为是*追加*。用户说了什么 → 追加到消息。工具返回了什么 → 追加到消息。发生了错误 → 追加到消息。上下文窗口单调递增，直到你达到 token 上限，此时框架要么从前面截断（丢失关键指令），要么做摘要（丢失关键细节）。

Factor 3 说：**停止追加，开始策展。**

你的上下文窗口不是对话日志。它是一块你为每次 LLM 调用精心设计的*工作记忆*。你决定什么放进去、什么被压缩、什么被驱逐。合适的比喻不是聊天记录——而是一份手术简报。

### 实践中的样子

不要这样做：

```
messages = [system_prompt] + all_previous_messages + [new_user_message]
```

而是有意识地构建每个上下文窗口：

```python
def build_context(state, current_step):
    return [
        system_prompt_for_step(current_step),
        relevant_state_summary(state),
        recent_errors_if_any(state),
        current_task_description(state),
        available_tools_for_step(current_step),
    ]
```

注意缺失了什么：所有中间的闲聊。"好的，我来做"这样的回复。步骤 2 中与步骤 7 无关的工具调用。已经被解析和压缩过的冗长错误堆栈。

### 上下文健康度：颜色系统

在采用了这个因素的生产系统中，上下文健康度成为一个可监控的指标：

| 状态 | 阈值 | 行为 |
|------|------|------|
| 🟢 绿色 | <50% 容量 | 正常运行 |
| 🟡 黄色 | 50–70% | 开始将已完成的工作压缩为摘要 |
| 🟠 橙色 | 70–85% | 主动压缩：将决策移到外部状态，归档假设 |
| 🔴 红色 | >85% | 强制检查点：将状态序列化到存储，然后以全新上下文继续 |

这不是理论。C31 工程系统（本系列后续会介绍）完全按照这个颜色系统实现了运行协议。当上下文达到橙色时，Agent 会主动将工作状态外化——不是因为 token 用完了，而是因为*注意力质量在远未达到硬限制之前就已经开始退化了*。

### 为什么这个因素很难

因为它要求你对领域有足够深的理解，才能知道每一步什么是相关的。你不能把所有东西都扔给 LLM，然后寄希望于注意力机制自己整理出来。你需要对信息层级做出编辑决策。那是工程工作，不是提示工程。

---

## 深入解析：Factor 8 — 掌控你的控制流

这是最直接攻击"Agentic 循环"范式的因素。

在典型的 Agent 框架中，控制流看起来是这样的：

```
while not done:
    response = llm.chat(messages)
    if response.has_tool_calls():
        results = execute_tools(response.tool_calls)
        messages.append(results)
    else:
        done = True
```

优雅、通用——然后是生产噩梦。

问题在于 `while not done` 承担了所有工作，而 LLM 在做所有关于何时停止、下一步做什么、如何从错误中恢复的决策。你把控制流让渡给了一个随机系统。你的 Agent 行为现在是*涌现的*，而不是*设计的*。

Factor 8 说：**让控制流显式化。** 使用状态机。使用有向图。使用普通的 `if/else` 语句。用任何能让转换可见且可测试的方式。

### 状态机替代方案

```python
class AgentState(Enum):
    GATHERING_REQUIREMENTS = "gathering_requirements"
    ANALYZING_DATA = "analyzing_data"
    GENERATING_REPORT = "generating_report"
    AWAITING_APPROVAL = "awaiting_approval"
    EXECUTING_ACTION = "executing_action"
    DONE = "done"

def step(state: AgentState, context: dict) -> AgentState:
    match state:
        case AgentState.GATHERING_REQUIREMENTS:
            # LLM extracts structured requirements from user input
            requirements = llm_extract_requirements(context)
            context["requirements"] = requirements
            return AgentState.ANALYZING_DATA

        case AgentState.ANALYZING_DATA:
            # Deterministic code fetches and processes data
            data = fetch_data(context["requirements"])
            context["analysis"] = analyze(data)
            return AgentState.GENERATING_REPORT

        case AgentState.AWAITING_APPROVAL:
            # Agent pauses. Resumes when human approves.
            if context.get("approved"):
                return AgentState.EXECUTING_ACTION
            return AgentState.AWAITING_APPROVAL  # stay paused

        # ... and so on
```

看看发生了什么变化：

1. **每个可能的状态都被枚举出来了。** 不会有意外的涌现状态。
2. **转换是显式的。** 你可以画出这个系统的图。你可以独立测试每个转换。
3. **LLM 被精确地使用。** 它在某一步提取需求，在另一步生成报告。它不决定整体流程。
4. **暂停变得很简单。** `AWAITING_APPROVAL` 只是一个状态。序列化上下文、关闭进程、等人类响应后恢复。不需要维护 WebSocket 连接，不需要看管长时间运行的进程。

### 3–10 步启发式规则

Factor 10 强化了这一点：如果你的 Agent 需要超过 10 个步骤，那它太大了。把它拆分成更小的 Agent，每个拥有一个子工作流。这不是关于能力——现代 LLM 可以处理复杂推理。这是关于*可调试性*。当你的 47 步 Agent 在第 31 步失败时，祝你好运找出哪里出了问题。当你的 7 步 Agent 在第 4 步失败时，爆炸半径是可控的，修复方法是显而易见的。

---

## 深入解析：Factor 12 — Agent 即 Stateless Reducer

这是架构上的压轴石。如果你只接受一个因素，就接受这个。

```
new_state = reduce(current_state, event)
```

你的 Agent 是一个纯函数。它接收当前状态和一个事件（用户消息、工具结果、定时器触发、Webhook 载荷），然后返回一个新状态。没有隐藏变量。没有在调用之间存活的内存缓存。没有跨调用不断累积的全局 `conversation_history` 列表。

### 为什么是无状态的？

三个理由，全部是运维层面的：

**1. 可复现性。** 如果 `reduce(state, event)` 是纯函数，你可以通过输入相同的状态和事件来重放任何故障。对于有状态的 Agent 这是不可能的，因为故障取决于之前所有交互的累积记忆。

**2. 可扩展性。** 无状态 Reducer 可以在任何地方运行。启动一个新容器，喂入状态，获取结果。不需要粘性会话（sticky session）。不需要亲和性要求。不会出现"这个 Agent 必须在同一台机器上运行因为它有内存中的状态"。

**3. 持久性。** 当你在每一步之间都序列化状态时，你就获得了免费的崩溃恢复。进程在第 5 步挂了？没问题——从存储读取状态，重新分发事件。已完成的工作不会丢失，因为它被持久化为状态，而不是保存在内存中。

### 状态长什么样

```json
{
  "agent_id": "invoice-processor-abc123",
  "current_step": "awaiting_approval",
  "created_at": "2025-06-15T10:30:00Z",
  "context": {
    "invoice_id": "INV-2025-0847",
    "extracted_fields": {
      "vendor": "Acme Corp",
      "amount": 14250.00,
      "due_date": "2025-07-15"
    },
    "validation_results": {
      "vendor_match": true,
      "amount_within_policy": true,
      "duplicate_check": "clear"
    }
  },
  "pending_action": "human_approval",
  "error_log": []
}
```

这就是 Agent 的全部真相。没有隐藏状态。没有内存中的对话缓冲区。一个全新的进程可以捡起这个状态，精确地知道下一步该做什么。这就是 Horthy 所说的"Agent 即 Stateless Reducer"——Agent 是一个函数，状态是一个文档，每次执行都是一次全新的规约。

### 反模式："我记得你之前说过……"

如果你的 Agent 说了"我记得你之前说过你偏好 X"，而那个"记忆"来自于序列化外部状态以外的任何东西——你就有了隐藏状态。而隐藏状态就是生产 Bug 藏身的地方。

---

## C31 整合：一个真实系统如何采纳了这些原则

12 个因素在设计上是抽象的——它们是原则，不是实现。C31 工程系统（一个在多个团队生产环境中使用的复合工程框架）提供了一个具体案例，展示这些因素如何转化为运行协议。

### F3 → 上下文健康度颜色系统

C31 将 Factor 3 实现为一个字面上的交通信号灯监控系统，用于监控上下文窗口。Agent 实时追踪其上下文使用率，并在每个阈值触发自动化压缩行为。在 🟡 黄色时，已完成的工作被压缩为一行摘要。在 🟠 橙色时，决策被外化到文件中。在 🔴 红色时，强制执行完整检查点——状态被序列化到 `session_state.json` 后才继续。

这就是 F3 的运营化：上下文不仅仅被策展，它被*治理*了。

### F5 → 会话状态回写

C31 的 `session_state.json` 是跨会话连续性的唯一真相来源。当一个 Skill（可复用的工作流单元）完成执行时，其结果——文档路径、修改过的文件、新的知识产出——被写回 `session_state.json` 的 `completed_tasks` 下。这直接实现了 Factor 5 对执行状态和业务状态的统一：Agent 的进度*就是*应用状态，并且它存在于一个地方。

```json
{
  "last_topic": "refactoring auth module",
  "open_todos": ["update API docs", "add integration test for edge case"],
  "completed_tasks": [
    {
      "skill": "ce-compound",
      "output_path": "docs/solutions/auth/token-refresh-fix.md",
      "files_modified": ["src/auth/refresh.ts", "tests/auth/refresh.test.ts"]
    }
  ],
  "last_flush": "2025-06-15T14:30:00Z"
}
```

### F7 → 决策边界结构化暂停

Factor 7 说人类接触应该是一个工具调用，而不是一个逃生出口。C31 通过其**决策边界（Decision Boundary）**协议更进一步：每个操作被分类为*执行层*（10 分钟内可逆——Agent 自主进行）或*决策层*（不可逆或高影响——Agent 暂停等待人工确认）。

暂停不是一个模糊的"你确定吗？"对话框。它是结构化的：

```
⏸️ [Type: File Overwrite] [Impact: Will replace production config] [Need: Confirm/Modify/Cancel]
```

这就是 HITL 作为一等公民——不是事后拼凑到自主循环上的补丁，而是一个带有明确触发条件和结构化输出的设计交互模式。

### F12 → Stateless Reducer 作为架构法则

C31 把 F12 视为其根本架构原则。每次 Agent 执行都是 `f(context) → action`，没有隐藏状态。跨会话的连续性完全来自 `session_state.json` 和外部文件。子代理只接收完成其任务所需的最小上下文。任何 Agent 都可以从其序列化状态重新启动并产生相同的结果。

反模式被明确指出：如果 Agent 说"我记得你之前说过……"，而那个记忆不是来自于一个外部状态文件，那它就是不可靠的，应该被如此对待。

---

## 局限性与诚实的批评

没有一份宣言是没有盲点的，12-Factor Agents 的设计有几个值得承认的局限：

### 1. 状态机陷阱

Factor 8（掌控你的控制流）可能被过度运用。显式的状态机对于充分理解的工作流很棒。但某些领域天然是探索性的——研究、开放式分析、创意工作——你*事先不知道*有哪些状态。将状态机强加于一个本质上开放的过程，可能产生僵化的、过度规格化的系统，和它们所替代的 Agentic 循环一样糟糕。

缓解方法：用状态机管理*外层工作流*，允许有界的 Agentic 循环用于*内层步骤*。让 LLM 在一个步骤内探索，但不要让它选择下一个步骤。

### 2. 没有框架 = 没有防护栏

宣言刻意不提供参考实现，这既是它的优势也是它的弱点。理解原则的开发者可以在任何语言、任何技术栈中实现它们。但对 Agent 架构不熟悉的开发者可能难以在没有具体示例的情况下将原则转化为代码。在"掌控你的上下文窗口"和知道*如何*掌控之间存在鸿沟。

### 3. 无状态的复杂性成本

Factor 12 的 Stateless Reducer 模式增加了真实的复杂性。在每一步之间序列化和反序列化状态、管理状态存储、处理状态 schema 变更时的版本迁移——这些都是非平凡的工程成本。对于一个快速的内部工具或原型，这种开销可能不合理。这些因素是为需要可靠运行数月的生产系统优化的，而不是黑客马拉松项目。

### 4. HITL 延迟

Factor 7（用工具调用联系人类）将人类响应时间引入了关键路径。对于速度至关重要的工作流——实时客户支持、时间敏感的交易——在决策边界等待人工审批可能是不可接受的。宣言隐式地承认了这一点（Factor 6 的暂停/恢复机制就是为此设计的），但对于*何时*应该自动化 vs. 何时应该询问，没有提供太多指导。

### 5. "小型 Agent"的分解问题

Factor 10 的 3–10 步启发式规则很有用，但它把复杂性从 Agent *内部*推到了 Agent *之间*。Agent 编排——决定调用哪个子 Agent、如何在它们之间传递状态、如何处理链式调用中的部分失败——本身就是工程地狱的一个圈层。宣言没有深入讨论多 Agent 协调。

---

## 底线

12-Factor Agents 宣言之所以成功，是因为它问了正确的问题。不是"我们如何让 LLM 更自主？"而是"我们如何构建恰好使用了 LLM 的可靠软件系统？"

它不是每个 Agent 架构挑战的答案。但它是起点。如果你正在为生产环境构建 Agent，你应该像部署到云之前阅读 *The Twelve-Factor App* 一样阅读它——不是当作圣经，而是当作一组约束，它们将帮你避开最常见、最昂贵的失败模式。

能在生产环境中存活下来的 Agent，不会是最聪明的那些。而是工程最扎实的那些。

---

## 🧵 Twitter 帖子版（8 条推文）

> **1/** 2011 年，Heroku 的 Twelve-Factor App 改变了我们构建 Web 服务的方式。2025 年，@dexhorthy 的 12-Factor Agents 正在为 AI Agent 做同样的事。24k GitHub 星。无需安装代码。只有那些会拯救你生产 Agent 的观点。🧵

> **2/** 核心洞察：Agent 失败不是因为 LLM 蠢。而是因为开发者把 LLM 当成了控制面而不是组件。别再构建"自主 Agent"了。开始构建利用 LLM 做特定决策的软件。

> **3/** Factor 3 — 掌控你的上下文窗口。停止往消息列表里追加所有东西。你的上下文窗口是一份手术简报，不是聊天日志。策展放进去的内容。压缩已完成的工作。驱逐无关历史。像监控 CPU 一样监控上下文健康度。

> **4/** Factor 8 — 掌控你的控制流。`while not done: ask_llm()` 循环很优雅，但也是生产噩梦。使用显式的状态机。枚举你的状态。让转换可测试。让 LLM 在*步骤内做决策*，而不是决定步骤本身。

> **5/** Factor 12 — Agent 即 Stateless Reducer。`new_state = reduce(state, event)`。纯函数。无隐藏记忆。每步之间序列化状态。免费获得崩溃恢复、可复现的调试和水平扩展。这是架构的压轴石。

> **6/** 3–10 步法则：如果你的 Agent 需要超过 10 步，就分解它。不是因为 LLM 处理不了复杂性——是因为*你*在凌晨 3 点调试不了一个 47 步的故障。小 Agent = 可控的爆炸半径。

> **7/** HITL 不是逃生出口——它是 Factor 7。"用工具调用联系人类。"人工审批是 Agent 可以调用的一等公民工具，带有结构化的暂停/恢复机制。为中断而设计，不仅仅是为自动化。

> **8/** 没有 pip install。没有 npm 包。只有 12 条约束，让你的 Agent 在生产环境中活下来。如果你正在为真实工作负载构建 Agent，在写下一行代码之前先读这个。github.com/humanlayer/12-factor-agents

---

*Agent Harness Papers 系列下一篇：Part 2 — [待定]*

*本文是 Agent Harness Papers 多篇系列的第一篇，探讨 2025 年塑造生产级 AI Agent 架构的基础文档、框架和工具。*
