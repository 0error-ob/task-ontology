# Glossary

One-line entries with cross-references into the source documents. Refresh by extraction whenever a primitive, dimension, or definition changes.

---

## Primitives

- **Actor** — the system-under-evaluation as a delimited entity. → [PRIMITIVES §1](./PRIMITIVES.md#1-actor)
- **Goal** — the terminal condition or transformation that defines what *done* means. → [PRIMITIVES §2](./PRIMITIVES.md#2-goal)
- **Initial state** — what the actor inherits at t=0. → [PRIMITIVES §3](./PRIMITIVES.md#3-initial-state)
- **Target state** — the state that satisfies the goal predicate. → [PRIMITIVES §4](./PRIMITIVES.md#4-target-state)
- **Constraints** — rules and budgets that exclude regions of the action space. → [PRIMITIVES §5](./PRIMITIVES.md#5-constraints)
- **Tools** — action affordances and their preconditions. → [PRIMITIVES §6](./PRIMITIVES.md#6-tools)
- **Environment** — everything that mutates outside the actor's deliberation. → [PRIMITIVES §7](./PRIMITIVES.md#7-environment)
- **Feedback** — signals returned to the actor during execution. → [PRIMITIVES §8](./PRIMITIVES.md#8-feedback)
- **Oracle** — what determines correctness, when, and by what authority. → [PRIMITIVES §9](./PRIMITIVES.md#9-oracle)
- **Cost** — units of effort consumed in producing a trajectory. → [PRIMITIVES §10](./PRIMITIVES.md#10-cost)
- **Risk** — the irreversibility surface and blast radius of failures. → [PRIMITIVES §11](./PRIMITIVES.md#11-risk)
- **Time horizon** — minimum, expected, and maximum trajectory length. → [PRIMITIVES §12](./PRIMITIVES.md#12-time-horizon)
- **Decomposition structure** — how the task naturally breaks into subtasks, or refuses to. → [PRIMITIVES §13](./PRIMITIVES.md#13-decomposition-structure)
- **Verification regime** — who or what re-checks the result, against what reference. → [PRIMITIVES §14](./PRIMITIVES.md#14-verification-regime)

## Dimensions

- **Goal clarity** — explicit / latent / contested. → [DIMENSIONS §1](./DIMENSIONS.md#1-goal-clarity)
- **State observability** — full / partial / hidden. → [DIMENSIONS §2](./DIMENSIONS.md#2-state-observability)
- **Oracle availability** — rich / poor / absent. → [DIMENSIONS §3](./DIMENSIONS.md#3-oracle-availability)
- **Decomposition depth** — single-shot / shallow / deep. → [DIMENSIONS §4](./DIMENSIONS.md#4-decomposition-depth)
- **Tool dependence** — none / optional / required. → [DIMENSIONS §5](./DIMENSIONS.md#5-tool-dependence)
- **Feedback latency** — immediate / delayed / terminal-only. → [DIMENSIONS §6](./DIMENSIONS.md#6-feedback-latency)
- **Environment mutability** — static / actor-driven / exogenous. → [DIMENSIONS §7](./DIMENSIONS.md#7-environment-mutability)
- **Failure reversibility** — reversible / costly / irreversible. → [DIMENSIONS §8](./DIMENSIONS.md#8-failure-reversibility)
- **Context locality** — local / repo-scale / cross-system. → [DIMENSIONS §9](./DIMENSIONS.md#9-context-locality)
- **Ambiguity load** — low / medium / high. → [DIMENSIONS §10](./DIMENSIONS.md#10-ambiguity-load)
- **Verification hardness** — cheap / expensive / undecidable. → [DIMENSIONS §11](./DIMENSIONS.md#11-verification-hardness)
- **Coordination requirement** — solo / human-in-loop / multi-agent. → [DIMENSIONS §12](./DIMENSIONS.md#12-coordination-requirement)
- **Regression risk** — none / local / global. → [DIMENSIONS §13](./DIMENSIONS.md#13-regression-risk)

## Concepts

- **Constructed task** — the task as cut and framed by the modeler, harness designer, evaluator, or observer; the third layer of the task hierarchy. → [README](./README.md#the-three-layers-of-a-task)
- **Diagnostic gap** — the seven questions current benchmarks rarely answer about the tasks they project. → [README](./README.md#the-diagnostic-gap)
- **Extractive constraint** — every primitive or dimension in this repo must come with a concrete signature in benchmark spec, harness config, or agent trace. → [CONTRIBUTING](./CONTRIBUTING.md#the-extractive-constraint)
- **Operational vs. intended goal** — the gap between what the harness scores and what the user actually wants. → [PRIMITIVES §2](./PRIMITIVES.md#2-goal)
- **Projection** — the process by which a benchmark fixes one cut and one interface over an underlying task; benchmarks are projections, not measurements. → [README](./README.md#three-claims)
- **Residual structure** — five mirrors describing what is missed when tasks are treated as given objects. → [README](./README.md#the-residual-structure)
- **Surface / Structural / Constructed task** — the three layers at which a task can be described. → [README](./README.md#the-three-layers-of-a-task)
- **Task-conditional behavior shift** — the correct unit of analysis for "agent regression," in place of a scalar pass-rate change. → [CASE-STUDIES §2](./CASE-STUDIES.md#2-what-does-it-mean-for-an-agent-to-regress)
