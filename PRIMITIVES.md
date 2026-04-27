# Primitives

The building blocks of a task. Anything that can be called a task can be parsed into some subset of these. They are not algorithms; they are the *parts of speech* of task description.

Each entry below carries: a definition, a **concrete signature** (where you would observe it in benchmark spec, harness config, or agent trace), and **three worked examples** in distinct task contexts. The three-example requirement is not stylistic — it is the [extractive constraint](./CONTRIBUTING.md): if a primitive does not show up in three different real settings, it is not a primitive yet.

---

## 1. Actor

**Definition.** The system-under-evaluation as a delimited entity — what counts as "the agent" and what is treated as outside.

**Signature.** Drawn by the harness boundary. Look for: which subprocesses are scored, which calls are billed to the actor, what is exempt from the constraint budget.

**Examples.**
- *Coding agent in a CLI harness*: actor is the LLM plus its tool-use loop; the test harness, the human reviewer, and the editor are outside.
- *Bare LLM as classifier*: actor is just the model plus decoding parameters; there is no persistent scratch.
- *Multi-agent orchestration*: actor boundary is contested — is "the agent" the single LLM, the orchestrator, or the entire swarm? The benchmark must declare this, and that declaration is theory.

The actor boundary is the first act of construction. Moving it changes the task even when nothing else changes.

---

## 2. Goal

**Definition.** The terminal condition or transformation that defines what *done* means for this task.

**Signature.** Encoded as a success predicate (`pass/fail`), a rubric, a target answer, or a natural-language objective; sometimes split between an *operational goal* (what the harness scores) and an *intended goal* (what the user actually wants).

**Examples.**
- *SWE-bench-Verified*: operational goal is "the held-out test suite passes." A developer's intended goal is closer to "tests pass *and* no regression *and* minimal change *and* maintainable."
- *GAIA*: goal is "match the gold final answer string." The reasoning trace is invisible to scoring.
- *Customer-support dialog*: goal is "the user's problem is resolved within policy" — only partially formalizable; humans adjudicate edge cases.

The gap between operational and intended goal is where most evaluation surprises live.

---

## 3. Initial state

**Definition.** What the actor inherits at t=0 — context, scratch, prior history, configured tools, environment snapshot.

**Signature.** System prompt; repo snapshot at a specific commit; dialog history seed; tool list; available files; environment variables.

**Examples.**
- *Code repair*: initial state includes the repo at HEAD, the failing test suite, the issue text, and a writable workspace.
- *Multi-turn dialog*: initial state includes a persona prompt and the conversation up to the current turn.
- *Browser task*: initial state includes the rendered DOM, the active tab, login cookies, and user preferences.

Two harnesses with the same goal but different initial states are running different tasks.

---

## 4. Target state

**Definition.** The state that, when reached, satisfies the goal predicate. May be a unique state or an equivalence class.

**Signature.** Test suite at green; gold answer matched; rubric thresholds met; user-acceptance signal received.

**Examples.**
- *Coding*: target state is a repo where the held-out tests pass and the original passing tests still pass.
- *Web task*: target state may be a confirmation page (purchase complete, form submitted).
- *Open-ended writing*: target state is a draft satisfying an opaque rubric — often only the human evaluator's intuition fully encodes it.

When the target state is an equivalence class, the size of that class is itself a task property: a wider class is a more *forgiving* task.

---

## 5. Constraints

**Definition.** Rules and budgets that exclude regions of the action space — what the actor must obey, what it may not violate.

**Signature.** Tool whitelist; maximum step count; token budget; deadline; policy rules ("never call this endpoint," "never read this file").

**Examples.**
- *τ-bench*: explicit policy constraints — a customer-service agent must follow company rules even when the user pressures it to break them.
- *Coding agent under budget*: maximum tool calls, maximum file size, allowed languages.
- *Browser agent*: forbidden URLs, forbidden form submissions, no payment actions.

Constraints are part of the task definition, not external to it. Lifting a constraint changes which agent wins.

---

## 6. Tools

**Definition.** Action affordances and their preconditions — what the actor can do, when, and at what cost.

**Signature.** Tool schemas (JSON definitions, function signatures); command lists; API surface; available shell commands.

**Examples.**
- *Coding agent*: `Read`, `Edit`, `Write`, `Grep`, `Bash`, `WebSearch`.
- *Browser agent*: `click`, `type`, `scroll`, `navigate`, `wait`.
- *Bare LLM with no external tools*: tools reduce to `{decode next token}`. This is still a valid tool primitive — and it is exactly the limit case that explains why tool-augmented agents and bare LLMs cannot share a benchmark without re-projection.

The tool set is not background; it is part of the task.

---

## 7. Environment

**Definition.** Everything that mutates outside the actor's deliberation — including stochastic responses, other agents, and the human user when the user is not the actor.

**Signature.** Is the test suite fixed or recomputed? Is the DOM dynamic? Do other agents act between the actor's steps? Are tool outputs deterministic or sampled?

**Examples.**
- *MMLU and most static QA*: environment is frozen; identical input always returns identical context.
- *WebArena, OSWorld*: environment is highly mutable — pages load asynchronously, popups appear, sessions expire.
- *Multi-agent debate*: each agent is environment to every other agent. There is no "background."

Environment mutability is one of the strongest predictors of evaluation noise.

---

## 8. Feedback

**Definition.** Signals returned to the actor during execution — distinct from terminal scoring.

**Signature.** stdout/stderr from tools; HTTP status codes; partial-credit indicators; observation deltas; intermediate confirmation.

**Examples.**
- *Coding*: rich feedback — test runner stack traces, type-checker errors, lint warnings, file diffs.
- *Single-shot QA*: zero in-trajectory feedback; only the terminal score.
- *Negotiation or persuasion task*: feedback is encoded in the counterparty's responses — qualitatively rich, quantitatively sparse.

Feedback shapes what the agent can learn *within* a trajectory; its absence is a task property, not an oversight.

---

## 9. Oracle

**Definition.** What determines correctness, when, and by what authority.

**Signature.** Test suite; gold-answer string match; rubric scored by a second model; human grader; learned reward model; user-acceptance signal.

**Examples.**
- *Code with a comprehensive test suite*: oracle-rich. Cheap, fast, deterministic, narrow.
- *Product-design rubric*: oracle-poor. Slow, contested between human reviewers, unstable across raters.
- *Open-ended research*: oracle-absent. The "truth" of a research direction may not be settled for years; any in-task scoring is a stand-in.

The oracle is often confused with the goal. They are different: the goal is *what success looks like*; the oracle is *how we decide whether success has happened*. A goal can be perfectly clear and still oracle-poor.

---

## 10. Cost

**Definition.** Units of effort consumed in producing a trajectory.

**Signature.** Logged token counts (input, output, cached); tool-call counts; wallclock; dollar billing; attention or review-time of any human in the loop.

**Examples.**
- *API agent on a coding task*: cost is dominated by tokens — input + output + cache miss penalties.
- *Browser agent on OSWorld*: cost is dominated by wallclock — page loads and rendering time outweigh model inference.
- *Human-in-the-loop review*: cost is the reviewer's time per decision; the model's inference cost may be negligible.

Cost is a first-class primitive because two trajectories with identical outcomes but different costs are not equivalent agent performances.

---

## 11. Risk

**Definition.** The irreversibility surface — what failures cannot be undone, and the blast radius they imply.

**Signature.** Whether actions write to durable state; whether they can be rolled back; what downstream systems depend on the affected state.

**Examples.**
- *Local code patch in a sandbox*: low risk — `git reset` reverts.
- *Production database migration*: high risk — a partial migration can corrupt state irrecoverably.
- *Autonomous trade execution or external API call with side effects*: catastrophic risk — transactions are settled, emails are sent, and there is no undo.

Risk is what separates "evaluable in a benchmark" from "deployable in production." Many tasks that look identical by goal differ enormously in risk.

---

## 12. Time horizon

**Definition.** Minimum, expected, and maximum trajectory length — measured in turns, tool calls, or wallclock.

**Signature.** The trace-length distribution: mean, tail, and ratio of long-trace failures.

**Examples.**
- *Single-shot QA*: horizon ≡ 1.
- *Coding agent on SWE-bench*: typical horizon 30–100 tool calls, minutes of wallclock.
- *Operational agent persisting across sessions*: horizon may extend across hours or days, with state-resumption challenges that do not exist in single-session benchmarks.

Time horizon interacts with cost and verification: long-horizon tasks compound feedback latency and verification expense in ways short-horizon benchmarks cannot reveal.

---

## 13. Decomposition structure

**Definition.** How the task naturally breaks into subtasks — or refuses to.

**Signature.** Are subgoals explicit in the spec? Do intermediate sub-rewards exist? Can intermediate verification be performed without finishing? Are subtasks independent or sequentially dependent?

**Examples.**
- *Bug fix in a real repo*: heavy decomposition — locate, understand, patch, test, verify, explain. Each sub-step is locally checkable.
- *Single factual lookup*: no decomposition — one retrieval, one return.
- *Creative writing or research synthesis*: refuses decomposition — quality is gestalt; sub-scoring of intermediate paragraphs is noisy and often misleading.

Whether a task decomposes is not a property of the agent's strategy; it is a property of the task.

---

## 14. Verification regime

**Definition.** Who or what re-checks the result, against what reference, at what cost.

**Signature.** Auto-graders; second-opinion models; human reviewers; peer-review or staged-rollout protocols; downstream production telemetry.

**Examples.**
- *Test-suite verification*: cheap, fast, narrow — captures behavioral correctness, misses maintainability or regression risk.
- *LLM-as-judge*: cheap, fast, broad in coverage — but introduces its own biases (length, formatting, paraphrase preferences) that become part of the task.
- *Human expert review*: expensive, slow, unscalable — but most defensible against contested cases.

The verification regime is often confused with the oracle. The oracle decides whether the answer is right *in principle*; the verification regime is *how we actually find out*. Agents that pass the oracle but fail the verification regime have not succeeded at the task.

---

## How these primitives compose

A task description is a configuration of these 14 primitives. Many task differences that look like differences of *kind* (coding vs. dialog vs. web) turn out, on this account, to be differences of *configuration*. That is the load-bearing claim of this ontology: tasks are not categorical labels; they are points in a structured space.

The axes of that space are described in [DIMENSIONS.md](./DIMENSIONS.md).
