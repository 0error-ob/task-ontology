# Case Studies

These walkthroughs show the ontology working — taking a phenomenon that is normally narrated as "the model did X" or "the benchmark is noisy" and re-describing it as a structural fact about the task.

---

## 1. Two identical runs, different outcomes

A common observation in agent evaluation: the same agent, given the same input, on the same benchmark, produces different outcomes across runs. The aggregate pass rate is stable; the per-task outcome is not. Discussions of this usually settle on "stochastic decoding" or "tool noise" as the explanation. The ontology suggests that explanation is shallow.

Consider a single coding task: the agent receives an issue and a repo at a fixed commit, and must produce a patch that passes a held-out test suite.

**What does "identical input" actually fix?**

Mapping onto primitives:

| Primitive | Fixed across runs? | Notes |
|---|---|---|
| Actor | Yes | Same model, same decoding parameters |
| Goal | Yes | Same test suite |
| Initial state | Yes | Same repo commit, same issue text |
| Target state | Yes | Defined by the same oracle |
| Constraints | Yes | Same budget |
| Tools | Yes | Same tool surface |
| **Environment** | **No** | Tool returns can be order-dependent; race conditions in test runners; non-deterministic tool outputs |
| **Feedback** | **No** | Same actions can yield slightly different observations |
| Oracle | Yes | Same test suite |

Two of the primitives — *Environment* and *Feedback* — are not actually fixed. The agent's first move is sampled. That move triggers an observation. That observation conditions the next move. Small early-trajectory differences compound through *Decomposition structure*. Add in the *State observability* dimension being only partial (the agent must build its map of the repo), and what looked like "the same task" was never the same trajectory from the second tool call onward.

**What this re-description changes.**

Calling this "noise" suggests it is something to be averaged away with more runs. The ontology says it is something to be *characterized*: the variance is concentrated in a few primitives (Environment mutability, Feedback determinism), and a few dimensions (State observability, Decomposition depth, Feedback latency). Two benchmarks with the same nominal task can have very different per-task variance depending on these settings — and which agent wins on the leaderboard can depend on which benchmark has higher variance, not on which agent is "better."

The remediation prescribed by the ontology is not "report N=3 averages." It is: report the configuration of the variance-bearing primitives and dimensions, so a reader can tell whether one benchmark's claimed superiority is a real ranking or a reweighting of the noise.

---

## 2. What does it mean for an agent to regress?

When a deployed agent "gets worse," the conversation usually settles on one of three answers: the model changed (a silent provider update), the harness changed (a prompt or scaffold tweak), or the perception is wrong (selection bias, recency bias). All three can be true simultaneously, which is part of why the question is hard to settle.

The ontology suggests the question itself is malformed. *Worse at what?* is the prior question. An agent does not regress as a scalar. It regresses *task-conditionally* — better on some primitives, worse on others, neutral on the rest. The right unit of analysis is **task-conditional behavior shift**.

Six observable shift dimensions, expressible from agent traces alone (no model internals required):

### 2.1 Repo-navigation efficiency

*Where it lives.* `Initial state` × `State observability` × `Decomposition structure`.

*Signature in trace.* Number of file reads before the first edit; ratio of unique files read to total reads; whether the agent re-reads the same file across long stretches of trajectory.

*What a shift means.* If the agent now needs more reads to localize the same kind of bug, its mental model of the repo is degrading, even when the patch eventually lands.

### 2.2 Multi-step planning coherence

*Where it lives.* `Goal` × `Decomposition structure` × `Time horizon`.

*Signature in trace.* Whether early actions point toward the eventual fix; whether the agent abandons subgoals; whether trajectory shape is a clean path or a wandering walk.

*What a shift means.* Coherence loss often shows up in benchmark scores only on the deepest tasks; shallow tasks remain unaffected. A scalar pass-rate may stay flat while operational reliability collapses on the long tail.

### 2.3 Tool-use loop length

*Where it lives.* `Tools` × `Cost` × `Feedback latency`.

*Signature in trace.* Tool calls per task at constant pass rate. A 30% increase in tool calls without a corresponding pass-rate gain is a regression in *cost-per-success* even when *success-per-task* is unchanged.

*What a shift means.* Some "no regression" reports are reports that pass-rate held while cost rose. Whether that is a regression depends on what the deployment cares about — and most deployments care.

### 2.4 Edit-versus-read action ratio

*Where it lives.* `Tools` × `State observability` × `Verification regime`.

*Signature in trace.* Ratio of write actions to read actions. A sharp shift toward writes (less reading, more attempting) often precedes a degradation in patch quality.

*What a shift means.* The agent is becoming more confident with less context — exactly the failure mode that is invisible until a regression is introduced. This dimension is a leading indicator, not a lagging one.

### 2.5 Verification discipline

*Where it lives.* `Verification regime` × `Feedback` × `Oracle`.

*Signature in trace.* Frequency of self-checks: does the agent re-run tests after edits? Does it verify the file it wrote matches what it intended? Does it check whether unrelated tests still pass?

*What a shift means.* Verification erosion is one of the most consequential regressions to detect because it shows up *after* deployment, in the form of bugs the agent could have caught for itself.

### 2.6 Context-reconstruction cost

*Where it lives.* `Initial state` × `Cost` × `Time horizon`.

*Signature in trace.* How often the agent re-reads files it has already read; how often it re-asks for information already in context; how much of its trajectory is spent rebuilding state versus advancing the task.

*What a shift means.* A regression in context-reconstruction is a regression in the agent's ability to *remain in the task* — and it is a load-bearing capacity for any non-trivial deployment.

### What this reframing buys

The reframe replaces "did the agent regress?" — which is unanswerable — with six questions that are answerable from logs an evaluator already has:

- Did navigation efficiency drop?
- Did planning coherence drop on long tasks?
- Did tool-use loop length grow at constant pass rate?
- Did the edit:read ratio shift toward writes?
- Did self-verification rate decline?
- Did context-reconstruction cost rise?

Each question maps onto a small number of primitives and dimensions. Each is independently measurable. Each isolates a different failure mode of the same agent. None of them is captured by pass-rate alone.

This is the template the ontology offers for any "the model got worse" conversation: stop arguing about whether the agent regressed, and ask *along which task-conditional axes the behavior has shifted*. The shift may be a real degradation, or it may be a workload change with no model-side cause — but the question becomes answerable.

---

## What both case studies have in common

In each case, the ontology converts a phenomenon usually narrated as either "noise" or "vibes" into a structured description. It does not give you a new metric. It gives you a way of asking which metrics would tell you what you actually want to know — and that, the rest of agent evaluation can build on.
