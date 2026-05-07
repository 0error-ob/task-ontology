# Benchmark Maps

This document re-describes existing agent benchmarks in the vocabulary of [PRIMITIVES.md](./PRIMITIVES.md) and [DIMENSIONS.md](./DIMENSIONS.md). The goal is not to critique these benchmarks — they are real artifacts that have moved the field. The goal is to make explicit *which task* each one constructs, stated in terms that allow direct comparison across benchmarks and across task structures.

A benchmark instantiates a task by fixing a set of primitives at specific values and a measurement interface. Re-describing it in this vocabulary makes the construction explicit.

For each benchmark below: a primitives sketch, a dimensions sketch, and a note on **what the re-description makes visible that the benchmark name alone does not.**

---

## SWE-bench-Verified

A benchmark of real GitHub issues from popular Python repositories. The agent receives the issue text and the repo at a specific commit; it must produce a patch that makes a held-out test suite pass.

**Primitives sketch.**

| Primitive | Configuration |
|---|---|
| Actor | LLM with tool-use loop (Read / Edit / Write / Grep / Bash) |
| Goal | Held-out test suite passes; original passing tests still pass |
| Transform type | Transformation (patch) composed with Extraction (locate change site) |
| Initial state | Repo at fixed commit + issue text + failing tests |
| Target state | Repo where the test suite is green |
| Constraints | Token / time budget; usually no internet |
| Tools | Shell, file ops, sometimes Python execution |
| Environment | Actor-driven — only the agent modifies repo state |
| Feedback | Test runner output, type-checker errors, file diffs |
| Cost | Tokens, tool calls |
| Risk | Low (sandboxed) |
| Time horizon | Typically 30–100 tool calls |
| Decomposition | Deep — locate → understand → patch → test → verify |

**Dimensions sketch.**

| Dimension | Position |
|---|---|
| Goal clarity | Explicit |
| State observability | Partial (large repo, agent builds its own map) |
| Determinism | Bounded stochastic (multiple valid patches accepted) |
| Decomposition depth | Deep |
| Tool dependence | Required |
| Feedback latency | Delayed (often many edits per test cycle) |
| Environment mutability | Actor-driven |
| Failure reversibility | Reversible (sandbox) |
| Context locality | Repo-scale |
| Ambiguity load | Low (issue text is provided) |
| Verification hardness | Cheap |
| Coordination requirement | Solo |
| Regression risk | Local |
| Evidence surface | Test-visible (repo and some tests visible; held-out tests hidden) |

**What the re-description makes visible.** Pass rate compresses at least four distinct task structures: locating the right code (Extraction), understanding the failure (Classification), producing a minimal patch (Transformation), and not breaking adjacent invariants (Regression control). Two agents at the same pass rate may differ enormously on patch surface area, files touched, and whether they fixed the symptom or the cause. Failures outside the held-out test suite — maintainability collapse, hidden invariant violations, over-broad edits — are structurally invisible to the task as constructed. The benchmark answers a narrower question than its name implies.

---

## GAIA

A benchmark of "general AI assistant" questions requiring real-world reasoning, web research, multi-modal inputs, and tool use. Each question has a single canonical answer; agents are scored by exact match against the gold final answer string.

**Primitives sketch.**

| Primitive | Configuration |
|---|---|
| Actor | LLM with tool-use (web search, browser, file readers, calculator) |
| Goal | Emit a final-answer string matching the gold answer |
| Transform type | Extraction-dominant for factual questions; Decision-dominant for multi-step reasoning |
| Initial state | Question text, sometimes an accompanying file |
| Target state | Emitted answer ≡ gold answer (after light normalization) |
| Constraints | None explicit |
| Tools | Search, browser, file parsing, calculator |
| Environment | Exogenous — the live web |
| Feedback | Tool outputs only; no in-task scoring signal |
| Cost | Tokens + wallclock for browsing |
| Risk | Low |
| Time horizon | Variable, often deep |
| Decomposition | Shallow to deep depending on question |

**Dimensions sketch.**

| Dimension | Position |
|---|---|
| Goal clarity | Explicit |
| State observability | Hidden (information lives on the web) |
| Determinism | Fully deterministic (one gold answer) |
| Decomposition depth | Variable |
| Tool dependence | Required |
| Feedback latency | Delayed |
| Environment mutability | Exogenous (the live web changes) |
| Failure reversibility | Reversible |
| Context locality | Cross-system |
| Ambiguity load | Low |
| Verification hardness | Cheap |
| Coordination requirement | Solo |
| Regression risk | None (single output) |
| Evidence surface | Behavior-visible / web-visible (question and tool outputs visible; gold answer hidden) |

**What the re-description makes visible.** The benchmark conflates at least three distinct capabilities: reasoning (Decision), search-tool fluency (Extraction from the live web), and resilience to exogenous environment change. A weaker reasoner with better search habits can outperform a stronger reasoner with worse ones; the score cannot distinguish them. The exact-match success predicate makes the task allergic to legitimate paraphrase — correct answers formatted differently fail. This defines a narrower and more specific task than "general AI assistant ability."

---

## τ-bench

A benchmark of agents playing customer-service roles that must resolve user issues while obeying explicit company policies. The user is simulated by another LLM. Scoring is programmatic: did the database end in the correct state, and was the policy obeyed throughout the dialog?

**Primitives sketch.**

| Primitive | Configuration |
|---|---|
| Actor | LLM playing the agent role |
| Goal | Resolve the user's issue subject to policy |
| Transform type | Decision-dominant (select action under policy constraints) with Generation (compose response) |
| Initial state | Persona prompt, policy document, user dialog seed |
| Target state | Database state ≡ expected; policy violations = 0 |
| Constraints | Policy rules (which actions are disallowed) |
| Tools | Function calls (lookup, refund, cancel, etc.) |
| Environment | Simulated user (LLM-driven) + tool side-effects on a mock DB |
| Feedback | Tool returns and user dialog turns |
| Cost | Tokens (both sides), turns |
| Risk | Low (simulated) |
| Time horizon | Tens of turns |
| Decomposition | Shallow |

**Dimensions sketch.**

| Dimension | Position |
|---|---|
| Goal clarity | Latent (multiple resolutions may satisfy the spec) |
| State observability | Partial — user intent must be inferred from dialog |
| Determinism | Bounded stochastic (multiple valid resolution paths) |
| Decomposition depth | Shallow |
| Tool dependence | Required |
| Feedback latency | Immediate |
| Environment mutability | Exogenous (user-LLM is unpredictable) |
| Failure reversibility | Reversible (simulated) |
| Context locality | Local |
| Ambiguity load | Medium |
| Verification hardness | Cheap |
| Coordination requirement | Human-in-loop, simulated |
| Regression risk | Local (policy violations) |
| Evidence surface | Spec-visible (policy and tool state visible; simulator internals hidden) |

**What the re-description makes visible.** Scores reflect not only the agent under test but the cooperation profile of the simulated user. Because the user is exogenous and stochastic, identical agent strategies face different conversational difficulty across runs. τ-bench is one of the better-constructed benchmarks for latent goal clarity and medium ambiguity load; the task it constructs is genuinely close to a real operational setting. Comparing two agents' scores without controlling for user-side variance compares two different difficulty distributions, not two agent configurations on the same task.

---

## OSWorld

A benchmark in which agents complete real operating-system tasks (file management, app interaction, workflow automation) in a virtualized desktop environment, controlling the screen via mouse and keyboard.

**Primitives sketch.**

| Primitive | Configuration |
|---|---|
| Actor | Agent with screen + keyboard + mouse tool surface |
| Goal | Bring the OS / app state to a specified configuration |
| Transform type | Transformation (state change via UI interaction) composed with Extraction (locate current state from screen) |
| Initial state | VM snapshot with installed apps and starting file state |
| Target state | File / app state matching the task spec |
| Constraints | None explicit beyond the VM |
| Tools | Screenshot, click, type, scroll, keyboard shortcuts |
| Environment | Highly exogenous — async loading, popups, OS notifications |
| Feedback | Screen state per step |
| Cost | Wallclock-dominant (rendering, page loads) |
| Risk | Costly within VM (state changes), reversible by snapshot |
| Time horizon | Deep — many tens of steps for non-trivial tasks |
| Decomposition | Deep |

**Dimensions sketch.**

| Dimension | Position |
|---|---|
| Goal clarity | Explicit |
| State observability | Partial — agent only sees the screen |
| Determinism | Bounded stochastic (multiple valid action paths to the same end state) |
| Decomposition depth | Deep |
| Tool dependence | Required |
| Feedback latency | Immediate |
| Environment mutability | Exogenous (highest of the four benchmarks here) |
| Failure reversibility | Costly (VM revert) |
| Context locality | Cross-system (multiple apps) |
| Ambiguity load | Low |
| Verification hardness | Cheap |
| Coordination requirement | Solo |
| Regression risk | Local |
| Evidence surface | Behavior-visible (screen state and app behavior visible; evaluator checks hidden) |

**What the re-description makes visible.** Performance is dominated by environment mutability and screen-grounding fidelity rather than reasoning. App version differences, rendering jitter, and asynchronous popups produce variance that pass-rate flattens. The task as constructed measures the joint behavior of (agent capability, harness reliability, environment dynamics) — a more precise description than "OS task ability." Two agents with identical reasoning capabilities but different screen-grounding strategies will diverge substantially on this task.

---

## What this exercise is for

When benchmarks that all claim to measure "agent capability" rank models differently, the cause is not usually noise. It is that they construct different tasks. The vocabulary above lets us state what each one actually constructs — which is the prerequisite for any defensible comparison across benchmarks, or for any decision about which one to trust for a given question.

Mapping a benchmark is roughly an afternoon's work once the vocabulary is fixed. Patterns and pull requests welcome: see [CONTRIBUTING.md](./CONTRIBUTING.md).
