# Task Ontology

### A Substrate for Agent Evaluation

> **Benchmarks do not merely measure tasks. They instantiate a theory of what a task is.**

> *Task definition is not neutral. It encodes the evaluator's theory of intelligence.*

---

## What this is

A vocabulary for describing how tasks are constructed, projected, and operationalized in agent evaluation. Primitives, dimensions, and benchmark re-descriptions — not algorithms, not a framework, not a leaderboard.

Before we ask whether an agent succeeds, this asks: *what kind of task has been constructed, what has been made observable, what has been hidden, and what does the scoring interface assume?*

## What this is not

- **Not** a planning formalism in the MDP / POMDP tradition.
- **Not** a cognitive task analysis taxonomy in the GOMS / KLM / HCI tradition.
- **Not** a specification language for RL environments or agent harnesses.

Those traditions answer *how to solve a task once it has been formalized.* This project asks the prior question: *how is a task formalized in the first place, and what does that choice cost?*

---

## A motivating example: what is a "coding task"?

The phrase *"AI helps engineers write code"* names a real problem. But the moment we try to evaluate it, we have to cut. And how we cut determines almost everything that follows.

The same underlying problem can be cut as:

| Cut | What gets evaluated |
|---|---|
| Coding benchmark | Does the patch pass the test suite? |
| Debugging task | Can the agent localize and explain the root cause? |
| Agent planning task | Does the agent decompose work into sound subgoals? |
| Tool-use task | Does the agent select, sequence, and parameterize tools efficiently? |
| Repo-navigation task | Can the agent build a working map of an unfamiliar codebase? |
| Regression-control task | Does the patch avoid breaking invariants elsewhere? |
| Human-AI collaboration task | Is the agent legible and steerable to a co-working developer? |
| Cost-accuracy optimization | Does it succeed under a token / wallclock / dollar budget? |
| Reliability-under-uncertainty | Does it recognize when it does not have enough information? |

Each cut yields a different benchmark, surfaces a different model preference, draws a different evaluation conclusion, and points to a different product direction. **The cut is the theory.** Most of the field debates models. Very little of the field debates the cut.

---

## The three layers of a task

| Layer | Definition | Where it lives |
|---|---|---|
| **Surface** | Input → output → score. | Most current benchmarks. |
| **Structural** | A state-transformation under constraints, with tools, oracle, feedback, decomposition. | Closer to real agent work. |
| **Constructed** | The task as cut and framed by the modeler, harness designer, evaluator, observer. | Where observer bias, modeler priors, certainty-seeking, and framing residual operate. |

A task ontology has to address all three. The surface layer is what we score; the structural layer is what we actually do; the constructed layer is what we cannot see when we are inside our own evaluation choices.

---

## Three claims

**1. Tasks are constructed, not given.** No agent task arrives free of framing. A modeler chose where to draw the boundary, what counts as input, what the success signal is, what the actor is allowed to observe. Those choices are theory-laden, and they propagate downstream into every metric.

**2. Benchmarks are projections of tasks under chosen interfaces, not measurements of intrinsic difficulty.** A benchmark fixes one cut and one interface. Two benchmarks targeting "the same" capability can diverge on which model wins, because they project the underlying task differently. Treating a leaderboard as a measurement of capability hides the projection.

**3. Agent failures are diagnostic of task structure, not just model capability.** When an agent fails, the failure has a structure: at the oracle, at decomposition, at tool selection, at context reconstruction, at verification. That structure is information about the task as much as about the agent. Pass-rate erases it.

---

## The diagnostic gap

Existing benchmarks rarely answer the questions that would explain their own results:

1. Why do these problems form a *task family*?
2. What are the boundaries of the task — what is in scope, what is out?
3. What are the dominant *failure modes*, and what do they implicate?
4. What cognitive or operational abilities does the task actually depend on?
5. What is *observable* to the agent, and what has been hidden by interface choice?
6. Is the score measuring capability, or is it measuring the joint product of model + harness + prompt + scaffold + oracle?
7. What information is *lost* when the task is compressed into evaluable samples?

A repo can answer those questions for a benchmark only if the underlying task object has been described. That is what this project provides.

---

## The residual structure

Five mirrors that show the gap from the other side:

- We compare models without first defining the tasks the models are competing on.
- We build benchmarks without stating the theory of task they project.
- We discuss agent failure without attributing it to task structure.
- We optimize harnesses without recognizing that the harness changes the task.
- We pursue certainty when the task itself is an *ambiguity-bearing object*.

---

## Positioning

This is not a critique of any particular benchmark. It is an observation about a class of objects: tasks are treated as given, but they are the most under-explained object in agent evaluation. Naming them — primitives, dimensions, projections, distortions — is the first move that makes benchmark and harness choices auditable.

---

## How this connects

**Research layer** — *what is a task.* Primitives, dimensions, projections, distortions. The vocabulary of [PRIMITIVES.md](./PRIMITIVES.md) and [DIMENSIONS.md](./DIMENSIONS.md).

**Evaluation layer** — *how do benchmarks instantiate tasks.* [BENCHMARK-MAPS.md](./BENCHMARK-MAPS.md) re-describes existing benchmarks in this vocabulary, surfacing what each leaves visible and what each hides.

**Engineering layer** — *how agent systems behave differently across task structures.* [CASE-STUDIES.md](./CASE-STUDIES.md) walks through worked examples, including reframing agent regression as *task-conditional behavior shift*.

---

## How to read this repo

- **Want to use it in 10 minutes?** [QUICKSTART.md](./QUICKSTART.md) — three diagnostic templates, with worked applications in [examples/](./examples/).
- New here? Start with [PRIMITIVES.md](./PRIMITIVES.md) — the building blocks of any task description.
- Want to know the axes? [DIMENSIONS.md](./DIMENSIONS.md) — the properties along which tasks vary.
- Want concrete? [BENCHMARK-MAPS.md](./BENCHMARK-MAPS.md) and [CASE-STUDIES.md](./CASE-STUDIES.md).
- Looking up a term? [GLOSSARY.md](./GLOSSARY.md).
- Want to extend the ontology? [CONTRIBUTING.md](./CONTRIBUTING.md) — read the *extractive constraint* before opening a PR.

---

## Status

**v0.1** — intentionally incomplete. The aim of this release is to lock the vocabulary, not to enumerate every possible task. Extension is governed by the extractive constraint in [CONTRIBUTING.md](./CONTRIBUTING.md): *every primitive or dimension must come with a concrete signature in benchmark spec, harness config, or agent trace. If you cannot point to where it would be observed, do not add it.*

> **Without a task ontology, agent evaluation collapses into leaderboard engineering.**

> *You are not building another benchmark. You are defining the object that exists before the benchmark.*

---

## Cite

If you use this vocabulary in published work:

```bibtex
@software{chen_2026_task_ontology,
  author  = {Caesar Chen},
  title   = {{Task Ontology: A Substrate for Agent Evaluation}},
  year    = {2026},
  version = {0.1.1},
  doi     = {10.5281/zenodo.19828487},
  url     = {https://github.com/0error-ob/task-ontology}
}
```

Machine-readable metadata: [CITATION.cff](./CITATION.cff) · DOI: [10.5281/zenodo.19828487](https://doi.org/10.5281/zenodo.19828487)
