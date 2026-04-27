# Dimensions

The properties along which tasks vary. Where [PRIMITIVES.md](./PRIMITIVES.md) gives the parts a task is built from, this document gives the *axes* on which two tasks built from those parts can differ.

A useful task description is not a category label ("coding," "dialog," "web") but a position in this space. The same primitive — say, *Oracle* — looks very different at high oracle availability (a comprehensive test suite) and at oracle absence (an open-ended research direction).

Each entry below carries: a definition, a **scale** for the dimension, an **instrumentation note** for how to measure it from real artifacts, and **three worked examples** in distinct task contexts. The three-example requirement is the [extractive constraint](./CONTRIBUTING.md): a dimension that does not vary across at least three real task settings is not a dimension yet.

---

## 1. Goal clarity

**Scale.** Explicit / latent / contested.

**Instrumentation.** Count of independent reformulations of "what success looks like" before agreement; inter-rater agreement among evaluators on whether a sample trajectory succeeded.

**Examples.**
- *Coding with a held-out test suite*: **explicit** — the goal is "tests pass," and two evaluators will agree.
- *Customer-support agent under a policy*: **latent** — many resolution paths satisfy the policy; reasonable evaluators may disagree on whether a borderline case is acceptable.
- *Long-horizon research direction*: **contested** — domain experts disagree on what would count as a successful answer, and that disagreement is durable.

Goal clarity is not the same as oracle availability. A task can have a perfectly clear goal that is nonetheless impossible to verify cheaply.

---

## 2. State observability

**Scale.** Full / partial / hidden.

**Instrumentation.** Ratio of state variables exposed to the agent versus total state required to act optimally.

**Examples.**
- *MMLU and most static QA*: **full** — the input contains everything needed to answer.
- *SWE-bench*: **partial** — the full repo exists, but the agent must build a working map; relevant state is large and only some is loaded into context.
- *Production incident debugging*: **hidden** — much of the relevant state lives server-side, behind logs the agent cannot directly query without privileged tools.

Observability is a property of the task, not of the agent. Adding context-window size or RAG addresses observability by *changing the task* — that is exactly the point.

---

## 3. Oracle availability

**Scale.** Rich / poor / absent.

**Instrumentation.** Count of automated test cases per task; cost ratio of one verification to one trajectory.

**Examples.**
- *Code repair with a 50-test held-out suite*: **rich** — high-coverage, deterministic verification at near-zero marginal cost.
- *Product design judged by rubric*: **poor** — humans must score; raters disagree; cost per verification is minutes.
- *"Identify a research direction that will matter in five years"*: **absent** — there is no operationalizable oracle inside the task horizon.

Oracle scarcity is the most common reason a task that looks tractable in principle becomes intractable to evaluate.

---

## 4. Decomposition depth

**Scale.** Single-shot / shallow (2–5 steps) / deep (6+).

**Instrumentation.** Minimum number of distinct subgoals visible in successful trajectories; depth of the call/dependency tree in agent traces.

**Examples.**
- *Single factual lookup or arithmetic*: **single-shot**.
- *Renaming a function across a project*: **shallow** — locate references, edit, verify.
- *Multi-file refactor or SWE-bench bug fix*: **deep** — locate, understand, patch, test, verify, explain, often with iteration.

Decomposition depth is not the same as time horizon. A task can be deep but quick (compiled in milliseconds) or shallow but slow (one step that takes hours).

---

## 5. Tool dependence

**Scale.** None / optional / required.

**Instrumentation.** Performance gap between a tool-less baseline and a tool-augmented agent on the same task.

**Examples.**
- *Pure language tasks (sentiment, paraphrase)*: **none** — tools add no value.
- *Math word problems*: **optional** — calculators help but are not strictly required for capable models.
- *Web automation, code execution, real-world browsing*: **required** — bare LLMs cannot complete these tasks regardless of capability.

Tool dependence is what makes most "agent benchmarks" not directly comparable to LLM benchmarks. A leaderboard mixing the two is comparing different tasks.

---

## 6. Feedback latency

**Scale.** Immediate / delayed / terminal-only.

**Instrumentation.** Median elapsed time (or steps) between an actor action and the signal that informs the next action.

**Examples.**
- *REPL or interactive shell*: **immediate** — every action returns observable consequence within milliseconds.
- *Edit-compile-test cycles*: **delayed** — many edits before a single test run.
- *Long-horizon strategy, annual planning*: **terminal-only** — no in-trajectory feedback; only end-state evaluation.

Tasks with high feedback latency punish agents that lack credit-assignment skill, even when their underlying capability is identical.

---

## 7. Environment mutability

**Scale.** Static / actor-driven / exogenous.

**Instrumentation.** Variance in observation given identical action sequences across replays.

**Examples.**
- *Frozen QA datasets*: **static** — same input, same output, every time.
- *Code editing in a sandbox*: **actor-driven** — only the agent's own actions change the world.
- *Browser tasks (WebArena, OSWorld), live APIs, multi-agent settings*: **exogenous** — pages re-render, sessions expire, other agents act between the actor's steps.

Exogenous mutability is the largest single source of evaluation variance for the same agent on the same task.

---

## 8. Failure reversibility

**Scale.** Reversible / costly / irreversible.

**Instrumentation.** Existence and cost of an undo path; count of downstream systems that depend on the affected state.

**Examples.**
- *Local code patch in a sandbox*: **reversible** — `git reset` or container teardown undoes everything.
- *Sent email, posted comment, deployed staging change*: **costly** — partial undo possible, full reputation/state restoration is not.
- *Settled financial transaction, dropped database, executed missile launch*: **irreversible** — no undo at any cost.

Reversibility is the dimension along which "evaluable in a benchmark" diverges most from "deployable in the world."

---

## 9. Context locality

**Scale.** Local / repo-scale / cross-system.

**Instrumentation.** Number of distinct files, services, or knowledge sources that must be consulted to complete a successful trajectory.

**Examples.**
- *LeetCode-style problems*: **local** — the function under test and its docstring suffice.
- *Real-world repo bugs*: **repo-scale** — multiple files, the test suite, and conventions encoded across the project.
- *Production incident spanning microservices*: **cross-system** — logs in one service, schema in another, deploy history in a third.

A leaderboard score on local-context tasks transfers poorly to repo-scale tasks, and almost not at all to cross-system tasks. Treating "coding ability" as a single quantity hides this.

---

## 10. Ambiguity load

**Scale.** Low / medium / high.

**Instrumentation.** Information-completeness ratio of the input; intent specificity (how much of the goal must be inferred rather than stated).

**Examples.**
- *"Solve x² = 4"*: **low** — input fully specifies the problem.
- *"Make the homepage feel faster"*: **medium** — actionable, but the agent must guess what the user means by *feel*, what trade-offs are acceptable, and what counts as *enough*.
- *"Help us understand customer churn"*: **high** — half the work is converting the prompt into a specific question.

Ambiguity load measures how much of the task is *task definition* the agent must do for itself.

---

## 11. Verification hardness

**Scale.** Cheap / expensive / undecidable.

**Instrumentation.** Cost ratio of verification to production for one task instance.

**Examples.**
- *Sorted-output verification* or *code-passes-test*: **cheap** — verify in O(n) where production is O(n log n) or worse.
- *Architectural design review, multi-component code review*: **expensive** — a senior engineer-hour per artifact.
- *Long-horizon ethics or safety judgment*: **undecidable in-task** — no in-horizon protocol that all evaluators would accept.

Verification hardness composes badly with low oracle availability. When both are high, reported scores measure the rater pool more than the agent.

---

## 12. Coordination requirement

**Scale.** Solo / human-in-loop / multi-agent.

**Instrumentation.** Count of non-actor decision-makers whose actions are required on the critical path to completion.

**Examples.**
- *Code patch passing a test suite*: **solo** — no other actor required.
- *Pair-programming workflow, IDE assistant*: **human-in-loop** — a human must accept, reject, or steer at decision points; latency and legibility become first-class.
- *Cross-team incident response, supply-chain operation*: **multi-agent** — multiple agents (and humans) coordinate; protocol design dominates capability.

Coordination requirement determines whether *reliability* and *legibility* are first-class success criteria or merely nice-to-have properties.

---

## 13. Regression risk

**Scale.** None / local / global.

**Instrumentation.** Count of invariants the task can violate while still satisfying its operational oracle (i.e., how often pass-by-oracle is failure-by-system).

**Examples.**
- *Pure function with full behavioral test coverage*: **none** — passing tests implies correctness within the task scope.
- *Refactor in a legacy codebase with partial coverage*: **local** — adjacent modules may break in untested ways.
- *Schema migration in a production system*: **global** — downstream services, ETL pipelines, and analytics dashboards may break in ways the migration's own oracle cannot see.

Regression risk is the dimension that makes a "passing" agent dangerous. It is what most current benchmarks structurally cannot measure, because their oracle is the only ground truth in the loop.

---

## How dimensions compose

Two tasks may share every primitive yet differ across these dimensions in ways that determine which agents win and which fail. Mapping a benchmark onto these dimensions is the work of [BENCHMARK-MAPS.md](./BENCHMARK-MAPS.md). Mapping an agent failure onto them is the work of [CASE-STUDIES.md](./CASE-STUDIES.md).
