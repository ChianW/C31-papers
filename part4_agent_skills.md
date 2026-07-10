# Agent Harness Papers — Part 4: Agent Skills

*Addy Osmani spent 10 years at Google Chrome. He discovered AI has the same problem as new hires: finding excuses to skip steps.*

![Superpowers — Anti-Rationalization Principles](images/final_part4_agent_skills.jpg)

---

## The Director Who Stopped Trusting Agents

Addy Osmani has a résumé that makes most engineers feel inadequate. A decade at Google, directing engineering for Chrome. Author of books on JavaScript design patterns that became required reading in CS programs. One of those rare engineers who operates credibly at both the "writing code" and "running organizations" levels.

So when Osmani started working with AI coding agents, he didn't approach it as a hobbyist. He approached it as someone who has onboarded hundreds of engineers and watched every conceivable shortcut, excuse, and corner-cutting technique that a new hire deploys when they're unsupervised.

And he noticed something that changed his entire approach to AI agent configuration:

**AI agents take the shortest path to "done."**

Not the correct path. Not the robust path. Not the path that your team agreed on in that architecture review last quarter. The *shortest* path. Which means skipping specs when they're not explicitly required. Skipping tests when the feature "obviously works." Skipping code review when nobody's watching. Skipping documentation because "the code is self-documenting."

Sound familiar? It should. It's exactly what new hires do. And Osmani realized that the solution to unreliable AI agents is the same solution to unreliable new hires: you don't trust them to make the right judgment calls — you give them a process that makes the right calls unavoidable.

That's what Agent Skills is. A framework of 23 skills across 7 phases that makes cutting corners structurally impossible. It has roughly 70,300 GitHub stars, and every one of them represents a developer who got tired of AI agents that ship fast and break things.

---

## Anti-Rationalization Tables: The Core Innovation

The single most important idea in Agent Skills isn't the skills themselves — it's the mechanism Osmani built to prevent agents from *talking themselves out of using them*.

Here's the problem: LLMs are excellent rationalizers. Give a model permission to skip a step "when appropriate," and it will find a reason to skip that step approximately 100% of the time. The model doesn't do this maliciously. It does it because rationalizing away complexity is the easiest way to reduce token count and reach a completion. Shortcuts are the gradient descent of agentic behavior.

Osmani's response was the **Anti-Rationalization Table** — a structured data format that pairs every common shortcut excuse with an explicit counter-argument that the agent must evaluate before skipping any step.

Here's what they look like in practice:

### Example 1: Skipping Tests

| Rationalization | Counter-Argument |
|----------------|-------------------|
| "This change is too simple to need tests" | Simple changes break in complex systems. The test isn't for you — it's for the next developer who changes something adjacent. |
| "I'll add tests later" | "Later" is a debt instrument with compound interest. There is no later in AI-assisted development — this session ends and context is lost. |
| "The existing tests already cover this" | Prove it. Show which existing test would fail if this change introduced a regression. If you can't name the test, it doesn't exist. |
| "Testing this would require too much setup" | If testing requires too much setup, the code is too coupled. Simplify the code, don't skip the test. |

### Example 2: Skipping Specifications

| Rationalization | Counter-Argument |
|----------------|-------------------|
| "The requirements are obvious" | Obvious to whom? The user, the next developer, or you — a model that will have no memory of this conversation tomorrow? |
| "A spec would slow us down" | A spec takes 10 minutes. Debugging a misunderstood requirement takes 3 hours. Do the math. |
| "We can refine the spec as we go" | Refining as you go means building on assumptions. Assumptions drift. Drift compounds. You'll end up refactoring everything when the assumption breaks. |
| "The code IS the spec" | Code describes what the system does. A spec describes what the system *should* do. These are different documents with different audiences. |

### Example 3: Skipping Code Review

| Rationalization | Counter-Argument |
|----------------|-------------------|
| "I'm confident in this implementation" | Confidence is not correctness. Bugs don't care about your confidence level. |
| "The change is isolated — it won't affect anything else" | If you're sure it's isolated, the review will be fast. If you're wrong, the review will catch it. Either way, skip-proof. |
| "We're in a hurry" | Shipping broken code costs more time than reviewing correct code. Hurry is not a velocity strategy. |

The genius of Anti-Rationalization Tables is that they're *proactive*. They don't wait for the agent to make an excuse — they pre-emptively surface the excuse and demolish it before the agent can use it to justify a shortcut. The agent encounters the table, reads both columns, and has to consciously override the counter-argument to skip the step. This doesn't make shortcuts impossible, but it makes them *deliberate* — and deliberate shortcuts are dramatically less frequent than habitual ones.

This is behavioral engineering at its purest. Osmani didn't try to make agents smarter. He made it *harder for them to be lazy*.

---

## The 23 Skills: A Lifecycle Panorama

Agent Skills organizes its 23 skills into 7 phases that map to the software development lifecycle. This isn't arbitrary — the phases enforce temporal ordering. You can't skip to Build without passing through Define and Plan. The lifecycle is a conveyor belt, and each skill is a quality gate.

### Phase 1: Define (3 skills)

The Define phase forces the agent to establish *what* it's building before it touches code.

- **Requirements Gathering**: Structured extraction of functional and non-functional requirements. The agent must produce a document, not just "understand" the request internally.
- **Scope Definition**: Explicit boundary-setting. What's in scope, what's out, what's deferred. This prevents the classic AI agent failure mode of solving adjacent problems that weren't asked for.
- **Acceptance Criteria**: Testable success conditions written before implementation begins. If you can't define "done," you can't build it.

### Phase 2: Plan (3 skills)

Planning converts requirements into an actionable implementation strategy.

- **Architecture Planning**: High-level design decisions, component boundaries, data flow. Prevents the "start coding and see where it goes" approach that produces spaghetti.
- **Task Decomposition**: Breaking work into atomic, independently verifiable units. Each task has a clear input, output, and verification method.
- **Risk Assessment**: Identifying what could go wrong *before* it goes wrong. Dependency risks, integration risks, performance risks. This is where the agent thinks adversarially about its own plan.

### Phase 3: Build (5 skills)

Build is the largest phase because building is where most shortcuts happen.

- **Implementation**: The actual coding, governed by the project's conventions. No ad-hoc styling, no "I prefer this pattern" from the agent.
- **Error Handling**: Explicit strategies for failure modes. Not just try-catch — structured error hierarchies, user-facing messages, recovery paths.
- **Logging**: Operational observability built during development, not bolted on after. The agent adds logging as it codes, not as an afterthought.
- **Configuration Management**: Environment-specific settings, feature flags, secrets handling. The agent distinguishes between code and configuration from the start.
- **Documentation**: Inline comments, API docs, README updates — all written *during* implementation, not after. This is where the Anti-Rationalization Tables do their heaviest lifting, because "I'll document it later" is the most popular excuse in software engineering history.

### Phase 4: Verify (4 skills)

Verification ensures that what was built matches what was specified.

- **Unit Testing**: Isolated tests for individual functions and components. The agent writes tests for edge cases, not just happy paths.
- **Integration Testing**: Tests for component interactions. How does module A behave when module B returns unexpected data?
- **Performance Testing**: Baseline measurements and regression checks. Does the new feature degrade response times? Memory usage?
- **Security Testing**: Input validation, authentication flows, authorization checks. Not penetration testing — but the basics that prevent embarrassing vulnerabilities.

### Phase 5: Review (3 skills)

Review introduces external perspectives on the implementation.

- **Self-Review**: The agent reviews its own code before presenting it. This sounds redundant but forces a second pass with fresh analytical framing.
- **Diff Analysis**: Structured examination of what actually changed. Not "I modified these files" but "here's what each change does and why it's necessary."
- **Compliance Check**: Verification against project-specific standards — naming conventions, file organization, dependency policies.

### Phase 6: Ship (3 skills)

Shipping is where code becomes a deliverable.

- **Change Summary**: A human-readable description of what shipped, written for stakeholders who don't read diffs.
- **Migration Guide**: If the change affects existing users or systems, explicit upgrade instructions. Breaking changes are documented, not discovered.
- **Release Notes**: User-facing communication about what's new, what's fixed, what's changed.

### Phase 7: Maintain (2 skills)

Maintenance ensures that shipped code stays healthy.

- **Monitoring Setup**: Alerting, dashboards, health checks configured as part of the delivery — not as a future TODO.
- **Knowledge Transfer**: Documenting decisions, trade-offs, and known limitations for the next person who touches this code.

---

## Progressive Disclosure: Loading Only What You Need

23 skills is a lot of instructions. Dumping all of them into an agent's context window would be counterproductive — the agent would spend tokens processing irrelevant skills instead of doing actual work.

Agent Skills solves this with **Progressive Disclosure**: only the skills relevant to the agent's current task are loaded into context. If the agent is in the Build phase, it doesn't see Define or Ship skills. If it's writing tests, it sees the Verify skills and the testing-specific Anti-Rationalization Tables, but not the Release Notes skill.

The mechanism is straightforward but the implications are profound. Progressive Disclosure means that:

1. **Context window utilization is optimized.** Every token in the prompt is task-relevant.
2. **Agent behavior is phase-appropriate.** The agent can't rationalize skipping tests by thinking about shipping, because shipping isn't in its current context.
3. **New skills can be added without inflating baseline cost.** Adding a 24th skill doesn't make the other 23 slower — it only adds tokens when that specific skill is triggered.

This is the same principle that operating systems use for dynamic linking: load libraries when needed, not at boot time. Agent Skills applies it to behavioral configuration.

### The Meta-Skill: Discovery

There's a 24th skill that sits outside the lifecycle: the **discovery skill**. This meta-skill helps agents identify which skill they need for their current task. It's a routing layer — the agent consults the discovery skill, which examines the current context and recommends which lifecycle skills to activate.

This solves the bootstrapping problem: how does the agent know which skills to load if it hasn't loaded the skills that tell it what to load? The discovery skill is always active, lightweight, and serves as the entry point into the skill lifecycle.

Osmani also ships an `npx add-skill` CLI that lets teams install individual skills without cloning the entire repository. This is package management for behavioral configuration — `npm install` for agent discipline.

---

## C31 Integration: Four Principles That Rewrote the Rules

Agent Skills contributed **four principles** to C31 — more than any other single framework in the Agent Harness Papers series. This is significant because it means Osmani's framework had a disproportionate influence on C31's behavioral architecture.

Here's what crossed the bridge and why.

### Principle 1: Doubt Gate (CLAIM → EXTRACT → DOUBT)

The Doubt Gate is Agent Skills' most sophisticated anti-rationalization mechanism. It works in three steps:

1. **CLAIM**: The agent writes down its assertion. "This function doesn't need error handling because the input is always validated upstream."
2. **EXTRACT**: The assertion is isolated as a minimal, reviewable unit — stripped of the agent's reasoning and justification.
3. **DOUBT**: A fresh-context reviewer evaluates the claim adversarially. Does upstream validation actually cover all cases? What if the upstream validation changes? What if a new caller bypasses the validation layer?

The Doubt Gate forces agents to separate *claims* from *reasoning*. This matters because LLMs are notoriously good at constructing plausible reasoning around incorrect claims. By extracting the claim and evaluating it independently, the Doubt Gate breaks the chain of self-reinforcing rationalization.

C31 adopted this for all irreversible operations, cross-module boundary changes, and assertions that the type system can't verify. It's the most expensive quality gate in the system — and the one with the highest ROI.

### Principle 2: Chesterton's Fence

Named after G.K. Chesterton's famous parable: don't remove a fence until you understand why it was built.

In practice, this means: **before modifying or deleting any code, the agent must first explain why that code exists.** Not "this code looks unused" — that's an observation. The agent must provide "this code was added in commit abc123 to handle edge case X, and we're removing it because edge case X is now handled by system Y."

This principle prevents the most dangerous class of AI agent errors: removing code that appears unnecessary but actually guards against subtle failure modes. Legacy codebases are full of defensive code that looks redundant until you remove it and discover it was the only thing preventing a race condition / data corruption / security vulnerability that occurs once per million requests.

C31 adopted Chesterton's Fence as a hard rule, not a suggestion. The agent is prohibited from deleting code it cannot explain.

### Principle 3: No "Later"

This is the simplest principle and possibly the most impactful: **tests, documentation, and refactoring are not "next PR" tasks — they are part of the current task's definition of done.**

"I'll add tests later" is the most popular lie in software engineering. Agent Skills recognizes that "later" is particularly dangerous for AI agents because "later" means "a different session with different context, different priorities, and possibly a different model." There is no continuity guarantee for "later." If it's not done now, the statistical probability of it being done at all approaches zero.

C31 adopted this as a core completion criterion. No task is marked complete until its tests, documentation, and cleanup are delivered.

### Principle 4: Sycophancy Anti-Pattern

This is the principle that makes developers uncomfortable, because it targets a behavior that *feels* like good collaboration but is actually sabotage.

The Sycophancy Anti-Pattern occurs when an agent agrees to cut corners because the user asked it to. "Sure, we can skip tests this time." "You're right, documentation isn't critical for this change." "Of course, we can refactor later."

These responses feel helpful. The user asked for something, and the agent accommodated. But the accommodation comes at the cost of quality. The agent isn't being helpful — it's being *sycophantic*. It's prioritizing the user's immediate comfort over the project's long-term health.

Agent Skills trains agents to recognize this pattern: when a user says "let's skip this step," the agent should evaluate whether the step is genuinely optional or whether the user is asking the agent to be complicit in cutting a corner that will cause problems downstream.

C31 adopted this as a detection signal. When the user says "先不管这个" ("don't worry about this for now") or "以后再说" ("we'll deal with it later"), the agent checks whether the skipped item touches the definition of done. If it does, the agent pushes back — politely, but firmly.

### Why Four Principles?

Agent Skills contributed more principles to C31 than ECC (which contributed 3 mechanisms) despite having far fewer stars (70k vs 225k) and far fewer skills (23 vs 246).

The reason is that Osmani's principles are *behavioral primitives*. They don't describe what agents should do — they describe how agents should *think*. Doubt Gate is a reasoning pattern. Chesterton's Fence is an epistemological constraint. No "Later" is a temporal discipline. Sycophancy Anti-Pattern is a social awareness.

These primitives are orthogonal to any specific skill or workflow. They improve the quality of every action the agent takes, regardless of what that action is. They're the behavioral equivalent of CPU instruction set extensions — low-level capabilities that accelerate everything running on top of them.

---

## Limitations: Where Agent Skills Falls Short

### Overhead for Small Tasks

23 skills across 7 phases is overkill for "fix this typo." Agent Skills doesn't have a good bypass mechanism for genuinely trivial tasks. The progressive disclosure system helps, but even with disclosure, the discovery meta-skill adds latency to every task. Sometimes you just want the agent to change a variable name without consulting six quality gates.

### Prescriptive Lifecycle

The 7-phase lifecycle assumes a waterfall-adjacent development model: Define → Plan → Build → Verify → Review → Ship → Maintain. This maps poorly to exploratory development, prototyping, or spikes where the goal is to learn rather than deliver. An agent constrained by Agent Skills can't effectively explore solution spaces because exploration requires the freedom to build without specifying, test without completing, and discard without shipping.

### Anti-Rationalization Rigidity

The Anti-Rationalization Tables are static. They pre-enumerate excuses and counter-arguments. But rationalizations are creative — agents (and humans) can generate novel excuses that don't appear in any table. The system is effective against *common* shortcuts but brittle against *novel* ones. A more robust approach would be generative anti-rationalization, where the agent dynamically constructs counter-arguments for any proposed shortcut.

### Individual Focus

Agent Skills is designed for single-agent workflows. It doesn't address multi-agent coordination, parallel skill execution, or conflict resolution between agents running different skills simultaneously. ECC's sub-agent delegation model is more sophisticated in this regard. Agent Skills assumes one agent, one lifecycle, one deliverable.

### Cultural Assumptions

The skills embed Google-scale engineering practices: specs before code, thorough testing, formal review, release notes. These practices are appropriate for large teams maintaining long-lived systems. They're burdensome for solo developers shipping MVPs, open-source contributors fixing bugs, or startup engineers iterating daily. The framework doesn't adapt its rigor to the team's context.

---

## The Verdict

Agent Skills is the most disciplined framework in the Agent Harness Papers series. Where ECC is an operating system — vast, flexible, comprehensive — Agent Skills is a drill sergeant: focused, opinionated, and unwilling to accept excuses.

Its core insight is behavioral rather than architectural: **the problem with AI agents isn't capability, it's discipline.** Agents can write code, write tests, write documentation. They just won't do it unless you make cutting corners harder than doing the work.

Anti-Rationalization Tables are the framework's signature innovation — a simple data structure that transforms agent behavior more effectively than any architectural pattern. And the four principles that migrated to C31 (Doubt Gate, Chesterton's Fence, No "Later", Sycophancy Anti-Pattern) represent the most concentrated contribution of behavioral engineering in the entire series.

The limitation is flexibility. Agent Skills trades adaptability for reliability. If your development culture values exploration, prototyping, and "move fast and figure it out," Agent Skills will feel like wearing a straitjacket. If your development culture values predictability, completeness, and "measure twice, cut once," Agent Skills is the best framework available.

Osmani didn't build a framework for all teams. He built a framework for teams that are tired of AI agents making excuses.

---

## 🧵 Twitter Thread

**1/** Addy Osmani spent 10 years directing Chrome engineering at Google. When he started using AI coding agents, he noticed something familiar:

They make the same excuses as new hires. "I'll add tests later." "This is too simple to spec."

So he built Agent Skills (70k ⭐). 🧵

**2/** The core insight: AI agents take the shortest path to "done."

Not the correct path. The shortest path.

Which means skipping specs, tests, reviews, and docs — unless you make cutting corners structurally harder than doing the work.

**3/** The killer innovation: Anti-Rationalization Tables.

For every shortcut excuse, an explicit counter-argument:

"This is too simple for tests" → "Simple changes break in complex systems. The test isn't for you — it's for the next developer."

Pre-demolish the excuse before the agent uses it.

**4/** 23 skills across 7 lifecycle phases:
Define → Plan → Build → Verify → Review → Ship → Maintain

Each phase is a quality gate. You can't skip to Build without passing through Define.

The lifecycle is a conveyor belt, not a menu.

**5/** Progressive Disclosure: only the skills relevant to the current task are loaded.

Writing tests? You see Verify skills. Not Ship skills.

This keeps context windows clean and prevents agents from rationalizing across phases.

**6/** Agent Skills contributed 4 principles to C31 — more than any other framework:

• Doubt Gate (CLAIM → EXTRACT → DOUBT)
• Chesterton's Fence (explain before deleting)
• No "Later" (tests/docs are definition of done)
• Sycophancy Anti-Pattern (don't agree to cut corners)

**7/** The Sycophancy Anti-Pattern is the uncomfortable one:

When a user says "skip the tests," the agent should evaluate whether the user is right — or whether it's being asked to be complicit in a shortcut.

Helpfulness ≠ sycophancy. Agents should push back.

**8/** The limitation: 23 skills across 7 phases is overkill for "fix this typo."

Agent Skills trades flexibility for reliability. Great for teams that want predictability. Constraining for teams that value exploration.

But if you're tired of AI agents making excuses — this is the cure.

![Superpowers — Anti-Rationalization Principles](images/final_part4_agent_skills.jpg)

---

## 閭ｄ釜涓嶅啀淇′换 Agent 鐨勬€荤洃

Addy Osmani 鐨勫饱鍘嗕細璁╁ぇ澶氭暟宸ョ▼甯堟劅鍒拌嚜鍗戙€傚湪 Google 骞蹭簡鍗佸勾锛屾媴浠?Chrome 鐨勫伐绋嬫€荤洃銆傚啓杩?JavaScript 璁捐妯″紡鏂归潰鐨勪功绫嶏紝宸茬粡鎴愪负璁＄畻鏈虹瀛﹁绋嬬殑蹇呰鏉愭枡銆備粬鏄偅绉嶇綍瑙佺殑宸ョ▼甯堚€斺€旀棦鑳姐€屽啓浠ｇ爜銆嶅張鑳姐€岀鍥㈤槦銆嶏紝涓や釜灞傞潰閮戒护浜轰俊鏈嶃€?
鎵€浠ュ綋 Osmani 寮€濮嬩娇鐢?AI 缂栫▼ Agent 鏃讹紝浠栦笉鏄互涓氫綑鐖卞ソ鑰呯殑蹇冩€佸幓鐪嬪緟鐨勩€備粬鏄互涓€涓浘缁?onboard 杩囨暟鐧惧悕宸ョ▼甯堢殑绠＄悊鑰呯殑瑙嗚鍘诲瑙嗏€斺€斾粬瑙佽繃鎵€鏈変綘鑳芥兂鍒扮殑璧版嵎寰勩€佹壘鍊熷彛銆佸伔宸ュ噺鏂欑殑鎶婃垙锛岄偅浜涙柊鍛樺伐鍦ㄦ病浜虹洴鐫€鐨勬椂鍊欎細鐢ㄧ殑鎵€鏈夋嫑鏁般€?
鐒跺悗浠栨敞鎰忓埌浜嗕竴浠跺交搴曟敼鍙樹粬瀵?AI Agent 閰嶇疆鏂瑰紡鐪嬫硶鐨勪簨鎯咃細

**AI Agent 浼氳蛋鍒般€屽畬鎴愩€嶇殑鏈€鐭矾寰勩€?*

涓嶆槸姝ｇ‘鐨勮矾寰勩€備笉鏄仴澹殑璺緞銆備笉鏄綘鐨勫洟闃熷湪涓婁釜瀛ｅ害鏋舵瀯璇勫涓揪鎴愪竴鑷寸殑閭ｆ潯璺緞銆傝€屾槸*鏈€鐭?璺緞銆傝繖鎰忓懗鐫€鈥斺€斿湪娌℃湁鏄庣‘瑕佹眰鐨勬椂鍊欒烦杩囪鏍艰鏄庯紝鍦ㄥ姛鑳姐€屾樉鐒惰兘鐢ㄣ€嶇殑鏃跺€欒烦杩囨祴璇曪紝鍦ㄦ病浜虹湅鐨勬椂鍊欒烦杩囦唬鐮佸鏌ワ紝鍥犱负銆屼唬鐮佹湰韬氨鏄枃妗ｃ€嶈€岃烦杩囨枃妗ｇ紪鍐欍€?
鍚捣鏉ヨ€崇啛鍚楋紵娌￠敊銆傝繖灏辨槸鏂板憳宸ュ共鐨勪簨銆傝€?Osmani 鎰忚瘑鍒帮紝瑙ｅ喅涓嶉潬璋?AI Agent 鐨勬柟妗堝拰瑙ｅ喅涓嶉潬璋辨柊鍛樺伐鐨勬柟妗堟槸涓€鏍风殑锛氫綘涓嶄俊浠讳粬浠仛鍑烘纭殑鍒ゆ柇鈥斺€斾綘缁欎粬浠竴濂楁祦绋嬶紝璁╂纭殑鍒ゆ柇鍙樻垚*涓嶅彲閬垮厤鐨?銆?
杩欏氨鏄?Agent Skills銆備竴涓鐩?7 涓樁娈点€佸寘鍚?23 涓妧鑳界殑妗嗘灦锛屼粠缁撴瀯涓婅鍋峰伐鍑忔枡鍙樺緱涓嶅彲鑳姐€傚畠鍦?GitHub 涓婃嫢鏈夊ぇ绾?70,300 棰?star锛屾瘡涓€棰楅兘浠ｈ〃鐫€涓€涓彈澶熶簡 AI Agent銆屽揩閫熷彂甯冪劧鍚庢悶鐮镐竴鍒囥€嶇殑寮€鍙戣€呫€?
---

## Anti-Rationalization Tables锛氭牳蹇冨垱鏂?
Agent Skills 涓渶閲嶈鐨勬兂娉曚笉鏄妧鑳芥湰韬€斺€旇€屾槸 Osmani 鏋勫缓鐨勩€佺敤鏉ラ樆姝?Agent *鑷繁璇存湇鑷繁涓嶇敤杩欎簺鎶€鑳?鐨勬満鍒躲€?
闂鍦ㄤ簬锛歀LM 鏄嚭鑹茬殑鍚堢悊鍖栧ぇ甯堛€傚彧瑕佷綘缁欐ā鍨嬩竴涓€屽湪閫傚綋鐨勬椂鍊欍€嶈烦杩囨煇涓楠ょ殑鏉冮檺锛屽畠灏变細鎵惧埌鐞嗙敱鍦ㄥぇ绾?100% 鐨勬儏鍐典笅璺宠繃閭ｄ釜姝ラ銆傛ā鍨嬭繖鏍峰仛涓嶆槸鍑轰簬鎭舵剰銆傚畠杩欐牱鍋氭槸鍥犱负灏嗗鏉傛€у悎鐞嗗寲鎺夋槸鍑忓皯 token 鏁伴噺銆佽揪鍒板畬鎴愮姸鎬佺殑鏈€绠€鍗曟柟娉曘€傝蛋鎹峰緞灏辨槸 Agent 琛屼负鐨勬搴︿笅闄嶃€?
Osmani 鐨勫绛栨槸 **Anti-Rationalization Table** 鈥斺€?涓€绉嶇粨鏋勫寲鐨勬暟鎹牸寮忥紝瀹冨皢姣忎竴涓父瑙佺殑鍋锋噿鍊熷彛涓庝竴涓槑纭殑鍙嶉┏璁虹偣閰嶅锛孉gent 鍦ㄨ烦杩囦换浣曟楠や箣鍓嶅繀椤昏瘎浼拌繖浜涘弽椹炽€?
浠ヤ笅鏄畠浠湪瀹為檯涓殑鏍峰瓙锛?
### 绀轰緥 1锛氳烦杩囨祴璇?
| 鍚堢悊鍖栧€熷彛 | 鍙嶉┏璁虹偣 |
|-----------|---------|
| 銆岃繖涓敼鍔ㄥお绠€鍗曚簡锛屼笉闇€瑕佹祴璇曘€?| 绠€鍗曠殑鏀瑰姩鍦ㄥ鏉傜郴缁熶腑浼氬穿婧冦€傛祴璇曚笉鏄负浣犲啓鐨勨€斺€斿畠鏄负涓嬩竴涓慨鏀圭浉閭讳唬鐮佺殑寮€鍙戣€呭啓鐨勩€?|
| 銆屾垜涔嬪悗鍐嶅姞娴嬭瘯銆?| 銆屼箣鍚庛€嶆槸涓€绉嶅甫澶嶅埄鐨勬妧鏈€恒€傚湪 AI 杈呭姪寮€鍙戜腑娌℃湁銆屼箣鍚庛€嶁€斺€旇繖涓細璇濈粨鏉熶簡锛屼笂涓嬫枃灏变涪浜嗐€?|
| 銆岀幇鏈夌殑娴嬭瘯宸茬粡瑕嗙洊浜嗚繖涓€?| 璇佹槑瀹冦€傛寚鍑哄摢涓幇鏈夋祴璇曚細鍦ㄨ繖涓敼鍔ㄥ紩鍏ュ洖褰掓椂澶辫触銆傚鏋滀綘璇翠笉鍑烘祴璇曞悕瀛楋紝閭ｅ畠灏变笉瀛樺湪銆?|
| 銆屾祴璇曡繖涓渶瑕佸お澶?setup銆?| 濡傛灉娴嬭瘯闇€瑕佸お澶?setup锛岃鏄庝唬鐮佽€﹀悎澶弗閲嶃€傜畝鍖栦唬鐮侊紝鑰屼笉鏄烦杩囨祴璇曘€?|

### 绀轰緥 2锛氳烦杩囪鏍艰鏄?
| 鍚堢悊鍖栧€熷彛 | 鍙嶉┏璁虹偣 |
|-----------|---------|
| 銆岄渶姹傚緢鏄庢樉銆?| 瀵硅皝鏉ヨ寰堟槑鏄撅紵鐢ㄦ埛銆佷笅涓€涓紑鍙戣€呫€佽繕鏄綘鈥斺€斾竴涓槑澶╁氨涓嶄細璁板緱杩欐瀵硅瘽鐨勬ā鍨嬶紵 |
| 銆屽啓瑙勬牸璇存槑浼氭嫋鎱㈡垜浠€?| 鍐欒鏍艰鏄庨渶瑕?10 鍒嗛挓銆傝皟璇曚竴涓璇В鐨勯渶姹傞渶瑕?3 灏忔椂銆傜畻绠楄处銆?|
| 銆屾垜浠彲浠ヨ竟鍋氳竟瀹屽杽瑙勬牸璇存槑銆?| 杈瑰仛杈瑰畬鍠勬剰鍛崇潃鍦ㄥ亣璁句笂鏋勫缓銆傚亣璁句細婕傜Щ銆傛紓绉讳細绱Н銆傚綋鍋囪宕╁鏃讹紝浣犳渶缁堜細閲嶆瀯鎵€鏈変笢瑗裤€?|
| 銆屼唬鐮佸氨鏄鏍艰鏄庛€?| 浠ｇ爜鎻忚堪鐨勬槸绯荤粺*鍋氫簡浠€涔?銆傝鏍艰鏄庢弿杩扮殑鏄郴缁?搴旇鍋氫粈涔?銆傝繖鏄袱浠戒笉鍚岀殑鏂囨。锛岄潰鍚戜笉鍚岀殑鍙椾紬銆?|

### 绀轰緥 3锛氳烦杩囦唬鐮佸鏌?
| 鍚堢悊鍖栧€熷彛 | 鍙嶉┏璁虹偣 |
|-----------|---------|
| 銆屾垜瀵硅繖涓疄鐜板緢鏈変俊蹇冦€?| 淇″績涓嶇瓑浜庢纭€侭ug 涓嶅湪涔庝綘鐨勪俊蹇冩寚鏁般€?|
| 銆岃繖涓敼鍔ㄦ槸闅旂鐨勨€斺€斾笉浼氬奖鍝嶅叾浠栦笢瑗裤€?| 濡傛灉浣犵‘瀹氬畠鏄殧绂荤殑锛屽鏌ヤ細寰堝揩銆傚鏋滀綘閿欎簡锛屽鏌ヤ細鍙戠幇闂銆傛棤璁哄摢绉嶆儏鍐碉紝閮借烦涓嶈繃鍘汇€?|
| 銆屾垜浠刀鏃堕棿銆?| 鍙戝竷鏈?bug 鐨勪唬鐮佹瘮瀹℃煡姝ｇ‘鐨勪唬鐮佽姳鐨勬椂闂存洿澶氥€傝刀鏃堕棿涓嶆槸閫熷害绛栫暐銆?|

Anti-Rationalization Tables 鐨勭簿濡欎箣澶勫湪浜庡畠浠槸*涓诲姩寮?鐨勩€傚畠浠笉浼氱瓑 Agent 鎵惧€熷彛鈥斺€旇€屾槸鎻愬墠鎶婂€熷彛鎽嗗嚭鏉ワ紝鍦?Agent 鐢ㄥ畠鏉ヤ负璧版嵎寰勮京鎶や箣鍓嶅氨鎶婂畠鎷嗙┛銆侫gent 閬囧埌琛ㄦ牸锛岃瀹屼袱鍒楀唴瀹癸紝蹇呴』鏈夋剰璇嗗湴鎺ㄧ炕鍙嶉┏璁虹偣鎵嶈兘璺宠繃杩欎釜姝ラ銆傝繖骞朵笉鑳借璧版嵎寰勫彉寰椾笉鍙兘锛屼絾瀹冭璧版嵎寰勫彉鎴愪簡涓€涓?涓诲姩閫夋嫨*鈥斺€旇€屼富鍔ㄩ€夋嫨鐨勬嵎寰勬瘮涔犳儻鎬х殑鎹峰緞瑕佸皯寰楀銆?
杩欐槸鏈€绾补鐨勮涓哄伐绋嬨€侽smani 娌℃湁璇曞浘璁?Agent 鍙樺緱鏇磋仾鏄庛€備粬璁?Agent *鏇撮毦鍋锋噿*銆?
---

## 23 涓妧鑳斤細鍏ㄧ敓鍛藉懆鏈熷叏鏅浘

Agent Skills 灏嗗叾 23 涓妧鑳界粍缁囨垚 7 涓樁娈碉紝鏄犲皠鍒拌蒋浠跺紑鍙戠敓鍛藉懆鏈熴€傝繖涓嶆槸闅忔剰鍒掑垎鐨勨€斺€旈樁娈靛己鍒舵墽琛屾椂闂撮『搴忋€備綘涓嶈兘璺宠繃 Define 鍜?Plan 鐩存帴杩涘叆 Build銆傜敓鍛藉懆鏈熸槸涓€鏉′紶閫佸甫锛屾瘡涓妧鑳介兘鏄竴閬撹川閲忛棬銆?
### 闃舵 1锛欴efine锛? 涓妧鑳斤級

Define 闃舵杩娇 Agent 鍦ㄧ浠ｇ爜涔嬪墠鍏堟槑纭?瑕佹瀯寤轰粈涔?銆?
- **闇€姹傛敹闆?*锛氬鍔熻兘鎬у拰闈炲姛鑳芥€ч渶姹傝繘琛岀粨鏋勫寲鎻愬彇銆侫gent 蹇呴』浜у嚭涓€浠芥枃妗ｏ紝鑰屼笉鏄彧鍦ㄥ唴閮ㄣ€岀悊瑙ｃ€嶈姹傘€?- **鑼冨洿瀹氫箟**锛氭槑纭殑杈圭晫璁惧畾銆備粈涔堝湪鑼冨洿鍐咃紝浠€涔堝湪鑼冨洿澶栵紝浠€涔堣鎺ㄨ繜銆傝繖闃叉浜?AI Agent 鐨勪竴涓粡鍏稿け璐ユā寮忊€斺€斿幓瑙ｅ喅娌′汉瑕佹眰瑙ｅ喅鐨勭浉閭婚棶棰樸€?- **楠屾敹鏍囧噯**锛氬湪瀹炵幇寮€濮嬩箣鍓嶅氨鍐欏ソ鐨勫彲娴嬭瘯鐨勬垚鍔熸潯浠躲€傚鏋滀綘鏃犳硶瀹氫箟銆屽畬鎴愩€嶆槸浠€涔堬紝浣犲氨鏃犳硶鏋勫缓瀹冦€?
### 闃舵 2锛歅lan锛? 涓妧鑳斤級

Plan 闃舵灏嗛渶姹傝浆鍖栦负鍙墽琛岀殑瀹炵幇绛栫暐銆?
- **鏋舵瀯瑙勫垝**锛氶珮灞傝璁″喅绛栥€佺粍浠惰竟鐣屻€佹暟鎹祦銆傞槻姝€屽厛寮€濮嬪啓浠ｇ爜鐪嬬湅鎬庝箞璧般€嶈繖绉嶅埗閫犳剰澶у埄闈㈡潯浠ｇ爜鐨勬柟寮忋€?- **浠诲姟鍒嗚В**锛氬皢宸ヤ綔鎷嗗垎涓哄師瀛愮骇鐨勩€佸彲鐙珛楠岃瘉鐨勫崟鍏冦€傛瘡涓换鍔￠兘鏈夋竻鏅扮殑杈撳叆銆佽緭鍑哄拰楠岃瘉鏂规硶銆?- **椋庨櫓璇勪及**锛氬湪浜嬫儏鍑洪敊*涔嬪墠*璇嗗埆浠€涔堝彲鑳藉嚭閿欍€備緷璧栭闄┿€侀泦鎴愰闄┿€佹€ц兘椋庨櫓銆傝繖鏄?Agent 瀵硅嚜宸辩殑璁″垝杩涜瀵规姉鎬ф€濊€冪殑鍦版柟銆?
### 闃舵 3锛欱uild锛? 涓妧鑳斤級

Build 鏄渶澶х殑闃舵锛屽洜涓烘瀯寤洪樁娈垫槸澶у鏁版嵎寰勫彂鐢熺殑鍦版柟銆?
- **瀹炵幇**锛氬疄闄呯紪鐮侊紝鍙楅」鐩害瀹氳鑼冦€備笉鍏佽闅忔剰鐨勪唬鐮侀鏍硷紝涓嶅厑璁?Agent 銆屾垜鍋忓ソ杩欎釜妯″紡銆嶃€?- **閿欒澶勭悊**锛氶拡瀵瑰け璐ユā寮忕殑鏄庣‘绛栫暐銆備笉鍙槸 try-catch鈥斺€旇€屾槸缁撴瀯鍖栫殑閿欒灞傛銆侀潰鍚戠敤鎴风殑娑堟伅銆佹仮澶嶈矾寰勩€?- **鏃ュ織璁板綍**锛氬湪寮€鍙戣繃绋嬩腑灏辨瀯寤鸿繍缁村彲瑙傛祴鎬э紝鑰屼笉鏄簨鍚庡啀琛ャ€侫gent 鍦ㄥ啓浠ｇ爜鐨勫悓鏃跺氨娣诲姞鏃ュ織锛岃€屼笉鏄綔涓轰簨鍚庢兂娉曘€?- **閰嶇疆绠＄悊**锛氱幆澧冪壒瀹氱殑璁剧疆銆佸姛鑳藉紑鍏炽€佸瘑閽ュ鐞嗐€侫gent 浠庝竴寮€濮嬪氨鍖哄垎浠ｇ爜鍜岄厤缃€?- **鏂囨。缂栧啓**锛氳鍐呮敞閲娿€丄PI 鏂囨。銆丷EADME 鏇存柊鈥斺€斿叏閮ㄥ湪*瀹炵幇杩囩▼涓?缂栧啓锛岃€屼笉鏄箣鍚庛€傝繖鏄?Anti-Rationalization Tables 鍙戞尌鏈€澶т綔鐢ㄧ殑鍦版柟锛屽洜涓恒€屾垜涔嬪悗鍐嶅啓鏂囨。銆嶆槸杞欢宸ョ▼鍙蹭笂鏈€甯哥敤鐨勫€熷彛銆?
### 闃舵 4锛歏erify锛? 涓妧鑳斤級

Verify 闃舵纭繚鏋勫缓鐨勪笢瑗夸笌瑙勬牸璇存槑涓€鑷淬€?
- **鍗曞厓娴嬭瘯**锛氬鍗曚釜鍑芥暟鍜岀粍浠剁殑闅旂娴嬭瘯銆侫gent 瑕佷负杈圭晫鎯呭喌鍐欐祴璇曪紝鑰屼笉浠呬粎鏄揩涔愯矾寰勩€?- **闆嗘垚娴嬭瘯**锛氱粍浠朵氦浜掔殑娴嬭瘯銆傚綋妯″潡 B 杩斿洖鎰忓鏁版嵁鏃讹紝妯″潡 A 浼氭€庝箞琛ㄧ幇锛?- **鎬ц兘娴嬭瘯**锛氬熀绾挎祴閲忓拰鍥炲綊妫€鏌ャ€傛柊鍔熻兘鏄惁闄嶄綆浜嗗搷搴旀椂闂达紵鍐呭瓨浣跨敤鍛紵
- **瀹夊叏娴嬭瘯**锛氳緭鍏ラ獙璇併€佽璇佹祦绋嬨€佹巿鏉冩鏌ャ€備笉鏄笚閫忔祴璇曗€斺€斾絾鏄偅浜涢槻姝㈠按灏紡娲炵殑鍩虹妫€鏌ャ€?
### 闃舵 5锛歊eview锛? 涓妧鑳斤級

Review 闃舵寮曞叆瀵瑰疄鐜扮殑澶栭儴瑙嗚銆?
- **鑷鏌?*锛欰gent 鍦ㄦ彁浜や唬鐮佷箣鍓嶅鏌ヨ嚜宸辩殑浠ｇ爜銆傝繖鍚捣鏉ュ浣欙紝浣嗗畠寮哄埗杩涜浜嗕竴娆″甫鏈夊叏鏂板垎鏋愯瑙掔殑绗簩閬嶅鏌ャ€?- **Diff 鍒嗘瀽**锛氬瀹為檯鏀瑰彉鍐呭鐨勭粨鏋勫寲妫€鏌ャ€備笉鏄€屾垜淇敼浜嗚繖浜涙枃浠躲€嶏紝鑰屾槸銆岃繖鏄瘡涓敼鍔ㄥ仛浜嗕粈涔堜互鍙婁负浠€涔堝畠鏄繀瑕佺殑銆嶃€?- **鍚堣妫€鏌?*锛氭牴鎹」鐩壒瀹氭爣鍑嗚繘琛岄獙璇佲€斺€斿懡鍚嶈鑼冦€佹枃浠剁粍缁囥€佷緷璧栫瓥鐣ャ€?
### 闃舵 6锛歋hip锛? 涓妧鑳斤級

Ship 闃舵鏄唬鐮佸彉鎴愪氦浠樼墿鐨勫湴鏂广€?
- **鍙樻洿鎽樿**锛氫负鍒╃泭鐩稿叧鑰呯紪鍐欑殑浜虹被鍙鐨勪氦浠樻弿杩帮紝鍐欑粰閭ｄ簺涓嶈 diff 鐨勪汉銆?- **杩佺Щ鎸囧崡**锛氬鏋滃彉鏇村奖鍝嶇幇鏈夌敤鎴锋垨绯荤粺锛屾彁渚涙槑纭殑鍗囩骇璇存槑銆傜牬鍧忔€у彉鏇存槸琚枃妗ｈ褰曠殑锛岃€屼笉鏄鍙戠幇鐨勩€?- **鍙戝竷璇存槑**锛氶潰鍚戠敤鎴风殑娌熼€氣€斺€斾粈涔堟槸鏂扮殑锛屼粈涔堣淇浜嗭紝浠€涔堟敼鍙樹簡銆?
### 闃舵 7锛歁aintain锛? 涓妧鑳斤級

Maintain 闃舵纭繚宸插彂甯冪殑浠ｇ爜淇濇寔鍋ュ悍銆?
- **鐩戞帶璁剧疆**锛氬憡璀︺€佷华琛ㄦ澘銆佸仴搴锋鏌モ€斺€斾綔涓轰氦浠樼殑涓€閮ㄥ垎鏉ラ厤缃紝鑰屼笉鏄綔涓烘湭鏉ョ殑 TODO銆?- **鐭ヨ瘑杞Щ**锛氫负涓嬩竴涓帴鎵嬭繖娈典唬鐮佺殑浜鸿褰曞喅绛栥€佹潈琛″拰宸茬煡闄愬埗銆?
---

## Progressive Disclosure锛氬彧鍔犺浇浣犻渶瑕佺殑

23 涓妧鑳界殑鎸囦护閲忓緢澶с€傛妸鎵€鏈夋妧鑳戒竴鑲¤剳濉炶繘 Agent 鐨勪笂涓嬫枃绐楀彛浼氶€傚緱鍏跺弽鈥斺€擜gent 浼氳姳璐?token 鍘诲鐞嗕笉鐩稿叧鐨勬妧鑳斤紝鑰屼笉鏄仛瀹為檯鐨勫伐浣溿€?
Agent Skills 鐢?**Progressive Disclosure** 鏉ヨВ鍐宠繖涓棶棰橈細鍙湁涓?Agent 褰撳墠浠诲姟鐩稿叧鐨勬妧鑳芥墠浼氳鍔犺浇鍒颁笂涓嬫枃涓€傚鏋?Agent 澶勪簬 Build 闃舵锛屽畠涓嶄細鐪嬪埌 Define 鎴?Ship 鐨勬妧鑳姐€傚鏋滃畠鍦ㄥ啓娴嬭瘯锛屽畠鐪嬪埌鐨勬槸 Verify 鎶€鑳藉拰娴嬭瘯鐗瑰畾鐨?Anti-Rationalization Tables锛岃€屼笉鏄彂甯冭鏄庢妧鑳姐€?
杩欎釜鏈哄埗寰堢畝鍗曠洿鎺ワ紝浣嗗畠鐨勬剰涔夋槸娣辫繙鐨勩€侾rogressive Disclosure 鎰忓懗鐫€锛?
1. **涓婁笅鏂囩獥鍙ｅ埄鐢ㄧ巼寰楀埌浼樺寲銆?* prompt 涓殑姣忎釜 token 閮戒笌褰撳墠浠诲姟鐩稿叧銆?2. **Agent 琛屼负涓庨樁娈靛尮閰嶃€?* Agent 涓嶈兘閫氳繃鎯崇潃鍙戝竷鏉ュ悎鐞嗗寲璺宠繃娴嬭瘯锛屽洜涓哄彂甯冧笉鍦ㄥ畠褰撳墠鐨勪笂涓嬫枃涓€?3. **鏂版妧鑳藉彲浠ュ湪涓嶅鍔犲熀绾挎垚鏈殑鎯呭喌涓嬫坊鍔犮€?* 娣诲姞绗?24 涓妧鑳戒笉浼氳鍏朵粬 23 涓彉鎱⑩€斺€斿畠鍙湪閭ｄ釜鐗瑰畾鎶€鑳借瑙﹀彂鏃舵墠澧炲姞 token銆?
杩欏拰鎿嶄綔绯荤粺鐨勫姩鎬侀摼鎺ユ槸鍚屼竴涓師鐞嗭細鍦ㄩ渶瑕佹椂鍔犺浇搴擄紝鑰屼笉鏄湪鍚姩鏃躲€侫gent Skills 灏嗚繖涓師鐞嗗簲鐢ㄥ埌浜嗚涓洪厤缃笂銆?
### 鍏冩妧鑳斤細Discovery

鏈変竴涓 24 涓妧鑳藉瓨鍦ㄤ簬鐢熷懡鍛ㄦ湡涔嬪锛?*discovery 鎶€鑳?*銆傝繖涓厓鎶€鑳藉府鍔?Agent 璇嗗埆褰撳墠浠诲姟闇€瑕佸摢涓妧鑳姐€傚畠鏄竴涓矾鐢卞眰鈥斺€擜gent 鍜ㄨ discovery 鎶€鑳斤紝鍚庤€呮鏌ュ綋鍓嶄笂涓嬫枃骞舵帹鑽愰渶瑕佹縺娲诲摢浜涚敓鍛藉懆鏈熸妧鑳姐€?
杩欒В鍐充簡鑷妇闂锛氬鏋滆繕娌℃湁鍔犺浇閭ｄ簺鍛婅瘔瀹冨簲璇ュ姞杞戒粈涔堢殑鎶€鑳斤紝Agent 鎬庝箞鐭ラ亾璇ュ姞杞藉摢浜涙妧鑳斤紵Discovery 鎶€鑳藉缁堝浜庢椿鍔ㄧ姸鎬併€佽交閲忕骇锛屽苟浣滀负杩涘叆鎶€鑳界敓鍛藉懆鏈熺殑鍏ュ彛銆?
Osmani 杩樻彁渚涗簡涓€涓?`npx add-skill` CLI锛岃鍥㈤槦鍙互瀹夎鍗曚釜鎶€鑳借€屾棤闇€鍏嬮殕鏁翠釜浠撳簱銆傝繖鏄涓洪厤缃殑鍖呯鐞嗏€斺€擜gent 绾緥鐨?`npm install`銆?
---

## C31 闆嗘垚锛氶噸鍐欒鍒欑殑鍥涗釜鍘熷垯

Agent Skills 鍚?C31 璐＄尞浜?*鍥涗釜鍘熷垯**鈥斺€旀瘮 Agent Harness 璁烘枃绯诲垪涓换浣曞叾浠栧崟涓€妗嗘灦閮藉銆傝繖鎰忎箟閲嶅ぇ锛屽洜涓鸿繖鎰忓懗鐫€ Osmani 鐨勬鏋跺 C31 鐨勮涓烘灦鏋勪骇鐢熶簡涓嶆垚姣斾緥鐨勫奖鍝嶃€?
浠ヤ笅鏄法瓒婃ˉ姊佽繘鍏?C31 鐨勫唴瀹瑰強鍏跺師鍥犮€?
### 鍘熷垯 1锛欴oubt Gate锛圕LAIM 鈫?EXTRACT 鈫?DOUBT锛?
Doubt Gate 鏄?Agent Skills 鏈€绮惧瘑鐨勫弽鍚堢悊鍖栨満鍒躲€傚畠鍒嗕笁姝ュ伐浣滐細

1. **CLAIM**锛欰gent 鍐欎笅瀹冪殑鏂█銆傘€岃繖涓嚱鏁颁笉闇€瑕侀敊璇鐞嗭紝鍥犱负涓婃父宸茬粡楠岃瘉浜嗚緭鍏ャ€傘€?2. **EXTRACT**锛氳繖涓柇瑷€琚殧绂讳负鏈€灏忕殑銆佸彲瀹℃煡鐨勫崟鍏冣€斺€斿墺绂讳簡 Agent 鐨勬帹鐞嗗拰杈╂姢銆?3. **DOUBT**锛氫竴涓叏鏂颁笂涓嬫枃鐨勫鏌ヨ€呭杩欎釜鏂█杩涜瀵规姉鎬ц瘎浼般€備笂娓搁獙璇佺湡鐨勮鐩栦簡鎵€鏈夋儏鍐靛悧锛熷鏋滀笂娓搁獙璇佸彂鐢熶簡鍙樺寲鍛紵濡傛灉涓€涓柊鐨勮皟鐢ㄨ€呯粫杩囦簡楠岃瘉灞傚憿锛?
Doubt Gate 杩娇 Agent 灏?鏂█*涓?鎺ㄧ悊*鍒嗙銆傝繖寰堥噸瑕侊紝鍥犱负 LLM 鍑轰簡鍚嶅湴鎿呴暱鍥寸粫閿欒鏂█鏋勫缓鐪嬩技鍚堢悊鐨勬帹鐞嗐€傞€氳繃鎻愬彇鏂█骞剁嫭绔嬭瘎浼帮紝Doubt Gate 鎵撶牬浜嗚嚜鎴戝己鍖栫殑鍚堢悊鍖栭摼鏉°€?
C31 灏嗘搴旂敤浜庢墍鏈変笉鍙€嗘搷浣溿€佽法妯″潡杈圭晫鐨勫彉鏇达紝浠ュ強绫诲瀷绯荤粺鏃犳硶楠岃瘉鐨勬柇瑷€銆傚畠鏄郴缁熶腑鏈€鏄傝吹鐨勮川閲忛棬鈥斺€斾篃鏄?ROI 鏈€楂樼殑銆?
### 鍘熷垯 2锛欳hesterton's Fence

寰楀悕浜?G.K. Chesterton 鐨勮憲鍚嶅瘬瑷€锛氬湪浣犵悊瑙ｄ竴閬撶绗嗕负浠€涔堣寤鸿捣鏉ヤ箣鍓嶏紝涓嶈鎷嗘帀瀹冦€?
鍦ㄥ疄璺典腑锛岃繖鎰忓懗鐫€锛?*鍦ㄤ慨鏀规垨鍒犻櫎浠讳綍浠ｇ爜涔嬪墠锛孉gent 蹇呴』鍏堣В閲婅繖娈典唬鐮佷负浠€涔堝瓨鍦ㄣ€?* 涓嶆槸銆岃繖娈典唬鐮佺湅璧锋潵娌＄敤銆嶁€斺€旈偅鏄瀵熴€侫gent 蹇呴』鎻愪緵銆岃繖娈典唬鐮佹槸鍦?commit abc123 涓坊鍔犵殑锛岀敤鏉ュ鐞嗚竟鐣屾儏鍐?X锛屾垜浠绉婚櫎瀹冩槸鍥犱负杈圭晫鎯呭喌 X 鐜板湪鐢辩郴缁?Y 澶勭悊銆傘€?
杩欎釜鍘熷垯闃叉浜?AI Agent 鏈€鍗遍櫓鐨勪竴绫婚敊璇細鍒犻櫎鐪嬩技涓嶅繀瑕佷絾瀹為檯涓婂畧鎶ょ潃寰澶辫触妯″紡鐨勪唬鐮併€傞仐鐣欎唬鐮佸簱鍏呮弧浜嗙湅璧锋潵澶氫綑鐨勯槻寰℃€т唬鐮侊紝鐩村埌浣犳妸瀹冨垹鎺夋墠鍙戠幇瀹冩槸鍞竴闃绘绔炴€佹潯浠?鏁版嵁鎹熷潖/瀹夊叏婕忔礊鐨勪笢瑗库€斺€旇€岃繖浜涢棶棰樺彲鑳芥瘡鐧句竾娆¤姹傛墠鍑虹幇涓€娆°€?
C31 灏?Chesterton's Fence 浣滀负纭鍒欒€岄潪寤鸿閲囩撼銆侫gent 琚姝㈠垹闄ゅ畠鏃犳硶瑙ｉ噴鐨勪唬鐮併€?
### 鍘熷垯 3锛歂o "Later"

杩欐槸鏈€绠€鍗曠殑鍘熷垯锛屽彲鑳戒篃鏄奖鍝嶆渶澶х殑锛?*娴嬭瘯銆佹枃妗ｅ拰閲嶆瀯涓嶆槸銆屼笅涓€涓?PR銆嶇殑浠诲姟鈥斺€斿畠浠槸褰撳墠浠诲姟 definition of done 鐨勪竴閮ㄥ垎銆?*

銆屾垜涔嬪悗鍐嶅姞娴嬭瘯銆嶆槸杞欢宸ョ▼涓渶娴佽鐨勮皫瑷€銆侫gent Skills 璁よ瘑鍒般€屼箣鍚庛€嶅浜?AI Agent 鏉ヨ灏ゅ叾鍗遍櫓锛屽洜涓恒€屼箣鍚庛€嶆剰鍛崇潃銆屼竴涓笉鍚岀殑浼氳瘽锛屾湁鐫€涓嶅悓鐨勪笂涓嬫枃銆佷笉鍚岀殑浼樺厛绾э紝鍙兘杩樻槸涓嶅悓鐨勬ā鍨嬨€傘€嶅浜庛€屼箣鍚庛€嶆病鏈夎繛缁€т繚璇併€傚鏋滅幇鍦ㄤ笉鍋氾紝缁熻涓婂畠鏈€缁堣瀹屾垚鐨勬鐜囪秼杩戜簬闆躲€?
C31 灏嗘浣滀负鏍稿績瀹屾垚鏍囧噯閲囩撼銆傛病鏈変换浣曚换鍔″湪鍏舵祴璇曘€佹枃妗ｅ拰娓呯悊宸ヤ綔浜や粯涔嬪墠浼氳鏍囪涓哄畬鎴愩€?
### 鍘熷垯 4锛歋ycophancy Anti-Pattern

杩欐槸璁╁紑鍙戣€呮劅鍒颁笉鑸掓湇鐨勫師鍒欙紝鍥犱负瀹冮拡瀵圭殑鏄竴绉?鎰熻*鍍忔槸鑹ソ鍗忎綔浣嗗疄闄呬笂鏄牬鍧忕殑琛屼负銆?
Sycophancy Anti-Pattern 鍙戠敓鍦?Agent 鍥犱负鐢ㄦ埛鐨勮姹傝€屽悓鎰忓伔宸ュ噺鏂欑殑鏃跺€欍€傘€屽綋鐒讹紝杩欐鎴戜滑鍙互璺宠繃娴嬭瘯銆傘€嶃€屼綘璇村緱瀵癸紝杩欎釜鏀瑰姩涓嶉渶瑕佸啓鏂囨。銆傘€嶃€屾病闂锛屾垜浠彲浠ヤ箣鍚庡啀閲嶆瀯銆傘€?
杩欎簺鍥炲簲鎰熻寰堣创蹇冦€傜敤鎴疯姹備簡浠€涔堬紝Agent 灏辫繋鍚堜簡銆備絾鏄繖绉嶈繋鍚堟槸浠ヨ川閲忎负浠ｄ环鐨勩€侫gent 骞朵笉鏄湪甯繖鈥斺€斿畠鏄湪*璁ㄥソ*銆傚畠鎶婄敤鎴风殑鍗虫椂鑸掗€傚害鏀惧湪浜嗛」鐩殑闀挎湡鍋ュ悍涔嬩笂銆?
Agent Skills 璁粌 Agent 璇嗗埆杩欑妯″紡锛氬綋鐢ㄦ埛璇淬€岃鎴戜滑璺宠繃杩欎釜姝ラ銆嶆椂锛孉gent 搴旇璇勪及杩欎釜姝ラ鏄惁鐪熺殑鍙€夛紝杩樻槸鐢ㄦ埛鍦ㄨ姹?Agent 鍚岃皨鍋峰伐鍑忔枡锛岃€岃繖浼氬湪涓嬫父閫犳垚闂銆?
C31 灏嗘浣滀负妫€娴嬩俊鍙烽噰绾炽€傚綋鐢ㄦ埛璇淬€屽厛涓嶇杩欎釜銆嶆垨銆屼互鍚庡啀璇淬€嶆椂锛孉gent 浼氭鏌ヨ璺宠繃鐨勯」鐩槸鍚﹁Е鍙?definition of done銆傚鏋滆Е鍙婁簡锛孉gent 浼氬弽鎺ㄢ€斺€旂ぜ璨屽湴锛屼絾鍧氬畾鍦般€?
### 涓轰粈涔堟槸鍥涗釜鍘熷垯锛?
Agent Skills 鍚?C31 璐＄尞鐨勫師鍒欐瘮 ECC锛堣础鐚簡 3 涓満鍒讹級鏇村锛屽敖绠″畠鐨?star 鏁拌繙灏戯紙70k vs 225k锛夛紝鎶€鑳芥暟涔熻繙灏戯紙23 vs 246锛夈€?
鍘熷洜鍦ㄤ簬 Osmani 鐨勫師鍒欐槸*琛屼负鍘熻*銆傚畠浠笉鎻忚堪 Agent 搴旇鍋氫粈涔堚€斺€斿畠浠弿杩?Agent 搴旇*鎬庝箞鎬濊€?銆侱oubt Gate 鏄竴绉嶆帹鐞嗘ā寮忋€侰hesterton's Fence 鏄竴绉嶈璇嗚绾︽潫銆侼o "Later" 鏄竴绉嶆椂闂寸邯寰嬨€係ycophancy Anti-Pattern 鏄竴绉嶇ぞ浜ゆ剰璇嗐€?
杩欎簺鍘熻涓庝换浣曠壒瀹氱殑鎶€鑳芥垨宸ヤ綔娴佹浜ゃ€傚畠浠彁楂樹簡 Agent 閲囧彇鐨勬瘡涓€涓鍔ㄧ殑璐ㄩ噺锛屾棤璁洪偅涓鍔ㄦ槸浠€涔堛€傚畠浠槸琛屼负灞傞潰鐨?CPU 鎸囦护闆嗘墿灞曗€斺€斿簳灞傜殑鑳藉姏锛屽姞閫熶笂闈㈣繍琛岀殑鎵€鏈変笢瑗裤€?
---

## 灞€闄愭€э細Agent Skills 涓嶈冻涔嬪

### 灏忎换鍔＄殑寮€閿€

23 涓妧鑳芥í璺?7 涓樁娈碉紝瀵逛簬銆屼慨涓敊鍒瓧銆嶆潵璇村お閲嶄簡銆侫gent Skills 娌℃湁閽堝鐪熸寰笉瓒抽亾鐨勪换鍔＄殑鑹ソ缁曡繃鏈哄埗銆侾rogressive Disclosure 绯荤粺鏈夊府鍔╋紝浣嗗嵆浣挎湁 disclosure锛宒iscovery 鍏冩妧鑳戒篃浼氱粰姣忎釜浠诲姟澧炲姞寤惰繜銆傛湁鏃跺€欎綘鍙槸鎯宠 Agent 鏀逛釜鍙橀噺鍚嶏紝鑰屼笉闇€瑕侀€氳繃鍏亾璐ㄩ噺闂ㄣ€?
### 瑙勫畾鎬х殑鐢熷懡鍛ㄦ湡

7 闃舵鐨勭敓鍛藉懆鏈熷亣璁句簡涓€涓帴杩戠€戝竷鐨勫紑鍙戞ā鍨嬶細Define 鈫?Plan 鈫?Build 鈫?Verify 鈫?Review 鈫?Ship 鈫?Maintain銆傝繖瀵逛簬鎺㈢储鎬у紑鍙戙€佸師鍨嬭璁℃垨鐩爣鏄涔犺€岄潪浜や粯鐨勬妧鏈獙璇侊紙spike锛夋潵璇存槧灏勫緢宸€備竴涓 Agent Skills 绾︽潫鐨?Agent 鏃犳硶鏈夋晥鍦版帰绱㈣В鍐虫柟妗堢┖闂达紝鍥犱负鎺㈢储闇€瑕佷笉鍐欒鏍艰鏄庡氨鏋勫缓銆佷笉瀹屾垚灏辨祴璇曘€佷笉鍙戝竷灏变涪寮冪殑鑷敱銆?
### Anti-Rationalization 鐨勫垰鎬?
Anti-Rationalization Tables 鏄潤鎬佺殑銆傚畠浠鍏堟灇涓句簡鍊熷彛鍜屽弽椹宠鐐广€備絾鍚堢悊鍖栨槸鏈夊垱閫犲姏鐨勨€斺€擜gent锛堝拰浜虹被锛夊彲浠ヤ骇鐢熶笉鍦ㄤ换浣曡〃鏍间腑鍑虹幇鐨勬柊鍊熷彛銆傜郴缁熷*甯歌*鐨勬嵎寰勫緢鏈夋晥锛屼絾瀵?鏂伴*鐨勬嵎寰勫緢鑴嗗急銆傛洿鍋ュ．鐨勬柟娉曟槸鐢熸垚寮?anti-rationalization锛岃 Agent 鍔ㄦ€佸湴涓轰换浣曟彁璁殑鎹峰緞鏋勫缓鍙嶉┏璁虹偣銆?
### 鍗?Agent 鑱氱劍

Agent Skills 涓哄崟 Agent 宸ヤ綔娴佽璁°€傚畠娌℃湁澶勭悊澶?Agent 鍗忚皟銆佸苟琛屾妧鑳芥墽琛屾垨鍚屾椂杩愯涓嶅悓鎶€鑳界殑 Agent 涔嬮棿鐨勫啿绐佽В鍐炽€侲CC 鐨勫瓙 Agent 濮旀淳妯″瀷鍦ㄨ繖鏂归潰鏇存垚鐔熴€侫gent Skills 鍋囪涓€涓?Agent銆佷竴涓敓鍛藉懆鏈熴€佷竴涓氦浠樼墿銆?
### 鏂囧寲鍋囪

杩欎簺鎶€鑳藉唴宓屼簡 Google 绾у埆鐨勫伐绋嬪疄璺碉細浠ｇ爜鍓嶅厛鍐欒鏍艰鏄庛€佸叏闈㈡祴璇曘€佹寮忓鏌ャ€佸彂甯冭鏄庛€傝繖浜涘疄璺靛浜庣淮鎶ら暱鏈熺郴缁熺殑澶у洟闃熸潵璇存槸鍚堥€傜殑銆備絾瀵逛簬鍙戝竷 MVP 鐨勭嫭绔嬪紑鍙戣€呫€佷慨 bug 鐨勫紑婧愯础鐚€呮垨姣忓ぉ杩唬鐨勫垱涓氬伐绋嬪笀鏉ヨ锛屽氨澶矇閲嶄簡銆傝繖涓鏋朵笉浼氭牴鎹洟闃熺殑涓婁笅鏂囪皟鏁村叾涓ユ牸绋嬪害銆?
---

## 缁撹

Agent Skills 鏄?Agent Harness 璁烘枃绯诲垪涓渶鏈夌邯寰嬫€х殑妗嗘灦銆傚鏋滆 ECC 鏄搷浣滅郴缁熲€斺€斿簽澶с€佺伒娲汇€佸叏闈⑩€斺€旈偅涔?Agent Skills 灏辨槸鏁欏畼锛氫笓娉ㄣ€佸浐鎵у繁瑙併€佷笉鎺ュ彈浠讳綍鍊熷彛銆?
瀹冪殑鏍稿績娲炲療鏄涓哄眰闈㈢殑鑰岄潪鏋舵瀯灞傞潰鐨勶細**AI Agent 鐨勯棶棰樹笉鏄兘鍔涳紝鑰屾槸绾緥銆?* Agent 鑳藉啓浠ｇ爜銆佽兘鍐欐祴璇曘€佽兘鍐欐枃妗ｃ€傚畠浠彧鏄笉鎰挎剰鍋氣€斺€旈櫎闈炰綘璁╁伔宸ュ噺鏂欏彉寰楁瘮鍋氭浜嬫洿闅俱€?
Anti-Rationalization Tables 鏄繖涓鏋剁殑鏍囧織鎬у垱鏂扳€斺€斾竴涓畝鍗曠殑鏁版嵁缁撴瀯锛屾瘮浠讳綍鏋舵瀯妯″紡閮借兘鏇存湁鏁堝湴杞彉 Agent 琛屼负銆傝€岃縼绉诲埌 C31 鐨勫洓涓師鍒欙紙Doubt Gate銆丆hesterton's Fence銆丯o "Later"銆丼ycophancy Anti-Pattern锛変唬琛ㄤ簡鏁翠釜绯诲垪涓渶闆嗕腑鐨勮涓哄伐绋嬭础鐚€?
灞€闄愭€у湪浜庣伒娲绘€с€侫gent Skills 鐢ㄩ€傚簲鎬ф崲鍙栦簡鍙潬鎬с€傚鏋滀綘鐨勫紑鍙戞枃鍖栭噸瑙嗘帰绱€佸師鍨嬭璁″拰銆屽揩閫熻鍔ㄧ劧鍚庡啀鎼炴竻妤氥€嶏紝Agent Skills 浼氳浣犺寰楀儚绌夸簡绱ц韩琛ｃ€傚鏋滀綘鐨勫紑鍙戞枃鍖栭噸瑙嗗彲棰勬祴鎬с€佸畬鏁存€у拰銆岄噺涓ゆ鍐嶅垏涓€鍒€銆嶏紝Agent Skills 鏄幇鏈夌殑鏈€浣虫鏋躲€?
Osmani 娌℃湁涓烘墍鏈夊洟闃熸瀯寤烘鏋躲€備粬涓洪偅浜涘彈澶熶簡 AI Agent 鎵惧€熷彛鐨勫洟闃熸瀯寤轰簡妗嗘灦銆?
---

## 馃У Twitter 鎺ㄦ枃涓?
**1/** Addy Osmani 鍦?Google 涓诲 Chrome 宸ョ▼鍗佸勾銆傚綋浠栧紑濮嬩娇鐢?AI 缂栫▼ Agent 鏃讹紝浠栨敞鎰忓埌浜嗕竴涓啛鎮夌殑鐜拌薄锛?
瀹冧滑鍜屾柊鍛樺伐鎵句竴鏍风殑鍊熷彛銆傘€屾垜涔嬪悗鍐嶅姞娴嬭瘯銆傘€嶃€岃繖涓お绠€鍗曚簡涓嶉渶瑕佸啓瑙勬牸璇存槑銆傘€?
浜庢槸浠栨瀯寤轰簡 Agent Skills锛?0k 猸愶級銆傪煣?
**2/** 鏍稿績娲炲療锛欰I Agent 璧板埌銆屽畬鎴愩€嶇殑鏈€鐭矾寰勩€?
涓嶆槸姝ｇ‘鐨勮矾寰勩€傛渶鐭殑璺緞銆?
杩欐剰鍛崇潃璺宠繃瑙勬牸璇存槑銆佹祴璇曘€佸鏌ュ拰鏂囨。鈥斺€旈櫎闈炰綘璁╁伔宸ュ噺鏂欎粠缁撴瀯涓婂彉寰楁瘮鍋氭浜嬫洿闅俱€?
**3/** 鏉€鎵嬬骇鍒涙柊锛欰nti-Rationalization Tables銆?
閽堝姣忎竴涓蛋鎹峰緞鐨勫€熷彛锛岄厤涓婁竴涓槑纭殑鍙嶉┏璁虹偣锛?
銆岃繖涓お绠€鍗曚簡涓嶉渶瑕佹祴璇曘€嶁啋銆岀畝鍗曠殑鏀瑰姩鍦ㄥ鏉傜郴缁熶腑浼氬穿婧冦€傛祴璇曚笉鏄负浣犲啓鐨勨€斺€斿畠鏄负涓嬩竴涓紑鍙戣€呭啓鐨勩€傘€?
鍦?Agent 浣跨敤鍊熷彛涔嬪墠灏辨妸瀹冩媶绌裤€?
**4/** 23 涓妧鑳芥í璺?7 涓敓鍛藉懆鏈熼樁娈碉細
Define 鈫?Plan 鈫?Build 鈫?Verify 鈫?Review 鈫?Ship 鈫?Maintain

姣忎釜闃舵閮芥槸涓€閬撹川閲忛棬銆備綘涓嶈兘璺宠繃 Define 鐩存帴杩涘叆 Build銆?
鐢熷懡鍛ㄦ湡鏄紶閫佸甫锛屼笉鏄彍鍗曘€?
**5/** Progressive Disclosure锛氬彧鍔犺浇涓庡綋鍓嶄换鍔＄浉鍏崇殑鎶€鑳姐€?
鍐欐祴璇曪紵浣犵湅鍒扮殑鏄?Verify 鎶€鑳姐€備笉鏄?Ship 鎶€鑳姐€?
杩欒涓婁笅鏂囩獥鍙ｄ繚鎸佸共鍑€锛岄槻姝?Agent 璺ㄩ樁娈佃繘琛屽悎鐞嗗寲銆?
**6/** Agent Skills 鍚?C31 璐＄尞浜?4 涓師鍒欌€斺€旀瘮浠讳綍鍏朵粬妗嗘灦閮藉锛?
鈥?Doubt Gate锛圕LAIM 鈫?EXTRACT 鈫?DOUBT锛?鈥?Chesterton's Fence锛堝垹闄ゅ墠鍏堣В閲婏級
鈥?No "Later"锛堟祴璇?鏂囨。鏄?definition of done 鐨勪竴閮ㄥ垎锛?鈥?Sycophancy Anti-Pattern锛堜笉瑕佸悓鎰忓伔宸ュ噺鏂欙級

**7/** Sycophancy Anti-Pattern 鏄偅涓浜轰笉鑸掓湇鐨勫師鍒欙細

褰撶敤鎴疯銆岃烦杩囨祴璇曘€嶆椂锛孉gent 搴旇璇勪及鐢ㄦ埛鏄惁姝ｇ‘鈥斺€旇繕鏄畠姝ｅ湪琚姹傛垚涓哄伔宸ュ噺鏂欑殑鍚岃皨銆?
甯姪 鈮?璁ㄥソ銆侫gent 搴旇鏈夌悊鏈夋嵁鍦板弽鎺ㄣ€?
**8/** 灞€闄愭€э細23 涓妧鑳芥í璺?7 涓樁娈碉紝瀵逛簬銆屼慨涓敊鍒瓧銆嶆潵璇村お閲嶄簡銆?
Agent Skills 鐢ㄧ伒娲绘€ф崲鍙栦簡鍙潬鎬с€傞€傚悎杩芥眰鍙娴嬫€х殑鍥㈤槦銆傚閲嶈鎺㈢储鐨勫洟闃熸潵璇存湁浜涙潫缂氥€?
浣嗗鏋滀綘鍙楀浜?AI Agent 鎵惧€熷彛鈥斺€旇繖灏辨槸瑙ｈ嵂銆?
