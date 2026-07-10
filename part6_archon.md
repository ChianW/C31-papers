# The Agent Harness Papers, Part 6: Archon — Dockerfiles for AI Coding

*Series: The Agent Harness Papers — 7 frameworks, 1 personal AI operating system, 5 months of production use*

![Archon — AI Coding Pipeline (DAG)](images/final_part6_archon.jpg)

---

If GitHub Actions standardized CI/CD, Archon is trying to standardize AI coding workflows. The pitch is simple and audacious: define your development process in YAML. Execute it the same way every time. Replace the unpredictable "mood" of LLM sessions with deterministic, repeatable pipelines.

This is the most infrastructure-oriented framework in our survey. While others provide principles, psychology, or skills, Archon provides a literal **workflow engine**.

---

## The Analogy That Explains Everything

Think about how software deployment evolved:

- **2005**: SSH into server, run commands manually. Hope you remember all the steps.
- **2013**: Write a Dockerfile. Declare your environment. Deterministic execution.
- **2024**: Push code, CI/CD runs automatically. Same pipeline every time.

Now think about how AI coding works today:

- **2025**: Open a chat window, type your requirements. Hope the AI follows your conventions. Hope it doesn't skip tests. Hope it reviews its own work.

Archon's argument: this is the SSH-into-server era of AI coding. We need a Dockerfile.

---

## The YAML Workflow

An Archon workflow looks like this:

```yaml
name: feature-development
steps:
  - name: plan
    type: ai
    prompt: "Analyze the requirements and create an implementation plan"
    
  - name: implement
    type: ai
    prompt: "Implement the plan from the previous step"
    depends_on: [plan]
    
  - name: test
    type: bash
    command: "npm test"
    depends_on: [implement]
    
  - name: review
    type: ai
    prompt: "Review the implementation for bugs, security issues, and style"
    depends_on: [test]
    
  - name: fix-review-issues
    type: ai
    prompt: "Fix any issues found in the review"
    depends_on: [review]
    condition: "review.issues_found > 0"
    
  - name: create-pr
    type: bash
    command: "gh pr create --title '$TITLE' --body '$BODY'"
    depends_on: [fix-review-issues]
```

This is the core innovation. Your development process becomes **declarative**. You're not describing *how* to write code — you're describing *what sequence of operations* constitutes your workflow.

---

## Seven Node Types

Archon's power comes from mixing deterministic and non-deterministic nodes in the same workflow:

| Node Type | Deterministic? | What It Does |
|-----------|---------------|--------------|
| `ai` | No | LLM-driven generation, planning, review |
| `bash` | Yes | Shell commands — tests, builds, deployments |
| `git` | Yes | Branch creation, commits, PR operations |
| `condition` | Yes | Control flow gating based on previous outputs |
| `loop` | Mixed | Repeat steps until condition is met |
| `parallel` | Mixed | Execute multiple steps concurrently |
| `human` | No | Pause for human approval before continuing |

The insight: AI coding isn't *all* AI. Real development workflows are a mix of:
- AI decisions (what to build, how to structure it)
- Deterministic operations (run tests, check lint, create branches)
- Human gates (approve the plan, approve the PR)

Archon lets you compose all three into a single pipeline.

---

## The DAG Executor

Workflows don't execute linearly — they're compiled into a **DAG** (Directed Acyclic Graph). The `depends_on` field creates the execution graph. Steps with no dependencies run in parallel. Steps with dependencies wait.

```
plan ──→ implement ──→ test ──→ review ──→ fix ──→ create-pr
                                  │
                                  └──→ (skip fix if no issues)
```

This is categorically different from chat-based workflows where you type one instruction at a time. The entire workflow is pre-defined, optimized for parallelism, executed in deterministic order.

### Error Handling in the DAG

When a node fails, Archon doesn't simply crash. Behavior depends on configuration:

- **retry**: Re-execute the node (good for flaky tests or API timeouts)
- **fallback**: Execute an alternative node
- **abort**: Stop the entire workflow
- **continue**: Mark as failed but keep going (good for non-critical steps like lint warnings)

This is CI/CD thinking applied to AI coding. Jenkins figured this out a decade ago. Archon brings it to LLM workflows.

---

## Git Worktree Isolation

Every Archon workflow execution gets its own **git worktree**. This is the same isolation pattern Superpowers uses — not a coincidence. Two frameworks independently reached the same conclusion: AI coding needs sandboxing.

Why worktrees instead of branches?

- Branches exist in the same working directory. If the AI modifies files, your main checkout is affected.
- Worktrees are separate directories linked to the same repository. The AI can write code, compile, test, even break things — without touching your current work.

```
my-project/           ← your main checkout (untouched)
my-project-wt-feat-1/ ← Archon-created worktree for feature-1
my-project-wt-fix-2/  ← Archon-created worktree for fix-2
```

Multiple workflows can run in parallel, each in their own worktree, completely isolated.

---

## Multi-Entry Triggers

One workflow definition, multiple trigger points:

| Trigger | How |
|---------|-----|
| CLI | `archon run feature-development` |
| Web UI | Dashboard with workflow visualization |
| Slack | `/archon run feature-development` |
| Telegram | Bot commands |
| GitHub | PR event webhooks |
| Cron | Scheduled execution |

This is where the Dockerfile analogy is most apt. You write the workflow once, trigger it from anywhere. The execution is identical regardless of the entry point.

---

## The Full Python → TypeScript Rewrite

Archon recently completed a full rewrite from Python to TypeScript. This is worth noting because it signals the project's direction:

- **TypeScript** aligns with the web-first ecosystem (VS Code extensions, browser-based UI, npm distribution)
- The rewrite brought stronger typing for workflow definitions
- Better async/await support for parallel node execution
- Native integration with the npm ecosystem

The rewrite is also a confidence signal. The team was willing to throw out their entire codebase to build the right thing. That takes conviction.

---

## What C31 Takes From Archon

Archon's contribution to C31 isn't its workflow engine — C31 uses a skill-based architecture, not YAML pipelines. Instead, C31 extracts two **behavioral principles**:

### Rollback-First Thinking

> Before making high-risk changes, plan the rollback path first.

This sounds obvious. It isn't. The human instinct is: plan change → execute → hope it works → figure out rollback if it doesn't.

Archon inverts this: plan rollback → verify rollback is feasible → then execute the change. If you can't articulate how to undo something, the change is too risky to make.

In C31, this becomes part of the Pre-Action Checklist:
```
3. Reversibility: Can this be undone without damage? 
   If not → inform the user before executing.
```

### No Autonomous Lifecycle Mutation

> When you can't reliably distinguish "another process is still running" from "it's crashed and left an orphan," don't autonomously mark it as failed.

This principle addresses a specific failure mode in multi-agent systems: Agent A spawns Agent B. Agent B goes quiet. Is it still working? Did it crash? Agent A doesn't know — and guessing wrong is dangerous.

Archon's rule: **surface ambiguous state to humans**. Don't guess. Provide one-click actions and let humans decide.

In C31, this becomes part of Error Handling:
```
Escalate: When you can't reliably determine process state, 
surface the ambiguity to the user with actionable options.
```

---

## Honest Limitations

### 1. Workflow Rigidity

YAML workflows are deterministic by design. But real development is messy. Sometimes you discover mid-implementation that the plan was wrong. Archon handles "throw out the plan and start over" less gracefully than freeform chat sessions.

### 2. Learning Curve

Defining workflows in YAML requires thinking through your process *before* you start. Most developers don't work this way. The overhead of workflow definition only pays off for repeatable processes — one-off tasks get no benefit.

### 3. No Knowledge Compounding

Archon executes workflows but doesn't learn from them. There's no equivalent to CEP's Compound step. A workflow that succeeded yesterday doesn't make today's execution smarter.

### 4. Opacity of LLM Nodes

When an `ai` node produces unexpected output, debugging requires inspecting the LLM's response — which is inherently non-deterministic. The deterministic *pipeline* wraps a non-deterministic *core*. The shell helps, but the fundamental unpredictability remains.

### 5. Adoption Friction

Switching from chat-based AI coding to workflow-based AI coding requires a habit change. Most developers prefer the flexibility of conversation to the structure of pipelines. Archon is optimized for teams and repeatable processes, not individual exploration.

---

## The Verdict

Archon is for teams that want **repeatable, auditable AI coding pipelines**. If you repeatedly do the same class of work — feature development, bug fixes, PR reviews — and you want consistency, Archon's YAML workflows deliver it.

If you're exploring, prototyping, or doing one-off work, Archon's overhead doesn't justify itself.

The Dockerfile analogy is apt but incomplete. Dockerfiles standardize *environment* (objective: this OS, these packages). Archon standardizes *process* (subjective: this planning style, this review depth). Whether YAML can capture the nuance of good engineering judgment remains an open question.

---

*Next: [Part 7 — GSD Core: The Last Line of Defense Against Context Rot](part7_gsd_core.md)*
