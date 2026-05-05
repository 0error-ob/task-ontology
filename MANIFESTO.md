# Manifesto

The long-form argument behind Task Ontology.

---

## What this is not

- **Not** a planning formalism in the MDP / POMDP tradition.
- **Not** a cognitive task analysis taxonomy in the GOMS / KLM / HCI tradition.
- **Not** a specification language for RL environments or agent harnesses.

Those traditions answer *how to solve a task once it has been formalized.* This project asks the prior question: *how is a task formalized in the first place, and what does that choice cost?*

---

## A motivating example: what is a "coding task"?

The phrase *"AI helps engineers write code"* names a real problem. But the moment we try to instantiate it — as a product, a benchmark, a workflow — we have to cut. And how we cut determines almost everything that follows.

The same underlying problem can be cut as:

| Cut | What the actor is actually asked to do |
|---|---|
| Coding benchmark | Produce a patch that passes the test suite |
| Debugging task | Localize and explain the root cause |
| Agent planning task | Decompose work into sound subgoals |
| Tool-use task | Select, sequence, and parameterize tools efficiently |
| Repo-navigation task | Build a working map of an unfamiliar codebase |
| Regression-control task | Avoid breaking invariants elsewhere |
| Human-AI collaboration task | Remain legible and steerable to a co-working developer |
| Cost-accuracy optimization | Succeed within a token / wallclock / dollar budget |

Each cut is a different task. Not a different *version* of the same task — a structurally different object with a different Actor boundary, different Transform Type, different Feedback, different Risk profile. A model that excels at one cut may be mediocre at another. The label "coding" hides this entirely.

**The cut is the theory.** Most of the field debates models. Very little of the field debates the cut.

---

## The three layers of a task

| Layer | Definition | Where it lives |
|---|---|---|
| **Surface** | The problem as stated: input, instruction, expected output format. | Most task descriptions stop here. |
| **Structural** | The actual transformation required: state-change under constraints, with tools, environment, feedback, decomposition. | Where agent behavior is actually determined. |
| **Constructed** | The task as designed by its framer: boundary decisions, success criteria, observation rules, what gets left out. | Where the theory lives — often invisible to the agent and the evaluator alike. |

A task ontology has to operate at all three layers. The surface is what you read; the structural is what you do; the constructed is the set of choices that made the surface look the way it does.

---

## Three claims

**1. Tasks are constructed, not given.** No agent task arrives free of framing. A modeler chose where to draw the actor boundary, what counts as input, what the success signal is, what the agent is allowed to observe. Those choices are theory-laden and propagate downstream into every behavior the agent produces.

**2. Tasks are configurations, not categories.** Two tasks labeled "coding" or "analysis" may share a surface label while differing across every structural dimension: goal clarity, transform type, feedback availability, horizon, risk. The category label explains nothing about which agents succeed or why. The configuration does.

**3. Agent behavior is a function of task structure.** When an agent succeeds or fails, the explanation is in the joint object of (agent, task structure) — not in agent capability alone. An agent that fails at a Generation task may succeed at the equivalent Extraction task. An agent that performs well in a low-risk, oracle-rich setting may behave entirely differently under high irreversibility and weak feedback. Task structure is not context; it is the independent variable.

---

## Positioning

This is not a critique of any particular benchmark or evaluation method. It is a claim about a class of objects: tasks are structurally rich objects that exist prior to any evaluation framework. Naming that structure — primitives, dimensions, configurations — is the precondition for reasoning clearly about what agents can and cannot do, and why.

Evaluation is downstream of task definition. You cannot interpret a score without first knowing what task was actually constructed.
