# The Agent Harness Papers, Part 11: From Harness to Loop — The Missing Automation Layer

*Series: The Agent Harness Papers — 7 frameworks, 1 personal AI operating system, 5 months of production use*

---

> *"I don't prompt Claude anymore. I have loops running that prompt Claude. My job is to write loops."*
> — Boris Cherny, Anthropic Claude Code Lead

---

## The Automation Ceiling

By the time I finished building C31’s 12-gate pipeline and the four parallel orchestrations, I had achieved what I set out to build: an ironclad **Agent Harness**. 

When I gave C31 a task, it executed it with discipline. It brainstormed, planned, coded, reviewed, tested, and documented. It didn't hallucinate structural assumptions because of the Context Engineering constraints. It didn't merge broken code because of the Parallel Adversarial Review. It didn't forget how it solved a problem because of the Knowledge Compounding flywheel.

But it still had one fundamental limitation: **it only did these things when I told it to.**

I was still the trigger. I was still the scheduler. If I didn't type `/ce-lfg`, nothing happened. 

The architecture I had built was a sophisticated engine, but it lacked an autonomous transmission. I was still manually shifting the gears. To cross the threshold from an "AI coding assistant" to a "truly autonomous AI system," I had to confront a new architectural challenge: **Loop Engineering**.

---

## Harness Engineering vs. Loop Engineering

To understand what was missing, we have to look at the complete AI engineering stack of 2026:

1. **Context Engineering**: What does the model see? (Solving the 200k window rot).
2. **Harness Engineering**: How does a single execution run safely? (C31's primary domain).
3. **Loop Engineering**: Who continuously triggers and verifies these executions? (The automation layer).
4. **Fleet Engineering**: How do multiple distinct loops coordinate?

C31 was a masterpiece of Harness Engineering. But as I studied `cobusgreyling/loop-engineering`, I realized that a Harness without a Loop is ultimately just a very smart script. 

A Harness is invoked. A Loop *runs*. 
A Harness waits for input. A Loop reads its environment.
A Harness needs human permission to start. A Loop needs human permission to stop.

To upgrade C31 from a "Human-Triggered Harness" to a **"Loop-Ready System,"** I didn't need to rebuild the core execution logic. I needed to wrap the Harness in three specific primitives: **State, Constraints, and Budgets**.

---

## The Three Pillars of a Loop-Ready System

### 1. Project-Level Boundaries (`CONSTRAINTS.md`)

In a Harness, safety is managed by asking the human for permission before dangerous actions (e.g., overriding a file). But in an unattended Loop, asking the human blocks execution and defeats the purpose of the loop.

The solution is not to remove safety, but to shift it from "runtime confirmation" to "ahead-of-time declarative boundaries."

We introduced `CONSTRAINTS.md` — a project-level file that explicitly maps the "no-go zones." 
- *Never edit files in `/payments`.*
- *Any change touching `/db/migrations` requires human review.*
- *Never auto-merge to `main`.*

Now, when a fully automated C31 pipeline (like `C31-lfg`) wakes up, its very first action (Gate 0) is to ingest `CONSTRAINTS.md`. If a planned action violates a constraint, the Loop halts and escalates. If it doesn't, it proceeds silently. The safety boundary moved from the human's attention to the repository's configuration.

### 2. The Budget Constraint (`loop-budget.md`)

An unattended AI is a financial risk. An infinite loop that hallucinates and retries API calls can burn hundreds of dollars overnight. 

To make C31 Loop-Ready, we implemented strict token and execution budgets. Before a loop triggers a C31 execution, it checks the `loop-budget.md`. If the budget is exhausted, the loop sleeps. It forces the system to be not just operationally constrained, but economically constrained.

### 3. Explicit Autonomy Levels (L1 / L2 / L3)

Trust is the bottleneck of AI adoption. If a user doesn't know what a skill will do, they won't let it run unattended.

We retrofitted every C31 skill with an explicit, frontmatter-declared **Autonomy Level**:

- **L1 (Observer)**: Read-only. Generates reports. Zero side-effects. (e.g., `C31-research`, `C31-review`).
- **L2 (Co-pilot)**: Executes work, but requires human confirmation at critical decision gates. (e.g., `C31-plan`, `C31-debug`).
- **L3 (Autonomous)**: Full 12-gate pipeline, runs unattended to completion. (e.g., `C31-lfg`, `C31-loop`).

When an L3 skill is scheduled in a loop, the user has a cryptographic guarantee: this process is designed, tested, and bounded to run without me.

---

## The 5-Question Deployment (`c31-loop`)

The final piece of the puzzle was UX. Deploying a cron job or a GitHub Action to trigger an AI agent traditionally requires writing complex YAML or PowerShell scripts.

We encapsulated this into a single skill: `c31-loop`.

Instead of writing configurations, the user types `c31 loop init` and answers five questions:
1. *What does this loop do?* (e.g., daily dependency sweep, issue triage)
2. *How often does it run?*
3. *Where does it run?* (GitHub Actions, Windows Task Scheduler)
4. *Who gets notified on failure?*
5. *What is the token budget?*

The skill automatically generates the `STATE.md`, the `CONSTRAINTS.md`, the `loop-budget.md`, and the platform-specific deployment scripts.

---

## The Result: Waking Up to Finished Work

This is what C31 looks like in production today.

At 3:00 AM, a GitHub Action wakes up. It reads the `STATE.md` to see where it left off yesterday. It reads the `CONSTRAINTS.md` to know its boundaries. It scans the open issues and outdated dependencies. It triggers `C31-lfg` (an L3 pipeline) to fix a minor bug. The pipeline runs the parallel review, passes the tests, and opens a Pull Request. It updates the `STATE.md` and goes back to sleep.

When I wake up at 8:00 AM, I don't start my day by writing code. I start my day by reviewing the Pull Request C31 wrote while I was sleeping.

My job is no longer just to prompt the AI. 
**My job is to write the loops that prompt the AI.**
