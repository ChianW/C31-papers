# Agent Harness Papers, Part 5: Compound Engineering Plugin (CEP) — The Framework That Remembers

**Series:** Agent Harness Papers
**Author:** Research notes compiled from public repositories and documentation
**Subject:** Compound Engineering Plugin by Every Inc. (~22,900 ★)

![Knowledge Compounding — Time to Solve vs Projects Completed](images/final_part5_compounding.jpg)

---

> *"Each unit of engineering work should make subsequent units easier — not harder."*
> — Compound Engineering Plugin, core philosophy

---

## The Hook

Most frameworks focus on the quality of current code. They ask: is this function correct? Is this test passing? Is this PR clean?

CEP asks a deeper question: **did today's work make tomorrow easier?**

It's a question that sounds almost philosophical — the kind of thing you'd expect from a management consultant, not a `.md` file in a GitHub repository. But the Compound Engineering Plugin, built by Every Inc. and sitting at roughly 22,900 stars, has turned this philosophy into a concrete, executable workflow. And the results are genuinely interesting.

Where other frameworks in this series have focused on discipline (Superpowers), operational completeness (ECC), anti-shortcutting (Agent Skills), or architectural purity (12-Factor Agents), CEP focuses on something none of them directly address: **institutional memory**. The idea that every bug you fix, every architectural decision you make, every problem you solve should leave behind a trail that makes the *next* occurrence of that problem trivially easy.

This is the framework that refuses to let knowledge evaporate.

---

## 1. The Compounding Philosophy

The name is not an accident. "Compound Engineering" is a direct reference to compound interest — the most powerful force in finance, where returns generate their own returns, and small advantages accumulate into enormous ones over time.

CEP applies this principle to software engineering. The argument goes like this:

1. Most engineering teams solve the same problems repeatedly. Not identical problems, but problems in the same *family*. The same category of race condition. The same pattern of API misuse. The same architectural anti-pattern rediscovered by each new team member.

2. Each time a problem is solved, the solution exists in exactly one place: the developer's head (or the AI agent's context window). When the session ends, the solution evaporates. The next person who hits the same problem starts from scratch.

3. This is a compounding *failure*. Not only does the team not benefit from prior work — the prior work actively contributes nothing to the future. You're running on a treadmill.

4. CEP's intervention is simple: after every meaningful unit of work, **document what you learned in a structured, discoverable format**. Then, before starting any new work, **search that documentation for prior art**.

The math is straightforward. If each solution takes 2 hours and you document it in 15 minutes, the second occurrence takes 15 minutes instead of 2 hours. The third occurrence takes 5 minutes (you already know where the doc is). By the fifth occurrence, it's essentially free. That 15-minute investment compounds.

This sounds obvious. It's the kind of thing every engineering team claims to do ("we have a wiki!"). The difference is that CEP doesn't *suggest* documentation — it **embeds documentation as a mandatory step in the workflow**, and it gives the AI agent a structured protocol for both writing and retrieving it.

---

## 2. The Core Loop: Brainstorm → Plan → Work → Simplify → Review → Compound

CEP's workflow is a six-step loop, and each step is a distinct skill with its own execution protocol:

### Brainstorm (`ce-brainstorm`)

The exploratory phase. Before any code is written, the agent and developer discuss requirements, explore approaches, and identify edge cases. The output is a requirements document — not a formal PRD, but a shared understanding of what "done" looks like.

This step is intentionally conversational. CEP recognizes that many engineering failures happen not because of bad code, but because of bad requirements. The brainstorm phase is designed to surface ambiguity *before* it becomes a bug.

### Plan (`ce-plan`)

The requirements become an execution plan. CEP's planning phase includes threat modeling (what could go wrong?), verification gates (how do we know each step worked?), and dependency ordering (what has to happen first?).

The plan is a first-class artifact — a document that can be reviewed, modified, and referenced throughout execution. This is a deliberate contrast to the "just start coding" approach that many AI agents default to.

### Work (`ce-work`)

The implementation phase. Code is written, tests are created, features are built. CEP doesn't impose a particular coding methodology here — it delegates to the developer's preferences and the project's conventions.

What it *does* impose is discipline around verification. Each unit of work must have a clear verification step. "I wrote the code" is not sufficient; "I wrote the code and the tests pass" is the minimum bar.

### Simplify (`ce-simplify-code`)

This is where CEP gets genuinely interesting, and where version 3.19 introduced a significant innovation.

After code is written but *before* it's reviewed, CEP runs a **simplification pass**. Three parallel reviewer agents examine the code from different angles:

- **Code-reuse reviewer**: Is there existing code that does the same thing? Are we duplicating logic that already exists elsewhere in the codebase?
- **Code-quality reviewer**: Is the code readable? Are the abstractions appropriate? Are there unnecessary complications?
- **Efficiency reviewer**: Are there performance concerns? Is the code doing unnecessary work?

These three agents run in parallel, and their findings are merged into a single simplification report. The developer then applies the simplifications before the code goes to review.

The insight here is that code review is expensive — it requires human attention, which is a scarce resource. If the AI can catch and fix obvious issues *before* review, the reviewer can focus on the things that actually require human judgment: architectural decisions, business logic correctness, and long-term maintainability.

This is the "clean your room before Mom inspects it" principle, systematized.

### Review (`ce-code-review`)

Multi-persona code review. CEP uses tiered persona agents — each with a different expertise and a different set of concerns — to review the code from multiple angles. Findings are confidence-gated (the reviewer must state how confident they are in each finding) and merged through a deduplication pipeline.

The confidence gating is important. It prevents the review from drowning in low-confidence nitpicks while ensuring high-confidence issues are never missed.

### Compound (`ce-compound`)

The final step — and the one that gives the framework its name. This is where knowledge is extracted, documented, and made discoverable. We'll go deep on this in the next section.

---

## 3. The Compound Step: Deep Dive

The `ce-compound` step is CEP's signature contribution. Here's how it works:

### Trigger Points

`ce-compound` is designed to trigger automatically — the agent doesn't wait for the developer to ask. The trigger conditions are:

- You modified ≥2 files in a session
- A design decision was made after multiple iterations (rejected approach A, adopted approach B)
- A bug was fixed that took more than one iteration to solve
- The user confirms the solution works ("looks good", "that fixed it")

The key insight is that `ce-compound` fires at the moment when knowledge is *freshest*. If you wait until the end of the sprint, or the end of the quarter, the context is gone. The developer has moved on. The agent's context window has been cleared. The solution exists in the git history, but good luck finding it.

### The Documentation Protocol

When `ce-compound` fires, it runs a structured extraction process:

1. **Context Analyzer**: What was the problem? What was the environment? What were the symptoms?
2. **Solution Extractor**: What was the fix? Why did it work? What alternatives were considered and rejected?
3. **Related Docs Finder**: Does existing documentation cover part of this? Does this solution update or supersede a previous one?

These three analyses can run in parallel (as subagents), and their output is merged into a solution document saved to `docs/solutions/[category]/`.

### The INDEX.md Requirement

Here's the detail that separates CEP from a glorified wiki: every solution document must be immediately registered in `docs/solutions/INDEX.md`. This is not optional. The documentation explicitly states:

> *"Without an INDEX entry, the doc is invisible to future agents."*

This is a design choice that reveals deep understanding of how AI agents actually work. An agent searching for prior art doesn't crawl the filesystem hoping to stumble on relevant documents. It reads the INDEX, identifies potentially relevant entries by keyword matching, and then reads the specific documents. No INDEX entry = no discoverability = the documentation might as well not exist.

### The Pre-Search Protocol (Solutions Recall)

The other half of the compound equation is **retrieval**. Before starting any non-trivial task, the agent silently checks the solutions store:

1. Read the global solutions registry (which lists all known solution stores and their paths)
2. For each registered store, check `INDEX.md` for category overview
3. Search relevant categories with keywords from the current task

If a match is found, the agent surfaces it: *"📋 Found relevant prior art: {filename}"* and injects the guidance into the current context.

If no match is found, the agent silently continues. This is important — the pre-search should be invisible when it doesn't find anything. No one wants to see "I checked and found nothing" on every task.

### The Compounding Math

Over time, the solutions store grows. Each new entry makes the next similar problem cheaper to solve. But more importantly, the *quality* of solutions improves — because each new solution can reference and build on previous ones. Solution #3 for a category isn't written from scratch; it's written with awareness of Solutions #1 and #2.

This is genuine compounding. Not linear accumulation (1, 2, 3, 4…) but exponential improvement (1, 2, 4, 8…) as solutions cross-reference and reinforce each other.

---

## 4. The Deletion Test

CEP v3.19 introduced a deceptively simple quality principle for skill instructions:

> *"If removing an instruction wouldn't change the output, it's a no-op — delete it."*

This is the **Deletion Test**, and it's one of the most practically useful ideas in the entire framework.

The problem it solves is **instruction bloat**. AI agent configurations tend to accumulate instructions over time. Each new edge case gets a new rule. Each new failure mode gets a new guard. Eventually, the configuration file is 500 lines of instructions, half of which are redundant, contradictory, or simply ignored by the model.

The Deletion Test provides a systematic way to prune this bloat:

- **Delete generic adjectives**: Words like "comprehensive", "thorough", "carefully" don't change model behavior. They're the instruction equivalent of empty calories. "Carefully analyze the code" produces the same output as "analyze the code." Delete them.

- **Delete instructions that repeat existing context**: If the system prompt already says "follow the project's coding conventions," a skill that also says "follow the project's coding conventions" is a no-op. Delete it.

- **Every instruction must verifiably change behavior**: If you can't point to a specific, observable difference in the agent's output that results from an instruction, that instruction is noise. Delete it.

This principle has implications beyond CEP. It's a general hygiene practice for anyone writing AI agent configurations, system prompts, or skill files. The Deletion Test is to prompt engineering what "YAGNI" is to software engineering — a systematic bias toward less.

---

## 5. CONCEPTS.md and the Repo Profile Cache

### CONCEPTS.md: The Shared Vocabulary

One of the subtler problems in AI-assisted development is **terminology drift**. The developer calls it a "subscription." The AI calls it a "membership." The codebase uses "plan." These are all the same thing, but the inconsistency creates confusion, naming collisions, and bugs.

CEP's solution is the `CONCEPTS.md` file — a shared vocabulary document placed at the project root that defines project-specific terminology:

```markdown
# Project Concepts

## Subscription
A recurring billing arrangement between a user and the platform.
NOT the same as a "plan" (which defines pricing tiers) or a
"membership" (which defines access levels).
```

During the grounding phase of any skill execution (brainstorm, plan, work, review, compound), if a `CONCEPTS.md` exists, the agent **must** read it. This ensures that all agents — across all skills, across all sessions — use the same vocabulary.

The mechanism is simple, but the problem it solves is real. In large codebases with multiple contributors (human or AI), terminology drift is a constant source of friction. `CONCEPTS.md` is a low-cost, high-impact intervention.

### Repo Profile Cache

CEP also maintains a **Repo Profile Cache** — a git-keyed cache of project characteristics. This includes things like:

- Primary language and framework
- Test runner and conventions
- Directory structure patterns
- Code style preferences

The cache is keyed to the git repository, so it persists across sessions and is invalidated when the repository changes significantly. This prevents the agent from re-discovering basic project facts on every interaction.

The Profile Cache is less glamorous than the Compound step, but it solves a real usability problem: without it, the agent wastes time (and context window) re-learning that "this project uses pytest" and "this project uses tabs, not spaces" on every session.

---

## 6. The Behavior Change Contract

CEP v3.19 introduced a requirement that any behavior change must come with **verification evidence**:

- The names and results of relevant tests
- New or modified tests that cover the change
- Red/green failure evidence
- Output from verification runs

The rule is stark: **no verification_evidence = unverified = untrustworthy**.

This is CEP's answer to a common failure mode in AI-assisted development: the agent says "done," but the definition of "done" is "I wrote the code." Not "the tests pass." Not "the feature works." Not "I verified my changes against the requirements." Just "I wrote the code."

The Behavior Change Contract makes verification a mandatory deliverable, not an optional add-on. You can't claim a behavior change without proving it works.

---

## 7. C31 Integration: From Plugin to Philosophy

The relationship between CEP and the C31 configuration (covered in earlier parts of this series) is one of the more interesting stories in the framework ecosystem.

C31 didn't just *adopt* CEP — it **absorbed** it. The integration went deep:

### Direct Adoption

- `ce-compound` became `C31-compound`: The entire compound workflow was adopted wholesale, with C31-specific customizations layered on top.
- The full core loop (brainstorm → plan → work → simplify → review → compound) was integrated into C31's workflow engine.
- CEP's skills became C31 skills, maintaining their original structure but operating within C31's session management and state persistence framework.

### Principles Imported from v3.19

C31 imported four specific principles from CEP v3.19:

1. **The Deletion Test**: Applied not just to skill instructions but to all C31 configuration — rules, instincts, and session state.
2. **CONCEPTS.md grounding**: Made mandatory in C31's skill execution protocol.
3. **Inline the Trigger, Not the Content**: YAML frontmatter contains only trigger information; execution logic lives in the body; large reference material goes to `references/` subdirectories.
4. **Runtime vs. Authoring Separation**: Runtime rules (how the agent behaves) are separated from authoring rules (how to write skills), preventing circular dependencies.

### The Meta-Lesson

The C31 integration demonstrates a meta-property of well-designed frameworks: they're **composable**. CEP wasn't designed as a monolithic system that demands exclusive control. It was designed as a set of principles and skills that could be adopted individually, mixed with other frameworks, and customized for specific contexts.

This composability is itself a form of compounding. CEP's ideas don't just compound within a single project — they compound across the ecosystem, as other frameworks adopt and adapt them.

---

## 8. Limitations

CEP is not without weaknesses, and intellectual honesty demands we name them:

### Documentation Overhead

The Compound step adds time to every task. For small fixes — a one-line typo correction, a simple config change — the documentation overhead can exceed the value of the fix itself. CEP doesn't have a clear threshold for when compounding is worth the cost, which can lead to either over-documentation (every trivial fix gets a solution doc) or under-documentation (developers skip the compound step because "this one's too small").

### Index Maintenance

The `INDEX.md` file is a single point of failure. If it falls out of sync with the actual solution documents — entries that point to deleted files, documents that exist without INDEX entries — the entire retrieval system degrades. CEP provides no automated consistency checking for the INDEX.

### Cold Start Problem

A new project has no solutions store. The compound loop provides zero value until enough solutions have accumulated to make pre-search useful. For short-lived projects (prototypes, hackathon entries, one-off scripts), the compound overhead is pure cost with no return.

### Vocabulary Enforcement

`CONCEPTS.md` defines terms, but it doesn't *enforce* them. An agent can read `CONCEPTS.md` and then still use inconsistent terminology in its output. The mechanism relies on the model's ability to internalize and apply the vocabulary — which is generally good but not guaranteed.

### Simplify Step Noise

The three parallel reviewer agents in the Simplify step can generate conflicting recommendations. The code-reuse reviewer might suggest extracting a function, while the efficiency reviewer might argue that the function call overhead isn't worth it. CEP's merge process handles obvious contradictions, but subtle conflicts can slip through.

### Scalability of Solutions Store

As the solutions store grows, keyword-based retrieval becomes less precise. With 10 solution documents, keyword matching is nearly perfect. With 1,000, you start getting false positives — irrelevant documents that happen to match keywords. CEP doesn't provide a semantic search mechanism for the solutions store, relying entirely on keyword matching against the INDEX.

---

## 9. Verdict

CEP's contribution to the AI engineering framework landscape is unique and genuine. While other frameworks focus on *how to do work* (discipline, architecture, orchestration), CEP focuses on *how to learn from work*. The compound loop is a simple idea executed with real rigor, and the results — a growing, discoverable institutional memory — are valuable in a way that's hard to achieve through other means.

The Deletion Test alone is worth the price of admission. It's a universally applicable quality principle that every framework and every prompt engineer should adopt.

But CEP is best understood as a *complement* to other frameworks, not a replacement. It provides the memory layer that other frameworks lack, but it doesn't provide the discipline layer (Superpowers), the operational layer (ECC), or the architectural layer (12-Factor Agents). The ideal setup is CEP's compound loop integrated into a broader framework — which is exactly what C31 did.

**Bottom line**: CEP is the only framework in this series that asks "what did we learn?" after every unit of work. That question, systematically asked and systematically answered, is worth more than most people realize.
