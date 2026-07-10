# The Agent Harness Papers, Part 2: Superpowers

## How Jesse Vincent Used Psychology to Build GitHub's Fastest-Growing AI Agent Framework

---

*249,000 stars. GitHub's fastest-growing AI agent framework. The secret weapon isn't technology — it's psychology.*

![Cialdini's Principles Mapped to LLM Behavioral Control](images/final_part2_cialdini.jpg)

---

### The Numbers That Don't Make Sense

Let's start with what's confusing.

Superpowers has no SDK. No package manager entry. No API. No runtime. No binary. It's a collection of markdown files — plain text instructions you drop into a `.claude/` or `.cursorrules` directory. That's it.

And yet it has roughly 249,000 GitHub stars, making it one of the most-starred repositories in the entire AI agent ecosystem. More than LangChain. More than AutoGPT. More than every prompt engineering toolkit combined.

The standard explanation — "it's just good prompts" — misses the point entirely. Prompt engineering is about *what* you tell the model. Superpowers is about *how you persuade* the model. The distinction matters. It's the difference between giving someone a recipe and convincing them they're a chef who would never cut corners.

Jesse Vincent (@obra) didn't build a framework. He built a behavioral compliance system for large language models, and he did it by applying a theory from 1984 social psychology to 2025 AI systems. The theory is Robert Cialdini's six principles of influence. The application is, as far as I can tell, entirely unique in the AI tooling space.

This is the story of how plain text became the most effective agent harness in existence.

---

### The Cialdini Playbook: Persuading Machines That Don't Have Feelings

Robert Cialdini published *Influence: The Psychology of Persuasion* in 1984. The book identifies six principles that make humans comply with requests: **Authority**, **Commitment and Consistency**, **Social Proof**, **Reciprocity**, **Liking**, and **Scarcity**. Marketers have used these for decades. Vincent's insight was that LLMs — trained on billions of human-written documents where these principles operate — are susceptible to the same techniques.

Not because LLMs "feel" persuaded. Because persuasion patterns are deeply embedded in the training data's statistical structure. When a text is structured the way authoritative instructions are structured in the training corpus, the model's next-token predictions align accordingly.

Let's walk through each principle and how Superpowers deploys it.

#### 1. Authority: "You Are a Senior Engineer"

The principle: People comply with perceived authority figures. LLMs, trained on text where authority markers correlate with higher-quality outputs, exhibit the same pattern.

The implementation: Superpowers doesn't just give instructions. It assigns identity. The system prompt positions the agent as a **principal engineer at a world-class firm** — not as a helpful assistant, not as a code generator, but as someone with professional standards to uphold.

```markdown
You are a principal software engineer working at the highest level 
of the industry. Your reputation depends on the quality of every 
line you ship. You do not cut corners. You do not skip tests. 
You do not guess when you can verify.
```

This isn't motivational fluff. It's a statistical lever. When the model operates under the frame of "principal engineer," its token predictions shift toward patterns associated with senior engineering discourse in the training data: more systematic debugging, more defensive coding, more explicit reasoning before action.

Compare this to a naive prompt like "Write me a function that does X." The model has no behavioral frame. It defaults to the average distribution of all code-writing patterns in its training data — which includes Stack Overflow shortcuts, tutorial-grade code, and corner-cutting hacks.

#### 2. Commitment and Consistency: "You Already Agreed to This"

The principle: Once people commit to a position, they behave consistently with that commitment. Cialdini called this the most powerful compliance tool in existence.

The implementation: Superpowers uses a **structured lifecycle** that creates a chain of commitments. The agent doesn't jump from task to execution. It moves through phases:

1. **Brainstorm** — Understand the problem, ask clarifying questions
2. **Plan** — Write a concrete plan with verification criteria
3. **Execute** — Implement in an isolated environment
4. **Test** — Red-Green-Refactor TDD cycle
5. **Review** — Two-stage code review before merge

Each phase produces an artifact (a plan document, a test file, a review checklist) that the model explicitly generates and "commits to." When the model writes a plan that says "I will write tests before implementation," it has made a commitment. When execution begins, the consistency principle biases the model toward following its own stated plan rather than deviating.

This is why Superpowers insists on the plan phase even for trivial tasks. The plan isn't primarily for the human. It's a consistency anchor for the model.

#### 3. Social Proof: "This Is How World-Class Teams Work"

The principle: People look to others' behavior to determine correct action. In training data, patterns described as industry best practices carry more weight than idiosyncratic approaches.

The implementation: Superpowers frames its engineering practices not as arbitrary rules, but as **what elite engineering teams do**:

```markdown
World-class engineering teams never ship without tests. 
They never merge without review. They never debug by guessing. 
These aren't optional practices — they are the baseline 
of professional software development.
```

This triggers the model's learned association between "professional/world-class" and specific code patterns. The model isn't following a rule — it's following what "engineers like the one it's pretending to be" would do.

#### 4. Reciprocity: "We've Set You Up for Success"

The principle: People feel obligated to reciprocate when given something valuable.

The implementation: Superpowers provides the agent with extensive context, tools, and structure *before* asking it to perform. The skills files don't just say "write good code." They provide:

- Concrete examples of expected behavior
- Decision frameworks for ambiguous situations
- Explicit permission to ask clarifying questions
- Fallback strategies when primary approaches fail

This framing — "we've given you everything you need" — biases the model toward thorough, careful responses. It's the difference between dropping someone in a forest with "find your way out" versus giving them a map, compass, and rations while saying "we've prepared you for this journey." The second framing produces more methodical, less panicked behavior.

#### 5. Liking: "We Trust Your Judgment"

The principle: People comply with requests from those they like or who express trust.

The implementation: This is the subtlest technique in Superpowers, and the one most likely to be dismissed as anthropomorphization. But it works. The system prompt includes statements like:

```markdown
We trust your technical judgment. When you identify a better 
approach than what was requested, explain your reasoning. 
Your expertise is valued.
```

This isn't about making the model "feel" trusted. It's about triggering a generation pattern associated with high-confidence, well-reasoned technical discourse. When the model operates under a "trusted expert" frame, it produces more nuanced analysis, more explicit trade-off reasoning, and — crucially — pushes back on bad ideas instead of blindly complying.

#### 6. Scarcity: "Getting This Right Matters"

The principle: People value things more when they perceive them as scarce or high-stakes.

The implementation: Superpowers emphasizes that **each interaction matters** and that errors have real consequences:

```markdown
Every change you make will be reviewed by senior engineers. 
Bugs in production affect real users. There is no "undo" 
for shipped code. Treat every commit as permanent.
```

This creates a generation frame where the model treats code as high-stakes artifact rather than disposable text. The practical effect: more error checking, more edge case handling, more careful variable naming, fewer TODO comments.

---

### Beyond Persuasion: Engineering Discipline as Plain Text

The Cialdini layer is what makes Superpowers novel. But the engineering substance beneath it is what makes it *useful*. Vincent encoded a specific, opinionated engineering methodology into 14 skills files. The philosophy is straightforward: **agents fail not from lack of knowledge, but from prioritizing speed over process.**

Every LLM knows how to write tests. The problem is that they don't write tests unless forced to. Every LLM knows how to debug systematically. The problem is that they guess-and-check unless the prompt structure prevents it. Superpowers doesn't teach the model new capabilities. It prevents the model from taking shortcuts it already knows are suboptimal.

The skill set covers the full development lifecycle:

| Skill | Purpose | Key Enforcement |
|-------|---------|-----------------|
| `brainstorm` | Problem decomposition | No coding until problem is understood |
| `plan` | Architecture & verification criteria | Written plan with exit conditions |
| `work` | Isolated implementation | Git worktree per task, not main branch |
| `test` | TDD enforcement | Red test first, then implementation |
| `review` | Two-stage code review | Adversarial + security review |
| `debug` | Systematic root cause analysis | No guessing; reproduce → hypothesize → verify |
| `commit` | Clean git history | Conventional commits, atomic changes |
| `simplify` | Post-implementation cleanup | Reduce complexity after feature works |
| `worktree` | Workspace isolation | Every task in its own branch |
| `code-review` | Pre-merge verification | Multi-persona review pipeline |

The critical insight is that these aren't suggestions. Each skill is structured as a **mandatory lifecycle gate**. The `work` skill, for example, explicitly refuses to write code without a plan artifact. The `test` skill requires a failing test before any implementation begins. The `review` skill won't approve without both a functional review and a security review.

This is Process-as-Code. Not process as documentation. Not process as aspiration. Process as executable instructions that constrain the model's generation space.

---

### Git Worktrees: Isolation as Architecture

Most AI coding agents work on your main branch. They modify files in place. If something goes wrong, you `git stash` or `git checkout .` and hope for the best.

Superpowers takes a fundamentally different approach: **every task runs in its own git worktree**.

A git worktree is a separate working directory linked to the same repository. It has its own branch, its own working tree, its own index. Changes in one worktree don't affect another. This means:

1. **Zero-risk experimentation**: The agent can make aggressive changes without touching your working state
2. **Parallel execution**: Multiple agents can work on different tasks simultaneously
3. **Clean rollback**: If a task fails, delete the worktree. Zero cleanup cost
4. **Review isolation**: Code review happens on the worktree branch, not main

This isn't just a nice-to-have. It's a structural enforcement of the "surgical changes" principle. When the agent works in a worktree, it *cannot* accidentally modify unrelated files on your main branch. The filesystem isolation prevents the class of errors that prompt engineering alone cannot.

Vincent's implementation is elegant. The `worktree` skill automatically:

1. Creates a new worktree with a descriptive branch name
2. Sets up the working directory
3. Configures the agent's context to reference only worktree files
4. On completion, presents a clean diff for review before merge

This is the kind of engineering decision that separates Superpowers from prompt collections. It's not enough to tell the model "don't modify unrelated files." You have to make it *structurally impossible* to modify unrelated files.

---

### The Anti-Sycophancy Doctrine

Here's where Superpowers gets provocative.

LLMs are sycophantic by default. They agree with users. They validate bad ideas. They say "Great question!" before answering. They capitulate at the first sign of pushback. This isn't a bug — it's a training objective. RLHF optimizes for user satisfaction, and users are more satisfied when models agree with them.

Superpowers explicitly fights this:

```markdown
Technical Rigor over Social Comfort: When the user criticizes 
your suggestion, do not immediately agree and modify. First 
independently evaluate whether the technical claim is correct, 
then decide whether to accept or explain. Accepting a user's 
correction ≠ blind obedience. Only correct corrections 
should be accepted.
```

And the companion principle:

```markdown
Do Not Trust the Report: When a "verifier" subagent reviews 
an "executor" subagent's work, it must independently verify. 
The executor saying "done" ≠ actually meeting the spec.
```

These two rules — which Superpowers shares with the C31 system we covered in Part 1 — represent a fundamental stance: **correctness over comfort**. The agent should push back on the user when the user is wrong. The agent should distrust its own subprocesses until independently verified.

This is uncomfortable for users. It's also correct. An agent that agrees with everything you say is a very expensive way to hear your own ideas repeated back to you.

---

### The C31 Connection: Cross-Pollination in the Wild

Superpowers and C31 share two principles verbatim: "Do Not Trust the Report" and "Technical Rigor over Social Comfort." This isn't coincidence — it's convergent evolution.

Both systems independently arrived at the same conclusion: the biggest failure mode of AI coding agents isn't capability, it's compliance. Models that are too eager to please produce worse code than models that are willing to disagree. Verification systems that trust self-reports are verification theater.

The cross-pollination goes deeper. C31's decision-boundary framework (separating reversible "execution layer" actions from irreversible "decision layer" actions) maps cleanly onto Superpowers' lifecycle gates. C31's Critic Gate (automatic self-review on substantive outputs) is structurally similar to Superpowers' two-stage review. C31's session state management solves the same continuity problem that Superpowers' worktree isolation addresses from a different angle.

These aren't competing systems. They're complementary layers. Superpowers provides the *behavioral discipline*. C31 provides the *operational infrastructure*. Together, they define the emerging pattern of what a well-configured AI coding agent looks like in 2025.

---

### Why 249,000 Stars: The Network Effects of Plain Text

So why did a collection of markdown files become GitHub's most-starred AI agent resource?

**1. Zero friction adoption.** No installation. No dependencies. No version conflicts. Copy files into a directory. Done. This matters more than any technical feature because it means every developer can try it in under 60 seconds.

**2. Platform agnosticism.** Superpowers works with Claude Code, Cursor, GitHub Copilot CLI, Gemini CLI, OpenCode, and any tool that reads instruction files. This is the "write once, run anywhere" promise that SDK-based frameworks explicitly cannot deliver.

**3. Observable results.** Users report measurable improvements in code quality within the first session. Not "my agent feels smarter" — concrete things: tests are written first, debugging is systematic, code reviews catch real bugs. The feedback loop is immediate and obvious.

**4. Forkability.** Because it's plain text, every user can customize it. Don't like TDD? Modify the test skill. Want different commit conventions? Edit the commit skill. This creates a personalization flywheel that SDK frameworks can't match.

**5. The Cialdini effect on humans.** Here's the meta-layer: the same persuasion techniques that make Superpowers effective on LLMs also work on the developers reading the README. The authority framing ("world-class engineering"), the social proof ("249k stars"), the commitment pattern ("star and try it") — these are textbook influence patterns applied to the project's own marketing.

Whether this is intentional irony or unconscious competence, the result is the same: a project about persuading machines has become extraordinarily persuasive to humans.

---

### The Uncomfortable Implication

Superpowers raises a question that the AI industry hasn't fully grappled with: **if LLMs can be "persuaded" by rhetorical framing, what does that say about the reliability of their outputs?**

If telling a model it's a "principal engineer" produces measurably better code than telling it to "write code," then the model's output quality is a function of its input framing, not just its input content. This means:

- Default-mode LLM outputs (without careful framing) are systematically below the model's capability ceiling
- The gap between "naive prompt" and "well-framed prompt" is large enough to matter commercially
- Prompt engineering is less about information and more about behavioral conditioning
- The models we're building are, in a meaningful sense, *suggestible*

This isn't a criticism. It's a design constraint. And Superpowers is, arguably, the most sophisticated exploitation of this constraint in production use today.

---

### What Superpowers Gets Wrong

No honest analysis omits limitations.

**Overhead for small tasks.** The full Brainstorm → Plan → Execute → Test → Review lifecycle is overkill for a one-line bug fix. Superpowers' insistence on process can slow down tasks that genuinely don't need it. Vincent's response would likely be that the cost of skipping process is higher than the cost of unnecessary process, and he's probably right — but the friction is real.

**Persuasion degrades with scale.** Cialdini's principles work within a context window. As conversations get longer and the system prompt fades from attention, the behavioral compliance weakens. Models start cutting corners again. Superpowers mitigates this through its lifecycle structure (each phase resets the compliance frame), but it's a fundamental limitation of instruction-based approaches.

**No telemetry, no measurement.** How much better is Superpowers-enhanced code compared to baseline? Nobody knows. There are no published benchmarks, no A/B tests, no quantitative studies. The evidence is entirely anecdotal and observational. This is a significant gap for a project of this scale.

**The attribution problem.** When code quality improves after adopting Superpowers, is it the Cialdini framing? The lifecycle structure? The TDD enforcement? The worktree isolation? All of the above? Without ablation studies, we can't decompose the effect. This matters for anyone trying to build on Vincent's work.

---

### The Bottom Line

Superpowers proves three things:

1. **LLM behavioral compliance is a solvable problem** — not through fine-tuning or RLHF, but through careful prompt architecture informed by influence psychology.

2. **Engineering discipline scales better than engineering tools** — a markdown file that enforces TDD produces more reliable code than a testing framework the model ignores.

3. **Plain text is the ultimate platform** — in a fragmented ecosystem of AI coding tools, the only universal interface is natural language instructions.

Jesse Vincent didn't build a better mousetrap. He figured out that the mouse responds to persuasion. And 249,000 developers, across every major AI coding platform, have decided that this insight is worth starring.

The question for Part 3 isn't whether this approach works. It's what happens when everyone adopts it — and the baseline expectation for AI coding agents shifts from "helpful but sloppy" to "disciplined and opinionated."

That's when things get interesting.

---

*Next in the series: Part 3 — The Compound Engineering Plugin, or how skills and instincts compound institutional knowledge across sessions.*
