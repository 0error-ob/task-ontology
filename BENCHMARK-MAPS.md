# Benchmark Maps

This document re-describes existing agent benchmarks in the vocabulary of [PRIMITIVES.md](./PRIMITIVES.md) and [DIMENSIONS.md](./DIMENSIONS.md). The goal is not to critique these benchmarks. They are real artifacts that have moved the field. The goal is to make explicit *which task* each one projects, and what its leaderboard score does and does not measure.

A benchmark is a projection of a task under chosen interfaces. Two benchmarks targeting "the same capability" can project differently — and the projection determines which models win.

For each benchmark below: a primitives sketch, a dimensions sketch, and a paragraph on **what this re-description makes visible that the leaderboard view hides.**

---

## SWE-bench-Verified

A benchmark of real GitHub issues from popular Python repositories. The agent receives the issue text and the repo at a specific commit; it must produce a patch that makes a held-out test suite pass.

**Primitives sketch.**

| Primitive | Configuration |
|---|---|
| Actor | LLM with tool-use loop (Read / Edit / Write / Grep / Bash) |
| Goal | Held-out test suite passes; original passing tests still pass |
| Initial state | Repo at fixed commit + issue text + failing tests |
| Target state | Repo where the test suite is green |
| Constraints | Token / time budget; usually no internet |
| Tools | Shell, file ops, sometimes Python execution |
| Environment | Actor-driven — only the agent modifies repo state |
| Feedback | Test runner output, type-checker errors, file diffs |
| Oracle | Fixed pre-authored test suite |
| Cost | Tokens, tool calls |
| Risk | Low (sandboxed) |
| Time horizon | Typically 30–100 tool calls |
| Decomposition | Deep — locate → understand → patch → test → verify |
| Verification regime | Same as oracle (test suite) |

**Dimensions sketch.**

| Dimension | Position |
|---|---|
| Goal clarity | Explicit |
| State observability | Partial (large repo, agent builds its own map) |
| Oracle availability | Rich |
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

**What this re-description makes visible.** Pass rate is a single scalar that compresses at least four distinct task structures: locating the right code, understanding the failure, producing a minimal patch, and not breaking adjacent invariants. Two agents at the same pass rate may differ enormously on patch surface area, on number of files touched, on whether they fixed the symptom or the cause, and on whether they introduced regressions outside the held-out test suite. The benchmark's verification regime equals its oracle, so any failure mode visible only outside the test suite — maintainability collapse, hidden invariant violations, over-broad edits — is structurally invisible. The leaderboard does not lie; it just answers a narrower question than its name suggests.

---

## GAIA

A benchmark of "general AI assistant" questions requiring real-world reasoning, web research, multi-modal inputs, and tool use. Each question has a single canonical answer; agents are scored by exact match against the gold final answer string.

**Primitives sketch.**

| Primitive | Configuration |
|---|---|
| Actor | LLM with tool-use (web search, browser, file readers, calculator) |
| Goal | Emit a final-answer string matching the gold answer |
| Initial state | Question text, sometimes an accompanying file |
| Target state | Emitted answer ≡ gold answer (after light normalization) |
| Tools | Search, browser, file parsing, calculator |
| Environment | Exogenous — the live web |
| Feedback | Tool outputs only; no in-task scoring |
| Oracle | Exact-match against gold |
| Cost | Tokens + wallclock for browsing |
| Risk | Low |
| Time horizon | Variable, often deep |
| Decomposition | Shallow to deep depending on question |
| Verification regime | Exact match |

**Dimensions sketch.**

| Dimension | Position |
|---|---|
| Goal clarity | Explicit |
| State observability | Hidden (information lives on the web) |
| Oracle availability | Rich |
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

**What this re-description makes visible.** GAIA scores conflate at least three distinct capabilities: reasoning, search-tool fluency, and resilience to the live web. Because the environment is exogenous and the source information can move or change, scores have non-trivial run-to-run variance that pass-rate alone does not surface. A weaker reasoner with better search habits can outperform a stronger reasoner with worse search habits, and the leaderboard cannot distinguish them. The exact-match oracle, while convenient, makes the benchmark allergic to legitimate paraphrase — answers that are *correct* but formatted differently fail. This makes GAIA a strong measure of one specific projection of "general assistant" capability, not of the capability itself.

---

## τ-bench

A benchmark of agents playing customer-service roles that must resolve user issues while obeying explicit company policies. The user is simulated by another LLM. Scoring is programmatic: did the database end in the correct state, and was the policy obeyed throughout the dialog?

**Primitives sketch.**

| Primitive | Configuration |
|---|---|
| Actor | LLM playing the agent role |
| Goal | Resolve the user's issue subject to policy |
| Initial state | Persona prompt, policy document, user dialog seed |
| Target state | Database state ≡ expected; policy violations = 0 |
| Constraints | Policy rules (which actions are disallowed) |
| Tools | Function calls (lookup, refund, cancel, etc.) |
| Environment | Simulated user (LLM-driven) + tool side-effects on a mock DB |
| Feedback | Tool returns and user dialog turns |
| Oracle | Programmatic DB-state + policy-compliance check |
| Cost | Tokens (both sides), turns |
| Time horizon | Tens of turns |
| Decomposition | Shallow |
| Verification regime | Programmatic |

**Dimensions sketch.**

| Dimension | Position |
|---|---|
| Goal clarity | Latent (multiple resolutions may satisfy the spec) |
| State observability | Partial — user intent must be inferred from dialog |
| Oracle availability | Rich (programmatic) |
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

**What this re-description makes visible.** τ-bench scores reflect not only the agent under test but the cooperation profile of the simulated user. Because the user is exogenous and stochastic, identical agent strategies face different conversational difficulty across runs — and that variance is rarely controlled in reported numbers. The benchmark is among the better instances of *latent* goal clarity and *medium* ambiguity load made evaluable; that is what makes it interesting. But comparing agent A's score to agent B's score without controlling for user-side variance compares two different distributions of difficulty, not two agent capabilities.

---

## OSWorld

A benchmark in which agents complete real operating-system tasks (file management, app interaction, workflow automation) in a virtualized desktop environment, controlling the screen via mouse and keyboard.

**Primitives sketch.**

| Primitive | Configuration |
|---|---|
| Actor | Agent with screen + keyboard + mouse tool surface |
| Goal | Bring the OS / app state to a specified configuration |
| Initial state | VM snapshot with installed apps and starting file state |
| Target state | File / app state matching the task spec |
| Tools | Screenshot, click, type, scroll, keyboard shortcuts |
| Environment | Highly exogenous — async loading, popups, OS notifications |
| Feedback | Screen state per step |
| Oracle | Programmatic check on final VM state |
| Cost | Wallclock-dominant (rendering, page loads) |
| Risk | Costly within VM (state changes), reversible by snapshot |
| Time horizon | Deep — many tens of steps for non-trivial tasks |
| Decomposition | Deep |
| Verification regime | Programmatic state-diff |

**Dimensions sketch.**

| Dimension | Position |
|---|---|
| Goal clarity | Explicit |
| State observability | Partial — agent only sees the screen |
| Oracle availability | Rich |
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

**What this re-description makes visible.** OSWorld scores are dominated by environment mutability and screen-grounding fidelity, not by reasoning. App version differences, screen-rendering jitter, and asynchronous popups produce variance that any single pass-rate number flattens. The benchmark is best understood as measuring *the joint distribution of agent capability and harness reliability under exogenous environment dynamics* — a different and more honest description than "OS task ability." The leaderboard implies the latter; the task ontology forces the former.

---

## What this exercise is for

When four benchmarks all claim to measure agent capability and yet rank models differently, the cause is usually not noise. It is that they are projecting different tasks. The vocabulary above lets us state what each is actually projecting — which is the prerequisite for any defensible comparison across them, or for any decision about which one to trust for a given product question.

Mapping a benchmark is roughly an afternoon's work once the vocabulary is fixed. Patterns and pull requests welcome (see [CONTRIBUTING.md](./CONTRIBUTING.md)).
