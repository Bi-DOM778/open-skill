# Risk Lane Decision Guide

Use this guide when a task needs an explicit Fast, Controlled, or Strict
classification. The agent performs the technical classification and explains
it in consequences the owner can judge. Do not ask a non-technical owner to
certify code or infer the lane from implementation details.

## What The Lane Controls

The lane sets proportional verification, review, retry, rollback, and runtime
evidence. It does not:

- authorize work, network access, credentials, private data, publication, or
  retries;
- override repository rules, task cards, frozen allowlists, or owner gates;
- turn a passing test into acceptance of behavior the test did not exercise;
- make every adjacent step inherit the highest possible future risk.

## Decision Tree

Classify the effect that can occur in the current task or execution.

1. Does it read, write, transmit, expose, delete, migrate, publish, sign, or
   spend using real sensitive assets or an irreversible operation?
   - Yes: Strict for that boundary.
2. Otherwise, does it change a shared runtime contract, dependency, network
   path, concurrency, lifecycle, permission flow, background work, or local
   data shape?
   - Yes: Controlled.
3. Otherwise, is it public or synthetic, local, repeatable, reversible, and
   isolated from sensitive assets and shared runtime boundaries?
   - Yes: Fast.
4. If technical complexity is unknown but sensitive effects are ruled out, use
   Controlled while investigating.
5. If data sensitivity, authorization, reversibility, cost, deletion, or
   publication is unknown, stop the affected action until it is clarified.
   Do not downgrade an unknown consequence to Controlled.

Use the highest triggered lane only for the indivisible unit being approved.
Split a task when steps have materially different effects.

## Trigger Matrix

| Effect | Fast | Controlled | Strict |
| --- | --- | --- | --- |
| Data | Public fixtures, synthetic records | New empty schema or synthetic local state | Real private conversations, prompts, memory, credentials, or destructive migration |
| Network | None or fully mocked | Reversible public endpoint test without secrets/private payloads | Authenticated call, private payload, meaningful spend, or publication |
| Permissions | None | New reversible OS/app permission flow | Permission exposes or transmits sensitive real content, or crosses an authorization boundary |
| Persistence | None or disposable fixtures | New local shape with synthetic data and no real migration | Real long-term private data, encryption, backup/restore, deletion, or migration |
| Architecture | Local implementation detail | Shared interface, dependency, concurrency, lifecycle, or background scheduler | Security/auth boundary or irreversible cross-system contract |
| Delivery | Local repeatable build/test | Shared build or dependency change | Release signing, supply-chain publication, store/public release |
| Operation | Cheap and repeatable | Bounded reversible operation | Costly, destructive, externally visible, or genuinely one-shot operation |
| Documentation | Descriptive, non-authoritative content | Agent instructions, workflow policy, permissions, task templates, or other governance authority | Document contains/exposes real secrets or private evidence; classify the later live action separately |

## Fast Lane

Use Fast only when every current effect remains public or synthetic, local,
reversible, repeatable, and isolated from new sensitive/shared boundaries.

Typical examples:

- descriptive non-authoritative documentation and reference-product comparison;
- UI layout or copy using fake state;
- fake providers and public fixtures;
- ordinary local bug fixes and focused refactors;
- deterministic unit tests over synthetic data.

Minimum controls:

- focused scope and clean diff;
- relevant tests and build or static checks;
- direct diff review;
- correction in the same task while the goal and risk boundary remain stable.

Do not add by default:

- pre-execution source freeze;
- numeric one-shot budgets;
- mandatory independent Review;
- exhaustive adversarial matrices;
- a new protocol, runner, or evidence subsystem.

Escalate to Controlled when the fix changes a shared interface, dependency,
permission, lifecycle, concurrency, background behavior, or persistent shape.

## Controlled Lane

Use Controlled for reversible engineering changes with meaningful shared or
runtime effects but without a Strict asset or irreversible action.

Typical examples:

- introducing or changing a shared provider interface using fake data;
- adding a dependency or local schema before any real-data migration;
- permission UI and denial/recovery behavior using non-sensitive fixtures;
- cancellation, concurrency, lifecycle, or background-work changes;
- a repeatable public network probe with no secret and no private payload.
- authoritative agent instructions, workflow policy, permission rules, task
  templates, or automation configuration.

Minimum controls:

- explicit success, failure, cancellation, and recovery assertions where
  applicable;
- focused regression tests plus build/static checks;
- rollback or disable path proportional to the change;
- bounded retries based on actual cost and failure mode;
- independent Review only for the specific boundary that warrants it.

Controlled does not automatically require every failure permutation. Cover
representative and boundary-relevant cases. Escalate only when real sensitive
assets, authorization, irreversible effects, meaningful spend, or publication
enter the task.

## Strict Lane

Use Strict only for a concrete high-consequence boundary, including:

- real API keys, authentication, authorization, or credential storage;
- private prompts, conversations, long-term memory, or identifying media;
- encryption and key lifecycle;
- destructive real-data migration, delete/reset, backup, or restore;
- release signing, supply-chain publication, app-store/public release;
- externally visible publication, meaningful spend, or a truly non-repeatable
  execution.

Select controls from the actual threat and failure model; Strict is not a
synonym for maximal paperwork. Applicable controls may include:

- threat, privacy, retention, and exposure boundaries;
- exact stop, rollback, recovery, deletion, and cleanup behavior;
- frozen source identity and numeric execution budgets when the operation is
  genuinely non-repeatable or costly;
- qualified independent Review;
- authorized real runtime evidence;
- final owner authorization for the real sensitive or external effect.

Do not require a numeric budget when time, retries, or volume have no material
cost or safety consequence. Do not ask for private evidence to be persisted in
reports merely to prove the gate ran.

## Mixed-Task Splitting

Split by effect when possible:

```text
public research              -> Fast
synthetic interface design   -> Controlled
fake implementation/tests    -> Fast or Controlled by shared impact
credential storage           -> Strict
real private runtime call    -> Strict
sanitized result summary     -> Fast after the sensitive boundary is closed
```

Each step keeps its own authorization and evidence. Completion of an earlier
lane never authorizes a later one.

## Project Calibration

Keep project-specific examples, named interfaces, data classes, authorization
phrases, stage gates, and task-card fields in the versioned project repository.
This global guide supplies only the generic decision model. When project policy
is stricter because of a concrete domain risk, follow it. When it is stricter
only because of historical habit, surface the mismatch for owner review rather
than silently bypassing it.

## Escalation And De-Escalation

Escalate when inspection discovers a concrete higher-lane effect. Stop before
crossing it, explain the consequence, and obtain whatever authorization the
repository requires.

De-escalate when the higher-risk effect is removed or isolated, for example by
replacing real data with synthetic fixtures, mocking the network, or moving
publication into a separate task. Record the removed trigger; do not silently
lower the lane to save process.

Architecture complexity alone does not justify Strict. Repeated failure alone
does not justify Strict either. Repeated root cause, expanding support
machinery, or worsening value-to-complexity ratio triggers architecture reset.

## Owner-Facing Classification

Report the classification in this compact form:

```text
Lane: Controlled
Trigger: changes a shared provider interface and cancellation lifecycle
Not triggered: no real key, private prompt, persistence, publication, or spend
Controls: focused success/failure/cancel tests, build, diff review
Escalation point: first authenticated call or private payload
Owner decision: approve the product tradeoff, not the code details
```

The owner may need to answer only consequence questions such as whether real
private data, money, devices, deletion, or publication may be used. The agent
is responsible for mapping the implementation to the lane.

## Failure Patterns

Reject these classifications:

- "Strict because the project is important."
- "Strict because this may be used with credentials someday."
- "Fast because the code change is small."
- "Fast because only documentation changed."
- "Fast because tests pass."
- "Controlled because the owner does not understand the code."
- "All steps are Strict because the final release is Strict."
- "Review approval grants runtime or publication permission."

The deciding factor is the current observable effect and consequence, not
importance, code size, uncertainty, model confidence, or document length.
