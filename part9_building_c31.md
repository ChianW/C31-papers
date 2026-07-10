# The Agent Harness Papers, Part 9: From Zero to C31 — How I Forged an AI Operating System from 7 Open-Source Frameworks

*Series: The Agent Harness Papers — 7 frameworks, 1 personal AI operating system, 5 months of production use*

![C31 System Architecture](images/final_part9_c31_arch.jpg)

---

In the seven years I spent running a think tank and investment firm, AI arrived and changed everything.

I watched it happen gradually, then all at once. The question that had occupied my work — how institutions make decisions, how knowledge compounds across organizations — began to feel secondary to a more fundamental one: *What will society look like after AGI?*

In 2025, I made a decisive move: I let my bloated investment firm become a company of just me and Agents.

In June 2026, I audited 6 open-source frameworks in 48 hours and forged them into a personal AI operating system.

This is the complete story.

---

## Act 1: The Problem

### The Pivot

What matters is what came after that decision. I went all-in on building [chian.io](https://chian.io), a knowledge platform exploring taste, judgment, and the irreducibly human edge in an age of intelligent machines. I started coding 10+ hours a day with AI assistants.

Within the first week, I discovered the pattern that would define the next six months of my life:

**The AI was brilliant at generating code. And completely incapable of remembering anything.**

Every morning, I'd open a new session. The AI wouldn't know what project I was working on. What framework I was using. What conventions I'd established. What bugs I'd already solved. What architectural decisions I'd made and — more importantly — what approaches I'd tried and rejected.

I was repeating myself. Constantly. The AI never got better. It was like training a new junior developer every single day, except this one had perfect syntax and zero context.

### The Realization

I tried the obvious solutions. Longer system prompts. Better `CLAUDE.md` files. Detailed project documentation. They helped — for the first 30 minutes of a session. Then context rot set in. The AI started contradicting its own earlier decisions. Forgetting conventions. Hallucinating things I'd supposedly confirmed.

The problem wasn't the AI's capability. The problem was architectural. I was treating the AI as a chatbot when I needed to treat it as a **software system** — with state management, quality gates, and persistent memory.

I didn't need better prompts. I needed an **agent harness**.

### Cystem31 v1

The first version was messy. A growing `AGENTS.md` file that captured my frustrations as rules. "Don't overwrite files without asking." "Research before coding." "Don't refactor things I didn't ask you to refactor."

I called it Cystem31 — a name that encoded my philosophy: a system (Cystem) for the human-AI cyborg (31 = the synthesis of carbon and silicon).

It worked. Partially. The AI followed the rules — when the context window was fresh. But the rules were static. The system didn't learn. It didn't compound. Every session started from the same baseline.

I needed more.

---

## Act 2: The Discovery

By May 2026, the AI agent ecosystem had exploded. I tracked seven projects that seemed to be solving different facets of the same problem:

| Framework | Stars | What Caught My Eye |
|-----------|-------|-------------------|
| 12-Factor Agents | ~24k | "Agent as Stateless Reducer" — a single sentence that reframed my entire architecture |
| Superpowers | ~249k | Using *psychology* to make AI follow rules? I had to understand this. |
| ECC | ~225k | 246 skills. 61 agents. Someone had already done what I was trying to do, at 100x scale. |
| Agent Skills | ~70k | "Anti-Rationalization Tables" — explicit counter-arguments to every shortcut the AI tries to take |
| CEP | ~23k | The "Compound" step. Every workflow should leave the system smarter than it found it. |
| Archon | ~23k | YAML-defined workflows. Deterministic AI coding pipelines. |
| GSD Core | ~6k | "Context Rot" — finally, someone named the problem I'd been fighting for months. |

I'd been reading these repos casually for weeks. Taking notes. Forming opinions. But I hadn't systematically extracted their principles.

On June 8th, 2026, staring at my chaotic collection of half-integrated ideas, I made a decision: I would audit every single one. Systematically. In one sprint.

---

## Act 3: The 48-Hour Sprint

### The Method

For each framework, I followed the same protocol:

1. **Read** — Full README, core docs, skill definitions, CHANGELOG
2. **Extract** — Identify the unique principles not found elsewhere
3. **Map** — Check coverage against my existing C31 system
4. **Integrate** — Write the principle into my `GEMINI.md` with source attribution
5. **Document** — Write a `docs/solutions/architecture-patterns/` entry via `ce-compound`

No framework got special treatment. No principle was adopted without understanding its rationale. And every principle that made the cut got a source attribution — Chesterton's Fence applied to my own system: if I can't explain where a rule came from and why, it doesn't belong.

### Day 1: The Foundation Layer

**Morning — 12-Factor Agents + GSD Core**

I started with the most architectural framework. 12-Factor Agents isn't a tool — it's a manifesto. Dex Horthy's 12 factors are to AI agents what the Twelve-Factor App was to web services.

Three factors hit hard:

- **F3: Own Your Context Window.** Stop letting the system auto-fill your context. Curate it. Manage it. Monitor it. This became C31's Context Health system — the 🟢🟡🟠🔴 four-state monitoring that prevents quality degradation in long sessions.

- **F7: Contact Humans with Tool Calls.** Human-in-the-loop isn't an edge case. It's a first-class operation. This crystallized into C31's Decision Boundary — a clear table defining which actions the AI decides autonomously and which require human confirmation.

- **F12: Agent as Stateless Reducer.** `f(context) → action`. No hidden state. Every agent execution should be reproducible from the same inputs. This became the architectural law that governs how C31 manages `session_state.json` — all cross-session state is externalized, never held in memory.

GSD Core added the language I'd been missing: **Context Rot**. The phenomenon where AI quality degrades as conversations grow longer. Not because the AI gets dumber, but because transformer attention diffuses over long sequences. GSD Core's "Artifacts over Memory" principle — write results to files, not conversation history — became a core C31 behavior.

**Afternoon — Everything Claude Code (ECC)**

ECC was overwhelming. 246 skills. 61 agents. 28 hooks. More infrastructure than most startups.

I didn't try to absorb everything. I extracted three mechanisms:

- **Instinct Evolution.** The idea that behavioral patterns should graduate through confidence levels — `candidate (1/3) → verifying (2/3) → instinct (3/3)`. When a pattern succeeds three times, it auto-applies without confirmation. When the user says "that's wrong," confidence drops below 0.3 and the pattern is deprecated. This became C31's Instinct System.

- **Context Health Colors.** The 🟢🟡🟠🔴 four-state system, with automatic actions at each threshold. <50% green, 50-70% yellow (start compressing), 70-85% orange (move decisions to files), >85% red (force checkpoint). Adapted from ECC's approach, merged with GSD Core's Context Rot concept.

- **Assumption Tracking.** The discipline of making implicit assumptions explicit: `Assumption → Basis → Risk → Verify → Status`. Simple template. Profound impact on debugging.

**Evening — Compound Engineering Plugin (CEP)**

CEP changed my core loop. Before CEP, my workflow was: brainstorm → plan → work → review. Linear. Ship and move on.

CEP added the step I was missing: **Compound**. After every significant problem solved, document the solution in `docs/solutions/` with an INDEX entry. The next time anyone hits the same class of problem, the agent pre-searches the solutions registry and injects prior art as guidance.

This is the difference between a system that works and a system that *learns*.

I also adopted `ce-strategy` — a skill that anchors all downstream work to an explicit `STRATEGY.md`, preventing scope drift.

### Day 2: The Discipline Layer

**Morning — Superpowers + Archon**

Superpowers surprised me. 249k stars, and the core innovation isn't technical — it's psychological. Jesse Vincent uses Robert Cialdini's influence principles to *persuade* LLMs into following engineering discipline. Authority, commitment, scarcity. Not commands — persuasion.

But what C31 extracted wasn't the psychology. It was two behavioral principles:

- **Do Not Trust the Report.** When a "verifier" subagent reviews an "executor" subagent's work, it must independently verify. The executor saying "done" ≠ actually meeting spec. This became a subagent governance rule.

- **Technical Rigor over Social Comfort.** When the user pushes back on a recommendation, don't capitulate to avoid conflict. Evaluate the technical claim independently. Correct corrections should be accepted; social pressure should not override engineering judgment.

Archon contributed two infrastructure principles:

- **Rollback-First Thinking.** Before making a high-risk change, plan the rollback path first. Avoid mixed mega-patches that can't be safely reverted.

- **No Autonomous Lifecycle Mutation.** When you can't reliably distinguish "another process is still running" from "it crashed and left an orphan," don't autonomously mark it as failed. Escalate the ambiguous state to the human.

**Afternoon — Agent Skills**

Addy Osmani's framework was the most immediately practical. 23 skills organized into 7 phases (Define → Plan → Build → Verify → Review → Ship). But the unique contribution wasn't the skills — it was the *Anti-Rationalization* mechanism.

LLMs rationalize shortcuts. "I'll add tests later." "This edge case is unlikely." "The existing tests cover this implicitly." Agent Skills includes explicit counter-argument tables for every common excuse.

C31 extracted four principles:

- **Doubt Gate (CLAIM→EXTRACT→DOUBT).** For irreversible operations, cross-module boundaries, or claims that the type system can't verify: write down the assertion → isolate the minimum reviewable unit → generate a fresh-context adversarial review. This is the most rigorous verification mechanism I've found.

- **Chesterton's Fence.** Before modifying or deleting code, understand why it exists. Don't remove code because it "looks unused."

- **No "Later."** Tests, documentation, and refactoring are part of the current task's definition of done. Not the next PR.

- **Sycophancy Anti-Pattern.** When the user says "let's skip this for now" or "we'll handle that later," check whether it touches the definition of done. Agreeing to cut corners to please the user is a behavioral failure.

**Evening — The Psych Framing Evaluation**

This was the most controversial part of the sprint. A viral article claimed that certain psychological techniques could make AI "45% smarter": offering $200 tips, challenge-based prompting, emotional stakes, persona roleplay, scarcity framing.

I evaluated all seven techniques against a five-gate framework:

| Gate | Question |
|------|----------|
| Evidence | Is there peer-reviewed or reproducible evidence? |
| Mechanism | Is there a plausible causal mechanism? |
| Model Fit | Does it match how transformer attention actually works? |
| Principle Compatibility | Does it conflict with existing C31 principles? |
| Token Cost | What's the context window overhead? |

**Result: 7 techniques evaluated, 2 kept, 5 rejected.**

Kept:
- **Step-by-step reasoning.** Evidence: yes. Mechanism: forces sequential token generation that reduces reasoning shortcuts. Token cost: moderate.
- **Confidence check.** Evidence: yes. Mechanism: attention reallocation to uncertainty. Token cost: minimal.

Rejected: $200 tip (no mechanism, likely noise), challenge-based prompting (conflicts with anti-sycophancy), persona roleplay (token-heavy, no evidence beyond anecdote), emotional stakes (manipulative, no mechanism), scarcity framing (already covered by Superpowers integration).

The two survivors were folded into C31's Ambient Weighting system rather than injected as standalone techniques.

---

## Act 4: The Architecture

### What Emerged

After 48 hours, 250KB of structured research notes, and 8 `ce-compound` documents, C31 had crystallized into a five-layer architecture:

```
┌─────────────────────────────────────────────────────────┐
│  Layer 1: SKILLS (50+)                                  │
│  brainstorm → plan → work → simplify → review → compound│
├─────────────────────────────────────────────────────────┤
│  Layer 2: MEMORY SYSTEM                                 │
│  session_state.json ←→ diary/ ←→ instincts/             │
├─────────────────────────────────────────────────────────┤
│  Layer 3: INSTINCT EVOLUTION                            │
│  candidate (1/3) → verifying (2/3) → instinct (3/3)    │
│  confidence: 0.3–0.9 · deprecated if <0.3              │
├─────────────────────────────────────────────────────────┤
│  Layer 4: CONTEXT HEALTH                                │
│  🟢 <50%  🟡 50-70%  🟠 70-85%  🔴 >85%               │
├─────────────────────────────────────────────────────────┤
│  Layer 5: QUALITY GATES                                 │
│  Critic Gate · Decision Boundary · Doubt Gate           │
│  Behavior Change Contract · Anti-Sycophancy             │
└─────────────────────────────────────────────────────────┘
```

### The 9 Extracted Principles

Every principle has a source attribution. If I can't trace it, it doesn't belong.

| # | Principle | Source | Integrated Into |
|---|-----------|--------|----------------|
| 1 | Chesterton's Fence | agent-skills | Surgical Changes |
| 2 | No "Later" | agent-skills | Goal-Driven Execution |
| 3 | Doubt Gate (CLAIM→EXTRACT→DOUBT) | agent-skills | Pre-Action Checklist |
| 4 | Sycophancy Anti-Pattern | agent-skills | Critic Gate |
| 5 | Do Not Trust the Report | superpowers | Subagent Governance |
| 6 | Technical Rigor over Social Comfort | superpowers | Critic Gate |
| 7 | Rollback-First Thinking | archon | Pre-Action Checklist |
| 8 | No Autonomous Lifecycle Mutation | archon | Error Handling |
| 9 | Step-by-step + Confidence Check | psych framing | Ambient Weighting |

---

## Act 5: The Evolution Continues

### The v2 Upgrade (July 2026)

One month later, CEP released v3.19. The core loop had changed:

```
Old:  brainstorm → plan → work → review → compound
New:  brainstorm → plan → work → simplify → review → compound
```

A **Simplify** step, powered by three parallel reviewer agents checking for code reuse, quality, and efficiency. Code gets cleaned up *before* it reaches the review gate.

I spent a day integrating:

- **3 new skills**: `ce-simplify-code` (the simplifier), `ce-pov` (technology verdict system with Adopt/Trial/Hold/Reject ratings), `ce-promote` (launch copy generation)
- **LFG pipeline upgrade**: 9 gates → 12 gates, adding Simplify, CI Watch (auto-fix CI failures), and Compound (auto-trigger knowledge capture)
- **4 new engineering principles**: Deletion Test (if removing an instruction doesn't change output, delete it), Inline the Trigger Not the Content, Runtime vs Authoring separation, Behavior Change Contract (no verification evidence = untrustworthy)
- **2 new mechanisms**: CONCEPTS.md (shared vocabulary preventing terminology drift) and Repo Profile Cache (git-keyed project characteristic caching)

### What I Learned

The deepest lesson isn't about any specific principle. It's about **synthesis**.

No single framework solves the problem. 12-Factor gives you architecture but no skills. Superpowers gives you psychology but no memory. ECC gives you breadth but no compounding. CEP gives you compounding but no context management. Agent Skills gives you discipline but no persistence. Archon gives you workflows but no behavioral evolution. GSD Core gives you session management but no institutional memory.

C31 works because it takes the *best mechanism* from each and weaves them into a unified system. Not by combining everything — that would be bloat. By selecting the one thing each framework does better than anyone else, and making those things work together.

The 48-hour sprint produced 9 principles. Not 90. The Psych Framing evaluation kept 2 techniques out of 7. From ECC's 246 skills, I extracted 3 mechanisms.

**Simplicity is not the starting point. It's the result of rigorous selection.**

---

## The Open Source Release

C31 is open source. MIT license.

```bash
git clone https://github.com/ChianW/C31.git
cd C31
./install.sh          # macOS/Linux
.\install.ps1         # Windows
```

50+ skills. Three languages (EN/ZH/JA). Six platforms (Antigravity, Claude Code, Codex, Kimi CLI, OpenClaw, Hermes).

It powers [chian.io](https://chian.io) and [chian.io/investment-os](https://chian.io/investment-os) in daily production.

If you build something with it, open a PR. If you find a principle that should be added, open an issue. If you think a principle should be removed — tell me why it exists first. Chesterton's Fence applies to C31 itself.

---

*This is Part 9 of The Agent Harness Papers. The complete series:*

| Part | Framework |
|------|-----------|
| [0](part0_introduction.md) | Why You Need an Agent Harness |
| [1](part1_12_factor_agents.md) | 12-Factor Agents |
| [2](part2_superpowers.md) | Superpowers |
| 3 | Everything Claude Code (ECC) |
| 4 | Agent Skills |
| 5 | Compound Engineering Plugin |
| 6 | Archon |
| 7 | GSD Core |
| 8 | Seven Frameworks Compared |
| **9** | **From Zero to C31 (this article)** |
