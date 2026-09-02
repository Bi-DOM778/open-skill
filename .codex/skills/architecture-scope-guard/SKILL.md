---
name: architecture-scope-guard
description: Use when an architecture proposal, shared infrastructure change, or repeated correction chain may be broader or riskier than the current user outcome.
---

# Architecture Scope Guard

Choose the smallest design that safely delivers the current user outcome. The
agent owns technical inspection and verification; the owner decides product
outcomes, acceptable tradeoffs, and whether real sensitive or external effects
may occur.

## When To Use

Use this skill when:

- a proposal adds a component, service, table, protocol, state machine, shared
  interface, dependency, runner, or governance mechanism;
- a small user outcome is accumulating speculative flexibility or platform
  work;
- the same root mechanism keeps producing correction tasks or support tooling;
- a task needs proportional Fast, Controlled, or Strict verification;
- a non-technical owner needs consequences rather than unexplained code detail.

Do not use it merely to read one known file, report Git status, execute a
frozen mechanical command, or perform an ordinary review with no architecture,
scope, or risk decision.

## Responsibility Boundary

The owner decides:

- the user or business outcome;
- priorities and acceptable product tradeoffs;
- whether real private data, money, devices, deletion, or publication may be
  used;
- whether to continue after residual risk is explained plainly.

The agent independently:

- inspects relevant code, diffs, tests, existing patterns, and primary sources;
- identifies the minimum viable design and challenges unnecessary scope;
- maps current effects to the applicable risk lane;
- verifies the authorized outcome and explains remaining uncertainty.

Never ask the owner to certify implementation details they cannot reasonably
inspect. Translate technical choices into observable consequences first.

## Core Decisions

1. **Solve today's requirement.** Do not add speculative configurability,
   abstraction, future integrations, or platform layers.
2. **Reuse before inventing.** Prefer existing project patterns, mature tools,
   standards, and appropriately licensed reference implementations.
3. **Change surgically.** Every changed file and concept must trace to the
   current outcome. Preserve unrelated work.
4. **Verify the outcome.** Tests, diffs, builds, and runtime behavior outrank
   document volume, source counts, model agreement, and self-graded prose.
5. **Apply controls to actual effects.** A harmless preparatory step does not
   inherit the strictest controls of a possible future operation.
6. **Separate audit from routine context.** Broad reading can establish an
   authority map once; routine work should use the smallest current context
   that can decide and verify the task.
7. **Treat documentation as intent memory, not backup.** Exact code and data
   require committed, independently backed-up artifacts.

## Risk Lane Selection

Classify effects, not task labels or importance:

- **Fast:** public or synthetic, local, reversible, repeatable work with no new
  sensitive, shared runtime, or governance-authority boundary.
- **Controlled:** reversible changes to shared interfaces, dependencies,
  networking, concurrency, lifecycle, permissions, persistent shapes,
  background work, or authoritative workflow/policy documents.
- **Strict:** real credentials or private data, authentication/authorization,
  encryption, destructive real-data operations, signing, publication,
  meaningful spend, or genuinely irreversible execution.

Risk lanes choose evidence and controls; they never grant permission. Existing
repository rules, task scope, owner approvals, and execution budgets remain
binding.

Unknown technical complexity may be investigated as Controlled. Unknown data
sensitivity, authorization, reversibility, publication, or cost blocks the
affected action until clarified.

Split mixed tasks by effect. A public design may be Fast, its shared interface
Controlled, and a live authenticated call with private content Strict. Earlier
steps never authorize later ones.

When the lane affects a task or gate, read
[references/risk-lanes.md](references/risk-lanes.md). Project-specific examples
belong in versioned project instructions or policy, not this global skill.

## Minimum Sufficient Architecture Review

Before approving new architecture, answer:

1. What observable capability does this unlock now?
2. What existing pattern, mature tool, or public reference can be reused?
3. What is the smallest working design?
4. Which concrete risk trigger requires each extra control?
5. What acceptance result would fail if half the proposed pieces were removed?
6. Is support machinery becoming harder to validate than the capability?
7. Does an abstraction have concrete current consumers, or a real safety,
   external-boundary, replacement, or test-isolation reason to exist now?

Record only a lightweight complexity budget: changed files, new concepts,
persistent entities, states, dependencies, and externally visible behaviors.
These are review prompts, not universal numeric limits.

## Repeated-Failure Reset

Pause narrow patching and reassess architecture when evidence shows the same
root mechanism or failure class recurring, especially when each fix adds a
state, protocol field, wrapper, report, verifier, or gate. Also reset when a
helper blocks product value longer than the capability it supports, or when
verification mostly proves the helper itself.

The number of corrections alone is not a hard gate. Independent defects may
need independent fixes. The reset trigger is repeated root cause, expanding
support machinery, or worsening value-to-complexity ratio.

Compare:

- remove the mechanism;
- replace it with a mature tool or existing pattern;
- reduce it to a task-specific implementation;
- keep hardening only if it is confirmed as durable shared infrastructure.

Do not preserve prior effort merely because it already cost time.

## Reference-Product Use

Use reference products to establish expected user behavior and identify proven
patterns, only when access is permitted and licensing or provenance is clear.
Do not infer private internals, copy proprietary source or prompts, or import a
large product's scale into a smaller project without a current requirement.

## Evidence And Owner Output

- A passing test proves only what it exercises.
- Static shape, parser success, counts, and report assertions do not prove
  runtime behavior.
- Review blockers must identify a concrete user, data, security,
  maintainability, or recovery consequence.
- Use representative failure cases unless the selected risk lane justifies
  broader coverage.

Report:

1. current user outcome;
2. selected lane and exact trigger;
3. smallest viable approach;
4. complexity rejected or justified;
5. independent evidence actually checked;
6. remaining practical risk;
7. only the product or risk decision the owner must make.

For behavioral regression testing of this skill, use
[references/evaluation-scenarios.md](references/evaluation-scenarios.md).
